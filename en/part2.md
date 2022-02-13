# 2. HTTP today

HTTP 1.1 has turned into a protocol used for virtually everything on the Internet. Huge investments have been made in protocols and infrastructure that takes advantage of this, to the extent that it is often easier today to make things run on top of HTTP rather than building something new on its own.

## 2.1 HTTP 1.1 is huge

When HTTP was created and thrown out into the world, it was probably perceived as a rather simple and straightforward protocol, but time has proved that to be false. HTTP 1.0 in RFC 1945 is a 60-page specification released in 1996. RFC 2616 that describes HTTP 1.1 was released only three years later in 1999 and had grown significantly to 176 pages. Yet when we within IETF worked on the update to that spec, it was split up and converted into six documents with a much larger page count in total (resulting in RFC 7230 and family). By any count, HTTP 1.1 is big and includes a myriad of details, subtleties, and not the least, a lot of optional parts.

## 2.2 A world of options

HTTP 1.1's nature of having lots of tiny details and options available for later extensions has grown a software ecosystem where almost no implementation ever implements everything – and it isn't even really possible to exactly tell what “everything” is. This has led to a situation where features that were initially little-used saw very few implementations, and those that did implement the features then saw very little use of them.

Later on, this caused an interoperability problem when clients and servers started to increase the use of such features. HTTP pipelining is a primary example of such a feature.

## 2.3 Inadequate use of TCP

HTTP 1.1 has a hard time really taking full advantage of all the power and performance that TCP offers. HTTP clients and browsers have to be very creative to find solutions that decrease page load times.

Other attempts that have been going on in parallel over the years have also confirmed that TCP is not that easy to replace, and thus we keep working on improving both TCP and the protocols on top of it.

Simply put, TCP can be utilized better to avoid pauses or wasted intervals that could have been used to send or receive more data. The following sections will highlight some of these shortcomings.

## 2.4 Transfer sizes and number of objects

When looking at the trend for some of the most popular sites on the web today and what it takes to download their front pages, a clear pattern emerges. Over the years, the amount of data that needs to be retrieved has gradually risen up to and above 1.9 MB. What is more important in this context is that, on average, over 100 individual resources are required to display each page.

As the graph below shows, the trend has been going on for a while, and there is little to no indication that it will change anytime soon. It shows the growth of the total transfer size (in green) and the total number of requests used on average (in red) to serve the most popular websites in the world, and how they have changed over the last four years.

![transfer size growth](https://raw.githubusercontent.com/bagder/http2-explained/master/images/transfer-size-growth.png)

## 2.5 Latency kills

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/page-load-time-rtt-decreases.png" />

HTTP 1.1 is very latency-sensitive, partly because HTTP pipelining is still riddled with enough problems to remain switched off to a large percentage of users.

While we've seen a great increase in available bandwidth to people over the last few years, we have not seen the same level of improvements in reducing latency. High-latency links, like many of the current mobile technologies, make it hard to get a good and fast web experience even if you have a really high bandwidth connection.

Another use case requiring low latency is certain kinds of video, like video conferencing, gaming, and similar where there's not just a pre-generated stream to send out.

## 2.6. Head-of-line blocking

HTTP pipelining is a way to send another request while waiting for the response to a previous request. It is very similar to queuing at a counter at the bank or in a supermarket: you just don't know if the person in front of you is a quick customer or that annoying one that will take forever before he/she is done. This is known as head-of-line blocking.

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/head-of-line-blocking.jpg" />

Sure, you can attempt to pick the line you believe is the correct one, and at times you can even start a new line of your own. But in the end, you can't avoid making a decision. And once it is made, you cannot switch lines.

Creating a new line is also associated with a performance and resource penalty, so that's not scalable beyond a smaller number of lines. There's just no perfect solution to this.

Even today, most desktop web browsers ship with HTTP pipelining disabled by default.

Additional reading on this subject can be found in the Firefox [bugzilla entry 264354](https://bugzilla.mozilla.org/show_bug.cgi?id=264354).
