# Aprendendo API's Rest

Repositório voltado ao aprendizado e fixação do conceito de API's Rest.

#### Como principal fonte bibliográfica foi utilizado o livro:

* [REST Construa API's inteligentes de maneira simples](https://www.casadocodigo.com.br/pages/sumario-apis-rest)

## Conceitos

1. [O que é API?](#oqueeapi)
2. [O que é REST?](#oqueerest)
3. [O que é uma API REST?](#oqueeapirest)
4. [Por que razão utilizar REST?](#utilizarrest)
5. [Protocolo HTTP](#protocolohttp)
6. [Conceitos de REST](#conceitosrest)
7. [O que é servlet](#oqueeservlet)
8. [Implementação de serviços REST em Java utilizando servlets](#implementandocomjava)

<div id='oqueeapi'/>

## O que é API?

API é um termo inglês para Interface de Programação de Aplicação. No contexto de programação, e mais ainda no de API's, a palavra interface é se refere como um contrato de serviço entre duas aplicações, e a aplicação se refere a qualquer software que tenha uma função distinta. 

Além disso, esse contrato de serviço entre duas aplicações se comunica baseado em solicitações e respostas. Assim, para orientar como deve ser estruturada essas solicitações e respostas, é necessário que exista a documentação de suas respectivas API's orientando outros desenvolvedores. 

Como exemplo de uma documentação de API muito bem estruturada, recomendo a da corretora Binance. Para consultar ela, basta clicar [aqui](https://binance-docs.github.io/apidocs/spot/en/#change-log).

### Endpoints de API

Termo muito utilizado, endpoint de um web service é uma URI mapeada pela API onde é possível acessar a aplicação por um cliente. 

Todavia, é possível existir APIs sem endpoints, um exemplo de API assim seria algumas APIs do windows que trata de conteúdo local: exibir janelas; gerenciar teclado e mouse etc. Além disso, endpoints podem existir sem uma API, basta que a implementação seja simples, como retornar apenas a data e hora do servidor.

#### *Observação:*

>*Hoje em dia é comum se referir a uma coleção de endpoints pertencentes a um dado serviço como API, por proximidade e acoplamento; em muitos casos o serviço é desenhado e planejado tendo em mente a exposição via endpoints. [fonte: stackoverflow.](https://pt.stackoverflow.com/questions/86399/qual-a-diferen%C3%A7a-entre-endpoint-e-api#:~:text=Um%20endpoint%20de%20um%20web,e%20ferramentas%20para%20construir%20aplica%C3%A7%C3%B5es.)*

### Servidores de API vs Clientes de API

As APIs são hospedadas em servidores, sendo um ou mais, com foco em armazenar os dados e executar programas de software. O endpoint de uma API é quase sempre hospedado em um servidor.

Em contrapartida, do outro lado da conexão fica o cliente de API: uma entidade que visa enviar uma requisição a API. É comum chamar-mos o cliente de API de "usuário" da API, no entanto, atualmente a maior parte das chamadas de API é automatizada.

### Autenticação de clientes e endpoints

Toda API deve necessitar de autenticação para atender as chamadas de API, pois a ausência deixa o servidor de API propenso para receber dados maliciosos. Além disso, existem APIs que custam dinheiro, dessa forma, o servidor de API precisa verificar se as chamadas de API são originadas de um cliente pagante.

#### Como é realizada a autenticação?

Existem diversas formas de realizar o processo de verificar a identidade de um cliente, mas quatro formas se destacam:

* *Chave de API*: sequência de caracteres exclusive ao cliente da API que apenas ele e o serviço de API conhecem.
* *Nome de usuário e senha*: o cliente da API configura um nome de usuário e senha com serviço de API.
* *Token OAuth*: o servidor de API pode obter um token de autenticação junto a um servidor de autenticação confiável usando o protocolo OAuth.
* *TLS Mútuo*: o protocolo TLS cria uma conexão autenticada entre o cliente e o servidor. Este é o método de autenticação mais eficaz, pois ele autentica tanto o endpoint quanto o usuário, garantindo a legitimidade da fonte dos dados.


<div id='oqueerest'/>

## O que é REST?

REST é um termo inglês para Transferência de Estado Representativo. No contexto de programação, o termo se refere a uma arquitetura de desenvolvimento para web services com origem na tese de doutorado de Roy Fielding. Cabe ressaltar que o Roy é co-autor do protocolo HTTP, sendo compreensivel que o protocolo REST seja guiado pelas boas práticas de uso do HTTP.

Dessa forma, o REST segue os seguintes princípios:
* Uso adequado dos métodos HTTP ( principalmente os métodos: GET, POST, PUT e DELETE.);
* Utilização adequada de URL's;
* Utilização adequada dos cabeçalhos HTTP;
* Uso dos códigos de status padronizadas para para representar o sucesso e falha.

Além disso, teoricamente o REST não possui restrição com relação a protocolos utilizados, mas na prática, o único que se mostrou 100% compatível foi o HTTP. Cabe ressaltar que o HTTPS ainda é pertence ao protocolo HTTP, a diferença é que o mesmo possui uma camada a mais (camada de segurança).


<div id='oqueeapirest'/>

## O que é uma API RESTFul?

Visualizando os conceitos de API e REST, o nome já nos indica o que é uma API RESTful: uma API que é estruturada baseada no conceito de REST que respeita as restrições impostas pela arquitetura, e consequentemente, implementando o protocolo HTTP.

No entanto, podemos destacar como principal característica o fato da API RESTful possuir ausência de estado, que significa que os servidores não salvam os dados do cliente entre as solicitações. Cabe ressaltar que as solicitações podem se assemelhar as URL's e a resposta corresponde a dados simples.

Dessa forma, os princípios de uma API RESTful são:
* Servidor cliente: tal princípio se baseia de que o servidor e cliente devem ser isolados um do outro, além de ser possível seu desenvolvimento de forma independente. Consequentemente, ocorre a melhora da capacidade de gerenciamento e aumento da escalabilidade, pois as preocupações entre interface de usuário e armazenamento de dados são separados. 

* Não possuir estado: as APIs são sem estado, isso significa que as chamadas podem ser feitas sem depender de outras. Ou seja, toda solicitação enviada ao servidor pelo cliente deve incluir todas as informações necessárias.

* Armazenável em cache: como consequência da API não possuir estado pode ocorrer um aumento de sobrecarga de solicitações, para auxiliar nesse problema um design de API REST deve armazenar os dados em cache. Os dados em uma resposta devem ser indiretamente ou diretamente categorizados como armazenáveis ou não armazenáveis em cache. 
O cache tem direito de reciclar todos os dados de respostas armazenáveis para solicitações semelhantes.

* Interface Uniforme: é necessário ter uma interface unificada que permita o desenvolvimento do aplicativo sem acoplar seus serviços, modelos e ações à própria camada de API. Dessa forma, ocorre a otimização de toda a arquitetura do sistema, além de aumentar a visibilidade das comunicações. Portanto, para obter uma interface uniforme é necessário vários controles de arquitetura dentro da API REST. O princípio é definido por quatro controles de interface: identificação de recursos; gerenciamento de recursos por meio de representações; comunicações autodescritivas e hipermídia como mecanismo do estado do aplicativo (HATEOAS).

* Sistema em camadas: as interações podem ser mediadas por camadas adicionais, que podem ter como objetivo: balanceamento de carga; caches compartilhados ou segurança.


## Por que razão utilizar REST?

Os principais benefícios obtidos ao se adotar a arquitetura REST, são:

* *Integração*: permite a integração de novas aplicações com sistemas de software já existentes. Consequentemente, aumenta a velocidade de desenvolvimento pois cada funcionalidade não necessita ser escrita ado zero. Além disso, separa o front-end do back-end. 
* *Inovação*: dado que muitos setores mudam com a chegada de inovações, a arquitetura permite a resposta rápida da empresa à implantação de serviços inovadores, pois ela pode fazer isso fazendo alterações no nível da API sem necessitar reescrever o código inteiro.
* *Expansão*: permite o atendimento às necessidades de seus clientes em plataformas distintas.
* *Manutenção*: a API atua como um gateway entre os dois sistemas. Assim, qualquer alteração futura do código por uma parte não afeta a outra parte, pois cada sistema é obrigado a fazer alterações internas para que a API não seja afetada.

<div id='protocolohttp'/>

## Protocolo HTTP



<div id='conceitosrest'/>

## Conceitos de REST


<div id='tiposdedados'/>

## Tipos de dados

<div id='oqueeservlet'/>

## O que é servlet

