# 3. Estratégias para evitar a dor da latência

Como sempre quando se enfrentam problemas, as pessoas tentam encontrar soluções.
Algumas das soluções são inteligentes e úteis, outras são apenas horríveis.



## 3.1 Spriting
<img style="float: right;" src="https://raw.githubusercontent.com/bagder/http2-explained/master/images/spriting.jpg" />



Spriting é o termo corrente utilizado para descrever quando você põe várias imagens pequenas juntas numa só imagem grande. Depois utiliza javascript ou CSS para "cortar" pedaços dessa mesma imagem grande para mostrar imagens individuais pequenas. 


Um site utilizaria este truque para velocidade. Descarregar uma imagem grande é muito mais rápido em HTTP 1.1 do que descarregar 100 imagens individuais pequenas.

Claro que isto tem as suas desvantagens para uma página de um site que apenas quer mostrar uma ou duas das imagens pequenas ou similar. Isto também faz com que as imagens sejam descartadas do cache ao mesmo tempo em vez de deixar permanecer as que são utilizadas com mais frequência.


## 3.2 Inlining

Inlining é outro truque para evitar enviar imagens individuais, e isto é feito utilizando data: URLs embutidos num ficheiro CSS. Isto tem benefícios similares e incovinientes como no caso de spriting. 


    .icon1 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }

    .icon2 {
        background: url(data:image/png;base64,<data>) no-repeat;
    }


## 3.3 Concatenação

Um site grande pode ter vários ficheiros diferentes de javascript. Ferramentas Front-end vão ajudar os programadores a junta-los num só ficheiro grande que o browser irá descarregar em vez de várias dezenas de ficheiros pequenos. Muitos dados serão enviados quando apenas poucos são necessários. Muitos dados terão que ser recarregados quando uma alteração é necessária.

Esta prática é claramente uma inconveniência para os programadores envolvidos.


## 3.4 Sharding

O último truque que irei mencionar é normalmente referido como “sharding”. Basicamente significa servir aspetos do seu serviços num numero máximo possível de servidores. De inicio isto parece estranho mas existe uma boa razão por detrás da mesma.


Inicialmente a especificação do HTTP 1.1 dita que o cliente poderia utilizar um máximo de duas conexões TCP para cada anfitrião. Então, como forma de não violar a especificação sites inteligentes inventaram novos host names e - voilá - pode ter mais conexões ao seu site e diminuir o tempo que leva a carregar.

Ao longo do tempo, essa limitação foi removida e hoje os clientes facilmente utilizam 6-8 conexões por anfitrião mas continuam a ter um limite e como tal os sites continuam a utilizar esta técnica para aumentar o numero de conexões. Como o número de objectos está sempre a aumentar - como eu já demonstrei - o número elevado de conexões é apenas usado para ter a certeza que o HTTP executa corretamente e o site é rápido. Isto não é raro que um site utilize 50 ou mesmo 100 ou mais conexões para apenas um site utilizando esta técnica.  

Estatísticas recentes do httparchive.org mostram que os primeiros 300.000 URLs do mundo precisam em media de 40(!) conexões TCP para mostrar o site, e a expectativa é de crescer lentamente ao longo do tempo.

Outra razão é também de meter imagens ou recursos similares num anfitrião à parte que não utilize cookies, pelo que o tamanho das cookies nos dias correntes poder ser tanto ou quanto significante. Utilizando um anfitrião cookie-free pode por vezes aumentar a performance simplesmente permitindo pedidos de HTTP muito mais pequenos.

A imagem em baixo mostra como trace de packets é quando estamos a navegar um dos top sites da Suécia e como os pedidos estão distribuídos por vários anfitriões.

![image sharding at expressen.se](https://raw.githubusercontent.com/bagder/http2-explained/master/images/expressen-sharding.jpg)
