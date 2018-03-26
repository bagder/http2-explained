# 8. Um mundo http2

O que acontecerá quando http2 for adotado? Ele será adotado?

## 8.1. Como o http2 afetará os humanos normais?

http2 ainda não é amplamente implantado nem utilizado. Não podemos dizer com certeza exatamente como as coisas acontecerão. Vimos como o SPDY é utilizado e podemos fazer suposições e cálculos baseados nesse e em experimentos anteriores.

http2 reduz o número de "round-trips" de rede necessários e, com a multiplexação e o descarte rápido de fluxos indesejados, evita o problema de bloqueio de primeira fila.

O protocolo permite um grande número de fluxos (_streams_) paralelos que vão além dos sites atuais que utilizam a técnica de domínio fragmentado (_sharding_).

Com a funcionalidade de prioridades utilizada adequadamente nos fluxos, as chances dos clientes receberem os dados mais importantes antes dos menos importantes são muito maiores.
Tudo isso em conjunto, eu diria que temos boas chances de páginas sendo carregadas mais rapidamente e mais responsivas. Em poucas palavras: uma melhor experiência na _web_.

Creio que ainda não podemos dizer o quão mais rápido e quantas melhorias nós veremos. Em primeiro lugar, a tecnologia ainda é muito nova e ainda não temos visto clientes e servidores com implementações que realmente aproveitam todo o poder que o protocolo oferece.

## 8.2. Como o http2 afetará o desenvolvimento _web_?

Ao longo do anos, os desenvolvedores _web_ e os ambientes de desenvolvimento _web_ têm reunido uma caixa de ferramentas cheia de dicas para contornar os problemas com o HTTP 1.1, lembre-se que eu citei algumas delas no começo desse documento como justificativas para o http2.

Muitos desses contornos, que são atualmente são utilizados por padrão e sem pensar, irão provalmente prejudicar a performance do http2 ou, pelo menos, não tirar vantagem dos novos super poderes do novo protocolo. _Sprinting_ e _inlining_ não devem ser utilizados com http2. _Sharding_ será provavelmente prejudicial ao http2, pois utilizará menos conexões.

Um problema é que, certamente, os _sites_ e os desenvolvedores precisarão desenvolver para o mundo que, a curto prazo, funcionará com clientes HTTP1.1 e http2, e será um desafio ter uma melhor performance para todos os usuários sem disponibilizar duas versões diferentes de _front-end_.

Por estas razões, eu suspeito que haverá algum tempo antes de vermos todo o potencial do http2 sendo atingido.

## 8.3. Implementaçẽos http2

