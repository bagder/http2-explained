# 10. http2 e Chromium

O time do Chromium implementa o http2 e provê suporte para ele nos canais _dev_ e _beta_ por bastante tempo. Começando pelo Chrome 40, lançando em 27 de Janeiro de 2015, http2 está habilitado por padrão para um certo número de usuários. O número começou realmente pequeno e então foi incrementado gradualmente com o passar do tempo.

O suporte SPDY será eventualmente removido. Em um artigo no blog, o projeto anunciou em [Fevereiro de 2015](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html):

> “Chrome suporta SPDY desde o Chrome 6, mas a maior parte dos benefícios estão presentes no HTTP/2, é hora de dizer tchau. Nós planejamos remover o suporte do SPDY no começo de 2016”

## 10.1. Em primeiro lugar, verifique se está habilitado

Digite “chrome://flags/#enable-spdy4" na barra de endereços do seu navegador Chrome e clique em “enable” se ele ainda não está habilitado.

## 10.2. Somente TLS

Lembre-se que o Chrome implementa http2 somente sobre TLS. Você somente verá http2 em ação com Chrome quando navegar em _sites_ https:// e que ofereçam suporte http2.

## 10.3. Visualizar o uso do http2

Existem plugins do Chrome disponíveis que ajudam a visualizar se um _site_ está utilizando HTTP/2. Um deles é o [“HTTP/2 and SPDY Indicator”](https://chrome.google.com/webstore/detail/spdy-indicator/mpbpobfflnpcgagjijhmgnchggcjblin).

## 10.4. QUIC

Experiências atuais do Chrome com QUIC (veja seção 12.1) diluem os números HTTP/2 de alguma maneira.
