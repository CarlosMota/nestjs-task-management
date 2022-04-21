# Nest Zero to Hero - Modern Typescript back-end Development

## Seção 1: Indroduction to NestJS e Pre-requisites

### Installing the NestJS CLI

```bash
yarn global add @nestjs/cli
```

Adicionando de forma global somos capazes de verificar a versão

```bash
nest -v
```

Caso não apareça que o comando `nest` não foi encontrado, usar o comando `yarn global bin` para ver o caminho da pasta para adicionar no path das variáveis do sistema do windows

Para verificar se tudo foi configurado normalmente usa-se o comando `yarn start:dev` para executar o projeto.

## Seção 2: Task Management Application (REST API)

### Creating our project via the NestJ CLI

Para criar um novo projeto Nest usa-se o seguinte comando

```bash
nest new <nome do projeto>
```

### Introduction to NestJS Modules

É uma boa prática criar um `Module` por pasta

#### Module Decorator properties

* **providers**: Array of providers to be available within  the module via dependency injection.
* **controllers**: Array of controllers to be instantiated within the module.
* **exports**: Array of providers to export to other modules.
* **Imports**: List of modules required by this module. Any exported provider by these modules will now be available in our module via dependency injection.

```typescript
@Module({
    providers: [ForumService],
    controllers: [ForumController],
    imports: [
        PostModule,
        CommentModule,
        AuthModule
    ],
    exports: [
        ForumService
    ]
})

export class ForumModule {}
```

### Creating a Tasks Module

Para criar `schematic` no nest deve-se usar o comando `nest g <Schematic Name>`, para ver os *schematic* disponíveis pode
acessar usando o comando `nest g --help`

Para criar um *module* é usado o comando `nest g module <module name>`.

Ao criar o *module* de *tasks*

```typescript
nest g module tasks
```

É criado uma subpasta `src/tasks` com o arquivo `tasks.module.ts` dentro do projeto.

O segundo ponto é que o arquivo `app.module.ts` foi atualizado. Foi adicinada a importação do *module tasks* dentro do arquivo.

![Arquivo app.module.ts atualizado](/images/images_from_course/app_module_updated.png)

Ao execurtamos o comando `yarn start:dev`, você verificará que as dependências *module tasks* foram inicializadas.

![Arquivo app.module.ts atualizado](/images/images_from_course/task_module_importated.png)

### Introduction to NestJS Controllers

* Responsável por ouvir as chegadas dos *requests* e retorna uma resposta para o cliente.
* Vincula para um caminho específico (como exemplo, `/task`)
* Contain ***handlers***, cada ***handle endpoints*** e os métodos de *resquest* (Get,Post, Delete...)
* Tem a vantagem da ***dependency injection*** ou **injeção de dependência** para consumir o *providers* no mesmo *module*

#### Defining a Controller

* Controles são definidos em uma class com `@Controller` *decorator*.
* O *decorator* aceita uma string, que é o caminho a ser tratado pelo controlador.

```typescript
@Controller('/tasks')
export class TasksController{
    ...
}
```

### Creating at Tasks Controller

Para criar um controller usando *CLI* usa-se o comando:

```bash
nest g controller <controller-name>
```

### Instroduction to NestJS Providers and Services

* POde ser injetado dentro do construtor se decorado como um `@Injectable`, via injeção de dependência.
* Pode ser um valor plano (???), uma classe, sync/async factory etc.
* *Providers* devem ser fornecidos a um módulo para ele ser usado.
* Podem ser exportados para um módulo e depois disponibilizados para outros módulos que importam esse módulo.

#### Whats is a service?

* Os serviços são implementados usandos provedores, mas nem todos os provedores são serviços.
* Os serviços podem ser implemtados como sigletons
* A maior parte da regra de negócio é implementada na camada de serviço.

#### Providers in Modules

```typescript
import { TasksController } from './tasks.controller';
import { TasksService } from './tasks.service';
import { LoggerService } from '../shared/logger.service';

@Module({
    controllers: [
      TasksController
    ],
    providers: [
        TasksService,
        LoggerService
    ]
})
export class TasksModule {}
```

### Creating a Tasks Service

Para criar um serviço é usado o comando 

```bash
nest g service tasks
```