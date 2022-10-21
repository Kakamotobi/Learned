# NestJS

## Table of Contents
- [What is NestJS?](#what-is-nestjs)
- [Design Idea](#design-idea)
  - [Inversion of Control](#inversion-of-control)
    - [IoC in NestJS](#ioc-in-nestjs)
  - [Dependency Injection](#dependency-injection)
  - [Dependency Inversion Principle](#dependency-inversion-principle)
- [Key Concepts](#key-concepts)
  - [Controllers](#controllers)
  - [Providers/Services](#providersservices)
    - [Data Transfer Object (DTO)](#data-transfer-object-dto)
  - [Modules](#modules)
- [Getting Started](#getting-started)
  - [Installation](#installation)
  - [Setup](#setup)
  - [Core Files](#core-files)

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

## Design Idea
- The most core design idea in NestJS is the **Dependency Inversion Principle**, which embraces **Inversion of Control** and **Dependency Injection**.
### Inversion of Control
- Refers to reversing different kinds of controls in object-oriented design to achieve loose coupling.
  - "Control" refers to all of the processes in a class other than the completion of its main workflow, including control over the application flow, as well as the dependency object creation and binding process.
- **An object does not personally create/construct another object that it depends on. Instead, the object receives that other object already instantiated as an argument.**
- We just say who needs what. Then, the container looks at the type and determines what needs to be called.
#### IoC in NestJS
- Instance creation of dependencies in NestJS is delegated to the **IoC Container** (NestJS runtime).
- When the NestJS application starts, all classes are registered in the Dependency Injection Container.
  - The `@Injectable()` decorator is prefixed to the class --> the class is included in the Module's `providers` --> the `@Controller` decorator prefixed registers the controller to the Dependency Injection Container and generates an instance --> the Container identifies each class' dependencies --> the Container generates an instance of each class' dependencies and provides them (NestJS does this automatically) --> the constructed instance does is reused when needed (not removed or regenerated).
### Dependency Injection
- Refers to an object or function receiving other objects or functions that it depends on, as arguments.
- It is a form of Inversion of Control that aims to separate the concerns of constructing objects and using them, leading to loosely coupled program.
### Dependency Inversion Principle
- The idea behind this principle is also referred to as the Adapter Pattern, Facade Pattern, where a wrapper is created around external dependencies so that our codebase depends on the wrapper and not the actual implementation of the dependencies that are being used.
- **Therefore, the high-level class should rather define an interface for the low-level class that it wants to use, which the low-level classes will follow and implement.**
  - This makes it much easier to make changes/transformations in the future, by simply having to change the properties of the new instance that follows the interface.
#### Rules
1. High-level modules should not import anything from low-level modules. Both should depend on abstractions (Ex: interfaces).
2. Abstractions should not depend on details. Rather, details (concrete implementations) should depend on abstractions.
    - Ex: Paypal APIs (details/implementations) should adopt the interface provided by the high-level module.
#### Background
- If an application *directly* implements and calls APIs or other executions (the conventional way), the application is very coupled and dependent on those specific APIs (Ex: transaction APIs in e-commerce, using Mongoose to query MongoDB).
  - This has some problems:
    - It makes it hard to test our code because when testing, we don't want to call the APIs.
    - It makes it hard to switch to another set of APIs.
      - Ex: switching DBs will mean a lot of editing and refactoring in our application's codebase.
  - Store(Application) --> Paypal APIs
    - Ex: `paypal.checkout()` in the application.
- Therefore, in the Dependency Inversion Principle, an interface of sort is added in the middle between the application and APIs.
  - This interface will contain all of the behavior that we want the APIs to perform, regardless of whichever API is used (i.e. independent of anything).
  - This allows us to have multiple sets of APIs.
  - Store(Application) --> Payment Processor(Interface) <-- Paypal APIs, Stripe APIs 
    - Ex: Paypal APIs is an implementation of the Payment Processor Interface and is plugged into the Store. When the Store calls methods on the Payment Processor, the calls are delegated to the Paypal APIs.
#### Example
##### Conventional Approach
```js
// The Architecture that Dependency Inversion is trying to Solve
// Very exhaustive to make changes, especially when APIs have different approaches.

class Store {
  constructor(user) {
    // this.paypal = new Paypal(user);
    
    this.user = user;
    this.stripe = new Stripe(user);
  }
  
  purchaseLaptop(quantity) {
    // this.paypal.makePayment(2000 * quantity);
    
    this.stripe.makePayment(this.user, 2000 * quantity)
  }
}

// APIs
class Paypal {
  constructor(user) {
    this.user = user;
  }
  
  makePayment(amount) {
    console.log(`${this.user} made a payment of $${amount}`);
  }
}

class Stripe {
  makePayment(user, amount) {
    console.log(`${user} made a payment of $${amount}`);
  }
}

// Example
const store = new Store("Kakamotobi");
store.purchaseLaptop(2); // Kakamotobi made a payment of $4000
```
##### Dependency Inversion Approach
```ts
// Changes can now be made easily (Ex: switch from Paypal to Stripe) without having to change code in Store thanks to the Payment Processor interface that acts as a intermediary wrapping around the API.

class Store {
  constructor(paymentProcessor) {
    this.paymentProcessor = paymentProcessor;
  }
  
  purchaseLaptop(quantity) {
    this.paymentProcessor.makePayment(2000 * quantity);
  }
}

// Payment Processors
class PaypalPaymentProcessor {
  constructor(user) {
    this.paypal = new Paypal(user);
  }
  
  makePayment(amount) {
    this.paypal.makePayment(amount);
  }
}

class StripePaymentProcessor {
  constructor(user) {
    this.user = user;
    this.stripe = new Stripe();
  }
  
  makePayment(amount) {
    this.stripe.makePayment(this.user, amount);
  }
}

// APIs
class Paypal {
  constructor(user) {
    this.user = user;
  }
  
  makePayment(amount) {
    console.log(`${this.user} made a payment of $${amount} with Paypal`);
  }
}

class Stripe {
  makePayment(user, amount) {
    console.log(`${user} made a payment of $${amount}`);
  }
}

// Example
const storeWithPaypal = new Store(new PaypalPaymentProcessor("Kakamotobi"));
storeWithPaypal.purchaseLaptop(2); // Kakamotobi made a payment of $4000 with Paypal

const storeWithStripe = new Store(new StripePaymentProcessor("Kakamotobi"));
storeWithStripe.purchaseLaptop(2); // Kakamotobi made a payment of $4000 with Paypal
```

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
- Guards determine whether a given request will be handled by the route handler or not, depending on certain conditions (like permissions, roles, ACLs, etc.) present at run-time.
  - i.e. authorization.
- Request --> Guard --> Controller
- A provider can be implemented as a Guard to handle role-based user authentication.
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
- Pipes operate on the `arguments` that are being processed by a controller route handler.
- Use Cases
  - Transformation: transform input data to the desired form (Ex: string to integer).
  - Validation: evaluate input data and if valid, simply pass it through unchanged; otherwise, throw an exception when the data is incorrect.
- Request --> Pipe --> Controller
- A provider can be implemented as a Pipe to to validate and transform values in a controller.
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
### Setup
#### Environment
- The `ConfigModule`, which is provided by the `@nestjs/config` package, loads the `.env` file into the application.
  - It uses the `dotenv` library under the hood.
- Like any Module in NestJS, it also has a Service and uses dependency injection.
  - Therefore, we can import that Config Service inside any of our Modules of our application.
```ts
// app.module.ts

import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
    }),
  ],
})
export class AppModule {}
```
```ts
// auth.service.ts

import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';

@Injectable()
export class AuthService {
  constructor(private configService: ConfigService,) {}
  
  verifyToken(token: string) {
    const tokenSecret = configService.get('TOKEN_SECRET'); // from `.env` file.
  }
}
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
- **`app.service.ts`**
  - A basic service with a single method.

## Reference
[NestJS - A progressive Node.js framework](https://nestjs.com/)  
[NestJS in 100 Seconds - YouTube](https://www.youtube.com/watch?v=0M8AYU_hPas&ab_channel=Fireship)  
[Dependency Inversion Principle Explained - SOLID Design Principles - YouTube](https://www.youtube.com/watch?v=9oHY5TllWaU&ab_channel=WebDevSimplified)  
[Dependency inversion principle - Wikipedia](https://en.wikipedia.org/wiki/Dependency_inversion_principle)  
[Dependency injection - Wikipedia](https://en.wikipedia.org/wiki/Dependency_injection)  
[[Nestjs]IoC 와 DI](https://velog.io/@jeong3320/NestJsIoC-%EC%99%80-DI)  
