# 3. 那些年，克服延迟之道

再困难的问题也有解决的方案，但这些方案却良莠不齐。

## 3.1 Spriting

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/spriting.jpg" />

Spriting是一种将很多较小的图片合并成一张大图，再用JavaScript或者CSS将小图重新“切割”出来的技术。

网站可以利用这一技巧来达到提速的目的——在HTTP 1.1里，下载一张大图比下载100张小图快得多。

但是当某些页面只需要显示其中一两张小图时，这种缓存整张大图的方案就显得过于臃肿。同时，当缓存被清楚的时候的时候，Spriting会导致所有小图片被同时删除，而不能选择保留其中最常用的几个。

## 3.2 内联（Inlining）

Inlining是另外一种防止发送很多小图请求的技巧，它将图片的原始数据嵌入在CSS文件里面的URL里。而这种方案的优缺点跟Spriting很类似。

    .icon1 {
        background: url(data:image/png;base64,<data>) no-repeat;
	  }
    .icon2 {
        background: url(data:image/png;base64,<data>) no-repeat;
	  }

## 3.3 拼接（Concatenation）

大型网站往往会包含大量的JavaScript文件。开发人员可以利用一些前端工具将这些文件合并为一个大的文件，从而让浏览器能只花费一个请求就将其下载完，而不是发无数请求去分别下载那些琐碎的JavaScript文件。但凡事往往有利有弊，如果某页面只需要其中一小部分代码，它也必须下载完整的那份；而文件中一个小小的改动也会造成大量数据的被重新下载。

这种方案也给开发者造成了很大的不便。

## 3.4 分片（Sharding）

最后一个我要说的性能优化技术叫做“Sharding”。顾名思义，Sharding就是把你的服务分散在尽可能多的主机上。这种方案乍一听比较奇怪，但是实际上在这背后却蕴藏了它独辟蹊径的道理！

最初的HTTP 1.1规范提到一个客户端最多只能对同一主机建立两个TCP连接。因此，为了不和规范冲突，一些聪明的网站使用了新的主机名，这样的话，用户就能和网站建立更多的连接，从而降低载入时间。

后来，两个连接的限制被取消了，现在的客户端可以轻松地和每个主机建立6-8个连接。但由于连接的上限依然存在，所以网站还是会用这种技术来提升连接的数量。而随着资源个数的提升（上面章节的图例），网站会需要更多的连接来保证HTTP协议的效率，从而提升载入速度。在现今的网站上，使用50甚至100个连接来打开一个页面已经并不罕见。根据[httparchive.org](https://httparchive.org/)的最新记录显示，在Top 30万个URL中平均使用40（！）个TCP连接来显示页面，而且这个数字仍然在缓慢的增长中。

另外一个将图片或者其他资源分发到不同主机的理由是可以不使用cookies，毕竟现今cookies的大小已经非常可观了。无cookies的图片服务器往往意味着更小的HTTP请求以及更好的性能！

下面的图片展示了访问一个瑞典著名网站的时产生的数据包，请注意这些请求是如何被分发到不同主机的。

![image sharding at expressen.se](https://raw.githubusercontent.com/bagder/http2-explained/master/images/expressen-sharding.jpg)
