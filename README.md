===============
================
二维码扫描和条码扫描，扫描界面不同
=============
扫描线扫描范围修改：drawScanner（）

translateY += 2f;
 
if (translateY >= CaptureActivity.bar_qcode_line) {  //tchl   670
            
         translateY = 5f;
                
}
         
================================================

扫描框大小修改：getFramingRect(). 

如高度的修改

 Log.i("tchl","!!!"+CaptureActivity.bar_qcode_rect)
 
            if (height < MIN_FRAME_HEIGHT) {
            
                height = MIN_FRAME_HEIGHT/ CaptureActivity.bar_qcode_rect;
                
            } else if (height > MAX_FRAME_HEIGHT) {
            
                height = MAX_FRAME_HEIGHT/CaptureActivity.bar_qcode_rect;
                
            }
=====================

=========
直接使用时将 MainActivity.java 中startActivity的SimpleCaptureActivity改为CaptureActivity。 

条码扫描时，点击返回按钮，会出现短暂的二维码扫描框的样式。这是因为创建view时用的是二维码扫描框的样式。

修改的地方：
         ViewfindView.java onDraw()
         
         frame = new Rect(leftOffset + RECT_OFFSET_X,
         
                    topOffset  + RECT_OFFSET_Y,
                    
                    leftOffset + width + RECT_OFFSET_X,
                    
                    (topOffset + height/CaptureActivity.bar_qcode_rect  + RECT_OFFSET_Y));//tchl
                    
            同时修改 “扫描框大小修改：getFramingRect(). ”(详见上一条)



QRCode
===
[![JitPack](https://jitpack.io/v/XuDaojie/QRCode-Android.svg)](https://jitpack.io/#XuDaojie/QRCode-Android)
[![API](https://img.shields.io/badge/API-14%2B-orange.svg?style=flat)](https://android-arsenal.com/api?level=14) <br>
本项目依赖于[ZXing](https://github.com/zxing/zxing) 3.2.1<br>
对6.0以上和第三方ROM自带的权限增加了权限判断

![Scan](https://github.com/XuDaojie/QRCode-Android/blob/master/art/scan_qrcode.gif)

## Use

### Add permission
``` xml
<uses-permission android:name="android.permission.CAMERA"/>
<uses-permission android:name="android.permission.VIBRATE" />
<uses-permission android:name="android.permission.FLASHLIGHT" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
```

### Register activity
``` xml
<activity android:name="io.github.xudaojie.qrcodelib.CaptureActivity"/>
```

### Code
`CaptureActivity` 对二维码扫描信息默认是将扫描结果传回上一个界面,如果你要在`CaptureActivity`中直接处理,可通过重写`handleResult(String resultString)`实现

#### 直接使用 

``` java
Intent i = new Intent(mContent, CaptureActivity.class);
startActivityForResult(i, REQUEST_QR_CODE);
```

``` java
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (resultCode == RESULT_OK
            && requestCode == REQUEST_QR_CODE
            && data != null) {
        String result = data.getStringExtra("result");
        Toast.makeText(MainActivity.this, result, Toast.LENGTH_SHORT).show();
    }
}
```

#### 重写handleResult(String resultString)

``` java
public class SimpleCaptureActivity extends CaptureActivity {
     @Override
    protected void handleResult(String resultString) {
        if (resultString.equals("")) {
            Toast.makeText(mActivity, R.string.scan_failed, Toast.LENGTH_SHORT).show();
            restartPreview();
        } else {
            // TODO: 16/9/17 ... 
//        restartPreview();
        }
    }
}
```

> Tips<br>
> 如果你的操作不会触发`SimpleCaptureActivity`的`onPause`、`onResume`生命周期,则需要在完成操作后,调用`restartPreview()`以重新启动扫描

## Including in your project

### Add repository
    
想引入你的项目，需要修改你的build.gradle
``` gradle
repositories {
    maven { url "http://repo1.maven.org/maven2" }
    maven { url 'https://jitpack.io' }
}
```

### Add dependency
``` gradle
compile 'com.github.XuDaojie:QRCode-Android:v0.4.2'
```

## 吃水不忘挖井人
[ZXing](https://github.com/zxing/zxing)<br>
[zxing扫描二维码和识别图片二维码及其优化策略](http://iluhcm.com/2016/01/08/scan-qr-code-and-recognize-it-from-picture-fastly-using-zxing/)<br>

## TODO
生成二维码
