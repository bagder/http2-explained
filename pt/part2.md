# 2. HTTP hoje

HTTP 1.1 transformou-se em num protocolo usado para praticamente tudo na Internet. Grandes investimentos foram feitos em protocolos e infra-estrutura que se aproveitam dele. Isto é considerado na medida em que muitas vezes é mais fácil hoje fazer as coisas correm em cima de HTTP ao invés de construir algo pŕopriamente novo.


## 2.1 HTTP 1.1 é enorme

Quando se criou o HTTP e foi dado ao mundo, foi concebido como um protocolo muito mais simples e direto, mas o tempo provou o contrário. HTTP 1.0 no RFC 1945 é uma espicificação
de 60 páginas publicada em 1996. O RFC 2616 que descreve o HTTP 1.1, foi publicado apenas três anos mais tarde em 1999 e cresceu significativamente para 176 páginas. Todavia quando trabalhámos dentro do IETF na actualização da especificação, foi repartida e convertida em seis documentos, com uma quantidade maior de páginas no total ( resultando no RFC 7230 e a sua família). De qualquer modo, HTTP 1.1 é grande e incluí uma grande variedade de detalhes e sutilezas, sem ignorar grandes quantidades de importantes peças opcionais.

## 2.2 Um mundo de opções

A natureza do HTTP 1.1 de ter vários pequenos detalhes e opções disponíveis para expansão futura, permitiu um crescimento de um ecosistema de software em que quase nenhuma implementação implementa tudo - e não é realmente possivel dizer o que "tudo" é. Isto levou a uma situação onde as funcionalidades que foram inicialmente pouco usadas viram poucas implementações e aqueles que implementaram estas funcionalidades viram serem pouco utilizadas.

Depois mais tarde, causaram um problema de interoperabilidade quandos os clientes e servidores começaram a utilizar estas funcionalidades . HTTP Pipelining é um exemplo primário destas funcionalidades.

## 2.3 Uso inadequado do TCP

Tem sido díficil o HTTP 1.1 tirar partido completo de todo o poder e performance que o TCP oferece. Clientes HTTP e browsers têm que ser muito criativos para encontrar soluções que diminuam o tempo que uma página leva a carregar.

Outras tentativas que foram feitas em paralelo ao longo dos anos também vieram provar que o TCP não é assim tão facil de substituir e como tal continuamos a melhorar ambos o TCP e os protocolos no topo dele.

Posto de forma simples, TCP pode ser melhor utilizado para evitar pausas ou momentos no tempo que poderiam ter sido utilizados para enviar ou receber mais dados. A próxima seção vai realçar algumas dessas deficiências.


## 2.4 Tamanhos de transferência e número de objetos

Ao olhar para a tendência de alguns dos sites mais populares na web de hoje, e o tempo que leva a fazer download das suas páginas principais, um padrão claro emerge. Ao londo dos anos a quantidade de dados que é necessário descarregar tem vindo a aumentar acima de 1.9MB. O que é mais importante neste contexto é uma média de uma centena de recursos individuais que são necessários para exibir cada página.


Como o gráfico abaixo mostra, a tendência já se arrasta por um tempo e há pouca ou nenhuma indicação de que isso venha a mudar tão cedo. Ele mostra o crescimento do tamanho total de transferência (em verde) e o número total de solicitações usada em média (em vermelho) para servir os sites mais populares do mundo, e como eles mudaram ao longo dos últimos quatro anos.

![transfer size growth](https://raw.githubusercontent.com/bagder/http2-explained/master/images/transfer-size-growth.png)

## 2.5 Latencia mata

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/page-load-time-rtt-decreases.png" />

HTTP 1.1 é muito sensível à latência, em parte porque HTTP Pipelining ainda é cheio de problemas o suficiente para permanecer desligada para uma grande porcentagem dos utilizadores.

Enquanto nós vimos um grande aumento na largura de banda disponível para as pessoas ao longo dos últimos anos, não temos visto o mesmo nível de melhorias na redução da latência. Links de alta latência, como muitas das atuais tecnologias móveis, torna realmente difícil de conseguir uma boa e rápida experiência web mesmo se você tiver uma largura de banda muito alta.

Outro caso que realmente precisa de baixa latência é a de certos tipos de vídeo, como videoconferência, jogos e similares, onde não há apenas um fluxo pré-gerado para enviar para fora.

## 2.6. Bloqueio do primeiro da fila

HTTP Pipelining é uma forma de enviar um outro pedido enquanto aguarda a resposta a um pedido anterior. É muito semelhante a filas de um banco ou de um supermercado. Você só não sabe se a pessoa que está há sua frente é um cliente rápido ou um cliente irritante que vai demorar uma eternidade antes que ele / ela finalize: Bloqueio do primeiro da fila.

<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/head-of-line-blocking.jpg" />

Claro que você pode ter cuidado com a fila que escolhe, escolhendo aquela que você realmente acredita que é a correta, e às vezes você pode até mesmo iniciar uma nova fila por si mesmo, mas no final você não pode evitar de tomar uma decisão, e uma vez que é feita você não pode alternar filas.

A criação de uma nova fila está também associada com o desempenho de recursos, de modo que não é escalável para além de um menor número de filas. Simplesmente não há solução perfeita para isso.

Ainda hoje, em 2015, a maioria dos browsers de desktop fornecido com o HTTP pipelining vêem desativados por padrão.

Leitura adicional sobre este assunto podem ser encontrados por exemplo no Firefox[bugzilla entry 264354](https://bugzilla.mozilla.org/show_bug.cgi?id=264354).
