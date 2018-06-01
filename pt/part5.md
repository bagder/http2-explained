# 5. Conceitos de http2

Afinal o que o http2 consegue fazer? Onde está o limite que o grupo HTTPbis se encarregou de fazer?

Na realidade foram restritos e mantiveram algumas restrições na capacidade de inovar dentro da equipa.


- Tem que manter paradigmas de HTTP. Ainda é um protocolo onde o cliente envia pedidos ao servidor através de TCP.

- http:// and https:// URLs não podem ser alterados. Não pode haver nenhum esquema novo para isto. A quantidade de conteúdo usado por URLs é demasiado grande para esperar uma mudança.

- Os servidores e clientes de HTTP1 vão existir algumas décadas, precisamos de fazer com que os proxies possam servir http2. 

- Subsequentemente, proxies precisam de mapear funcionalidades a clientes HTTP 1.1 uma a uma.

- Remover ou reduzir partes opcionais do protocolo. Isto não foi realmente um requisito mas sim o mantra que veio do SPDY e da equipa da Google. Tendo a certeza que tudo é mandatório não existe uma maneira que se possa implementar tudo agora e no futuro não posa haver uma ratoeira.

- Nenhuma versão menor. Foi decidido que os clientes e servidores são compatíveis com o http2 ou não são. Se existir a necessidade de estender o protocolo ou modificar algo, então o http3 nascerá. Não existem mais versões pequenas no http2. 


## 5.1. http2 para esquemas existentes URI 

Como já foi mencionado os esquemas URI existentes não podem ser modificados, o http2 tem que ser criado usando os existentes. Uma vez que não são usados hoje em dia no HTTP 1.x, precisamos de deixar obviamente uma maneira de poder fazer upgrade para o http2 ou pedir ao servidor para usar http2 em vez de protocolos anteriores. 

HTTP 1.1 define a maneira como isto se faz, especialmente o cabeçalho Upgrade:, que permite o servidor enviar de volta uma resposta utilizando um protocolo novo quando recebe esse pedido usando um protocolo antigo. Com um custo de um round-trip.

Essa penalização de roud-trip não era algo que a equipa de SPDY pudesse aceitar, e uma vez que eles apenas implementaram SPDY sobre TLS eles desenvolveram a nova extensão de TLS que é utilizada como atalho para uma negociação muito significativamente. Usando esta extensão, chamada NPN for Next Protocol Negotiation, o servidor diz ao cliente quais os protocolos que conhece e o cliente pode então escolher o protocolo que prefere. 


## 5.2. http2 para https://

Muito do foco do http2 foi de fazer com que este tenha um comportamento correcto sobre TLS. SPDY apenas funciona sobre TLS e existia muita pressão para que no http2 fosse também mandatório mas não se chegou a um consenso e o http2 foi lançado com o TLS como opcional. No entanto, dois implementadores mais destacados disseram claramente que iram implementar apenas o http2 sobre TLS: A iniciativa Mozilla Firefox e a iniciativa Google Chrome. Dois dos principais browsers hoje em dia. 

As razões para escolher apenas-TLS incluí respeito pela privacidade do utilizador e medições precoces mostraram que os novos protocolos têm maior taxa de sucesso quando utilizam TLS. Isto deve-se à suposição que qualquer tráfico que vá através da porta 80 é HTTP 1.1 faz com que qualquer dispositivo que intercepte o tráfico possa interferir ou destruir o mesmo quando se trata de um protocolo distinto de HTTP 1.1. 

O assunto de o TLS ser mandatário tem causado muito movimento e muitas vozes agitadas nas listas de correio e reuniões - é bom ou mau? É um tema infectado - tenha em conta isto se perguntares alguma coisa a um membro da HTTPbis!


De maneira similar, há um debate feroz e durador sobre se o http2 deve impor a lista de cifras que devem ser mandatórias quando se usa TLS, ou se talvez se deve-se bloquear algumas ou se não deveria ser um requisito de todo pela camada do TLS mas deixe isso para o grupo de trabalho do TLS. A especificação determinou que o TLS deveria pelo menos usar a versão 1.2 e há algumas restrições de cifras.


## 5.3. negociação http2 sobre TLS

Next Protocol Negotiation (NPN), é um protocolo usado para negociar SPDY com servidores TLS. Como não se tratava de um Standard, foi levado à IETF e daí surgiu a ALPN: Application Layer Protocol Negotiation. ALPN é o que tem sido promovido a ser usado com http2, enquanto os clientes de SPDY ainda usam a NPN.

O facto que a NPN existe antes da ALPN levou algum tempo até a sua estandardização e levou a muitas implementações antecedentes de clientes e servidores http2. Também, a NPN é usada para muitos servidores que usam tanto SPDY como http2, suportar tanto NPN como ALPN nesses servidores faz todo o sentido. 


A principal diferença entre ALPN e NPN é quem decide o protocolo a usar. com ALPN os clientes dizem ao servidor a lista de protocolos e a sua ordem de preferencia e o servidor escolhe a que quer, enquanto no NPN o cliente faz a escolha final. 


## 5.4. http2 para http://

Como já foi mencionado anteriormente, para HTTP 1.1 em pleno texto a forma de negociar http2 é de pedir ao servidor com um cabeçalho Upgrade:. Se o servidor falar http2 irá responder com um estado "101 Switiching" e a partir daí comunica em http2 nessa conexão. Você nota que isto é uma forma de upgrade que tem o custo de um round-trip completo, mas a vantagem é que a conexão pode ser mantida e reutilizada de maneira mais generalizada do que as conexões HTTP1

Enquanto alguns representantes de browsers declararam que não iriam implementar este modo de falar http2, a equipa do Internet Explorer comunicou que irá, assim como o curl que já suporta este modo.
