---
title: Bitmap的在不同的Drawable下的内存占用及分析
date: 2020-09-25 15:02:08
tags: 技术
categories: Android基础
---
>微信公众号：Lucidastar
如有问题或建议，请公众号留言
最近更新：`2020-09-25`

平常我们使用图片的地方很多，比如图片的压缩、加载、裁剪等等。今天我们主要从Bitmap的基础进行了解。
## 问题
### 1、一张图片放到手机中内存是如何计算的？
```
//获取占用内存大小
    public final int getByteCount(Bitmap bitmap) {
        // int result permits bitmaps up to 46,340 x 46,340
        KLog.d("getByteCount:"+bitmap.getByteCount());
        return bitmap.getRowBytes() * bitmap.getHeight();//图片的水平的字节数*图片的高度就是占用的内存
    }
```
这是通过api进行获取的，具体的就不解释了。

>**图片占用的内存=图片的宽度×图片的长度×单位像素占用的字节数**
<!-- more-->
- #### Android中默认单位像素占用的字节数是多少呢？
 ```
 Bitmap bitmap = BitmapFactory.decodeResource(getResources(), R.drawable.forself);
 KLog.d("bitmapConfig:"+bitmap3.getConfig());
 ```
 默认打印：`bitmapConfig:ARGB_8888`也就是默认设置的


| Bitmap.Config       | 值 | 描述 | 占用内存(字节) |
| ---- | ---- | ---- | ---- |
| Bitmap.Config     |  ARGB_8888  | 表示32位的ARGB位图 |4|
| Bitmap.Config   |  ARGB_4444  |   表示16位的ARGB位图 |2|
| Bitmap.Config |  RGB_565  | 表示16位的RGB位图 |2|
| Bitmap.Config |  ALPHA_8  | 表示8位的Alpha位图 |1|

- #### density，densityDpi，targetDensity的区别及关系
| density       | densityDpi | res |
| ---- | :----: | ---- |
|1     |  160  | mdpi |
| 1.5   |  240  | hdpi|
| 2|  320  | xhdpi|
| 3 |  480 | xxhdpi|
| 4 |  640  | xxxhdpi|

> - px = density * dp;
> - density = dpi / 160;
> - px = dp * (dpi / 160);

- #### 同样的一张图片放到不同的drawable文件夹下占用内存是多少呢？
 **使用的是小米node，分辨率是：1440x2560;图片大小：559x372**
```
public final int getByteCount(Bitmap bitmap) {
        // int result permits bitmaps up to 46,340 x 46,340
        KLog.d("getByteCount:"+bitmap.getByteCount());
        return bitmap.getRowBytes() * bitmap.getHeight();//图片的水平的字节数*图片的高度就是占用的内存
    }
```
- drawable-mdpi === 图片占用内存的大小:13308672  width:2236;height:1488(2236x1488x4=13308672)
- drawable-hdpi === 图片占用内存的大小:5916288   width:1491;height:992(1471x991x4=5916288)
- drawable-xhdpi === 图片占用内存的大小:3327168   width:1118;height:744(……)
- drawable-xxhdpi === 图片占用内存的大小:1478080  width:745;height:496(……)
- drawable-xxxhdpi === 图片占用内存的大小:831792  width:559;height:372(……)

这是什么原因呢？图片是同样的图片，只是放到了不同的drawable文件夹下，但是占用的内存是不一样的，但是获取到的bitmap的宽高也是不一样的，占用的内存大，获取的图片宽高就大，这是什么原因呢？我们分析一下。

- 也就是占用内存的大小=图片的宽x图片的高x一个像素占用了4个字节(ARGB_8888)

- 如果把这张放置在xxxhdpi的话，应该不会对图像进行放缩，也就是原始大小，所以我们在前面得到drawable-xxxh   dpi文件夹下，图片大小为559 372是完全可以理解的，就是图片本身的大小。

 - 相同的，如果一张图片很大，如果直接加载到内存中，那就会导致内存溢出，默认最大的内存是16M
```
private static final int DECODE_BUFFER_SIZE = 16 * 1024;
```

>当把图片放置在xxhdpi里面的时候，在xxxhdpi的设备上，图片长 = 559 (3/4) = 745.333，图片宽 = 372 (3/4) = 496，这与上面的测试结果是完全一致的。

