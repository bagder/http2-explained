# 1. 背景

这篇文档会从技术和协议层面来介绍http2。文档起源于2014年4月我在斯德哥尔摩做了一次相关的演讲，在那之后我对演讲内容的细节进行了一些解释和补充，从而写出了这篇文档。

正式版http2规格标准叫做RFC 7540，发布于2015年5月15日：https://www.rfc-editor.org/rfc/rfc7540.txt

如果你有在这篇文章中发现任何我的失误造成的错误或疏漏，请帮我指正。我会在后续版本中修改。

为了让阅读体验更流畅，在这篇文章中我会使用“http2”来指代这一新协议，但请记住该协议的正式名字是HTTP/2。

## 1.1 关于作者

我的名字叫做Daniel Stenberg，在Mozilla工作。在过去20年，我一直致力于开源事业，参与了多个网络方面的项目。可能我最广为人知的身份是curl和libcurl的首席开发者。同时，我也参与了IETF HTTPbis工作组多年，工作在HTTP 1.1和http2标准化的一线.

  Email: daniel@haxx.se

  Twitter: [@bagder](https://twitter.com/bagder)

  Web: [daniel.haxx.se](https://daniel.haxx.se/)

  Blog: [daniel.haxx.se/blog](https://daniel.haxx.se/blog/)

## 1.2 帮助我！

如果你在该文档里面发现任何错误、疏漏，请发送给我一份相关段落更改后的版本，我会进行修正并且注明所有对文档有贡献的人！希望能将这份文档变得越来越好。

这篇文档可以在[https://daniel.haxx.se/http2](https://daniel.haxx.se/http2)下载。

## 1.3 许可证

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

这篇文档基于Createive Commons Attribution 4.0发布： [https://creativecommons.org/licenses/by/4.0/](https://creativecommons.org/licenses/by/4.0/)

## 1.4 文档历史

该文档的第一版发布于2014年4月25日。下面是最近主要改动的更新历史。

### Version 1.13

- Converted the master version of this document to Markdown syntax
- 13: Mention more resources, updated links and descriptions
- 12: Updated the QUIC description with reference to draft
- 8.5: Refreshed with current numbers
- 3.4: The average is now 40 TCP connections
- 6.4: Updated to reflect what the spec says

### Version 1.12

- 1.1: HTTP/2 is now in an official RFC
- 6.5.1: Link to the HPACK RFC
- 9.1: Mention the Firefox 36+ config switch for http2
- 12.1: Added section about QUIC

### Version 1.11

- Lots of language improvements mostly pointed out by friendly contributors
- 8.3.1: Mention nginx and Apache httpd specific acitivities

### Version 1.10

- 1: The protocol has been “okayed”
- 4.1: Refreshed the wording since 2014 is last year
- Front: Added image and call it “http2 explained” there, fixed link
- 1.4: Added document history section
- Many spelling and grammar mistakes corrected
- 14: Added thanks to bug reporters
- 2.4: Better labels for the HTTP growth graph
- 6.3: Corrected the wagon order in the multiplexed train
- 6.5.1: HPACK draft-12

### Version 1.9

- Updated to HTTP/2 draft-17 and HPACK draft-11  
- Added section "10. http2 in Chromium" (== one page longer now)  
- Lots of spell fixes  
- At 30 implementations now  
- 8.5: Added some current usage numbers  
- 8.3: Mention internet explorer too  
- 8.3.1 Added "missing implementations"  
- 8.4.3: Mention that TLS also increases success rate


<!-- Review备注：这一章翻译已经没有明显问题。 -->
