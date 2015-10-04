# 1. 背景

この文書はhttp2を技術的にプロトコルレベルで記述したものです。始まりはDanielが2014年4月にストックホルムで行ったプレゼンテーションでした。後に拡張され、全ての詳細や丁寧な説明を含む立派な文書になりました。

RFC 7540は公式なhttp2仕様書で、2015年5月15日に発行されました: http://www.rfc-editor.org/rfc/rfc7540.txt

この文書におけるすべての誤りは私自身のものであり、私の至らなさの致すところです。指摘していただければ次のバージョンで修正します。

この文書では統一的に”http2”という単語でこの新しいプロトコルを呼称していますが、正式な名称はHTTP/2です。読みやすさと言語における収まりの良さのためにこの選択をしました。

## 1.1 著者

私の名前はDaniel Stenbergです。Mozillaで働いています。私はオープンソースとネットワークの分野で20年以上も様々なプロジェクトで働いています。おそらく私はcurlとlibcurlの開発リーダーとしてよく知られているかもしれません。私はIETF HTTPbisワーキンググループに数年間参加していて、最新のHTTP 1.1に追随するとともに、http2の標準化にも参加しました。

  Email: daniel@haxx.se

  Twitter: [@bagder](https://twitter.com/bagder)

  Web: [daniel.haxx.se](http://daniel.haxx.se/)

  Blog: [daniel.haxx.se/blog](http://daniel.haxx.se/blog/)

## 1.2 ご協力をお願い致します

間違いや脱字、エラー、明らかな嘘をこの文書に見つけた場合、修正した段落を私に送ってください。修正を取り込みます。私はもちろん助けてくれた人全てに適切な名誉を与えるつもりです。私はこの文書が時とともに改善されていくことを望んでいます。

この文書は[http://daniel.haxx.se/http2](http://daniel.haxx.se/http2)で利用可能です。

## 1.3 ライセンス

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

この文書はCreative Commons Attribution 4.0 license: http://creativecommons.org/licenses/by/4.0/ でライセンスされています。

## 1.4 履歴

この文書の最初のバージョンは2014年4月25日に発行されました。最近の文書における主たる変更点を以下に示します。

### Version 1.13

- Converted the master version of this document to Markdown syntax
- 13: mention more resources, updated links and descriptions
- 12: Updated the QUIC description with reference to draft
- 8.5: refreshed with current numbers
- 3.4: the average is now 40 TCP connections
- 6.4: Updated to reflect what the spec says

### Version 1.12

- 1.1: HTTP/2 is now in an official RFC
- 6.5.1: link to the HPACK RFC
- 9.1: mention the Firefox 36+ config switch for http2
- 12.1: Added section about QUIC

### Version 1.11

- Lots of language improvements mostly pointed out by friendly contributors
- 8.3.1: mention nginx and Apache httpd specific acitivities

### Version 1.10

- 1: the protocol has been “okayed”
- 4.1: refreshed the wording since 2014 is last year
- front: added image and call it “http2 explained” there, fixed link
- 1.4: added document history section
- many spelling and grammar mistakes corrected
- 14: added thanks to bug reporters
- 2.4: (better) labels for the HTTP growth graph
- 6.3: corrected the wagon order in the multiplexed train
- 6.5.1: HPACK draft-12

### Version 1.9

- Updated to HTTP/2 draft-17 and HPACK draft-11
- Added section "10. http2 in Chromium" (== one page longer now)
- Lots of spell fixes
- At 30 implementations now
- 8.5: added some current usage numbers
- 8.3: mention internet explorer too
- 8.3.1 "missing implementations" added
- 8.4.3: mention that TLS also increases success rate
