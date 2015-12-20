# 6. O protocolo http2

Suficiente sobre os bastidores, a história e política por trás do que nos trouxe até aqui. Vamos mergulhar nos detalhes do protocolo: os bits e os conceitos que constroem o http2.

## 6.1. Binário

http2 é um protocolo binário.

Apenas reflita sobre isso por um minuto. Se você tem se envolvido com protocolos da Internet antes, as chances são de que você começará a reagir instintivamente contra essa escolha, empacotando seus argumentos que dizem como protocolos baseados em texto/ascii são superiores porque humanos conseguem manusear requisições via telnet e assim por diante...

http2 é binário para tornar a construção muito mais fácil. Descobrir o início e o fim dos pacotes é uma das coisas mais complicadas no HTTP 1.1 e, na verdade, em protocolos baseados em texto em geral. Eliminando espaços em branco opcionais e diferentes formas de escrever a mesma coisa, a implementação se torna mais simples.

Além disso, se torna muito mais fácil separar as partes atuais do procolo da sua elaboração - o que é muito confuso e misturado no HTTP1.

O fato do protocolo oferecer compressão e que muitas vezes será executado sobre TLS diminui o valor do texto, pois você não verá texto trafegando de qualquer maneira. Nós simplesmente temos que nos acostumar com a ideia de utilizar uma ferramenta como o Wireshark para descobrir exatamente o que está acontecendo no nível do protocolo http2.

A depuração deste protocolo , provavelmente, será feita utilizando ferramentas como curl ou analisando o fluxo da rede com o _dissector_ de http2 do Wireshark ou algo similar.

## 6.2. O formato binário

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/frame-layout.png" />

http2 envia quadros binários. Existem diferentes tipos de quadro que podem ser enviados e todos têm a mesma configuração: _Length_ (comprimento), _Type_ (tipo), _Flags_ (configurações), _Stream Identifier_ (identificador de fluxo) e _frame payload_ (quadro de informação).

Existem 10 diferentes tipos de quadro definidos na especificação do http2 e talvez os dois tipos fundamentais que mapeiam para características do HTTP 1.1 são _DATA_ e _HEADERS_. Eu vou descrever alguns dos quadros com mais detalhes adiante.

## 6.3. Fluxos multiplexados

O identificador de fluxo (_Stream Identifier_) mencionado na seção anterior associa cada quadro enviar via http2 com um "fluxo". Um fluxo é uma sequência de quadros independentes e bi-direcionais trocadas entre o cliente e o servidor utilizando uma conexão http2.

Um única conexão http2 pode conter vários fluxos concorrentemente abertos, com cada extremidade entrelaçando múltiplos fluxos. Fluxos podem ser estabelecidos e utilizados unilateralmente ou compartilhados pelo cliente ou servidor e eles podem ser encerrados por qualquer extremidade. A ordem em que os quadros são enviados dentro de um fluxo é importamente. Destinatários processam quadros na ordem em que eles são recebidos.

Multiplexar o fluxo significa que os pacotes de vários fluxos são misturados sobre a mesma conexão. Dois (ou mais) trens de dados são transformados em um e depois são divididos novamente do outro lado. Aqui estão os dois trens:

![one train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-justin.jpg)
![another train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-ikea.jpg)

Os dois trens multiplexados na mesma conexão:

![multiplexed train](https://raw.githubusercontent.com/bagder/http2-explained/master/images/train-multiplexed.jpg)

## 6.4. Prioridades e dependências

Cada fluxo tem uma prioridade (também conhecido como "peso") que é utilizada para dizer ao par (_peer_) quais são os fluxos mais importantes, no caso de existirem restrições de recursos que forçam o servidor a selecionar quais fluxos enviar primeiro.

Utilizando o quadro PRIORITY, um cliente também pode dizer ao servidor qual outro fluxo este fluxo atual depende. Ele permite que o cliente construa uma árvore de prioridades onde vários "fluxos filho" podem depender da conclusão de "fluxos pai".

A prioridade dos pesos e as dependências podem ser modificadas dinamicamente em tempo de execução, o que pode permitir que os navegadores especifiquem, numa página cheia de imagens e o usuário utiliza a barra de rolagem, quais são as imagens mais importantes para serem carregadas, ou se você alterna as abas ele pode priorizar um novo grupo de fluxos que, de repente, entram em foco.

## 6.5. Header compression

HTTP is a stateless protocol. In short, this means that every request needs to bring with it as much detail as the server needs to serve that request, without the server having to store a lot of info and meta-data from previous requests. Since http2 doesn't change this paradigm, it has to work the same way.

This makes HTTP repetitive. When a client asks for many resources from the same server, like images from a web page, there will be a large series of requests that all look almost identical. A series of almost identical somethings begs for compression.

While the number of objects per web page has increased (as mentioned earlier), the use of cookies and the size of the requests have also kept growing over time. Cookies also need to be included in all requests, often the same ones in multiple requests.

The HTTP 1.1 request sizes have actually gotten so large that they sometimes end up larger than the initial TCP window, which makes them very slow to send as they need a full round-trip to get an ACK back from the server before the full request has been sent. This is another argument for compression.

### 6.5.1. Compression is a tricky subject

HTTPS and SPDY compression were found to be vulnerable to the [BREACH](http://en.wikipedia.org/wiki/BREACH_%28security_exploit%29) and [CRIME](http://en.wikipedia.org/wiki/CRIME) attacks. By inserting known text into the stream and figuring out how that changes the output, an attacker can figure out what's being sent in an encrypted payload.

Doing compression on dynamic content for a protocol - without becoming vulnerable to one of these attacks - requires some thought and careful consideration. This is what the HTTPbis team tried to do.

Enter [HPACK](http://www.rfc-editor.org/rfc/rfc7541.txt), Header Compression for HTTP/2, which – as the name suggests - is a compression format especially crafted for http2 headers, and it is being specified in a separate internet draft. The new format, together with other counter-measures (such as a bit that asks intermediaries to not compress a specific header and optional padding of frames), should make it harder to exploit compression.

In the words of Roberto Peon (one of the creators of HPACK):

> “HPACK was designed to make it difficult for a conforming implementation to
> leak information, to make encoding and decoding very fast/cheap, to provide
> for receiver control over compression context size, to allow for proxy
> re-indexing (i.e., shared state between frontend and backend within a proxy),
> and for quick comparisons of Huffman-encoded strings”.

## 6.6. Reset - change your mind

One of the drawbacks with HTTP 1.1 is that when an HTTP message has been sent
off with a Content-Length of a certain size, you can't easily just stop
it. Sure, you can often (but not always) disconnect the TCP connection, but that
comes at the cost of having to negotiate a new TCP handshake again.

A better solution would be to just stop the message and start a new. This can be done with http2's RST_STREAM frame which will help prevent wasted bandwidth and the need to tear down connections.

## 6.7. Server push

This is the feature also known as “cache push”. The idea is that if the client asks for resource X, the server may know that the client will probably want resource Z as well, and sends it to the client without being asked. It helps the client by putting Z into its cache so that it will be there when it wants it.

Server push is something a client must explicitly allow the server to do. Even then, the client can swiftly terminate a pushed stream at any time with RST_STREAM should it not want a particular resource.

## 6.8. Flow Control

Each individual http2 stream has its own advertised flow window that the other end is allowed to send data for. If you happen to know how SSH works, this is very similar in style and spirit.

For every stream, both ends have to tell the peer that it has enough room to handle incoming data, and the other end is only allowed to send that much data until the window is extended. Only DATA frames are flow controlled.
