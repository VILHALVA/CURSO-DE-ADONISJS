# MANUAL
## 1. INSTALAÇÃO DO ADONISJS:
Para instalar o AdonisJS, você precisa ter o Node.js e o npm (Node Package Manager) instalados em sua máquina. Recomendo ter a versão LTS (Long Term Support) do Node.js.

1. **Instale o AdonisJS CLI (Command Line Interface)**:
   ```sh
   npm install -g @adonisjs/cli
   ```

## 2. CRIAR UM NOVO PROJETO:
1. **Criar uma Interface**:
   ```sh
   adonis new nome-do-projeto
   ```

   Durante a criação do projeto, você será perguntado se deseja configurar algumas opções. Pode aceitar os padrões pressionando Enter ou configurá-los conforme necessário. Ou:

2. **Criar uma API REST:**

   Para criar uma API REST com AdonisJS, você pode usar o seguinte comando:

   ```bash
   adonis new nome-do-projeto --api-only
   ```

   Este comando criará um novo projeto AdonisJS configurado exclusivamente para criar uma API REST. Ele não incluirá recursos de interface de usuário, como visualizações e roteamento de página, mas fornecerá suporte completo para a criação de endpoints de API para manipular e fornecer dados de forma programática.

3. **Navegue até o diretório do projeto**:
   ```sh
   cd nome-do-projeto
   ```

4. **Instale as dependências**:
   ```sh
   npm install
   ```

## 3. EXECUTAR O PROJETO:
1. **Inicie o servidor de desenvolvimento**:
   ```sh
   adonis serve --dev
   ```

   O servidor estará disponível em `http://localhost:3333`.

## 4. CONFIGURANDO O BANCO DE DADOS:
1. **Configure a conexão do banco de dados no arquivo `.env`**:
   ```env
   DB_CONNECTION=sqlite
   SQLITE_DATABASE=./database/db.sqlite3
   ```

   Ou, se estiver usando outro banco de dados (por exemplo, MySQL):
   ```env
   DB_CONNECTION=mysql
   MYSQL_HOST=127.0.0.1
   MYSQL_PORT=3306
   MYSQL_USER=root
   MYSQL_PASSWORD=senha
   MYSQL_DB_NAME=adonis
   ```

2. **Edite a configuração do banco de dados em `config/database.ts`** para refletir suas variáveis de ambiente.

## 5. CRIANDO E RODANDO MIGRAÇÕES:
1. **Criar uma nova migração**:
   ```sh
   adonis make:migration users
   ```

2. **Editar a migração gerada** em `database/migrations/xxxx_users.ts`:
   ```ts
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

## 6. ESTRUTURA DE DIRETÓRIOS:
Após criar o projeto, você verá a seguinte estrutura de diretórios:

```
nome-do-projeto/
├── .adonisrc.json
├── .editorconfig
├── .env
├── .env.example
├── .gitignore
├── ace
├── config/
│   ├── app.ts
│   ├── database.ts
│   ├── ...
├── contracts/
│   ├── events.ts
│   ├── hash.ts
│   ├── ...
├── database/
│   ├── migrations/
│   ├── seeds/
│   ├── factory.ts
├── node_modules/
├── public/
│   ├── css/
│   ├── js/
│   ├── ...
├── resources/
│   ├── views/
├── start/
│   ├── kernel.ts
│   ├── routes.ts
├── app/
│   ├── Controllers/
│   │   ├── Http/
│   │       ├── ExampleController.ts
│   ├── Middleware/
│   ├── Models/
│   ├── Validators/
├── test/
│   ├── functional/
│   ├── unit/
├── .prettierrc
├── package.json
├── README.md
├── tsconfig.json
└── tslint.json
```

- **.adonisrc.json**: Arquivo de configuração do AdonisJS.
- **.env**: Variáveis de ambiente para configurar a aplicação.
- **config/**: Configurações da aplicação (banco de dados, autenticação, etc).
- **contracts/**: Definições de interfaces e contratos para o AdonisJS.
- **database/**: Arquivos relacionados ao banco de dados (migrações, seeds, factories).
- **public/**: Arquivos públicos, como CSS, JavaScript e imagens.
- **resources/**: Templates e views.
- **start/**: Arquivos de inicialização e configuração de rotas.
- **app/**: Contém a lógica principal da aplicação:
  - **Controllers/**: Controladores que manipulam as requisições.
  - **Middleware/**: Middleware que manipula requisições antes de alcançar os controladores.
  - **Models/**: Modelos que representam as tabelas do banco de dados.
  - **Validators/**: Validação de dados.
- **test/**: Testes unitários e funcionais.

## CONCLUSÃO:
Agora você deve ter um ambiente AdonisJS configurado, um servidor de desenvolvimento em execução e um banco de dados preparado para sua aplicação. A estrutura de diretórios está pronta para você começar a construir funcionalidades. 