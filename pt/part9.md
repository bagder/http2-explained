# 9. http2 e Firefox

Firefox vem acompanhando as alterações de perto e tem proporcionado implementações http2 de testes por muitos meses. Durante o desenvolvimento do protocolo http2, clientes e servidores têm que concordar sobre qual versão do rascunho (_draft_) do protocolo implementar, o que dificulta um pouco a execução dos testes. Fique atento para que o cliente e servidor implementem a mesma versão de rascunho do protocolo.

## 9.1. Em primeiro lugar, verifique se está habilitado

Em todas as versões do Firefox desde a versão 35, lançada em 13 de Janeiro de 2015, o suporte a http2 está habilitado por padrão.

Entre na seção 'about:config' na barra de endereços e procure pela opção “network.http.spdy.enabled.http2draft”. Tenha certeza que está definida para *true*. Firefox 36 adicionou outra configuração chamada “network.http.spdy.enabled.http2” que é definida como *true* por padrão. Esta última opção controla o http2 versão "simples" (_"plain"_), enquanto que a primeira opção habilita e desabilita a negociação de versões rascunho do http2. Ambas são definidas como _true_ desde o Firefox 36.

## 9.2. Somente TLS

Lembre-se que o Firefox somente implementa http2 sobre TLS. Você somente verá http2 em ação com Firefox quando navegar em _sites_ https:// e que ofereçam suporte http2.

## 9.3. Transparente!

![transparent http2 use](https://raw.githubusercontent.com/bagder/http2-explained/master/images/firefox-screenshot.png)

Não existe nenhum elemento de interface que indique que você está "falando" http2. Não é possível dizer facilmente. Uma forma de descobrir é habilitando o modo "Web developer -> Network" e verificar os cabeçalhos de resposta recebidos do servidor. A resposta é “HTTP/2.0” e o Firefox adiciona seu próprio cabeçalho chamado “X-Firefox-Spdy:”, como mostra a imagem acima.

Os cabeçalhos que você vê na aba de rede ao utilizar http2 foram convertidos a partir do formato binário do http2, para o estilo clássico de cabeçalhos do HTTP 1.x. 

## 9.4. Visualizar o uso do http2

Existem plugins do Firefox disponíveis que ajudam a visualizar se um _site_ está utilizando http2. Um deles é o [“HTTP/2 and SPDY Indicator”](https://addons.mozilla.org/en-US/firefox/addon/spdy-indicator/).
