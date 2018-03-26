# 11. http2 e curl

O [projeto curl](https://curl.haxx.se/) provê suporte experimental para http2 desde Setembro de 2013.

No espírito do curl, nós pretendemos suportar cada detalhe do http2 que seja possível. curl é frequentemente utilizado como uma ferramenta de teste e uma maneira de “pingar” manualmente _sites_ e nós pretendemos manter isto para http2 também.

curl utiliza uma biblioteca separada [nghttp2](https://nghttp2.org/) para a funcionalidade de camada de frame do http2. curl requer nghttp2 1.0 ou mais novo.

Note que, atualmente, no linux, o curl e libcurl nem sempre são instalados com o suporte ao protocolo HTTP/2 ativado.

## 11.1. Parecido com HTTP 1.x

Internamente, curl converterá cabeçalhos http2 de entrada para o formato de cabeçalhos HTTP 1.x e fornecê-los ao usuário, de forma semelhante ao HTTP existente. Isto permite uma transição mais fácil para quem está utilizando curl e HTTP hoje em dia. Da mesma forma, curl converterá cabeçalhos de saída no mesmo estilo: informe cabeçalhos HTTP 1.x para o curl e eles serão convertidos em tempo real quando estão conversando com servidores http2. Isto permite que os usuários não tenham que se preocupar demais com cada particularidade da versão HTTP em uso para cada conexão.

## 11.2. Texto puro, inseguro

curl suporta http2 sobre o padrão TCP via cabeçalho “Upgrade:”. Se realizar uma requisição HTTP e perguntar por HTTP 2, curl perguntará ao servidor se é possível atualizar a conexão para http2.

## 11.3. TLS e quais bibliotecas

curl suporta uma grande variedade de diferentes bibliotecas TLS para sua implementação TLS, e isso continua válido para o suporte http2. O desafio com TLS para o mundo http2 é o suporte para APLN e, de certa forma, o suporte NPN.

Compile o curl utilizando versões atuais do OpenSSL ou NSS para obter suporte para ALPN e NPN. Utilizando GnuTLS ou PolarSSL você terá suporte para ALPN, mas não para NPN.

## 11.4. Uso na linha de comando

Para dizer ao curl para utilizar http2, seja texto puro ou sobre TLS, deve ser utilizada a opção `--http2` (que é “traço traço http2”). O padrão no curl ainda é HTTP/1.1, portanto a opção extra é necessária para indicar o uso de http2.

## 11.5. Opções libcurl

### 11.5.1 Habilitar HTTP/2

Sua aplicação deve utilizar URLs https:// ou http:// normalmente, mas setar a opção `CURLOPT_HTTP_VERSION` do curl_easy_setopt para `CURL_HTTP_VERSION_2` para fazer uma tentativa do libcurl utilizar http2. Ele tentará conectar via http2 se for possível, mas continuará utilizando HTTP 1.1 caso ocorra algum problema.

### 11.5.2 Multiplexação

Como libcurl tenta manter o comportamento existente o máximo possível, será necessário habilitar multiplexação HTTP/2 para sua aplicação com a opção [CURLMOPT_PIPELINING](https://curl.haxx.se/libcurl/c/CURLMOPT_PIPELINING.html). Caso contrário, continuará utilizando uma requisição de cada vez por conexão.

Outro pequeno detalhe para ter em mente é que se várias requisições forem solicitadas de uma só vez com libcurl, utilizando sua interface múltipla, uma aplicação pode iniciar qualquer número de transferências de uma vez. Caso seja necessário que o libcurl espere um pouco para adicioná-las na mesma conexão, ao invés de abrir novas conexões para todas elas, a opção [CURLOPT_PIPEWAIT](https://curl.haxx.se/libcurl/c/CURLOPT_PIPEWAIT.html) pode ser utilizada para cada transferência que você prefira esperar.

### 11.5.3 Server push

libcurl 7.44.0 e posteriores suportam HTTP/2 server push. Para utilizar esta funcionalidade, indique um callback na opção [CURLMOPT_PUSHFUNCTION](https://curl.haxx.se/libcurl/c/CURLMOPT_PUSHFUNCTION.html). Se a aplicação aceitar o _push_, uma nova transferência será criada utilizando “CURL easy handle” e o conteúdo será entregue nele, assim como qualquer outra transferência.
