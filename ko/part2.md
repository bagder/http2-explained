# 2. HTTP 현재

HTTP 1.1 은 거의 모든 인터넷에서 사용되는 범용적인 프로토콜이 되었습니다. 프로토콜과 사회 기반으로 만들어져온 거대한 투자들은 프로토콜과 사회 기반 시설을 통해 만들어져 왔습니다. 그 연장선으로 생각 해볼 때 오늘날에는 아예 새로운 것을 만들어 내는 것보다, HTTP위에서 만들어내는 것이 더 쉽다고 볼 수 있습니다.

## 2.1 HTTP 1.1은 거대하다.
HTTP가 처음 탄생되어 세상에 알려졌을 때 많은 사람들은 그저 단순하고 직선적인 프로토콜로 인식되었습니다. 하지만 시간이 지나면서 그것이 틀렸다는 것이 증명되었습니다. RFC 1945에서의 HTTP는 1996년에 기술된 60페이지 분량의 문서입니다. HTTP1.1이 기술된 RFC 2616은 1999년에서 불과 3년이후 176페이지로 상당한 분량으로 확대되어 발표 되었습니다. 아직 IETF에 맞춰 업데이트를 할 때, 6개 문서로 분할 변환 되었고, 훨씬 많은 분량이 되었습니다.(결과적으로 RFC 7230과 가족이 되었습니다.) 
어떤 통계에 의하면 HTTP 1.1은 크고 수많은 상세 설명을 포함하고 있고,수많은 파트가 존재합니다.

## 2.2 옵션의 세계

HTTP 1.1's nature of having lots of tiny details and options available for later extensions has grown a software ecosystem where almost no implementation ever implements everything – and it isn't even really possible to exactly tell what “everything” is. This has led to a situation where features that were initially little used saw very few implementations and those who did implement the features then saw very little use of them.

Later on, this caused an interoperability problem when clients and servers started to increase the use of such features. HTTP Pipelining is a primary example of such a feature.

## 2.3 부적절한 TCP의 사용

HTTP 1.1 TCP의 모든 장점과 능력, 그리고 퍼포먼스 등을 다루는 것이 어려웠습니다.
HTTP 클라이언트와 브라우져들은 페이지 로딩시간을 줄이기 위해서 창의적인 해결책을 찾아야할 필요가 있었습니다.

수많은 시도들이 병행되었지만 TCP를 대체하는 일이 결코 쉽지 않다는 것을 알게되었고 결국 우리는 TCP와 프로토콜의 기능을 향상시키는 작업을 계속 했습니다.

간단히 말해서 TCP는 더 많은 데이터를 송수신 할 수 있었음에도 공간낭비 등을 막는데에 이용될 수 있었습니다. 다음장에서는 이러한 단점들에 대해 다룹니다.

## 2.4 Transfer sizes and number of objects

When looking at the trend for some of the most popular sites on the web today and what it takes to download their front pages, a clear pattern emerges. Over the years the amount of data that needs to be retrieved has gradually risen up to and above 1.9MB . What is more important in this context is that on average over a hundred individual resources are required to display each page.

As the graph below shows, the trend has been going on for a while and there is little to no indication that it will change anytime soon. It shows the growth of the total transfer size (in green) and the total number of requests used on average (in red) to serve the most popular web sites in the world, and how they have changed over the last four years.

![transfer size growth](https://raw.githubusercontent.com/bagder/http2-explained/master/images/transfer-size-growth.png)

## 2.5 Latency kills

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/page-load-time-rtt-decreases.png" />

HTTP 1.1 is very latency sensitive, partly because HTTP Pipelining is still riddled with enough problems to remain switched off to a large percentage of users.

While we've seen a great increase in available bandwidth to people over the last few years, we have not seen the same level of improvements in reducing latency. High latency links, like many of the current mobile technologies, make it really hard to get a good and fast web experience even if you have a really high bandwidth connection.

Another use case that really needs low latency is certain kinds of video, like video conferencing, gaming and similar where there's not just a pre-generated stream to send out.

## 2.6. Head of line blocking

HTTP Pipelining is a way to send another request while waiting for the response to a previous request. It is very similar to queuing at a counter at the bank or in a super market. You just don't know if the person in front of you is a quick customer or that annoying one that will take forever before he/she is done: head of line blocking.

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/head-of-line-blocking.jpg" />

Sure you can be careful about line picking so that you pick the one you really believe is the correct one, and at times you can even start a new line of your own but in the end you can't avoid making a decision and once it is made you cannot switch lines.

Creating a new line is also associated with a performance and resource penalty so that's not scalable beyond a smaller number of lines. There's just no perfect solution to this.

Even today, 2015, most desktop web browsers ship with HTTP pipelining disabled by default.

Additional reading on this subject can be found for example in the Firefox [bugzilla entry 264354](https://bugzilla.mozilla.org/show_bug.cgi?id=264354).
