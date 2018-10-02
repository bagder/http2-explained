# 11. http2 i curl

[curl-projektet](https://curl.haxx.se/) har tillhandahållit experimentell
support av http2 sedan september 2013.

I curls anda ämnar vi supporta varje aspekt av http2 som vi bara kan. curl
används ofta som ett testverktyg och en utforskares sätt att peka på
webbsajter och vi tänker fortsätta med det för http2 också.

curl använder det separata biblioteket [nghttp2](https://nghttp2.org/) för all
funktionalitet i http2-lagret. curl kräver nghttp2 1.0 eller senare.

Notera att just nu skippas curl på linux inte alltid med http2-support
påslaget.

## 11.1. HTTP 1.x-liknande

curl konverterar inkommande http2-headrar till HTTP 1.x-liknande headers och
skickar dem till användaren, så att de kommer vara väldigt lika de i
existerande HTTP. Det skapar en enkel övergång för vadsomhelst som använder
curl och HTTP idag. På sammas sätt konverterar curl utgående headrar. Ge dem
till curl i HTTP 1.x-stil och den gör om dem automatiskt när den pratar med
http2-servrar. Det låter också användare att inte behöva bry sig så mycket om
vilken specifik HTTP version som faktiskt används över kabeln.

## 11.2. Klartext, osäkert

curl stöder http2 över vanlig TCP mha Upgrade:-headern. Om du gör en
HTTP-request och ber om http2, kommer curl be servern att uppdatera kopplet
till http2 om det är möjligt.

## 11.3. TLS och vilka bibliotek

curl supportar en bred samling olika TLS-bibliotek för sin TLS-funktion, och
det gäller även http2-stödet. Utmaningen med TLS för http2 är ALPN-stödet och
i viss utsträckning stödet för NPN.

Bygg curl med en modern version av OpenSSL eller NSS för att få både ALPN- och
NPN-stöd. Använder du GnuTLS eller PolarSSL får du ALPN-stöd men inte NPN.

## 11.4. Användning på kommandorad

För att säga åt curl att använda http2, antingen i klartext eller över TLS,
så använder du dess `--http2` flagga (det är “minus minus http2”). curl
använder fortfarande per default HTTP/1.1 så den extra optionen behövs när du
vill ha http2.

## 11.5. libcurl-optioner

### 11.5.1 Slå på HTTP/2

Din applikation använder https:// eller http:// URLer precis som vanligt, men
du sätter curl_easy_setopt-optionen `CURLOPT_HTTP_VERSION` till
`CURL_HTTP_VERSION_2` för att få libcurl att försöka använda http2. Den kommer
då göra sitt bästa för att använda http2, men annars fortsätta använda HTTP
1.1.

### 11.5.2 Multiplexande

Eftersom libcurl försöker behålla nuvarande beteende så mycket som möjligt
måste du slå på http2 multiplexing för din applikation med
[CURLMOPT_PIPELINING-optionen](https://curl.haxx.se/libcurl/c/CURLMOPT_PIPELINING.html). Annars kommer den fortsätta använda en request i taget per koppel.

En annan liten detalj att ha i åtanke är att ifall du ber om flera
överföringar samtidigt med libcurl, mha dess multi-interface, är att en
applikation kan mycket väl starta ett antal överföringar samtidigt och ifall
du då hellre vill att libcurl ska vänta lite för att köra alla över samma
koppel istället för att starta nya koppel för alla, så använder du
[CURLOPT_PIPEWAIT-optionen](https://curl.haxx.se/libcurl/c/CURLOPT_PIPEWAIT.html)
för varje individuell överföring du hellre vill ska vänta.

### 11.5.3 Server push

libcurl 7.44.0 och senare stöder HTTP/2 server push. Du kan dra nytta av den
funktionen genom att sätta upp en push callback med
[CURLMOPT_PUSHFUNCTION-optionen](https://curl.haxx.se/libcurl/c/CURLMOPT_PUSHFUNCTION.html). Om
"pushen" accepteras av applikationen kommer den skapa en ny överföring som en
curl easy handle och leverera data över den, precis som vilken annan
överföring som helst.
