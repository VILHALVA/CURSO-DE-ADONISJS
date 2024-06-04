# SINTAXE
## ESTRUTURA BÁSICA:
1. **Estrutura do Projeto**:
   - **start**: Contém arquivos de configuração de inicialização.
   - **app**: Contém o código da aplicação (controladores, modelos, validadores, etc).
   - **config**: Configurações da aplicação.
   - **database**: Arquivos de migração e seeds.
   - **public**: Arquivos públicos (CSS, JS, imagens).
   - **resources**: Views e templates.
   - **routes**: Arquivos de definição de rotas.

2. **Rotas**:
   As rotas definem como as requisições HTTP são mapeadas para os controladores e ações.
   ```js
   // start/routes.ts
   import Route from '@ioc:Adonis/Core/Route'

   Route.get('/', async () => {
     return { hello: 'world' }
   })
   ```

3. **Controladores**:
   Controladores contêm a lógica para manipular as requisições.
   ```js
   // app/Controllers/Http/UsersController.ts
   import { HttpContextContract } from '@ioc:Adonis/Core/HttpContext'

   export default class UsersController {
     public async index({ response }: HttpContextContract) {
       response.send('List of users')
     }
   }
   ```

4. **Modelos**:
   Modelos representam tabelas no banco de dados e são usados para interagir com os dados.
   ```js
   // app/Models/User.ts
   import { DateTime } from 'luxon'
   import { BaseModel, column } from '@ioc:Adonis/Lucid/Orm'

   export default class User extends BaseModel {
     @column({ isPrimary: true })
     public id: number

     @column()
     public name: string

     @column()
     public email: string

     @column.dateTime({ autoCreate: true })
     public createdAt: DateTime

     @column.dateTime({ autoCreate: true, autoUpdate: true })
     public updatedAt: DateTime
   }
   ```

5. **Migrações**:
   Migrações são usadas para gerenciar a estrutura do banco de dados.
   ```js
   // database/migrations/1623350725231_users.ts
   import BaseSchema from '@ioc:Adonis/Lucid/Schema'

   export default class Users extends BaseSchema {
     protected tableName = 'users'

     public async up () {
       this.schema.createTable(this.tableName, (table) => {
         table.increments('id')
         table.string('name')
         table.string('email').unique()
         table.timestamps(true)
       })
     }

     public async down () {
       this.schema.dropTable(this.tableName)
     }
   }
   ```

## EXEMPLO PRÁTICO: CRUD DE USUÁRIOS:
1. **Criar Modelo e Migração**:
   ```sh
   node ace make:model User -m
   ```

2. **Editar a Migração** (arquivo gerado em `database/migrations`):
   ```js
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

3. **Rodar a Migração**:
   ```sh
   node ace migration:run
   ```

4. **Criar Controlador**:
   ```sh
   node ace make:controller UsersController
   ```

5. **Editar Controlador**:
   ```js
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

6. **Definir Rotas**:
   ```js
   // start/routes.ts
   import Route from '@ioc:Adonis/Core/Route'

   Route.get('/users', 'UsersController.index')
   Route.post('/users', 'UsersController.store')
   Route.get('/users/:id', 'UsersController.show')
   Route.put('/users/:id', 'UsersController.update')
   Route.delete('/users/:id', 'UsersController.destroy')
   ```

Com esses passos, você terá um CRUD básico de usuários funcionando. 