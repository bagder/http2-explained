# 4. Actualizando HTTP

Não seria bom fazer melhorias no protocolo? Algo incluindo...



1. Fazer com que o protocolo seja menos sensível a RTT.
2. Corrigir o pipelining e o primeiro da fila.
3. Parar o desejo e necessidade e continuar a aumentar o numero de conexões para cada anfitrião
4. Manter todas as interfaces existentes, todo o conteúdo, e os formatos e esquemas de URI
5. Isso seria feito com o grupo de trabalho IETF's HTTPbis

## 4.1. IETF e o grupo de trabalho HTTPbis

A Internet Engineering Task Force (IETF) é uma organização que desenvolve e promove standards de internet. Essencialmente ao nível de protocolo. Eles são especialmente conhecidos pela série de documentação RFC documentando tudo desde das melhores práticas até TCP, DNS, FTP, HTTP e numerosas variantes de protocolo que nunca se desenvolveram. 

Os grupos de trabalho dentro do IETF são formados com um âmbito limitado de trabalhar para um objectivo. Eles estabelecem um "carácter" com algumas guias e limitações que elas possam produzir. Qualquer pessoa e todo o mundo é permitido de se juntar às discussões e desenvolvimento. Alguém que se junte às discussões e diga algo tem as mesma possibilidade de afetar o resultado final e toda a gente conta como humanos e indivíduos, pouco conta para que empresas esses mesmo indivíduos trabalhem.   

O grupo de trabalho HTTPbis ( veja mais a baixo a explicação do nome ) foi formada durante o verão de 2007 e foi encomendada a tarefa de actualizar a especificação do HTTP 1.1. Dentro deste grupo as discussões sobre a nova versão do HTTP só começaram realmente no final de 2012. O trabalho de actualização do HTTP 1.1 foi completado no inicio de 2014 e resultou nas séries de [RFC 7320](https://tools.ietf.org/html/rfc7320).

Na reunião operativa para o grupo de trabalho HTTPbis foi feita na cidade de Nova York no inicio de 2014. As discussões seguintes e procedimentos IETF foram realizadas para ter de fato o RFC oficial que se sucederam até ao ano seguinte.


Alguns dos agentes principais no ramo de HTTP não estiveram nos debates e reuniões do grupo de trabalho. Eu não quero mencionar nenhuma das empresas ou nome de produtos aqui, mas claramente que alguns agentes da internet hoje em dia estão confiantes que o IETF fará um bom trabalho sem que estas empresas estejam envolvidas...


### 4.1.1. A parte "bis" do nome

O grupo chama-se HTTPbis de onde a parte "bis" vem do [advérbio de Latim dois ](http://en.wiktionary.org/wiki/bis#Latin). Bis é usado vulgarmente como sufixo ou parte do nome no IETF para uma actualização ou uma segunda revisão da especificação. Tal como no caso do HTTP 1.1.


## 4.2. http2 começou com o SPDY

[SPDY](http://en.wikipedia.org/wiki/SPDY) é um protocolo encabeçado e desenvolvido pela Google. Eles certamente que o desenvolveram abertamente ao mundo e convidaram toda a gente a participar mas era obvio que iriam beneficiar se fosse controlado por ambas implementações nos browsers e uma população significante de servidores que sejam muito utilizados.

Quando o grupo HTTPbis decidiu que era tempo de começar a trabalhar no http2, o SPDY ja tinha demonstrado que era um conceito que funcionava. Mostrou que era possível implementar na Internet e havia números publicados que provavam a sua performance. O trabalho http2 começou subsequentemente depois do rascunho do SPDY/3 que era basicamente o rascunho http2 draft-00 com um pouco de mudanças. 
  
