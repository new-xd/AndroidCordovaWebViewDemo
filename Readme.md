# Android CordovaWebView Demo
这个demo主要用于说明如何在已有的项目中使用cordova 使用CordovaWebView

工程使用eclipse环境，但是与android studio基本步骤一致，相信大家都能明白  
AndroidCordovaWebViewDemo是demo工程
CordovaLib是所依赖的cordova库，是随便从cordova项目中拷贝过来的

# 思路
就是仿照CordovaActivity的实现，做最少的改动

# 步骤
1. 为你的项目添加CordovaLib依赖
2. copy的前端代码www目录到assets目录下，注意cordova.js要是android平台的实现
3. copy config.xml 到 res/xml目录下
4. copy一份CordovaInterfaceImpl实现到你activity所在的包（这里是为了尽量的少改动，其实你也可以自己实现CordovaInterface）
5. 修改你的activity，因为修改内容较多，下面单列出来

# 修改activity （代码中搜索changed可见）
1. 把CordovaActivity的实现都copy过来
2. 自定义自己的布局  

        /*****************changed begin*******************/
        // 你放SystemWebView的容器
        private FrameLayout container;
        
        /*****************changed end*******************/
        
        /****************** changed begin ********************/
        // 在onCreate中 设置自定义的布局 初始化 加载页面
        setContentView(R.layout.activity_main);

        container = (FrameLayout) findViewById(R.id.container);

        init();

        loadUrl(launchUrl);

        /****************** changed end ********************/

3. 将webview添加到你的布局中

        /*****************changed begin*******************/
        // 在createViews 将SystemWebView加载到你自定义的布局中
        //setContentView(appView.getView());
        container.addView(appView.getView());

        /*****************changed end*******************/

4. 修改CordovaInterfaceImpl为你activity包下的CordovaInterfaceImpl
5. 修改引用CordovaActivity的地方，改为你的Activity

# 注意
这个demo主要是演示如何在已有的项目中添加和使用cordova的WebView  
对于想要直接对项目使用plugman命令安装和卸载插件，还需要其他配置  

demo为了做到最少改动，所以将CordovaActivity的实现直接copy过来了  
对于熟悉cordova的开发者，可以自己选择实现内容和实现方法

# 访问web站点上的页面
访问web站点 是需要安装白名单插件的，但是通过命令安装需要做一些配置  
为了方便使用，我把白名单插件内置到了代码里

## 主要改动
1. copy白名单插件源码
2. 在MainActivity中内置白名单插件

        /*****************changed begin*******************/
        // 内置白名单插件
        PluginEntry object = new PluginEntry("whiltelist", new WhitelistPlugin(this));
        pluginEntries.add(object);
        // 这个已经过时 不再使用 大胆的删掉
        //        Config.parser = parser;
        /*****************changed end*******************/

3. 在AndroidManifest.xml中添加网络权限

之后就可以修改config.xml啦，比如

        <content src="https://www.baidu.com/" />