<h1> Olá você!! </h1>

<h2>Introdução</h2>

<p>Este é um um teste de desenvolvimento front-end Vue.js</p>

Trata-se de um micro serviço CRUD, que contempla as seguintes funções:

- Cadastro de Cliente.
- Cadastro de Items.
- Edição de itens e clientes.
- Vinculo de itens aos clientes.

Tecnologias usadas:

- Javascript
- HTML
- CSS
- SASS pre processor
- Axios
- Vue.js
- Nuxt.js
- Firebase
- Gerenciamento de pacotes Yarn

A aplicação foi desenvolvida em inglês para prática da língua. 

Extras:

- Componentização
- Requisições usando a biblioteca Axios
- Nuxt 3 
- Arquitetura BEM CSS 1001
- Responsividade por mobilefirst
- Componentes feitos manualmente, sem o uso de bootstrap ou tailwind.

=============================================================================

<h2>Rodar o sistema</h2>

Tutorial para rodar a aplicação localmente:

1 - Abra um terminal em sua máquina, e certifique-se de ter instalado o 
programa de versionamento de código Git.

2 - Navegue para a pasta que deseja baixar estes repositório, utilizando:

cd + nome da pasta - Para avançar o diretório.
cd .. - Para voltar um diretório
ls - Para visualizar as pastar do diretório atual
mkdir + "nome" - Para criar uma nova pasta com nome informado

3 - Rode abaixo, para baixar o projeto:

    git clone https://github.com/caiomontenegro/customer.products.git

4 - Após baixar o projeto, rode o abaixo para instalar todos os pacotes
e dependencias necessários para rodar o projeto:

    yarn 

5 - Em seguida, rode o comando abaixo, para buildar o projeto em localhost.:

    yarn dev

Obs: A porta na qual o sistema irá a aplicação pode variar. Verique no terminal
em portal localhost o projeto será aberto. Por padrão a rota será:

http://localhost:3000/


==============================================================================

<h2>Uso da Aplicação</h2>

Apesar do conceito da aplicação ser extremamente forte e usado no mercado de 
sistemas WEB, a aplicação é bem simples. 

O objetivo do desenvolvedor, foi criar algo que usuário possa acessar e interagir
sem a necessidade de um manual ou tutorial de uso. 

Sendo assim, fique a vontade para "esmiuçar". Caso não entenda algo, ou fique
com dúvidas em determinado ponto, significa que o teste foi falho neste ponto.
Porém não hesite em entrar em contato comigo para expor o erro. Meus contatos
estão no rodapé deste documento!


=============================================================================

<h2>Melhorias</h2>

Observações:

Os dados estão mockados em um servidor de teste Firebase, que pode 
demorar um pouco para responder as requisições.

Possíveis melhorias:

- API - User um diretório nomeado de API, para alocar todas as requições HTTP
diminuindo o número de chamadas. (Desempenho)
- VUEX - Usar o gerenciamento de estados para armezenar informações, sem a necessidade
de requisições HTTP (Desempenho)
- SASS - A aplicação foi estilizada por SCSS, que oferece inúmeras funcionalidades nas quais
não foram implementadas, por se tratar de projeto pequeno. Por exemplo:
Fazer uso das propriedades SASS.  (Padronização e desempenho)
- Cypress - Escrever testes e2e, de todas as funcionadalides, ancoras, botões, intereções
e também um mock local para consultas, em caso de API FIrebase fora do ar.
A principio, os testes seriam feitos, porém devido a falta de tempo para o desenvolvimento. O
mesmo será feito posteriormente.
- Componenttização - Componentização do formulário, usando slots e javascripts para 
criar os inputs e labels nas páginas.


Contato:
email: caio.dev17@gmail.com </br>
linkedin: https://www.linkedin.com/in/caio-montenegro/
