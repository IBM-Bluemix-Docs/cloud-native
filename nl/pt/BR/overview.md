---

copyright:
  years: 2019
lastupdated: "2019-04-08"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}

# O que é a abordagem nativa de nuvem?
{: #overview}

Os ambientes de computação em nuvem são dinâmicos, com alocação sob demanda e liberação de recursos por meio de um conjunto virtualizado e compartilhado. Esses ambientes elásticos permitem opções de dimensionamento mais flexíveis quando comparadas à alocação de recurso up-front geralmente usada em data centers locais tradicionais.
{:shortdesc}

De acordo com a [Cloud Native Computing Foundation](https://cncf.io/about/charter){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo"), os sistemas nativos de nuvem têm os atributos a seguir:

- Os aplicativos ou processos são executados em contêineres de software como unidades isoladas.
- Os processos são gerenciados por processos de orquestração central para melhorar a utilização de recursos e reduzir os custos de manutenção.
- Os aplicativos ou serviços (microsserviços) são fracamente acoplados com dependências descritas explicitamente.

Esses atributos descrevem um sistema altamente dinâmico, composto de processos independentes que trabalham juntos para fornecer um valor de negócios: um sistema distribuído.

A computação distribuída é um conceito disseminado por décadas. As [falácias da computação distribuída](http://www.rgoarchitects.com/Files/fallacies.pdf){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") abordam as suposições a seguir, feitas por arquitetos e designers de sistemas distribuídos, que são comprovadamente erradas ao longo do tempo. 

* A rede é confiável.
* A rede é segura.
* A rede é homogênea.
* Não há latência.
* A largura da banda é infinita.
* A topologia não muda.
* Há um administrador.
* Não há custo de transporte.

Tecnologias de nuvem, como o Kubernetes e o Istio, visam abordar essas questões na própria infraestrutura.

## Doze fatores
{: #twelve-factors}

A metodologia de [aplicativo de doze fatores](http://12factor.net){: new_window} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") foi criada por desenvolvedores no Heroku. As características mencionadas nos doze fatores não são específicas de uma plataforma, uma linguagem ou um provedor em nuvem. Os fatores representam um conjunto de diretrizes ou melhores práticas para aplicativos móveis resilientes que se desenvolvem em ambientes de nuvem (especificamente aplicativos de Software como um Serviço). Os doze fatores são fornecidos na lista a seguir:

1. Há uma associação de um para um entre um código base com versão, por exemplo, um repositório git e um serviço implementado. O mesmo código base é usado para muitas implementações.
2. Os serviços declaram explicitamente todas as dependências e não contam com a presença de ferramentas ou bibliotecas de nível de sistema.
3. A configuração que varia entre ambientes de implementação é armazenada no ambiente, especificamente em variáveis de ambiente.
4. Todos os serviços auxiliares são tratados como recursos conectados, sendo gerenciados (conectados e desconectados) pelo ambiente de execução.
5. O delivery pipeline tem estágios estritamente separados: construção, liberação, execução.
6. Os aplicativos são implementados como um ou mais processos stateless. Os processos temporários, especificamente, são stateless e não fazem compartilhamentos. Os dados persistidos são armazenados em um serviço auxiliar apropriado.
7. Os serviços autocontidos tornam-se disponíveis para outros serviços, atendendo em uma porta especificada.
8. A simultaneidade é obtida dimensionando processos individuais (escala horizontal).
9. Os processos são descartáveis: os comportamentos de inicialização rápida e encerramento normal resultam em um sistema mais robusto e resiliente.
10. Todos os ambientes, desde o desenvolvimento local até a produção, são tão semelhantes quanto possível.
11. Os aplicativos produzem logs como fluxos de evento, por exemplo, gravando em `stdout` e `stderr`, e confiam no ambiente de execução para agregar fluxos.
12. Se tarefas únicas de administração forem necessárias, serão mantidas no controle de versão e empacotadas com o aplicativo para garantir que sejam executadas com o mesmo ambiente que ele.

Você não precisa seguir estritamente esses fatores para alcançar um ambiente de microsserviço de qualidade, no entanto, mantê-los em mente permite construir e manter aplicativos ou serviços móveis em ambientes de entrega contínua.

## Microsserviços
{: #microservices}

Um *microsserviço* é um conjunto de componentes arquiteturais pequenos e independentes, cada um com um único propósito, que se comunicam por meio de uma API leve e comum. Cada microsserviço no exemplo simples a seguir é um aplicativo de doze fatores que usa os serviços auxiliares substituíveis para armazenar dados e transmitir mensagens.

![Um aplicativo de microsserviços](images/microservice.png "Um aplicativo de microsserviços")

Os microsserviços são independentes. A agilidade é um dos benefícios das arquiteturas de microsserviço, mas existe somente quando os serviços são capazes de ser completamente reescritos sem perturbar outros. Isso provavelmente não acontecerá com frequência, mas explica o requisito. Limites de API claros fornecem à equipe trabalhando em um serviço a maior flexibilidade para desenvolver a implementação. Essa característica é o que permite a persistência e a programação poliglota.

Os microsserviços são resilientes. A estabilidade do aplicativo depende de que microsserviços individuais sejam robustos com relação às falhas. Essa é uma grande diferença de arquiteturas tradicionais, nas quais a infraestrutura auxiliar manipula falhas por você. Cada serviço precisa aplicar padrões de isolamento, como disjuntores e anteparas, para conter falhas e definir comportamentos de fallback apropriados para proteger os serviços de envio de dados.

Os microsserviços são processos temporários stateless. Isso não significa a mesma coisa que dizer que eles não podem ter estado. Significa que o estado deve ser armazenado em serviços de nuvem auxiliares externos, como o Redis, em vez de na memória. O comportamento de inicialização rápida e encerramento normal permite que os microsserviços funcionem corretamente em ambientes automatizados que criam e destroem instâncias em resposta ao carregamento ou para manter o funcionamento do sistema.

### O significado de "pequeno"
{: #small-microsvc}

O uso da palavra "pequeno", aplicada a um microsserviço, significa essencialmente que ele está focado em um propósito: ele deve fazer uma única coisa e fazê-la bem. Muitas descrições fazem paralelos entre as funções de microsserviços individuais e os comandos encadeados na linha de comandos do Unix:

```
ls | grep 'service' | sort -r
```
{:pre}

Esses comandos do Unix executam tarefas diferentes e é possível encadeá-las, independentemente da linguagem de programação ou da quantidade de código.

## Aplicativos poliglotas: escolhendo a ferramenta ideal para a tarefa
{: #polyglot-apps}

Ser poliglota é uma vantagem frequentemente citada das arquiteturas baseadas em microsserviços. Por um lado, a capacidade de escolher a linguagem ou o armazenamento de dados apropriado para a função fornecida por um serviço pode ser muito vantajosa e trazer muita eficiência. Por outro, o uso de tecnologias obscuras pode complicar a manutenção de longo prazo e dificultar a migração de desenvolvedores entre as equipes. 

Crie um equilíbrio entre os dois criando uma lista de tecnologias suportadas entre as quais escolher no início, com uma política definida para estender a lista com novas tecnologias ao longo do tempo. Certifique-se de que requisitos não funcionais ou regulamentares, como a sustentabilidade, a auditabilidade e a segurança de dados, possam ser atendidos, preservando a agilidade e suportando a inovação por meio da experimentação.

## REST e JSON
{: #rest-json}

Os aplicativos poliglotas somente são possíveis com protocolos abrangentes de linguagem. Os padrões de arquitetura REST definem diretrizes para criar interfaces uniformes que separam a representação de dados na rede da implementação do serviço.

O JSON emergiu em arquiteturas de microsserviços como o formato de ligação de escolha para dados baseados em texto, removendo o XML com sua simplicidade e concisão comparativas. Como uma comparação, o exemplo a seguir é um registro básico que contém dados sobre um funcionário em JSON:

  ```json
{
  "name": "Marley Cassin",
  "serial": 228264,
  "title": "Senior Software Engineer",
  "address": {
    "office": "501-B101",
    "street": "3858 Kuvalis Pass",
    "city": "East Craig",
    "state": "NC",
    "zip": "64519-8934"
  }
}
```
{: codeblock}

E o exemplo a seguir é o mesmo registro de funcionário em XML:

```xml
<person>
  <name>Marley Cassin</name>
  <serial>228264</serial>
  <title>Senior Software Engineer</title>
  <address>
    <office>501-B101</office>
    <street>3858 Kuvalis Pass</street>
    <city>East Craig</city>
    <state>NC</state>
    <zip>64519-8934</zip>
  </address>
</person>
```
{: codeblock}

O JSON usa os pares atributo/valor para representar objetos de dados em uma sintaxe concisa que preserva informações sobre alguns tipos básicos, como números, sequências, matrizes e objetos. O JSON e o XML representam claramente o objeto de endereço aninhado nos exemplos anteriores, mas o esquema XML associado é necessário para determinar o tipo do elemento `serial`. No JSON, a sintaxe esclarece que o valor de `serial` é um número e não uma sequência.