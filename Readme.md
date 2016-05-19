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