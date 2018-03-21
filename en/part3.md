# 3. Things done to overcome latency pains

When faced with problems, people tend to gather to find workarounds. Some of the workarounds are clever and useful, but others are just awful kludges.

## 3.1 Spriting
<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/spriting.jpg" />

Spriting is the term often used to describe combining multiple small images to form a single larger image. Then, using JavaScript or CSS, you “cut out” pieces of that big image to show smaller individual ones.

A site would use this trick for speed. Getting a single big image in HTTP 1.1 is much faster than getting 100 smaller individual ones.

Of course, this has its downsides for the pages of the site that only want to show one or two of the small pictures. Spriting also causes all images to be removed simultaneously when the cache is cleared instead of possibly letting the most commonly used ones remain.

## 3.2 Inlining

Inlining is another trick used to avoid sending individual images, and this is done by using data URLs embedded in the CSS file. This has similar benefits and drawbacks as the spriting case.

    .icon1 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }

    .icon2 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }


## 3.3 Concatenation

A big site can end up with a lot of different JavaScript files. Developers can use front-end tools to concatenate, or combine, multiple scripts so that the browser will get a single big file instead of dozens of smaller ones. Too much data is sent when only little is needed and, likewise, too much data needs to be reloaded when a change is made.

This practice is, of course, mostly an inconvenience to the developers involved.

## 3.4 Sharding

The final performance trick I'll mention is often referred to as “sharding.” It basically means serving aspects of your service on as many different hosts as possible. At first glance this seems strange, but there is sound reasoning behind it.

Initially, the HTTP 1.1 specification stated that a client was allowed to use a maximum of two TCP connections for each host. So, in order to not violate the spec, clever sites simply invented new host names and – voilà – you could get more connections to your site and decreased page load times.

Over time that limitation was removed, and today clients easily use six to eight connections per host name. But they still have a limit, so sites continue to use this technique to bump up the number of connections. As the number of objects requested over HTTP is ever-increasing – as I showed before – the large number of connections is then used to make sure HTTP performs well and allow your site to load quickly. It is not unusual for sites to use well over 50 or even up to 100 or more connections now for a single site using this technique. Recent stats from httparchive.org show that the top 300K URLs in the world need, on average, 40(!) TCP connections to display the site, and the trend says this is still increasing slowly over time.

Another reason for sharding is to put images or similar resources on a separate host name that doesn't use any cookies, as the size of cookies these days can be quite significant. By using cookie-free image hosts, you can sometimes increase performance simply by allowing much smaller HTTP requests!

The image below shows what a packet trace looks like when browsing one of Sweden's top web sites and how requests are distributed over several host names.

![image sharding at expressen.se](https://raw.githubusercontent.com/bagder/http2-explained/master/images/expressen-sharding.jpg)
