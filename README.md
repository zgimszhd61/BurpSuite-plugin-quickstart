# BurpSuite-plugin-quickstart

创建一个基于Java的Burp Suite插件涉及几个步骤，包括设置开发环境、实现必要的接口以及打包插件。下面我会提供一个简单的示例插件，该插件监听HTTP请求，并将请求的URL打印到标准输出。

### 步骤1: 设置开发环境

1. **安装Java开发工具箱 (JDK)**: 确保你的机器上安装了Java开发工具箱。Burp Suite插件需要Java 8或更高版本。
   
2. **安装IDE**: 推荐使用IntelliJ IDEA或Eclipse，这些IDE对Java有很好的支持。

3. **下载Burp Suite API**: 去 [PortSwigger](https://portswigger.net/burp/documentation/extending-burp/api) 的网站下载Burp Suite的Java API。

### 步骤2: 编写插件代码

下面是一个简单的插件示例，它实现了 `IBurpExtender` 和 `IHttpListener` 接口：

```java
import burp.*;

public class BurpExtender implements IBurpExtender, IHttpListener {
    private IBurpExtenderCallbacks callbacks;
    private IExtensionHelpers helpers;

    @Override
    public void registerExtenderCallbacks(IBurpExtenderCallbacks callbacks) {
        this.callbacks = callbacks;
        this.helpers = callbacks.getHelpers();
        callbacks.setExtensionName("Simple HTTP Listener");
        callbacks.registerHttpListener(this);
    }

    @Override
    public void processHttpMessage(int toolFlag, boolean messageIsRequest, IHttpRequestResponse messageInfo) {
        if (messageIsRequest) {
            // 获取请求的URL
            IRequestInfo reqInfo = helpers.analyzeRequest(messageInfo);
            String url = reqInfo.getUrl().toString();
            System.out.println("Request URL: " + url);
        }
    }
}
```

### 步骤3: 打包插件

1. **编译代码**: 使用你的IDE编译代码为 `.class` 文件。
   
2. **打包为JAR**: 将 `.class` 文件打包成一个JAR文件。确保在JAR文件的 manifest 中指定 `BurpExtender` 类为主类。

### 步骤4: 在Burp Suite中加载插件

1. 打开Burp Suite。
2. 导航到 "Extender" 选项卡。
3. 在 "Extensions" 子选项卡下，点击 "Add"。
4. 选择你的 `.jar` 文件，加载插件。

完成以上步骤后，每当有HTTP请求通过Burp时，你的插件将会输出请求的URL到标准输出。这是一个非常基础的例子，但它展示了如何监听和处理HTTP请求。你可以根据需要扩展此代码，加入更复杂的功能。

