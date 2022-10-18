# NestJS

## Table of Contents
- [What is NestJS?](#what-is-nestjs)
- [Key Concepts](#key-concepts)
  - [Controllers](#controllers)
  - [Providers](#providers)
  - [Modules](#modules)
- [Getting Started](#getting-started)

## What is NestJS?
> A progressive Node.js framework for building efficient, reliable and scalable server-side applications. | NestJS

- It provides a suite of tools that leverages HTTP Server microframeworks (either Express or Fastify).
  - Basically, enhancing them through a layer of abstraction but also exposing their APIs directly to the developer, which allows developers to use third-party modules for said framework.
- It supports REST and GraphQL APIs out of the box. Or it can be used to create a full-stack application using the MVC pattern.
- It contains built-in modules for building a server-side application.
  - Ex: modules to work with databases (Ex: MongoDB, Redis), handle security, implement streaming, caching, logging, cors, websockets, rate-limiting, etc.
- It has its own CLI, which can be used to create a new project (`nest new` command).
- NestJS is built around **Dependency Injection**.

## Key Concepts
### Controllers
- Controllers are responsible for handling incoming **requests** and returning **responses** to the client.
  - It's purpose is to receive specific requests for the application.
- Classes and **Decorators** are used to create controllers.
  - **Decorators enable Nest to create a routing map.**
  - The **Request Object** can be accessed using decorators such as `@Body()`, `@Param()`, `@Headers()`, `@Query()`.
- Controllers should handle HTTP requests and delegate more complex tasks to **providers**.
- Once controllers are fully defined, Nest needs to be informed about them in `app.module.ts`.
#### Example
```ts
// users.controller.ts

import { Controller, Get } from '@nestjs/common';
import { CreateUserDto, UpdateUserDto, ListAllEntities } from "./dto.ts";

@Controller('users')
export class UsersController {
  @Get() // GET /users
  findAll(@Query() query: ListAllEntities): string {
    return 'This action returns all users';
  }
  
  @Get(':id') // GET /users/:id
  findOne(@Param('id') id: string) {
    return `This action returns a user with id: ${id}`;
  }
  
  @POST() // POST /users
  @HttpCode(204) // response status code
  @Header('Cache-control', 'none') // custom response header
  create(@Body() createUserDto: CreateUserDto) {
    return 'This action adds a new user';
  }
  
  @PUT(':id')
  update(@Param('id') id: string, @Body() updateUserDto: UpdateUserDto) {
    return `This action updates a user with id: ${id}`;
  }
  
  @DELETE(':id')
  remove(@Param('id') id: string) {
    return `This action removes a user with id: ${id}`;
  }
}
```
```ts
// create-user.dto.ts
// Recommended to use classes (not TypeScript) to determine the DTO schema.
// Classes are preserved as real entities in the compiled JS so Nest can reference them.

export class CreateUserDto {
  name: string;
  age: number;
}

export class UpdateUserDto {
  // ...
}

export class ListAllEntities {
  // ...
}
```
### Providers
- Many of the basic Nest classes may be treated as a provider - services, repositories, factories, helpers, etc.
- Providers are JS classes that are declared as `providers` in a **module**.
- Providers can be **injected** as a dependency.
  - i.e. objects can create various relationships with each other.
#### Example
- **Services** are responsible for data storage and retrieval, and are designed to be used by Controllers.
```ts
// users.service.ts

import { Injectable } from '@nestjs/common';
import { User } from './interfaces/user.interface';

@Injectable() // attaches metadata, which declares that this class can be managed by the Nest IoC container.
export class UsersService {
  private readonly users: User[] = [];

  create(user: User) {
    this.users.push(user);
  }

  findAll(): User[] {
    return this.users;
  }
}
```
```ts
// users.controller.ts

import { Controller, Get, Post Body } from '@nestjs/common';
import { UsersService } from './users.service';
import { CreateUserDto } from './dto/create-user.dto';
import { User } from './interfaces/cat.interface';

@Controller('users')
export class UsersController {
  constructor(private usersService: UsersService) {}
  
  @Get()
  async findAll(): Promise<User[]> {
    return this.usersService.findAll();
  }
  
  @Post()
  async create(@Body() createUserDto: CreateUserDto) {
    this.usersService.create(createUserDto);
  }
}
```
### Modules
- A module is a class that is annotated with a `@Module()` decorator.
  - This decorator provides metadata for Nest to use to organize the application structure.
- Each feature can have a module which will ultimately need to be included in the application's root module.
  - The root module is the starting point at which Nest builds the application graph.
- Module properties include:
  - `imports`
    - The list of imported modules that export the providers which are required in this module.
  - `controllers`
    - The set of controllers in this module which have to be instantiated.
  - `providers`
    - The providers that will be instantiated by the Nest injector and that may be shared at least across this module.
  - `exports`
    - The subset of `providers` that are provided by this module and should be available in other modules which import this module.
#### Example
```ts
// users/users.module.ts

import { Module } from '@nestjs/common';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';

@Module({
  controllers: [usersController],
  providers: [UsersService]
})

export class UsersModule {}
```

## Getting Started
### Installation
- Creates a NestJS project preconfigured with Jest and TypeScript.
```zsh
nest new project-name
```
### Core Files
- **`main.ts`**
  - The entry file of the application which uses the core function `NestFactory` to create a Nest application instance.
  - It includes an async function that bootstraps the application.
  - Example
    ```ts
    import { NestFactory } from '@nestjs/core';
    import { AppModule } from './app.module';

    async function bootstrap() {
      const app = await NestFactory.create(AppModule); // returns an application object.
      await app.listen(3000); // start up HTTP requests listener.
    }
    bootstrap();
    ```
- **`app.service.ts`**
  - A basic service with a single method.
- **`app.module.ts`**
  - The root module of the application.
  - Example
    ```ts
    import { Module } from '@nestjs/common';
    import { AuthModule } from './auth/auth.module';
    import { UsersModule } from './users/users.module';

    @Module({
      imports: [
        AuthModule,
        UsersModule
      ]
    });

    export class AppModule {};
    ```
- **`app.controller.ts`**
  - A basic controller with a single route.
- **`app.controller.spec.ts`**
  - The unit tests for the controller.

## Reference
[NestJS - A progressive Node.js framework](https://nestjs.com/)  
