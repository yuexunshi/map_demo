
## 定位集成

#### 配置高德定位依赖库
新建 Flutter 项目，使用 Android Studio 打开项目里的 android 工程，或者右键 android 目录-> flutter -> open Android module in Android Studio。

![1.png](https://upload-images.jianshu.io/upload_images/1454742-724ed8e1798d918a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在 Android 工程里，切换为 Android 视图：

![2.png](https://upload-images.jianshu.io/upload_images/1454742-1d585af5fd5f707d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开 build.gradle 文件，添加定位依赖包：
```
implementation('com.amap.api:location:5.2.0')
```
![image.png](https://upload-images.jianshu.io/upload_images/1454742-701307865d8245c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击 Sync Now 或者 工具栏上的🐘图标同步依赖包：\
![image.png](https://upload-images.jianshu.io/upload_images/1454742-51c7259b10f78827.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开清单文件`AndroidManifest.xml`，配置权限和服务：
```
    <!--访问网络-->
    <uses-permission android:name="android.permission.INTERNET" />
    <!--粗略定位-->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <!--精确定位-->
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <!--申请调用A-GPS模块-->
    <uses-permission android:name="android.permission.ACCESS_LOCATION_EXTRA_COMMANDS" />
    <!--用于获取运营商信息，用于支持提供运营商信息相关的接口-->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <!--用于访问wifi网络信息，wifi信息会用于进行网络定位-->
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <!--用于获取wifi的获取权限，wifi信息会用来进行网络定位-->
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <!--用于读取手机当前的状态-->
    <uses-permission android:name="android.permission.READ_PHONE_STATE" />
    <!--用于写入缓存数据到扩展存储卡-->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```
```
      <!-- 配置定位Service -->
       <service android:name="com.amap.api.location.APSService"/>
```
![image.png](https://upload-images.jianshu.io/upload_images/1454742-28e9754fbb2487de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 配置签名文件
高德平台需要签名的 sha1 ，所以需要配置签名文件，debug 模式调试和正式包各需要一个，也可以使用同一个 keystore 。
工具栏 Build -> Generate Signed Bundle / APK -> 选择 APK -> Next -> Create new ，打开 New Key Store 窗口:

![image.png](https://upload-images.jianshu.io/upload_images/1454742-7e19aba9f882c710.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/1454742-963584257c392fe5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

key store path 选择当前项目根目录或者你喜欢的任何目录，命名为 debug.keystore 或者你喜欢的名字，两个地方的 Password 可以一样，Alias 需要填写，下面的 Certificate 填写一项即可:

![image.png](https://upload-images.jianshu.io/upload_images/1454742-046997fb7ae85b09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击 ok -> 点击 Next，选择 debug ，当然如果 debug 和 release 使用同一个签名文件的话也可同时选择 debug 和 release，并勾选 V2 签名：

![image.png](https://upload-images.jianshu.io/upload_images/1454742-47a794a16675c42b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

完成后可以看到项目里的 debug.keystore 文件：
![image.png](https://upload-images.jianshu.io/upload_images/1454742-eae7e5302c44b5c8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
重复上面方法，再创建一个 release.keystore 文件。点击 Project Structure:
![image.png](https://upload-images.jianshu.io/upload_images/1454742-70274464c63be35d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
选择 Modules -> app -> Signing Configs -> + -> 默认有 debug，再添加一个 release ，分别选择对应的 keystore：

![image.png](https://upload-images.jianshu.io/upload_images/1454742-8a4898b08d2cfb53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击 ok 后再次打开 build.gradle 文件，可以看到签名配置对应刚才配置的签名文件：
```
signingConfigs {
        debug {
            storeFile file('/Users/apple/AndroidStudioProjects/flutter/map_demo/android/debug.keystore')
            storePassword '123456'
            keyAlias 'amap'
            keyPassword '123456'
        }
        release {
            storeFile file('/Users/apple/AndroidStudioProjects/flutter/map_demo/android/release.keytore')
            storePassword '123456'
            keyAlias 'amap'
            keyPassword '123456'
        }
    }
```

### 高德平台 Key 申请

​	打开[高德开放平台 | 高德地图API (amap.com)](https://lbs.amap.com/)，注册成为高德开放平台用户，打开控制台创建一个应用，填入对应的信息，这里有个错误，SHA1 其实是 MD5 ，不要填写 SHA1。
![image.png](https://upload-images.jianshu.io/upload_images/1454742-1345a98e98002373.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
获取MD5:
在刚才的项目里，打开 Terminal,输入下面命令，路径就是 debug.keystore 和 release.keystore的路径：
```
keytool -list -v -keystore  ./debug.keystore
```
输入秘钥库口令，就是设置的 Password ,复制 MD5 ,去高德开放平台粘贴。

![image.png](https://upload-images.jianshu.io/upload_images/1454742-a5ebb31be8163e8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

发布版和调试版都设置后，复制 key 。

在清单文件里，配置`apikey`：
```
    <!-- 配置定位Service -->
       <service android:name="com.amap.api.location.APSService"/>
       <meta-data
           android:name="com.amap.api.v2.apikey"
           android:value="f6c46787c43cb7df5510d9f4c530fd1e"/>
```

### Flutter 文件配置
回到 Flutter 项目，添加高德定位库和权限申请依赖,执行` pub get` ：
```
  amap_flutter_location: ^2.0.0
  permission_handler:
```
在获取定位的入口配置权限申请,注册监听:
```
    /// 动态申请定位权限
    requestPermission();
    ///注册定位结果监听
    _locationListener = _locationPlugin
        .onLocationChanged()
        .listen((Map<String, Object> result) {
      setState(() {
        _locationResult = result;
      });
    });
```
只要获取到定位，这里都会回调，`result`包含了很全的定位信息。
如果运行报错：`INVALID_USER_KEY`,说明你的 keystore 的 MD5 不正确，也许平台会把 SHA1 改为真正的 SHA1 ，所以不妨试试填入 SHA1。

## 地图集成
### Android工程添加地图依赖
回到 Android 工程，添加地图依赖：
```
    implementation 'com.amap.api:3dmap:5.0.0'
```
回到 Flutter 工程，在`pubspec.yaml`里添加插件依赖：
```
  amap_flutter_map: ^2.0.1
```
在布局中使用 map ：
```
  final AMapWidget map = AMapWidget(
      onMapCreated: onMapCreated,
      // 定位小蓝点配置
      myLocationStyleOptions: MyLocationStyleOptions(true),
      // 是否指南针
      // compassEnabled: true,
    );
```
