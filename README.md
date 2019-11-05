zxing 条形码增强扫码

https://github.com/285298984/BarCodeZXing/blob/master/img/1.png

使用方法
========

1.添加依赖
--------------------
先在 build.gradle(Project:XXXX) 的 repositories 添加``` maven { url 'https://jitpack.io' }```
一定要加上这个，否则会提示依赖失败

```
allprojects {
    repositories {
        google()
        jcenter()
        maven { url 'https://jitpack.io' }
    }
}
然后在 build.gradle(Module:app) 的 dependencies 添加:
 dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:26.1.0'
    
    /*添加依赖*/
    implementation 'com.github.yuzhiqiang1993:zxing:2.2.5'
}

 2.权限
 --------------
 
 需要申请的权限有：

   Manifest.permission.CAMERA
   Manifest.permission.READ_EXTERNAL_STORAGE

   项目中用到的所有权限

   <uses-permission android:name="android.permission.CAMERA" />
   <uses-permission android:name="android.permission.FLASHLIGHT" />
   <uses-feature android:name="android.hardware.camera" />
   <uses-feature android:name="android.hardware.camera.autofocus" />
   <uses-permission android:name="android.permission.VIBRATE" />
   <uses-permission android:name="android.permission.WAKE_LOCK" />
   <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
   <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
   ```

 
3.跳转到扫一扫界面：
--------------

1.使用默认配置项，两行代码即可
Intent intent = new Intent(MainActivity.this, CaptureActivity.class);
startActivityForResult(intent, REQUEST_CODE_SCAN);

2.自定义配置项
Intent intent = new Intent(MainActivity.this, CaptureActivity.class);
/*ZxingConfig是配置类
*可以设置是否显示底部布局，闪光灯，相册，
* 是否播放提示音  震动
* 设置扫描框颜色等
* 也可以不传这个参数
* */
ZxingConfig config = new ZxingConfig();
config.setPlayBeep(true);//是否播放扫描声音 默认为true
config.setShake(true);//是否震动  默认为true
config.setDecodeBarCode(true);//是否扫描条形码 默认为true
config.setReactColor(R.color.colorAccent);//设置扫描框四个角的颜色 默认为白色
config.setFrameLineColor(R.color.colorAccent);//设置扫描框边框颜色 默认无色
config.setScanLineColor(R.color.colorAccent);//设置扫描线的颜色 默认白色
config.setFullScreenScan(false);//是否全屏扫描  默认为true  设为false则只会在扫描框中扫描
intent.putExtra(Constant.INTENT_ZXING_CONFIG, config);
startActivityForResult(intent, REQUEST_CODE_SCAN);


4.接收扫描结果
-------------------------------------------
注意：Constant.CODED_CONTENT引的是这个com.yzq.zxinglibrary.common.Constant

    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        // 扫描二维码/条码回传
        if (requestCode == REQUEST_CODE_SCAN && resultCode == RESULT_OK) {
            if (data != null) {

                String content = data.getStringExtra(Constant.CODED_CONTENT);
                result.setText("扫描结果为：" + content);
            }
        }
    }
