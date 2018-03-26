# 7. Extensões

O protocolo http2 obriga que o receptor leia e ignore todos os quadros desconhecidos (_unknown frame type_). Duas partes podem negociar o uso de novos tipos de quadros em uma forma _hop-by-hop_, mas esses quadros não podem mudar o estado e eles não terão controle de fluxo.

Houve uma ampla discussão durante a fase de desenvolvimento do protocolo http2 para decidir se ele deveria ou não permitir extensões, com opiniões a favor e contra. Depois do draft-12, o pêndulo oscilou pela última vez e as extensões foram finalmente autorizadas.

Extensões não fazem parte do atual protocolo, mas serão documentadas a parte do núcleo da especificação. Já existem dois tipos de quadro (_frame_) que estão em discussão para serem incluídos no protocolo e que, provavelmente, serão os primeiros quadros a serem enviados como extensões.

## 7.1. Serviços alternativos

Com a adoção do http2, existem razões para suspeitar que as conexões TCP serão mais demoradas e serão mantidas vivas por muito mais tempo do que nas conexões HTTP 1.x. Um cliente deve ser capaz de fazer o que quiser com uma única conexão para cada página/_host_, e essa conexão pode, potencialmente, ficar ativa por um longo tempo.

Isso afetará o trabalho de balanceamento de carga e podem surgir situações onde uma página queira sugerir ao cliente se conectar a outro servidor. Isto poderia acontecer por questões de desempenho (performance) ou se a página está em manutenção, etc.

O servidor enviará o cabeçalho [Alt-Svc: header](https://tools.ietf.org/html/draft-ietf-httpbis-alt-svc-07) (ou o quadro ALTSVC com http2) dizendo ao cliente sobre um serviço alternativo: outra rota para o mesmo conteúdo, utilizando outro serviço, servidor e porta.

Um cliente tentará se conectar ao serviço assincronamente e somente utilizar esta alternativa se a nova conexão for realizada com sucesso.

### 7.1.1. TLS oportunista

O cabeçalho Alt-Svc permite que o servidor que provê conteúdo por meio de http:// informar ao cliente que o mesmo conteúdo também está disponível através de uma conexão TLS.

Esta é uma característica um tanto discutível. Uma conexão deste tipo realizaria uma conexão TLS sem autenticar e não seria advertida como "segura" em nenhum lugar, não usaria nenhum cadeado na interface gráfica e, na verdade, não há como dizer ao usuário que não é o velho HTTP, mas é um TLS oportunista e algumas pessoas estão firmes contra este conceito.

## 7.2. Bloqueado

Um quadro (_frame_) deste tipo é utilizado uma única vez por um agente http2 quando ele tem dados para serem enviados, mas o controle de fluxo proíbe que seja enviada qualquer informação. A ideia é que se a implementação receber este quadro (_frame_), você sabe que algo está errado na sua implementação e/ou você está recebendo menos dados que a sua velocidade permite.

Uma citação do draft-12, antes deste quadro ser retirado para se tornar uma extensão:

> "O quadro BLOCKED está incluído nesta revisão para facilitar a experimentação. Se os resultados desta experiência não prover um resultado positivo, ele poderá ser removido"
