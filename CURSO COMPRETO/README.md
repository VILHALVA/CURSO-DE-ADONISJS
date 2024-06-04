# INSTRUÇÕES (GENERICAS)
## 01) O QUE É REST API COM ADONIS?
Uma REST API (Representational State Transfer Application Programming Interface) é um conjunto de padrões e convenções para a criação de serviços web que permitem a comunicação entre clientes e servidores via HTTP. Em AdonisJS, criar uma REST API envolve definir rotas, controladores e modelos para manipular dados e responder às requisições HTTP.

### Conceitos Básicos de REST API
- **Recursos**: Entidades que a API expõe. Cada recurso é acessível por meio de um endpoint específico.
- **Verbos HTTP**: Métodos utilizados para realizar operações sobre os recursos.
  - `GET`: Recuperar informações.
  - `POST`: Criar um novo recurso.
  - `PUT`: Atualizar um recurso existente.
  - `DELETE`: Remover um recurso.
- **Endpoints**: URLs onde os recursos estão disponíveis.
- **JSON**: Formato comum para troca de dados em APIs REST.

### Criando uma REST API com AdonisJS
Vamos criar um exemplo básico de uma REST API para gerenciar usuários.

#### 1. Configurar o Ambiente
Se você ainda não configurou seu ambiente, siga os passos de instalação e configuração descritos anteriormente.

#### 2. Definir o Modelo
Primeiro, vamos criar um modelo para representar os usuários.

1. **Criar o modelo User**:
   ```sh
   adonis make:model User -m
   ```

   Isso criará um modelo e uma migração.

2. **Editar a migração**:
   ```ts
   // database/migrations/xxxx_users.ts
   import BaseSchema from '@ioc:Adonis/Lucid/Schema'

   export default class Users extends BaseSchema {
     protected tableName = 'users'

     public async up () {
       this.schema.createTable(this.tableName, (table) => {
         table.increments('id')
         table.string('name', 255).notNullable()
         table.string('email', 255).notNullable().unique()
         table.timestamps(true)
       })
     }

     public async down () {
       this.schema.dropTable(this.tableName)
     }
   }
   ```

3. **Rodar a migração**:
   ```sh
   adonis migration:run
   ```

#### 3. Criar o Controlador
1. **Criar um controlador para os usuários**:
   ```sh
   adonis make:controller UsersController --apiOnly
   ```

   O `--apiOnly` cria um controlador sem métodos relacionados a views.

2. **Editar o controlador**:
   ```ts
   // app/Controllers/Http/UsersController.ts
   import { HttpContextContract } from '@ioc:Adonis/Core/HttpContext'
   import User from 'App/Models/User'

   export default class UsersController {
     public async index({ response }: HttpContextContract) {
       const users = await User.all()
       return response.json(users)
     }

     public async store({ request, response }: HttpContextContract) {
       const data = request.only(['name', 'email'])
       const user = await User.create(data)
       return response.json(user)
     }

     public async show({ params, response }: HttpContextContract) {
       const user = await User.findOrFail(params.id)
       return response.json(user)
     }

     public async update({ params, request, response }: HttpContextContract) {
       const user = await User.findOrFail(params.id)
       const data = request.only(['name', 'email'])
       user.merge(data)
       await user.save()
       return response.json(user)
     }

     public async destroy({ params, response }: HttpContextContract) {
       const user = await User.findOrFail(params.id)
       await user.delete()
       return response.json({ message: 'User deleted successfully' })
     }
   }
   ```

#### 4. Definir as Rotas
1. **Editar o arquivo de rotas**:
   ```ts
   // start/routes.ts
   import Route from '@ioc:Adonis/Core/Route'

   Route.get('/users', 'UsersController.index')
   Route.post('/users', 'UsersController.store')
   Route.get('/users/:id', 'UsersController.show')
   Route.put('/users/:id', 'UsersController.update')
   Route.delete('/users/:id', 'UsersController.destroy')
   ```

### Testar a API
Com o servidor de desenvolvimento em execução (`adonis serve --dev`), você pode testar os endpoints da API usando uma ferramenta como Postman ou cURL.

1. **Listar usuários**:
   ```sh
   GET http://localhost:3333/users
   ```

2. **Criar um novo usuário**:
   ```sh
   POST http://localhost:3333/users
   Body (JSON):
   {
     "name": "John Doe",
     "email": "john@example.com"
   }
   ```

3. **Obter detalhes de um usuário**:
   ```sh
   GET http://localhost:3333/users/1
   ```

4. **Atualizar um usuário**:
   ```sh
   PUT http://localhost:3333/users/1
   Body (JSON):
   {
     "name": "John Smith",
     "email": "johnsmith@example.com"
   }
   ```

5. **Deletar um usuário**:
   ```sh
   DELETE http://localhost:3333/users/1
   ```

### Conclusão
Você criou uma REST API básica usando AdonisJS. Este exemplo cobre as operações CRUD (Create, Read, Update, Delete) para um recurso de usuários. AdonisJS facilita a criação de APIs robustas e escaláveis com sua estrutura bem organizada e ferramentas integradas. 

