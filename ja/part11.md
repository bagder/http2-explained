# 11. curlにおけるhttp2

[curlプロジェクト](http://curl.haxx.se/)は試験的にhttp2のサポートを2013年9月から行っています。

curlの精神に則り、我々はできるかぎり全てのhttp2の機能を提供する予定です。curlはしばしばテストツール、そしてwebサイトをいろいろと弄くる開発者の手段として使われるので、http2でもこの伝統を引き継ぐ予定です。

curlは第三者のライブラリ[nghttp2](https://nghttp2.org/)をhttp2のフレームレイヤーの実装に使用しています。curlにはnghttp2 1.0以降が必要です。

curlとlibcurlは、Linuxのディストリビューションからインストールした場合、まだ必ずしもHTTP/2プロトコルがサポートされているわけではないことに注意してください。

## 11.1. HTTP 1.xそっくり

curlは内部的に受信したhttp2ヘッダーをHTTP 1.xスタイルのヘッダーに変換してユーザーに提示するので、既存のHTTPと同じように見えます。これにより既存のcurlとHTTPの使用からの移行が容易になります。送信するヘッダーについても同様です。HTTP 1.xスタイルでヘッダーをcurlに伝えると、http2サーバーと通信するときには自動で変換されます。これによりユーザーは特定のHTTPバージョンが使われているのかどうかといったことに気を使わなくて済むのです。

## 11.2. 安全ではない平文

curlはUpgradeヘッダーでhttp2を標準のTCP上でサポートしています。あなたがHTTPリクエストでHTTP/2を要求した場合、curlはサーバーに可能なら接続をhttp2にアップグレードするように依頼します。

## 11.3. TLSとそのライブラリ

curlは多くのTLSライブラリをサポートしていて、これはhttp2サポートでも同様です。TLSにおけるhttp2サポートの問題点はALPNのサポート、そしてそれほど重要ではないですがNPNのサポートです。

最近のOpenSSLまたはNSSとともにcurlをビルドしてALPNとNPN両方のサポートを手に入れることができます。GnuTLSやPolarSSLの場合、ALPNが利用できますが、NPNは利用できません。

## 11.4. コマンドラインでの使用

curlにhttp2を使うように指示するには、平文、TLSに関係なく、`--http2`オプションを使います（”
ダッシュ ダッシュ http2”）。curlはまだデフォルトがHTTP 1.1であり、http2を使う場合は追加のオプションが必要なのです。

## 11.5. libcurlのオプション

### 11.5.1 HTTP/2を有効にする

アプリケーションではhttps://やhttp:// URLを今までどおり使いますが、http2を使うにはcurl_easy_setoptの`CURLOPT_HTTP_VERSION`オプションを`CURL_HTTP_VERSION_2`にします。こうすることで出来る限りhttp2を使うようになりますが、それができない場合はHTTP 1.1が使われます。

### 11.5.2 多重化

libcurlは既存の振る舞いを維持しようとするので、HTTP/2の多重化をアプリケーションで有効にするには[CURLMOPT_PIPELINING](http://curl.haxx.se/libcurl/c/CURLMOPT_PIPELINING.html)オプションを使います。このオプションを使わない場合、今まで同様に接続あたりの同時リクエスト数は1になります。

もうひとつ注意してほしいことは、multiインターフェースをつかって複数の転送を同時にlibcurlで行う場合、複数の接続が使われることになります。libcurlを少し待たせて同じ接続にすべての転送を多重化するには、待たせる転送に対して[CURLOPT_PIPEWAIT](http://curl.haxx.se/libcurl/c/CURLOPT_PIPEWAIT.html)オプションを使います。

### 11.5.3 サーバープッシュ

libcurl 7.44.0以降はHTTP/2サーバープッシュをサポートしています。この機能を使うには[CURLMOPT_PUSHFUNCTION](http://curl.haxx.se/libcurl/c/CURLMOPT_PUSHFUNCTION.html)オプションを使ってプッシュコールバックをセットします。プッシュがアプリケーションによって受け入れられた場合、新しいCURL easy handleが作成されて、他の転送と同様にコンテンツを受信します。
