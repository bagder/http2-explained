# 9. Firefox里的http2

Firefox紧跟着草案，并且很早之前就实现了http2的测试实现。在http2协议开发的时候，客户端和服务器需要采用同一的协议草案版本，进行测试也变得比较繁琐。所以请一定注意你的客户端和服务器支持的是一样的版本。

## 9.1. 首先，确保它已被启用

从发布于2015年1月13日的Firefox 35之后，http2支持是默认开启的。

在地址栏里进入'about:config'，再搜索一个名为“network.http.spdy.enabled.http2draft”的选项，确保它被设置为`true`。Firefox 36添加了一个“network.http.spdy.enabled.http2”的配置项，并默认设置为*true*。后者控制的是“纯”http2版本，而前者控制了启用／禁用通过http2草案版本进行协商。从Firefox 36之后，这两者都默认为true。

## 9.2. 仅限TLS

请记住Firefox只在TLS上实现了http2。你只会看到http2只在`https://`的网站里得到支持。

## 9.3. 透明！ <!--这个标题改成： 一切都是透明的  怎么样-->

![transparent http2 use](https://raw.githubusercontent.com/bagder/http2-explained/master/images/firefox-screenshot.png)

在UI上，没有任何元素标明你正在使用http2。但想确认也并不复杂，一种方法是启用“Web developer->Network”，再查看响应头里面服务器发回来的内容。这个响应是“HTTP/2.0”，并且Firefox也插入了一个自己头“X-Firefox-Spdy:”，如上面截图所示。

你在这里看到的头文件是网络工具把二进制的http2格式转换成类似HTTP 1.x显示方式的文本格式。

## 9.4. 图形化HTTP/2

有一些Firefox的插件可以图形化HTTP/2，比如[“HTTP/2 and SPDY Indicator”](https://addons.mozilla.org/en-US/firefox/addon/spdy-indicator/)。