## 02) FERRAMENTAS PARA O ADONISJS
Para desenvolver com AdonisJS, é útil ter um conjunto de ferramentas que facilite o desenvolvimento, teste e depuração de suas aplicações. Abaixo estão algumas ferramentas recomendadas:

### Ferramentas Essenciais
1. **Node.js e npm (ou yarn)**:
   - AdonisJS é construído sobre Node.js, então ter a versão mais recente instalada é crucial.
   - [Node.js](https://nodejs.org/)
   - [npm](https://www.npmjs.com/)
   - [Yarn](https://yarnpkg.com/) (alternativa ao npm)

2. **AdonisJS CLI**:
   - A ferramenta de linha de comando oficial para inicializar e gerenciar projetos AdonisJS.
   - Instalação: `npm install -g @adonisjs/cli`

3. **Editor de Texto / IDE**:
   - Um bom editor de texto ou IDE é essencial para a produtividade.
   - [Visual Studio Code](https://code.visualstudio.com/) (VS Code) é altamente recomendado e possui várias extensões úteis para JavaScript/TypeScript e AdonisJS.

### Extensões e Plugins
1. **Extensões para VS Code**:
   - **AdonisJS Snippets**: Trechos de código que ajudam na escrita rápida e eficiente de código AdonisJS.
   - **ESLint**: Linter para JavaScript que ajuda a manter a consistência do código.
   - **Prettier**: Formatador de código que ajuda a manter um estilo de código consistente.
   - **REST Client**: Testa APIs diretamente do VS Code.

### Ferramentas de Banco de Dados
1. **MySQL/MariaDB** ou **PostgreSQL**:
   - AdonisJS suporta vários bancos de dados, mas esses dois são amplamente usados.
   - [MySQL](https://www.mysql.com/)
   - [PostgreSQL](https://www.postgresql.org/)

2. **Ferramentas de Administração de Banco de Dados**:
   - **TablePlus**: Cliente de banco de dados GUI que suporta MySQL, PostgreSQL e outros.
   - **DBeaver**: Ferramenta de banco de dados universal que suporta uma ampla gama de bancos de dados.

### Ferramentas de Teste
1. **Jest**:
   - Framework de teste JavaScript recomendado para AdonisJS.
   - [Jest](https://jestjs.io/)

2. **Supertest**:
   - Biblioteca para testar endpoints HTTP.
   - [Supertest](https://github.com/visionmedia/supertest)

### Ferramentas de Debugging
1. **Debugger do VS Code**:
   - VS Code possui um depurador embutido que pode ser configurado para Node.js.

### Gerenciamento de Dependências
1. **nvm (Node Version Manager)**:
   - Ferramenta para gerenciar várias versões do Node.js em um único sistema.
   - [nvm](https://github.com/nvm-sh/nvm)

### Containerização e DevOps
1. **Docker**:
   - Utilizado para criar ambientes de desenvolvimento consistentes e para a implantação de aplicativos.
   - [Docker](https://www.docker.com/)

### Gerenciamento de Projetos e Versionamento
1. **Git**:
   - Controle de versão para o gerenciamento de código fonte.
   - [Git](https://git-scm.com/)

2. **GitHub / GitLab / Bitbucket**:
   - Plataformas para hospedagem de repositórios Git.

### Complementos Úteis
1. **Insomnia ou Postman**:
   - Ferramentas para testar APIs.
   - [Insomnia](https://insomnia.rest/)
   - [Postman](https://www.postman.com/)

2. **Heroku ou Vercel**:
   - Plataformas para a implantação de aplicativos Node.js.
   - [Heroku](https://www.heroku.com/)
   - [Vercel](https://vercel.com/)

## 03) INSTALANDO ADONISJS
Para instalar o AdonisJS, você precisará ter o Node.js e o npm (ou Yarn) instalados em seu sistema. Aqui estão os passos básicos para instalar o AdonisJS:

### 1. Instalação do Node.js e npm
Se você ainda não tem o Node.js instalado, você pode baixá-lo e instalá-lo a partir do site oficial: [Node.js](https://nodejs.org/).

Após a instalação, o npm será instalado automaticamente junto com o Node.js.

### 2. Instalação do Adonis CLI
O Adonis CLI é uma ferramenta de linha de comando que facilita a criação e a gestão de projetos AdonisJS.

Para instalar o Adonis CLI globalmente em seu sistema, execute o seguinte comando no terminal:

```bash
npm install -g @adonisjs/cli
```

### 3. Criar um Novo Projeto AdonisJS
Depois de instalar o Adonis CLI, você pode criar um novo projeto AdonisJS usando o comando `new`:

```bash
adonis new nome_do_projeto
```

Substitua `nome_do_projeto` pelo nome desejado para o seu projeto. Isso criará uma nova pasta com o nome do projeto e instalará todas as dependências necessárias.

### 4. Iniciar o Servidor
Após criar o projeto, navegue até o diretório do projeto e inicie o servidor AdonisJS:

```bash
cd nome_do_projeto
adonis serve --dev
```

Isso iniciará o servidor em modo de desenvolvimento. Você pode acessar seu aplicativo em `http://localhost:3333`.

### 5. Opção de Banco de Dados (Opcional)
Se você planeja usar um banco de dados em seu projeto, o AdonisJS suporta uma variedade de opções de banco de dados, incluindo SQLite, MySQL, PostgreSQL, e outros. Você pode configurar as credenciais do banco de dados no arquivo `.env` gerado pelo Adonis CLI.

### 6. Instalação de Dependências
Por fim, se você deseja adicionar pacotes de terceiros ao seu projeto, você pode instalá-los via npm ou Yarn:

```bash
npm install nome_do_pacote
# ou
yarn add nome_do_pacote
```

Isso instalará o pacote e adicionará as dependências ao seu arquivo `package.json`.

Com esses passos, você terá o AdonisJS instalado e pronto para ser usado em seu projeto.

## 03.01) A DIFERENÇA ENTRE CRIAR UM PROJETO DE INSTERFACE E UMA API REST
Vamos explorar a diferença na prática usando comandos para criar um projeto de interface e uma API REST com AdonisJS.

1. **Criar um Projeto de Interface:**

Para criar um projeto de interface com AdonisJS, você pode usar o seguinte comando:

```bash
adonis new interface-project
```

Isso criará um novo projeto AdonisJS com suporte completo para criar páginas da web renderizadas pelo servidor, além de fornecer funcionalidades como roteamento, controladores e visualizações para criar uma interface de usuário interativa.

2. **Criar uma API REST:**

Para criar uma API REST com AdonisJS, você pode usar o seguinte comando:

```bash
adonis new api-rest-project --api-only
```

Este comando criará um novo projeto AdonisJS configurado exclusivamente para criar uma API REST. Ele não incluirá recursos de interface de usuário, como visualizações e roteamento de página, mas fornecerá suporte completo para a criação de endpoints de API para manipular e fornecer dados de forma programática.

Esses comandos demonstram a diferença na criação de um projeto de interface tradicional e uma API REST usando AdonisJS. O primeiro é voltado para criar uma experiência de usuário interativa, enquanto o segundo é focado em fornecer acesso aos dados de forma programática para outras aplicações.

* AVISO: Nesse curso iremos nos focar apenas na criação de `API REST` com ADONISJS.

## 04) ESTRUTURA BASICA
A estrutura básica de um projeto AdonisJS consiste em vários diretórios e arquivos que organizam o código-fonte da aplicação. Aqui está uma visão geral da estrutura básica de diretórios:

```
projeto-adonis/
├── app/
│   ├── Controllers/
│   ├── Exceptions/
│   ├── Middleware/
│   ├── Models/
│   ├── Policies/
│   ├── Providers/
│   └── ...
├── config/
│   ├── app.js
│   ├── auth.js
│   ├── database.js
│   └── ...
├── database/
│   ├── migrations/
│   └── seeds/
├── public/
├── resources/
│   ├── views/
│   └── ...
├── start/
│   ├── app.js
│   ├── kernel.js
│   └── routes.js
├── .env
├── ace
├── package.json
└── ...
```

Aqui está uma breve explicação de cada diretório e seus propósitos:

- **app/**: Este diretório contém a maior parte do código-fonte da sua aplicação. Ele é subdividido em subdiretórios para organizar diferentes tipos de classes e lógica de negócios.
   - **Controllers/**: Controladores para manipular solicitações HTTP e interagir com os modelos e serviços.
   - **Exceptions/**: Classes para lidar com exceções personalizadas.
   - **Middleware/**: Middleware para processar solicitações HTTP antes ou depois que elas atingem os controladores.
   - **Models/**: Modelos que representam as entidades do seu banco de dados.
   - **Policies/**: Políticas de autorização para definir permissões de acesso aos recursos.
   - **Providers/**: Provedores para configurar e vincular serviços à aplicação.

- **config/**: Este diretório contém arquivos de configuração da aplicação, como configurações de banco de dados, autenticação, middleware, etc.

- **database/**: Este diretório contém arquivos relacionados ao banco de dados.
   - **migrations/**: Arquivos para definir e executar migrações de banco de dados.
   - **seeds/**: Arquivos para definir e executar seeds de banco de dados.

- **public/**: Este diretório é usado para armazenar arquivos estáticos, como imagens, CSS e JavaScript, acessíveis publicamente.

- **resources/**: Este diretório é usado para armazenar arquivos de visualização, como modelos HTML, que serão renderizados pelo servidor.

- **start/**: Este diretório contém arquivos de inicialização da aplicação.
   - **app.js**: Arquivo de inicialização principal da aplicação.
   - **kernel.js**: Arquivo de configuração do middleware da aplicação.
   - **routes.js**: Arquivo de definição de rotas da aplicação.

- **.env**: Arquivo de ambiente que armazena variáveis de ambiente da aplicação.

- **ace**: CLI (Command-Line Interface) para executar tarefas AdonisJS.

- **package.json**: Arquivo de manifesto do projeto Node.js que lista as dependências do projeto e outros metadados.

## 05) CRIANDO ROTAS
Para criar rotas em um projeto AdonisJS, você pode usar o arquivo `start/routes.js`. Este arquivo contém a definição das rotas da sua aplicação. Aqui está um exemplo simples de como criar algumas rotas básicas:

```javascript
'use strict'

const Route = use('Route')

Route.get('/', () => {
  return { greeting: 'Welcome to my AdonisJS API!' }
})

Route.get('/users', 'UserController.index')
Route.get('/users/:id', 'UserController.show')
Route.post('/users', 'UserController.store')
Route.put('/users/:id', 'UserController.update')
Route.delete('/users/:id', 'UserController.destroy')
```

Neste exemplo:

- O método `Route.get()` define uma rota do tipo GET.
- O primeiro argumento é o caminho da rota.
- O segundo argumento pode ser um callback ou uma string no formato `'ControllerName.method'`, que aponta para o controlador e o método a serem executados quando a rota for acessada.
- Os métodos `post()`, `put()` e `delete()` funcionam de forma semelhante para criar rotas POST, PUT e DELETE, respectivamente.

Certifique-se de substituir `'UserController'` pelo nome do seu controlador e `'index'`, `'show'`, `'store'`, `'update'` e `'destroy'` pelos métodos do controlador que você deseja chamar.

Além disso, você pode definir rotas agrupadas, prefixadas, middleware e muito mais usando a API de roteamento do AdonisJS. Aqui está um exemplo de rota agrupada e prefixada:

```javascript
'use strict'

const Route = use('Route')

Route.group(() => {
  Route.get('/', 'UserController.index')
  Route.get('/:id', 'UserController.show')
}).prefix('api/v1/users')
```

Neste exemplo, todas as rotas dentro do grupo têm o prefixo `'api/v1/users'`.

Depois de definir suas rotas, não se esqueça de iniciar o servidor AdonisJS usando o comando `adonis serve` para que as rotas sejam acessíveis.

## 06) REGISTRO DE USUARIOS
Para implementar o registro de usuários em um projeto AdonisJS, você pode seguir estes passos:

1. **Crie um Controlador de Usuário**: Primeiro, você precisa criar um controlador para lidar com as operações relacionadas aos usuários. Você pode fazer isso executando o seguinte comando no terminal:

```bash
adonis make:controller UserController
```

Isso criará um arquivo chamado `UserController.js` dentro do diretório `app/Controllers/Http`.

2. **Defina as Funções de Registro no Controlador**: No arquivo `UserController.js`, defina as funções para criar e armazenar um novo usuário no banco de dados. Aqui está um exemplo básico:

```javascript
'use strict'

const User = use('App/Models/User')

class UserController {
  async store({ request, response }) {
    const userData = request.only(['username', 'email', 'password'])
    const user = await User.create(userData)
    return response.status(201).json(user)
  }
}

module.exports = UserController
```

3. **Defina a Rota para o Registro de Usuários**: Em `start/routes.js`, defina a rota para o registro de usuários e vincule-a à função `store` do controlador. Por exemplo:

```javascript
'use strict'

const Route = use('Route')

Route.post('/register', 'UserController.store')
```

4. **Envie Requisições de Registro**: Agora, você pode enviar uma solicitação POST para `/register` com os dados do usuário (username, email, password) para registrar um novo usuário.

Certifique-se de que sua aplicação esteja configurada corretamente para lidar com a criptografia de senha, geralmente usando um hash como o Bcrypt.

Após implementar esses passos, seu aplicativo AdonisJS deverá ser capaz de registrar novos usuários com sucesso.

## 07) LOGIN DE USUARIOS E JWT
Para implementar o login de usuários com JWT (JSON Web Tokens) em um projeto AdonisJS, você pode seguir os seguintes passos:

1. **Instalar Pacotes Necessários**: Certifique-se de que você tenha instalado os pacotes necessários para autenticação JWT. Você pode instalar o pacote `@adonisjs/auth` e `@adonisjs/jwt` com o seguinte comando:

```bash
npm install @adonisjs/auth @adonisjs/jwt
```

2. **Configurar Autenticação**: Configure o módulo de autenticação em seu projeto. Isso geralmente envolve configurar os provedores de autenticação e JWT. Execute o seguinte comando para gerar o arquivo de configuração:

```bash
adonis auth:setup
```

3. **Criar Middleware de Autenticação**: Crie um middleware para autenticar as rotas que exigem autenticação. Você pode fazer isso executando o seguinte comando:

```bash
adonis make:middleware Auth
```

Isso criará um arquivo chamado `Auth.js` dentro do diretório `app/Middleware`.

4. **Implementar Middleware de Autenticação**: No arquivo `Auth.js`, implemente a lógica para autenticar as solicitações usando JWT. Aqui está um exemplo básico:

```javascript
'use strict'

const jwt = require('jsonwebtoken')
const Env = use('Env')

class Auth {
  async handle ({ request, response, auth }, next) {
    const token = request.header('Authorization')

    if (!token) {
      return response.status(401).send({ error: 'Token não fornecido' })
    }

    try {
      const decoded = jwt.verify(token, Env.get('JWT_SECRET'))
      auth.user = decoded
    } catch (error) {
      return response.status(401).send({ error: 'Token inválido' })
    }

    await next()
  }
}

module.exports = Auth
```

5. **Registrar Middleware**: Registre o middleware de autenticação no arquivo `start/kernel.js` para que ele possa ser usado em suas rotas protegidas.

```javascript
const globalMiddleware = [
  // other global middleware
  'App/Middleware/Auth'
]
```

6. **Criar Rota de Login**: Defina uma rota para o endpoint de login em `start/routes.js` e vincule-a à função de login em seu controlador de usuário.

7. **Emitir Tokens JWT**: No controlador de autenticação, após verificar as credenciais do usuário, emita um token JWT válido. Aqui está um exemplo básico:

```javascript
const token = jwt.sign({ email: user.email, id: user.id }, Env.get('JWT_SECRET'))
```

8. **Usar Tokens JWT**: Depois que o usuário estiver autenticado com sucesso, você pode enviar o token JWT para o cliente. O cliente deve incluir esse token em cada solicitação subsequente em um cabeçalho de autorização.

Após seguir esses passos, seu aplicativo AdonisJS deve ser capaz de autenticar usuários usando JWT com sucesso. Certifique-se de revisar e adaptar o código conforme necessário para atender aos requisitos específicos do seu aplicativo.

## 08) CRIANDO MODELOS E RELAÇÕES
Para criar modelos e definir relações entre eles no AdonisJS, você pode seguir os passos abaixo:

1. **Criar modelos**:
   Você pode usar o comando `adonis make:model` para criar modelos. Por exemplo, se você quiser criar um modelo de usuário, execute o seguinte comando no terminal:

   ```
   adonis make:model User
   ```

   Isso criará um arquivo `User.js` dentro do diretório `app/Models` com um esqueleto básico para o modelo de usuário.

2. **Definir campos do modelo**:
   Abra o arquivo do modelo recém-criado (`User.js`, por exemplo) e defina os campos do modelo usando o método `static get table()` e o método `static get primaryKey()` conforme necessário.

3. **Definir relações**:
   Se o seu modelo tiver relacionamentos com outros modelos, você pode defini-los no método `static get relations()` do seu modelo. Por exemplo, se um usuário tiver muitos posts, você pode definir a relação assim:

   ```javascript
   static get relations() {
     return [
       { posts: this.hasMany('App/Models/Post') }
     ]
   }
   ```

4. **Migrar o banco de dados**:
   Após definir seus modelos, você precisará migrar o banco de dados para criar as tabelas correspondentes. Execute o seguinte comando no terminal:

   ```
   adonis migration:run
   ```

   Isso executará todas as migrações pendentes no seu projeto AdonisJS, incluindo aquelas relacionadas aos modelos que você criou.

Depois de seguir esses passos, seus modelos estarão prontos e você poderá começar a usá-los em seu aplicativo AdonisJS. Certifique-se de consultar a documentação oficial do AdonisJS para obter mais detalhes sobre a criação de modelos e definição de relacionamentos.

## 09) CRIANDO O METODO INDEX
Para criar o método `index` em um controller no AdonisJS, siga estas etapas:

1. **Criar um Controller**:
   Se você ainda não tiver um controller para a entidade que deseja listar, crie um usando o comando `adonis make:controller`. Por exemplo, se você deseja criar um controller para usuários, execute o seguinte comando no terminal:

   ```
   adonis make:controller UserController
   ```

   Isso criará um arquivo `UserController.js` dentro do diretório `app/Controllers/Http` com um esqueleto básico do controller.

2. **Implementar o Método Index**:
   Abra o arquivo `UserController.js` recém-criado e adicione o método `index`. Este método será responsável por listar todos os usuários. Um exemplo de implementação do método `index` pode ser o seguinte:

   ```javascript
   async index({ response }) {
     try {
       const users = await User.all(); // Supondo que User seja o modelo para usuários
       return response.status(200).json(users);
     } catch (error) {
       return response.status(500).json({ error: 'Internal Server Error' });
     }
   }
   ```

   Certifique-se de substituir `User` pelo nome do modelo correto se for diferente.

3. **Configurar a Rota**:
   Após criar o método `index`, você precisa configurar uma rota para acessá-lo. Abra o arquivo `start/routes.js` e adicione uma rota para o método `index` do controller `UserController`. Por exemplo:

   ```javascript
   Route.get('/users', 'UserController.index');
   ```

   Isso configurará uma rota GET para `/users`, que irá chamar o método `index` do `UserController` quando acessada.

Agora você tem um método `index` em seu controller que retorna todos os usuários quando chamado através da rota configurada. Certifique-se de ajustar os detalhes de acordo com a estrutura e os requisitos específicos do seu aplicativo.

## 10) CRIANDO O METODO CREAT
Para criar um método `create` em um controlador no AdonisJS, você precisa seguir alguns passos. Vou guiá-lo através do processo:

1. **Criando o Método `create` no Controlador**: Primeiro, você precisa adicionar o método `create` ao seu controlador. Isso geralmente é feito no controlador correspondente ao modelo que você deseja criar. Aqui está um exemplo de como seria um método `create` básico em um controlador de usuários:

```javascript
// app/Controllers/Http/UserController.js

'use strict'

const User = use('App/Models/User')

class UserController {
  async create({ request, response }) {
    const userData = request.only(['username', 'email', 'password'])
    const user = await User.create(userData)
    return response.status(201).json(user)
  }
}

module.exports = UserController
```

Neste exemplo, estamos recebendo os dados do usuário do corpo da solicitação, usando o método `request.only()` para garantir que apenas os campos desejados sejam usados, e então criamos um novo usuário com esses dados usando o método `User.create()`.

2. **Definindo a Rota para o Método `create`**: Em seguida, você precisa definir uma rota para acessar o método `create` do seu controlador. Aqui está um exemplo de como você pode fazer isso no arquivo `start/routes.js`:

```javascript
// start/routes.js

'use strict'

const Route = use('Route')

Route.post('/users', 'UserController.create')
```

Neste exemplo, estamos definindo uma rota POST para `/users` que chama o método `create` do controlador `UserController`.

3. **Testando o Método `create`**: Agora você pode testar o método `create` usando uma ferramenta como o Postman. Envie uma solicitação POST para a rota que você definiu, passando os dados do usuário no corpo da solicitação. Se tudo estiver configurado corretamente, você deve receber uma resposta com os detalhes do usuário recém-criado.

Certifique-se de que todos os nomes de arquivos e diretórios correspondam aos seus em seu projeto. Com esses passos, você deve ser capaz de criar com sucesso um método `create` para adicionar novos registros ao seu banco de dados no AdonisJS.

## 11) CRIANDO O METODO DESTROY
Para criar um método `destroy` em um controlador no AdonisJS para excluir recursos, você pode seguir este guia passo a passo:

1. **Adicionando o Método `destroy` no Controlador**: Primeiro, adicione o método `destroy` ao seu controlador correspondente ao modelo que você deseja excluir. Aqui está um exemplo de como seria um método `destroy` em um controlador de usuários:

```javascript
// app/Controllers/Http/UserController.js

'use strict'

const User = use('App/Models/User')

class UserController {
  async destroy({ params, response }) {
    const user = await User.find(params.id)

    if (!user) {
      return response.status(404).json({ message: 'User not found' })
    }

    await user.delete()

    return response.status(204).send()
  }
}

module.exports = UserController
```

Neste exemplo, estamos recebendo o ID do usuário a ser excluído dos parâmetros da solicitação e, em seguida, buscando o usuário correspondente no banco de dados. Se o usuário não for encontrado, retornamos um status 404. Caso contrário, excluímos o usuário usando o método `delete()` e retornamos um status 204 para indicar que a operação foi bem-sucedida.

2. **Definindo a Rota para o Método `destroy`**: Em seguida, defina uma rota para acessar o método `destroy` do seu controlador. Aqui está um exemplo de como você pode fazer isso no arquivo `start/routes.js`:

```javascript
// start/routes.js

'use strict'

const Route = use('Route')

Route.delete('/users/:id', 'UserController.destroy')
```

Neste exemplo, estamos definindo uma rota DELETE para `/users/:id`, onde `:id` é o ID do usuário que será excluído. Essa rota chama o método `destroy` do controlador `UserController`.

3. **Testando o Método `destroy`**: Agora você pode testar o método `destroy` usando uma ferramenta como o Postman. Envie uma solicitação DELETE para a rota que você definiu, passando o ID do usuário a ser excluído como parte da URL. Se tudo estiver configurado corretamente, o usuário deve ser excluído do banco de dados e você deve receber uma resposta com status 204.

Certifique-se de que todos os nomes de arquivos e diretórios correspondam aos seus em seu projeto. Com esses passos, você deve ser capaz de criar com sucesso um método `destroy` para excluir recursos no AdonisJS.

## 12) CRIANDO SERVIÇOS E EXCEÇÕES
Para criar serviços e exceções no AdonisJS, você pode seguir os seguintes passos:

### Criando um Serviço
1. **Crie o Serviço**: Primeiro, crie um novo arquivo para o seu serviço na pasta `app/Services`. Por exemplo, vamos criar um serviço de envio de e-mails.

   ```bash
   adonis make:service Email
   ```

   Isso criará um arquivo `EmailService.js` na pasta `app/Services`.

2. **Implemente a Lógica do Serviço**: No arquivo do serviço criado, implemente a lógica do serviço desejado. Por exemplo, se for um serviço de envio de e-mails, você pode usar uma biblioteca como o Nodemailer para enviar e-mails.

   ```javascript
   // app/Services/EmailService.js

   'use strict'

   const nodemailer = require('nodemailer')

   class EmailService {
     async sendEmail(to, subject, message) {
       // Lógica para enviar e-mail usando Nodemailer
     }
   }

   module.exports = EmailService
   ```

### Criando uma Exceção Personalizada
1. **Crie a Exceção**: O AdonisJS facilita a criação de exceções personalizadas. Você pode criar uma nova exceção usando o comando `adonis make:exception`.

   ```bash
   adonis make:exception CustomException
   ```

   Isso criará um arquivo `CustomException.js` na pasta `app/Exceptions`.

2. **Personalize a Exceção**: No arquivo da exceção criada, personalize a mensagem de erro e o status HTTP conforme necessário.

   ```javascript
   // app/Exceptions/CustomException.js

   'use strict'

   const { LogicalException } = require('@adonisjs/generic-exceptions')

   class CustomException extends LogicalException {
     constructor(message = 'Custom error message', status = 400) {
       super(message, status)
     }

     handle(error, { response }) {
       response.status(error.status).send({ error: error.message })
     }
   }

   module.exports = CustomException
   ```

### Usando o Serviço e a Exceção
Depois de criar o serviço e a exceção, você pode usá-los em seus controladores ou em outros lugares do seu aplicativo.

Por exemplo, em um controlador:

```javascript
// app/Controllers/Http/UserController.js

'use strict'

const EmailService = use('App/Services/EmailService')
const CustomException = use('App/Exceptions/CustomException')

class UserController {
  constructor() {
    this.emailService = new EmailService()
  }

  async sendEmail({ request, response }) {
    try {
      const { to, subject, message } = request.only(['to', 'subject', 'message'])
      await this.emailService.sendEmail(to, subject, message)
      return response.status(200).send('Email sent successfully')
    } catch (error) {
      throw new CustomException('Failed to send email', 500)
    }
  }
}

module.exports = UserController
```

Aqui, estamos injetando o serviço de e-mail no controlador e usando-o para enviar um e-mail. Se ocorrer um erro durante o envio do e-mail, uma exceção personalizada será lançada e tratada adequadamente.

Certifique-se de ajustar o código de acordo com suas necessidades específicas. Esses são apenas exemplos básicos para orientação.

## 13) CRIANDO O METODO UPDATE
Para criar o método `update` em um controlador no AdonisJS, você precisará seguir alguns passos. Vou mostrar como fazer isso:

1. **Adicione a Rota de Atualização**: Certifique-se de que você tenha uma rota configurada para lidar com solicitações de atualização. Isso geralmente é feito no arquivo `routes.js`. Por exemplo:

   ```javascript
   Route.put('users/:id', 'UserController.update')
   ```

   Isso definirá uma rota `PUT` para `/users/:id`, onde `:id` é o identificador do usuário que você deseja atualizar.

2. **Implemente o Método `update` no Controlador**: Agora, vá para o controlador relevante (por exemplo, `UserController.js`) e adicione o método `update`. Aqui está um exemplo básico:

   ```javascript
   // app/Controllers/Http/UserController.js

   'use strict'

   const User = use('App/Models/User')

   class UserController {
     async update({ params, request, response }) {
       try {
         const { id } = params
         const userData = request.only(['name', 'email', 'password']) // Dados que podem ser atualizados
         const user = await User.findOrFail(id)

         // Atualiza apenas os campos fornecidos na requisição
         user.merge(userData)
         await user.save()

         return response.status(200).send(user)
       } catch (error) {
         return response.status(500).send({ error: 'Failed to update user' })
       }
     }
   }

   module.exports = UserController
   ```

   Este método `update` recebe o `id` do usuário a ser atualizado na URL e os novos dados no corpo da solicitação. Ele procura o usuário pelo `id`, mescla os novos dados com os dados existentes e, em seguida, salva as alterações no banco de dados.

3. **Teste a Rota de Atualização**: Você pode usar uma ferramenta como o Postman para enviar uma solicitação `PUT` para a rota `/users/:id` com os dados de atualização e verificar se tudo está funcionando conforme o esperado.

   Por exemplo, um pedido PUT para `http://localhost:3333/users/1` com um corpo JSON contendo os dados atualizados:

   ```json
   {
     "name": "Novo Nome",
     "email": "novoemail@example.com",
     "password": "novasenha"
   }
   ```

   Isso atualizaria os dados do usuário com o ID `1` com os novos valores fornecidos.

Certifique-se de adaptar o código de acordo com suas necessidades específicas, como validação de entrada, autorização, etc. Este exemplo fornece apenas uma estrutura básica para começar.

## 14) CRIANDO O MODELO DE TAREFAS
Para criar um modelo de tarefa no AdonisJS, você pode seguir os passos abaixo:

1. **Crie o Modelo de Tarefa**: No terminal, execute o seguinte comando para criar um novo modelo chamado `Task`:

   ```
   adonis make:model Task -m
   ```

   Isso criará o modelo `Task` na pasta `app/Models` e também criará uma migração para a tabela de tarefas.

2. **Defina o Esquema da Tabela de Tarefas**: Abra o arquivo de migração recém-criado em `database/migrations`, que deve ter um nome semelhante a `timestamp_create_tasks.js`. Dentro deste arquivo, você pode definir os campos da tabela de tarefas. Por exemplo:

   ```javascript
   'use strict'

   const Schema = use('Schema')

   class CreateTasksSchema extends Schema {
     up () {
       this.create('tasks', (table) => {
         table.increments()
         table.string('title').notNullable()
         table.text('description')
         table.boolean('completed').defaultTo(false)
         table.timestamps()
       })
     }

     down () {
       this.drop('tasks')
     }
   }

   module.exports = CreateTasksSchema
   ```

   Isso define uma tabela `tasks` com os campos `title`, `description`, `completed`, e os campos de data/hora `created_at` e `updated_at`.

3. **Execute as Migrações**: Execute o seguinte comando para aplicar a migração e criar a tabela no banco de dados:

   ```
   adonis migration:run
   ```

4. **Use o Modelo em seu Aplicativo**: Agora você pode usar o modelo `Task` em seu aplicativo para interagir com as tarefas no banco de dados. Por exemplo, em seus controladores ou serviços, você pode importar e usar o modelo `Task` para criar, recuperar, atualizar ou excluir tarefas.

   ```javascript
   // Exemplo de uso do modelo Task em um controlador
   const Task = use('App/Models/Task')

   class TaskController {
     async index({ response }) {
       const tasks = await Task.all()
       return response.json(tasks)
     }

     async store({ request, response }) {
       const { title, description } = request.only(['title', 'description'])
       const task = await Task.create({ title, description })
       return response.json(task)
     }

     // Outros métodos como update e destroy podem ser implementados da mesma maneira
   }

   module.exports = TaskController
   ```

Agora você tem um modelo de tarefa configurado e pode começar a usar em seu aplicativo AdonisJS para gerenciar tarefas no banco de dados. Certifique-se de adaptar o código de acordo com suas necessidades específicas.

## 15) CRIANDO O METODO CREATE DE TAREFAS
Para criar o método `create` para suas tarefas no AdonisJS, você precisa seguir os passos abaixo:

1. **Abra o Controlador de Tarefas**: No seu controlador de tarefas (provavelmente localizado em `app/Controllers/Http/TaskController.js`), você pode adicionar o método `create`.

2. **Implemente o Método Create**: No método `create`, você irá receber os dados da nova tarefa através do objeto `request`, criar uma nova instância do modelo `Task` com esses dados e salvar no banco de dados. Aqui está um exemplo de como fazer isso:

```javascript
const Task = use('App/Models/Task')

class TaskController {
  async create({ request, response }) {
    try {
      // Recebe os dados da nova tarefa do corpo da requisição
      const { title, description } = request.only(['title', 'description'])

      // Cria uma nova instância do modelo Task com os dados recebidos
      const task = new Task()
      task.title = title
      task.description = description

      // Salva a nova tarefa no banco de dados
      await task.save()

      // Retorna a tarefa recém-criada como resposta
      return response.status(201).json(task)
    } catch (error) {
      // Retorna uma resposta de erro caso ocorra algum problema
      return response.status(500).json({ error: 'Erro ao criar a tarefa' })
    }
  }
}

module.exports = TaskController
```

3. **Defina a Rota para o Método Create**: No seu arquivo `start/routes.js`, defina a rota para o método `create` do controlador de tarefas. Por exemplo:

```javascript
Route.post('/tasks', 'TaskController.create')
```

Agora, quando você fizer uma solicitação POST para `/tasks` com os dados da nova tarefa no corpo da solicitação, o método `create` do seu controlador será executado, criando uma nova tarefa no banco de dados e retornando os detalhes da tarefa recém-criada como resposta. Certifique-se de ajustar o código de acordo com suas necessidades específicas.

## 16) CRIANDO O METODO INDEX DE TAREFAS
Para criar o método `index` para suas tarefas no AdonisJS, siga estes passos:

1. **Abra o Controlador de Tarefas**: No seu controlador de tarefas (geralmente localizado em `app/Controllers/Http/TaskController.js`), você pode adicionar o método `index`.

2. **Implemente o Método Index**: No método `index`, você irá recuperar todas as tarefas do banco de dados e retorná-las como resposta. Aqui está um exemplo de como fazer isso:

```javascript
const Task = use('App/Models/Task')

class TaskController {
  async index({ response }) {
    try {
      // Recupera todas as tarefas do banco de dados
      const tasks = await Task.all()

      // Retorna todas as tarefas como resposta
      return response.json(tasks)
    } catch (error) {
      // Retorna uma resposta de erro caso ocorra algum problema
      return response.status(500).json({ error: 'Erro ao buscar as tarefas' })
    }
  }
}

module.exports = TaskController
```

3. **Defina a Rota para o Método Index**: No seu arquivo `start/routes.js`, defina a rota para o método `index` do controlador de tarefas. Por exemplo:

```javascript
Route.get('/tasks', 'TaskController.index')
```

Agora, quando você fizer uma solicitação GET para `/tasks`, o método `index` do seu controlador será executado, recuperando todas as tarefas do banco de dados e retornando-as como resposta. Certifique-se de ajustar o código de acordo com suas necessidades específicas.

