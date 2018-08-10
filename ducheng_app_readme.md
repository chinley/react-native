## 项目上手说明

### 一、项目构建注意事项

- 项目脚手架为：  
  https://github.com/GeekyAnts/react-native-boilerplate-mobx-flow  
可此进行技术选型并下载相应版本：  
  https://reactnativeseed.com

- 若使用vscode作为编程器，decorator语法会报错(后续新vscode版本可能会支持此语法)，       可以项目要目录新增文件： tsconfig.json,  
配置内容为： (注意下面的 ` ` 仅仅是marddown文档的语法) 
` {
    "compilerOptions": {
        "experimentalDecorators": true,
        "allowJs": true
    }
} `  

- expo下载，官网可以直接下载apk https://expo.io/tools#client

- 关于expo app报错问题"React Native version mismatch" ，可参考：  
https://github.com/expo/expo/issues/1077  
尝试HugoMatilla的回答

- 推荐安装npm 4.6.1版本，因为npm 5有bug，ReactNative团队推荐使用4.6.1
- 项目启动： 
>     1.运行手机上expo应用，并让手机与电脑处于同一wifi中
>     2.在文件目录下运行终端npm start命令
>     3.在expo中输入url，等待加载 注意：ios版的expo无url输入框，若无法显示项目，可在终端运行exp run ios命令尝试
- 在genymotion上进行模拟的时候，**一定要关闭电脑的防火墙**，并且要把手机的wifi连上 （好像5.0以上的版本不会自动连接wifi），将模拟器的ip和端口设置为可访问本机的ip，如果是在手机上模拟，打开网页尝试是否可以访问，如果无法访问，关闭防火墙再尝试

- 项目打包：https://docs.expo.io/versions/v29.0.0/distribution/building-standalone-apps


### 二、相关规范的约定
- 脚手架的screens的命名为home/index.js, 所以工程内有多个index.js，这不利于在编辑器内快速打开文件（例如vscode ctrl+p,可以通过文件名模糊搜索打开文件），所以命名可定为类似于： homeIndex.js，甚至homeIndexScreen.js
- 脚手架中store里的ViewStore是用于检查输入验证，DomainStore是存放数据接口



### 三、部分设计参考
- 登录页处理，通过react-navigator的SwitchNavigator处理  
https://reactnavigation.org/docs/auth-flow.html


### 四、应用功能模块说明（模块划分、命名等）
- store目录存放数据以及接口，container目录存放界面中调用的方法，store目录存放纯界面代码，util存放工具类

### 五、样板代码（新增功能模块的样板代码）
- 定位：https://docs.expo.io/versions/v29.0.0/sdk/location

>  let location = await Location.getCurrentPositionAsync({});

>  const lng = location.coords.longitude

>  const lat = location.coords.latitude



- 扫码：https://docs.expo.io/versions/v29.0.0/sdk/camera

>     barcodeReceived = async (e) =>{
> 
>     if (e) {
> 
>      const customer_code = e.data
>            
>     } else {
> 
>       Alert.alert(
>        '提示',
>         '扫描失败'
>         {text:'确定'}])
>         
>      }
>      
>     }




### 六、编码过程遇到的问题及相关解决方案
#### 1.红屏报错
- 说明你要跳转的页面缺少子控件。所以检查是否缺少：`<View></View>`

  **注意：在本项目中`<Button></Button>`之间不可直接添加文字，需要嵌套`<Text></Text>`组件**
  
  **例：`<Button><Text>这是按钮</Text></Button>`**
  
  ![](https://upload-images.jianshu.io/upload_images/436736-5d363498f41d96ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/365)
  

- 首先检查包服务器是否运行正常。
  在项目文件夹下输入react-native start或者npm start均可开启服务器，但是需要在PC端确认包服务器是否运行正常。
  
  ![](https://upload-images.jianshu.io/upload_images/436736-1e2e20e8b0deea60.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/291)

- 说明没有在/src/App.js中注册场景，需要添加

  ![](https://upload-images.jianshu.io/upload_images/436736-2056d93ac3d77180.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/300)

- 设置背景图时出现以下报错

  **使用`<ImageBackground>`组件代替`<Image>`组件**
  
  ![](https://upload-images.jianshu.io/upload_images/2093433-1ce34d22124a4eea.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/672)
- Reload JS 多几次即可解决

  ![](https://upload-images.jianshu.io/upload_images/436736-2e0f228bb0de7a8f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/375)
- JSX语法必须用{}对变量进行赋值，检查是否缺少`{}`、`<>`、`,`

  ![](https://upload-images.jianshu.io/upload_images/436736-813059f0bda53166.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/375)
- 一般说明这个包与react native版本不匹配，检查组件或者react native版本是否过久或者过新 

  ![](https://upload-images.jianshu.io/upload_images/436736-4723b21ed6bb5431.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/375)
- undefined is not an object 
  
 1.检查有无拼写错误 
 
 2.对于一些方法添加`bind.(this)`
 
  ![](https://upload-images.jianshu.io/upload_images/436736-ca5eb06bb3e61509.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/622)
- Adjacent JSX elements must be wrapped in an enclosing tag.
  
  解决方法：` render: function() {return())`里只能包含一个根元素，只能返回一个element

#### 2.黄屏报错

- 在给属性赋值（setState）时没有进行判断，可能出现赋个空值（null）的情况，因此需要添加判断再进行赋值

  ![](https://upload-images.jianshu.io/upload_images/2093433-df0462800da6dd54.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)
- 在给属性赋值（setState）时没有进行判断，可能出现赋个空值（null）的情况，因此需要添加判断再进行赋值

  ![](https://upload-images.jianshu.io/upload_images/436736-a7a430b4cb6d622c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/375)
  

#### 3.功能问题
- 使用列表组件不渲染数据内容

  解决方法：查看数据数组是否越界，多数情况下是由于ListView的子组件渲染错误（如套数据时没有检查undefined等）引起
  
- 使用 `<Image>` 组件不显示图片

  解决方法：1.首先查看图片路径是否正确 2.使用 `<Image>` 组件必须指定宽高，否则react native不显示
  
- 时间显示Invalid Date

  解决方法：RN时间不支持显示 `new Date(YYYY-MM-DD)` 带有横折号的格式，需要转换
  
- 同样的布局在不同的手机上显示有差异

  解决方法：查看是否因为手机系统设置了文字大小，系统设置的文字大小会覆盖界面的文字大小，因而需要在 `<Text>` 组件中设置 `allowFontScaling={false}` 禁止文字跟随系统设置改变大小
- `<FlatList/>`不能被`<ScrollView/>`或者`<Content/>`包围，否则会无法正常调用`onEndReached()` 函数
- 列表数据中的某个键的值更新，界面不更新

  解决方法：React Native的`<ListView/>`和Native Base中的`<List/>`都是根据数据的行数发生改变而更新的，当长度不变而值更新时是不会刷新界面的，因而在发生值改变之后重新对列表数据进行赋值
  
  https://blog.csdn.net/lemongirls/article/details/52539462
  
- 无法定位

  解决方法：目前存在部分手机无法定位的情况，测试出来是android系统版本为7.0+的无法定位。定位功能首先需要用户在设置中开启位置，**并且定位模式设置为高精确度模式**，然后对手机应用进行可定位授权
### 七、遗留问题
- exp build会提示： Warning: Not using the Expo fork of react-native. See https://docs.expo.io/. 风险待评估
- iphoneX适配，可参考：https://reactnavigation.org/docs/en/handling-iphonex.html
- react native本地打包出来的apk和ipa闪退问题
- 并非所有手机都能运行定位功能