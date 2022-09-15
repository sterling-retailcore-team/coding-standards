# Development Guide
This is a set of conventions for backend engineers starting up or working on a JavaScript, Node Js or Nest Js application. The guide has been divided into sections for easy readability and walk-through.

## NodeJs/NestJs Specific

### Directory structure
The directory structure for a multi-repo member is : 
```
.
├── README.md
├── dist
├── nest-cli.json
├── package-lock.json
├── package.json
├── src
│   ├── main.ts
│   └── appModule
│       ├── dto
│       │   ├── create-appModule.dto.ts
│       │   └── update-appModule.dto.ts
│       ├── entities
│       │   └── appModule.entites.ts
│       ├── appModule.controller.spec.ts
│       ├── appModule.controller.ts
│       ├── appModule.module.ts
│       ├── appModule.service.spec.ts
│       └── appModule.service.ts
├── libs
│   ├── common
│   └── database
|   └── ...
├── test
│   ├── app.e2e-spec.ts
│   └── jest-e2e.json
├── tsconfig.build.json
└── tsconfig.json
```

### Naming convention
- Single character names should be avoided
- Camel case(e.g myName) convention is employed for all except classed where Pascal case(MyClass) is used.
- 

### Linters
For easy code readability and collaboration, the eslint and @typescript-eslint/parser is employed for formatting, parsing and cleanup.

## Shared libraries/packages
A shared library/package is one that houses function(s) that are reusable across modules or applications e.g text formatter, database setups etc.
- Every shared library/package goes into the `libs` folder
- Packages reusables across modules in an application goes inside `common ` subfolder of `libs`
- Packages reusable on different applications is registered as an independent package in `libs`



## General Guide(NodeJs/DotNet/Python)
===========================================================================

### Code Versioning(Git)
The version control system of choice is the git and github. When a task is assigned, a new task branch is expected to be created unless expressly agreed. The new task branch takes the form `task_type/task_description` where task_type can be any of:
``` 
- feature
- hotfix 
```
The task description is generic based of the task to be done.Below is the basic steps expected for every task assigned:

```
- Clone the repository i.e git clone <remote_repository_url>
- Checkout to develop branch i.e git fetch && git checkout develop 
- Update your local develop branch i.e git pull origin
- Create a task branch off the  develop branch i.e git checkout -b <task_branch_name>
- Implement the task requirement(s) and add changes i.e git add .
- Commit new changes i.e git commit -m "<action> on <entitity>"
- Push new changes to remote i.e git push origin <task_branch_name>:<remote_task_branch_name>
```

### Monorepo approach
There are cases where it is required/expected/best to put together closely related repositories such as  user and auth repositories. In such cases, a monorepo approach is employed to the limitation and dynamism of the platform(e.g NodeJs, NestJs or Python). A sample NestJs monorepo has the following structure:

```
.
├── Dockerfile
├── README.md
├── apps
│   ├── auth
│   │   ├── Dockerfile
│   │   ├── entrypoint.sh
│   │   ├── src
│   │   │   ├── main.ts
│   │   │   ├── authAppSectionName
│   │   │   │   ├── dto
│   │   │   │   │   └── custom.dto.ts
│   │   │   │   ├── entities
│   │   │   │   │   ├── category.entity.ts
│   │   │   │   │   └── permission.entity.ts
│   │   │   │   ├── authAppSectionName.controller.spec.ts
│   │   │   │   ├── authAppSectionName.controller.ts
│   │   │   │   ├── authAppSectionName.module.ts
│   │   │   │   ├── authAppSectionName.service.spec.ts
│   │   │   │   └── authAppSectionName.service.ts
│   │   │   ├── authAppSectionName2
│   │   │   │   ├── dto
│   │   │   │   │   ├──...
│   │   │   │   ├── entities
│   │   │   │   │   ├──...
│   │   │   │   ├── ...
│   │   ├── test
│   │   │   ├── app.e2e-spec.ts
│   │   │   └── jest-e2e.json
│   │   └── tsconfig.app.json
│   └── users
│       ├── Dockerfile
│       ├── entrypoint.sh
│       ├── src
│       │   ├── userAppSectionName
│       │   │   ├── userAppSectionName.controller.spec.ts
│       │   │   ├── userAppSectionName.controller.ts
│       │   │   ├── userAppSectionName.module.ts
│       │   │   ├── userAppSectionName.service.spec.ts
│       │   │   ├── userAppSectionName.service.ts
│       │   │   ├── dto
│       │   │   │   └── update-userAppSectionName.dto.ts
│       │   │   └── entities
│       │   │       └── userAppSectionName.entity.ts
│       │   ├── main.ts
│       ├── test
│       │   ├── app.e2e-spec.ts
│       │   └── jest-e2e.json
│       └── tsconfig.app.json
├── docker-compose.dev.yml
├── docker-compose.yml
├── entrypoint.sh
├── libs
│   ├── common
│   │   ├── src
│   │   │   ├── auth
│   │   │   │   ├── ...
│   │   │   ├── filter
│   │   │   │   ├── ...
│   │   │   ├── other
│   │   │   │   ├── ...
│   │   │   └── utils
│   │   │       ├── index.ts
│   │   │       ├── pagination
│   │   │       │   ├── constants.ts
│   │   │       │   ├── index.ts
│   │   │       │   └── interfaces.ts
│   │   │       └── validator
│   │   │           ├── boolean.ts
│   │   │           └── index.ts
│   │   └── tsconfig.lib.json
│   └── database
│       ├── src
│       │   ├── database.module.ts
│       │   ├── database.service.spec.ts
│       │   ├── index.ts
│       │   ├── seeds
│       │   │   ├── auth.seed.ts
│       │   │   └── user.seed.ts
│       │   │   └── *.seed.ts
│       │   ├── services
│       │   │   ├── auth.service.ts
│       │   │   ├── index.ts
│       │   │   └── user.service.ts
│       │   └── utills
│       │       └── filterUniquePermission.ts
│       └── tsconfig.lib.json
├── nest-cli.json
├── package-lock.json
├── package.json
├── tsconfig.build.json
└── tsconfig.json
```

### Service-to-Service communication
This is peculiar to applications with microservice architecture. exchange of data and information amongst parcticipating services is done through TCP connections and queue exchanges using RabbitMq.
For a NestJs hybrid application, the microservice connection is setup in the `main.ts` of a participating project/app. A sample setup involving a rabbitMq setup and a pure TCP connection is shown below: 

```
const rmqService = app.get<RmqService>(RmqService)
  app.connectMicroservice<RmqOptions>(rmqService.getOptions('OTHER', true))

  // connect all microservices
  app.connectMicroservice({
    transport: Transport.TCP,
    options: {
      host: configService.get('OTHER_SERVICE_HOST'),
      port: configService.get('OTHER_SERVICE_TCP_PORT'),
    },
  })

  await app.startAllMicroservices()
  await app.listen(configService.get('SERVICE_HTTP_PORT'))
```

* Note: the app.listen(...) comes last in all bootstrap functions