>当把图片放置在xhdpi里面的时候，在xxxhdpi的设备上，图片长 = 559 (2/4) = 1118，图片宽 = 372 (2/4) = 744，这与上面的测试结果是完全一致的。

- **结论**
> 1. 当图片放置在不同drawable文件夹中，且只有这一张图片时，运行设备会根据自身的屏幕密度，对图片进行放缩，放缩比例符合前面图上的规则

> 2. 图片文件的大小与在内存中占用的大小没关系，内存中实际占用大小与图片分辨率、像素显示参数有关


### 2、我们如何正确的把一张大图加载到界面的控件上呢(或者说固定大小的ImageView加载一张大图，我们该如何解决呢)？
>比如一张500x500的图，要放到60x60的ImageView的控件上，就没有必要把整张原图都加载到内存中。

引自android中文文档`http://hukai.me/android-training-course-in-chinese/graphics/displaying-bitmaps/load-bitmap.html`
- 为了告诉解码器去加载一个缩小版本的图片到内存中，需要在BitmapFactory.Options 中设置 inSampleSize 的值。例如, 一个分辨率为2048x1536的图片，如果设置 inSampleSize 为4，那么会产出一个大约512x384大小的Bitmap。加载这张缩小的图片仅仅使用大概0.75MB的内存，如果是加载完整尺寸的图片，那么大概需要花费12MB（前提都是Bitmap的配置是 ARGB_8888）。下面有一段根据目标图片大小来计算Sample图片大小的代码示例：
```
public static int calculateInSampleSize(
            BitmapFactory.Options options, int reqWidth, int reqHeight) {
    // Raw height and width of image
    final int height = options.outHeight;
    final int width = options.outWidth;
    int inSampleSize = 1;
    if (height > reqHeight || width > reqWidth) {
    
        final int halfHeight = height / 2;
        final int halfWidth = width / 2;

        // Calculate the largest inSampleSize value that is a power of 2 and keeps both
        // height and width larger than the requested height and width.
        while ((halfHeight / inSampleSize) > reqHeight
                && (halfWidth / inSampleSize) > reqWidth) {
            inSampleSize *= 2;
        }
    }

    return inSampleSize;
}
```
- 为了使用该方法，首先需要设置 inJustDecodeBounds 为 true, 把options的值传递过来，然后设置 inSampleSize 的值并设置 inJustDecodeBounds 为 false，之后重新调用相关的解码方法。

```
public static Bitmap decodeSampledBitmapFromResource(Resources res, int resId,
        int reqWidth, int reqHeight) {

    // First decode with inJustDecodeBounds=true to check dimensions
    final BitmapFactory.Options options = new BitmapFactory.Options();
    options.inJustDecodeBounds = true;
    BitmapFactory.decodeResource(res, resId, options);

    // Calculate inSampleSize
    options.inSampleSize = calculateInSampleSize(options, reqWidth, reqHeight);

    // Decode bitmap with inSampleSize set
    options.inJustDecodeBounds = false;
    return BitmapFactory.decodeResource(res, resId, options);
}
```
- 使用上面这个方法可以简单地加载一张任意大小的图片。如下面的代码样例显示了一个接近 60x60像素的缩略图：
```
mImageView.setImageBitmap(
    decodeSampledBitmapFromResource(getResources(), R.id.myimage, 100, 100));
```

#### inSampleSize是什么呢？有什么作用呢？
 - 如果设置为大于1的值，则请求解码器对原始图像进行子采样，返回较小的图像以节省内存。样本大小是对应于解码位图中单个像素的任一维度的像素数。例如，inSampleSize==4返回图像，该图像为原始图像宽度/高度的1/4，像素数的1/16。任何值<=1将被视为1。注：解码器使用基于2的幂的最终值，任何其他值都将向下舍入到最接近的2的幂次

>这是对inSampleSize的注释翻译

![Lucidastar](https://mmbiz.qpic.cn/mmbiz_jpg/iccib9G9IAFPhSwdibNCmTBzKD57YoqXpT0HhDKLNX4dVHIo44H5IiaOOHxpQUw0c8Yn63vGNBxyJPfYsqpraxiaeyQ/0?wx_fmt=jpeg)

|      |      |      |
| ---- | ---- | ---- |
|      |      |      |
|      |      |      |
|      |      |      |

