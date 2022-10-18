# NestJS

## Table of Contents
- [What is NestJS?](#what-is-nestjs)
- [Key Concepts](#key-concepts)
  - [Controllers](#controllers)
  - [Providers/Services](#providersservices)
    - [Data Transfer Object (DTO)](#data-transfer-object-dto)
  - [Modules](#modules)
- [Getting Started](#getting-started)

## What is NestJS?
> A progressive Node.js framework for building efficient, reliable and scalable server-side applications. | NestJS

- It provides a suite of tools that leverages HTTP server microframeworks (Ex: Express or Fastify).
- It supports REST and GraphQL APIs out of the box. Or it can be used to create a full-stack application using the MVC pattern.
- It contains built-in modules for building a server-side application.
  - Ex: modules to work with databases (Ex: MongoDB, Redis), handle security, implement streaming, caching, logging, cors, websockets, rate-limiting, etc.
- In NestJS, modularity is very important. It is built around **Dependency Injection**.
  - Dependency Injection is used to avoid having to manage where a class is created and who manages it.
  - It solves the problem of dependency management.
- It has its own CLI, which can be used to create a new project (`nest new` command).
### NestJS vs Microframeworks (Express, Fastify)
- NestJS solves a different problem from Express. It solves architecture.
- Express is very unopinionated and does not give a direction as to how the project should be structured or how to do particular things.
  - Therefore, you need to know how to structure and scale it properly.
- NestJS uses Express or Fastify under the hood. It is an abstraction of NestJS.
  - Basically, enhancing them through a layer of abstraction but also exposing their APIs directly to the developer, which allows developers to use third-party modules for said framework.
### Why use NestJS?
- Structure, modularity, native TypeScript support, built-in functionality to integrate REST API or GraphQL, and security.

## Key Concepts
- In a NestJS application, logic is largely separated into Controllers and Providers/Services.
- The Controller receives a request from a client and will call a function from the Service and return the response to the client.
  - But to do so, the Controller needs an instance of the Service class.
    - Instead of the Controller having to import and instantiate a Service class manually (Ex: `const service = new AuthService()`), the Controller receives an instance of the Service class through its constructor.
    - Ex: `constructor(private authService: AuthService)`. NestJS takes care of how to instantiate the Service and how to pass it to the Controller.
  - Functions in the Controller and Service will usually match each other.
### Controllers
- Controllers are responsible for handling incoming **requests** and returning **responses** to the client.
  - It's purpose is to receive specific requests for the application.
- To create a controller, add the `@Controller()` decorator to a class.
  - Inside the class, implement methods and decorate them with HTTP verbs, HTTP status code, HTTP headers, etc.
  - Use parameter decorators (`@Body()`, `@Param()`, `@Headers`) to access the request object.
  - The returned value in the method will be the response body.
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
### Providers/Services
- Providers/Services are responsible for executing business logic.
- Providers are JS classes that contain shared logic throughout the entire application and can be injected as a dependency where needed.
- To create a provider, add the `@Injectable()` decorator to a class.
  - Now this class can be injected in the constructor of another class.
#### Data Transfer Object (DTO)
- A DTO is an object that carries data between processes.
- It is an object where you push your data for example, from a request, and run validation on it.
- Example
  ```ts
  // auth.dto.ts
  
  import { IsEmail, IsString } from 'class-validator';
  
  export class AuthDto {
    @IsEmail()
    email: string;

    @IsString()
    password: string;
  }
  ```
  ```ts
  // auth.controller.ts
  
  import { Controller, Body, Post } from '@nestjs/common';
  import { AuthService } from './auth.service';
  import { AuthDto } from './auth.dto';
  
  @Controller('auth')
  export class AuthController {
    constructor(private authService: AuthService) {}
    
    @Post('signup')
    signup(@Body() dto: AuthDto) {
      return this.authService.signup(dto);
    }
  }
  ```
  ```ts
  // auth.service.ts
  
  import { Injectable } from '@nestjs/common';
  import { User, UserDocument } from './user.schema';
  import { InjectModel } from '@nestjs/mongoose';
  import { Model } from 'mongoose';
  import { AuthDto } from './auth.dto';
  
  @Injectable()
  export class AuthService {
    constructor(@InjectModel(User.name) private userModel: Model<UserDocument>) {}

    async signup(dto: AuthDto) {
      return await this.userModel.create({
        ...dto,
      });
    }
  }
  ```
#### Examples
##### Example 1 - Guard
- A **Guard** implements the `CanActivate` interface.
- A provider can be implemented as a Guard to handle role-based user authentication.
- Request --> Guard --> Controller
```ts
@Injectable() // attaches metadata, which declares that this class can be managed by the Nest IoC container.
export class AuthGuard implements CanActivate {
  canActivate() {
    if (isAdmin) return true;
  }
}

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}
}
```
##### Example 2 - Pipe
- A **Pipe** implements the `PipeTransform` interface.
- Pipes are functions that transform your data.
- A provider can be implemented as a Pipe to to validate and transform values in a controller.
- Request --> Pipe --> Controller
```ts
@Get(':id)
findOne(@Param('id', ValidationPipe) user) {
  return { user };
}
```
##### Example 3
- A provider responsible for data storage and retrieval, and are designed to be used by Controllers.
```ts
// users.service.ts

import { Injectable } from '@nestjs/common';
import { User } from './interfaces/user.interface';

@Injectable()
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
- Modules allow code to be organized into smaller chunks that can be lazy-loaded to run faster in serverless environments.
- To create a module, use the `@Module()` decorator.
  - This decorator provides metadata for Nest to use to organize the application structure.
- Each feature can have a module which will ultimately need to be included in the application's root module.
  - The root module is the starting point at which Nest builds the application graph.
### Module Properties
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
[NestJS in 100 Seconds - YouTube](https://www.youtube.com/watch?v=0M8AYU_hPas&ab_channel=Fireship)  
