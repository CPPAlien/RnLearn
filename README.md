### 创建项目

npm install -g create-react-native-app

create-react-native-app AwesomeProject

以上项目你无法嵌入自己的Native代码，也就是所谓的纯Native项目，这些代码可以运行在一个叫Expo(https://expo.io/)的应用中

 如果需要加入自己的Native代码，需要在工程目录下执行下面命令

npm run eject



npm start

开启RN服务

#### npm run android

R R  // 刷新

adb shell input keyevent 82  //menu 按钮

#### npm run ios

Command + R 刷新

Shift + Command + H  Home键



### 语法

一个js为一个module，多个module可以组成bundle

每个module只能有一个default export



props：子类中使用父类传入的属性

state：子类中定义的属性，可改变（Redux）

一个module中只有一个state，可以在constructor中声明

```
constructor(props) {
    super(props);
    this.state = {showText: true};
```





布局

flexbox



跨平台区分

1，

Platform.OS === 'ios'

iOS判断版本

`parseInt(Platform.Version, 10);`

Platform.OS === 'android'

Android判断SDK版本

`Platform.Version === 25`

2，

```
Platform.select({
      ios: {
        
      },
      android: {
        
      }
```

3，文件名后缀区分

```
BigButton.ios.js
BigButton.android.js
```



### JS代码

```
AppRegistry.registerComponent('FirstApp', () => App);
```

把APP这个Component命名为FirstApp，并注册

注册好后，在java代码中即可使用

```
@Override
protected String getMainComponentName() {
	return "FirstApp";
}
```



### Native代码

#### Native Module

继承 ReactContextBaseJavaModule

方法暴露 @ReactMethod

java —> js

```
Boolean -> Bool
Integer -> Number
Double -> Number
Float -> Number
String -> String
Callback -> function
ReadableMap -> Object
ReadableArray -> Array
```



继承后，需要返回getName，作为改Module在Js中的名字

```
@Override
public String getName() {
	return "TestModule";
}
```



```
UiThreadUtil.runOnUiThread  // 可以在ReactContextBaseJavaModule中使用此，来在Ui线程中运行带脉
getReactApplicationContext()  // 来获得Context
```



#### 注册

把Module加入到 ReactPackage中

让后注册这个ReactPackage

```
@Override
protected List<ReactPackage> getPackages() {
	return Arrays.<ReactPackage>asList(
		new MainReactPackage(),
		new CustomPackage()
	);
}
```



#### 使用

在Js中封装该Module

```
'use strict'

import { NativeModules } from 'react-native';
module.exports = NativeModules.TestModule;  // 注意名字与Java中 getName 名字返回一样
```

然后就可以通过以下方式使用

```
import PTest from './bridge/android/TestModule'
```





#### 生成bundle

```
react-native bundle --platform android --entry-file ./index.android.js  --bundle-output ./main.bundle --assets-dest .
```



```
react-native start // 运行RN服务器
```



#### 数据传递

Native -> RN

```
reactContext.getJSModule(DeviceEventManagerModule.RCTDeviceEventEmitter.class)  
                .emit(eventName, params);  
```

rn -> native   ReadableMap/ReadableArray

native -> rn    WritableMap/WritableArray



```
Native使用Callback/Promise回调RN
```