Tentar documentar implementações específicas em documentos como este é certamente fútil e condenado ao fracasso, pois ficará desatualizado em um curto espaço de tempo. Em vez disso, eu vou explicar a situação em termos mais amplos e indicar os leitores a [lista de implementações](https://github.com/http2/http2-spec/wiki/Implementations) no _site_ do http2.

Havia uma grande quantidade de implementações já no início e essa quantidade tem aumentado ao longo do trabalho no http2. No momento dessa escrita, existem mais de 40 implementações listadas e a maioria implementa a versão final.

### 8.3.1 Navegadores

Firefox é o navegado que tem encabeçado as implementações _drafts_. 
Twitter tem ficado a altura e ofereceu seus serviços sobre http2.
Google começou a oferecer suporte http2 em Abril de 2014 em alguns servidores de testes executando seus serviços e, desde Maio de 2014, eles oferecem suporte http2 nas versões de desenvolvimento do Chrome.
Microsoft apresentou um suporte para http2 na próxima versão do Internet Explorer.
Safari (com iOS 9 e Mac OS X El Capitan) e Opera disseram que vão suportar http2.

### 8.3.2 Servidores

Já existem vários servidores implementando http2.

O popular servidor Nginx oferece suporte http2 desde a versão [1.9.5](https://www.nginx.com/blog/nginx-1-9-5/) lançada em 22 de Setembro de 2015 (onde ele sobrescreve o módulo SPDY, ou seja, não é possível executar os dois módulos na mesma instância do servidor).

O servidor httpd Apache tem um módulo http2 [mod_http2](https://httpd.apache.org/docs/2.4/mod/mod_http2.html) desde a versão 2.4.17 que foi lançada em 9 de Outubro de 2015.

[H2O](https://h2o.examp1e.net/), [Apache Traffic Server](https://trafficserver.apache.org/), [nghttp2](https://nghttp2.org/), [Caddy](https://caddyserver.com/) e [LiteSpeed](https://www.litespeedtech.com/products/litespeed-web-server/overview) lançaram suas versões compatíveis com http2.

### 8.3.3 Outros

curl e libcurl suportam conexões http2 inseguras, assim como a versão baseada em TLS utilizando alguma de muitas bibliotecas TLS.

Wireshark suporta http2. A ferramenta perfeita para análise do tráfego de rede em http2.

## 8.4. Críticas comuns ao http2

Durante o desenvolvimento desse protocolo, houve muito debate e, certamente, há um certo número de pessoas que acreditam que esse protocolo acabou completamente errado. Eu quero mencionar alguns dos mais comuns e os argumentos contra eles:

### 8.4.1. “O protocolo é desenhado e feito pelo Google”

Existem variações indicando que o mundo ficaria ainda mais dependente e controlado pelo Google. Isso não é verdade. O protocolo foi desenvolvido no âmbito da IETF da mesma maneira que os protocolos são desenvolvidos por mais de 30 anos. No entanto, todos nós reconhecemos e admitimos o trabalho impressionante que o Google fez com o SPDY que não só provou ser possível implantar um novo protocolo dessa forma, mas que também forneceu números ilustrando quais ganhos poderiam ser obtidos.

Google [anunciou](https://blog.chromium.org/2015/02/hello-http2-goodbye-spdy.html) publicamente que irão remover o suporte para SPDY e NPN no Chrome em 2016 e eles insistem que os servidores migrem para HTTP/2.

### 8.4.2. “O protocolo é útil somente para os navegadores”

Isto é meio que verdade. Uma das principais razões por trás do desenvolvimento do http2 é a correção o HTTP _pipelining_. Se o seu caso originalmente não tem necessidade de _pipelining_, então http2 não trará muitos ganhos para você. Certamente não é a única melhoria no protocolo, mas uma das maiores.

Assim que os serviços começarem a perceber o poder e a capacidade da multiplexação de fluxos que uma única conexão traz, eu suspeito que veremos mais uso do http2.

Pequenas APIs REST e usos simples do HTTP 1.x podem não encontrar muitos ganhos no que o http2 tem a oferecer. Mas, também, deve haver poucas desvantagens no uso do http2 para a maioria dos usuários.

### 8.4.3. “O protocolo é útil somente para _sites_ grandes”

De forma alguma. A capacidade de multiplexação certamente ajudará a melhorar a experiência em conexões de alta latência que _sites_ pequenos possuem sem distribuições geográficas.

### 8.4.4. “O uso de TLS o torna mais lento”

Isto pode ser verdade, de certa forma. O “handshake” TLS adiciona um pequeno tempo extra, mas existem esforços em andamento para reduzir o número de viagens (“round-trips”) necessárias. A sobrecarga para fazer TLS por meio da conexão, ao invés de texto puro, não é insignificante e claramente consome mais CPU e energia será gasta da mesma forma que um tráfego não seguro. O quanto e o que isso impactará está sujeito a opiniões e medições. Veja, por exemplo, [istlsfastyet.com](https://istlsfastyet.com/) para mais informações.

Telecomunicações e outros operadores de rede, por exemplo o ATIS Open Web Alliance, dizem que eles [precisam de tráfego não criptografado](https://www.atis.org/openweballiance/docs/OWAKickoffSlides051414.pdf) para oferecer cachê, compressão e outras técnicas necessárias para prover uma rápida experiência _web_ via satélite, para aviões e similares. http2 não torna o uso de TLS obrigatório, portanto não devemos ter problemas com os termos.

Muitos usuários da Internet expressaram uma preferência por utilizar TLS de forma mais ampla e nós devemos ajudar a proteger a privacidade dos usuários.

As experiências também mostraram que, utilizando TLS, há uma chance maior de sucesso do que ao implementar novos protocolos de texto puro na porta 80, pois há muitas caixas no meio do caminho que inteferem, já que na porta 80 é comum imaginar o uso para HTTP.

Finalmente, graças a multiplexação dos fluxos http2 sobre uma única conexão, casos normais de uso do navegador podem diminuir substancialmente o número de _handshakes_ TLS e melhorar a performance em comparação com o HTTPS utilizando HTTP 1.1.

### 8.4.5. “Não ser ASCII é um fator decisivo”

Sim, nós gostamos de poder ver os protocolos claramente, pois é mais fácil depurar e rastrear. Mas protocolos baseados em texto são mais propensos ao erro e abertos aos erros de transformação e interpretação.

Se você realmente não suporta um protocolo binário, então você também não poderia lidar com TLS e compressão no HTTP 1.x e essa funcionalidade é utilizada por muito tempo.

### 8.4.6. “Não é mais rápido do que HTTP/1.1”

Naturalmente é um tema sujeito a debate e discussões sobre como medir o que é mais rápido, mas, nos cenários do SPDY, muitos testes foram realizados e o tempo de carga de uma página foi mais rápido (por exemplo ["How Speedy is SPDY?"](https://www.usenix.org/system/files/conference/nsdi14/nsdi14-paper-wang_xiao_sophia.pdf) pela University of Washington e ["Evaluating the Performance of SPDY-enabled Web Servers"](https://www.neotys.com/blog/performance-of-spdy-enabled-web-servers) por Hervé Servy) e tais experiências também foram feitas com o http2. Estou ansioso para ver mais testes e experiências sendo publicadas. Um [primeiro teste básico feito pelo httpwatch.com](https://blog.httpwatch.com/2015/01/16/a-simple-performance-comparison-of-https-spdy-and-http2) pode indicar que o HTTP/2 cumpre o que promete.

### 8.4.7. “Não respeita camadas”

Sério, esse é o seu argumento? Camadas não são pilares intocáveis e sagrados de uma religião mundial e, se nós cruzamos algumas áreas cinzentas durante a criação do http2, foi no interesse de fazer um protocolo bom e efetivo dentro das restrições inidicadas.

### 8.4.8. “Não corrige várias deficiências do HTTP/1.1”

É verdade. Com o objetivo específico de manutenção dos paradigmas do HTTP/1.1, houve vários recursos antigos do HTTP que tiveram que permanecer. Tais como os cabeçalhos comuns que também incluem os temidos _cookies_, cabeçalhos de autorização, entre outros. Mas como contraponto à manutenção desses paradigmas, temos um protocolo onde é possível ser implantado sem uma certa obrigação de substituir ou reescrever uma quantidade inconcebível de trabalho. http2 é basicamente uma nova camada de "framming".

## 8.5. http2 será amplamente utilizado?

Ainda é muito cedo para dizer com certeza, mas eu posso adivinhar e estimar e é isso que eu vou fazer aqui.

Os pessimistas dirão “veja o bem que o IPv6 fez”, como exemplo de um novo protocolo que demorou décadas para começar a ser amplamente utilizado. http2 não é um IPv6. Este é um protocolo no topo do TCP utilizando os mecanismos de atualização do HTTP, números de porta, TLS etc. Não exigirá nenhuma mudança de roteadores ou _firewalls_.

Google provou para o mundo com o trabalho no SPDY que um novo protocolo pode ser implantado e utilizado por navegadores e serviços com múltiplas implementações em um curto espaço de tempo. Enquanto o percentual de servidores na Internet que oferecem atualmente está na faixa de 1%, a quantidade de dados que estes servidores lidam é muito maior. Alguns dos mais populares _web sites_ oferecem SPDY.

http2, baseado nos mesmos paradigmas do SPDY, eu diria que tem mais chances de ser mais utilizado uma vez que é um protocolo IETF. A implementação do SPDY sempre foi retido pelo estigma de “ser um protocolo Google”.

Existem vários grandes navegadores por trás do lançamento. Representantes do Firefox, Chrome, Safari, Internet Explorer e Opera disseram que irão entregar navegadores com suporte ao http2 e mostraram trabalhos em desenvolvimento.

Existem vários operadores de servidores que estão dispostos a oferecer suporte ao http2 em breve, incluindo Google, Twitter e Facebook e nós esperamos ver esse suporte sendo adicionado em implementações de servidores populares como Apache HTTP Server e nginx. H2o é um novo servidor HTTP incrivelmente rápido com suporte ao http2 e que trás muito potencial.

Alguns dos maiores fornecedores de _proxy_, incluindo HAProxy, Squid and Varnish anunciaram suas intenções para apoiar http2.

Durante todo o ano de 2015, a quantidade de tráfego utilizando http2 tem aumentado. No início de Setembro, o uso no Firefox 40 foi de 13% de tráfego HTTP e 27% de tráfego HTTPS, enquanto o Google está recebendo aproximadamente 18% de requisições HTTP/2. Deve-se notar que o Google executa outras novas experiências com novos protocolos (veja QUIC em 12.1) o que faz com que o nível de uso do http2 seja menor do que seria.
