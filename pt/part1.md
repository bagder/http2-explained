# 1. Historial

Este documento descreve o http2 de uma perspectiva técnica e ao nível do protocolo. Começou
como uma apresentação que Daniel fez em Estocolmo em Abril 2014 que foi
subsequentemente convertido e alargado para um documento com todos os 
detalhes e explicações adequadas.

RFC 7540 é o nome oficial da especificação final do HTTP2 e que foi publicado em 15 de maio de 2015: http://www.rfc-editor.org/rfc/rfc7540.txt

Todos e quaisquer erros neste documento são da minha autoria e os resultados da minha 
falha. Por favor indique-nos para que possam ser corrigidos numa versão actualizada.


Neste documento, eu tentei usar consistentemente a palavra "HTTP2" para descrever
o protocolo em termos técnicos, o nome correcto é HTTP/2 Eu
fiz essa escolha por uma questão de legibilidade e de obter um melhor fluxo na
Língua.


## 1.1 Autor

O meu nome é Daniel Stenberg e trabalho para a Mozilla. Tenho trabalhado com
open source e redes há mais de vinte anos para numerosos projetos. Possivelmente sou
conhecido por ser o principal desenvolvedor do curl e libcurl. Fui envolvido
no grupo IETF HTTPbis durante vários anos e aí pude estar a par com o trabalho 
do HTTP 1.1 assim bem como no envolvimento do processo de estandardização do http2



  Email: daniel@haxx.se

  Twitter: [@bagder](https://twitter.com/bagder)

  Web: [daniel.haxx.se](http://daniel.haxx.se/)

  Blog: [daniel.haxx.se/blog](http://daniel.haxx.se/blog/)

## 1.2 Ajuda!


Se encontrar problemas, omissões, erros ou mentiras descaradas neste documento, por favor envie-me uma versão corrigida do parágrafo e eu farei versões corrigidas. Eu darei
os merecidos créditos a todas as pessoas que ajudarem! Eu espero fazer este documento melhor ao longo do tempo.

Este documento está disponível em [http://daniel.haxx.se/http2](http://daniel.haxx.se/http2)


## 1.3 Licença

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/creative-commons.png" />

Este documento está licenciado sob a licença Creative Commons Attribution 4.0: http://creativecommons.org/licenses/by/4.0/

## 1.4 História do documento

A primeira versão deste documento foi publicada em 25 de abril de 2014. Aqui segue as maiores alterações nas versões mais recentes do documento.

### Versão 1.13

- Convertido a versão mestre deste documento para sintaxe Markdown 
- 13: Menção de mais recursos, links atualizados e descrições
- 12: Atualizada a descrição QUIC com referência para a elaboração 
- 8.5: Actualizado com números actuais 
- 3.4: A média é de agora 40 conexões TCP
- 6.4: Atualizado para refletir o que a especificação diz

### Versão 1.12

- 1.1: HTTP / 2 está agora em um RFC oficial
- 6.5.1: Link para o RFC HPACK 
- 9.1: Menção para alterar a configuração do Firefox 36+ para http2 
- 12.1: Adicionada uma seção sobre QUIC 

### Versão 1.11

- Muitos melhoramentos de língua, a maioria indicados pelos amigos contribuidores
- 8.3.1: Menção de ações especificas de nginx e Apache httpd

### Versão 1.10

- 1: O protocolo foi "okayed"
- 4.1: Actualizada a redacção desde 2014 é o ano passado
- Frente: Adicionada imagem e chamada de “http2 explained”, link corrigido 
- 1.4: Adicionado a seção Historial do documento 
- Muitos erros ortográficos e gramaticais corrigidos 
- 14: Adicionado obrigado aos repórteres de bugs
- 2.4: Melhores legendas para os gráficos de crescimento HTTP 
- 6.3: Corrigida a ordem da carruagem na ordem to comboio multiplexado. 
- 6.5.1: HPACK rascunho-12 

### Versão 1.9

- Actualizado para HTTP/2 rascunho-17 e HPACK rascunho-11  
- Adicionada seção "10. http2 in Chromium" (== agora cumprimento de uma página)  
- Muitas correcções ortográficas
- 30 implementações agora 
- 8.5: Adicionado alguns números de uso atuais 
- 8.3: Mencione o Internet Explorer também  
- 8.3.1 Adicionado implementações "em falta"  
- 8.4.3: Mencionado que TLS também aumenta a taxa de sucesso
