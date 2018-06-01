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

## 6.5. Compressão do cabeçalho

HTTP é um protocolo sem estado (_stateless_).

HTTP is a stateless protocol. Em suma, isso significa que cada requisição necessita enviar ao servidor todos os detalhes necessários para atender essa solicitação, sem que o servidor necessite armazenar uma grande quantidade de informações e metadados de requisições anteriores. Como o http2 não muda esse paradigma, ele tem que funcionar da mesma forma.

Isso torna o HTTP repetitivo. Quando um cliente solicita muitos recursos do mesmo servidor, como imagens de uma página _web_, haverá uma série de requisições e todas parecerão idênticas. Uma séria de coisas quase idênticas implora por compressão.

Enquanto o número de objetos por página _web_ tem aumentado (como mencionado anteriormente), o uso de cookies e o tamanho das requisições também tem aumentado ao longo do tempo. Cookies também precisam ser incluídos em todas as requisições, muitas vezes os mesmos em várias requisições.

As requisições HTTP 1.1 tem realmente crescido tanto de tamanho que às vezes elas acabam ficando maior que a janela TCP inicial, o que as torna muito lentas para enviar, pois elas necessitam de um _round-trip_ completo para obter um ACK de volta do servidor antes que a requisição completa seja enviada. Esse é um outro argumento para compressão.

### 6.5.1. Compressão é um assunto complicado

A compressão de HTTPS e SPDY foi descoberta vulnerável aos ataques [BREACH](https://en.wikipedia.org/wiki/BREACH_%28security_exploit%29) e [CRIME](https://en.wikipedia.org/wiki/CRIME). Inserindo um texto conhecido no fluxo e descobrindo como isso altera a saída, um invasor pode descobrir o que está sendo enviado em uma carga criptografada.

Realizando compressão em conteúdo dinâmico para um protocolo - sem se tornar vulnerável a um destes ataques - exige um pouco de reflexão e análise cuidadosa. Isto é o que o time HTTPbis tentou fazer.

Entra o [HPACK](https://www.rfc-editor.org/rfc/rfc7541.txt), compressão de cabeçalho para HTTP/2, que - como o nome sugere - é um formato de compressão especialmente concebido para cabeçalhos http2, e está sendo especificado em um projeto separado. O novo formato, juntamente com outras medidas (como alguns pedem a intermediários que não comprimam cabeçalhos específicos e preenchimento opcional de quadros), deve torná-lo mais difícil de explorar a compressão.

Nas palavras de Roberto Peon (um dos criadores do HPACK):

> "HPACK" foi projetado para tornar o vazamento de informações mais difícil em
> uma implementação de acordo, realizar a codificação e decodificação mais rápida
> e barata, fornecer para o receptor controle sobre a compressão do tamanho do
> contexto, permitir a reindexação por parte do proxy (ou seja, estado compartilhado)
> entre _frontend_ e _backend_ dentro de um proxy), e comparações rápidas de
> _strings_ utilizando a codificação de Huffman".

## 6.6. Reset - mude de ideia

Uma das desvantagens do HTTP 1.1 é que, quando uma mensagem é enviada com o cabeçalho _Content-Length_ com um certo tamanho, não é possível cancelá-lo facilmente. Você pode (mas nem sempre) cancelar a conexão TCP, mas isso vem com o custo de negociar um _handshake_ TCP novo.

Uma solução melhor seria apenas parar a mensagem e enviar uma nova. Isto pode ser feito com o quadro RST_STREAM do http2 que ajudará a previnir desperdícios e a necessidade de cancelar conexões.

## 6.7. Server push

Esta funcionalidade também é conhecida como "cache push". A ideia é que, se o cliente solicita o recurso X, o servidor pode determinar que o cliente também solicitará o recurso Z e enviará sem que o cliente o solicite. Isso ajudará o cliente colocando o recurso Z em cache de forma que ele estará lá quando for solicitado.

O envio a partir do servidor (_server push_) é algo que o cliente deve explicitamente permitir que o servidor faça. Mesmo assim, o cliente pode rapidamente encerrar o fluxo a qualquer momento com RST_STREAM não permitindo assim nenhum recurso em particular.

## 6.8. Controle de fluxo

Cada fluxo http2 individual tem sua própria janela de fluxo anunciada, onde a outra extremidade pode enviar informações. Se você conhece como SSH funciona, isto é muito semelhante em estilo e espírito.

Para cada fluxo (_stream_), ambas as extremidades tem que informar ao seu par que possuem espaço suficiente para lidar com os dados de entrada, e a outra ponta só é permitida enviar a quantidade informada até que a janela se estenda. Só existe controle de fluxo para quadros (_frames_) DATA.
