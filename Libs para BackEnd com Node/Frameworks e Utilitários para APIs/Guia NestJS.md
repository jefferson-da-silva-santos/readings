# 🚀 Tudo sobre **NestJS** – O Framework Node.js Modular e Poderoso  

Se você já trabalhou com **Express.js**, sabe que montar uma API do zero pode ser um pouco bagunçado. Agora, imagina um framework que **organiza o código**, **traz modularidade**, **suporte a TypeScript** e ainda **segue padrões do Angular**.  

Esse é o **NestJS**, um dos frameworks mais poderosos para construir **APIs escaláveis** no Node.js. Bora ver **como ele funciona, como instalar, criar rotas, usar bancos de dados e muito mais!** 🔥  

---

## 🔥 O que é o **NestJS**?  

O **NestJS** é um framework para **Node.js** baseado em **TypeScript**, que usa uma arquitetura modular parecida com a do Angular.  

Ele é perfeito para **APIs REST, GraphQL, WebSockets e até microservices**.  

🔹 **Principais vantagens:**  
✅ **Organização modular** (cada funcionalidade vira um módulo separado)  
✅ **Baseado em TypeScript** (mas funciona com JavaScript também)  
✅ **Uso de Decorators** (@Controller, @Injectable, etc.)  
✅ **Facilidade para testar** (testes unitários e integração)  
✅ **Suporte nativo a WebSockets, GraphQL e gRPC**  
✅ **Facilidade para integrar bancos de dados (TypeORM, Prisma, Sequelize, Mongoose, etc.)**  

Ou seja, o **NestJS** é tipo um **Spring Boot para Node.js**! 🚀  

---

## 📦 Instalando o NestJS  

Primeiro, precisamos instalar a CLI do NestJS:  

```bash
npm install -g @nestjs/cli
```

Agora, bora criar um novo projeto:  

```bash
nest new meu-projeto
```

Ele vai perguntar qual gerenciador de pacotes usar (`npm` ou `yarn`). Escolhe um e aguarda.  

Depois que o projeto for criado, entre na pasta:  

```bash
cd meu-projeto
```

E rode o servidor:  

```bash
npm run start
```

Se estiver tudo certo, deve aparecer:  

```
🚀 Application is running on: http://localhost:3000
```

O servidor já está no ar! 🎉  

---

## 🏗 Estrutura do NestJS  

Quando criamos um projeto NestJS, ele já vem organizado assim:  

```
📂 src
 ├── app.controller.ts   # Define rotas e respostas
 ├── app.service.ts      # Lógica de negócios
 ├── app.module.ts       # Módulo principal
 ├── main.ts             # Arquivo de inicialização
```

Tudo no NestJS é baseado em **Módulos, Controllers e Services**.  

💡 **Resumindo:**  
- **Módulos (`.module.ts`)** agrupam funcionalidades  
- **Controllers (`.controller.ts`)** definem rotas  
- **Services (`.service.ts`)** lidam com regras de negócio  

Agora, bora ver como criar nossas próprias rotas! 🚀  

---

## 🌎 Criando nossa primeira Rota (Controller)  

O **Controller** define **rotas da API**. Vamos criar um **controller de usuários**:  

```bash
nest generate controller users
```

Isso cria um arquivo `users.controller.ts` dentro da pasta `src/users/`:  

```ts
import { Controller, Get } from '@nestjs/common';

@Controller('users') // Define a rota /users
export class UsersController {
  @Get()
  getAllUsers() {
    return [{ id: 1, nome: 'John Doe' }, { id: 2, nome: 'Jane Doe' }];
  }
}
```

Agora, se você acessar `http://localhost:3000/users`, vai ver:  

```json
[
  { "id": 1, "nome": "John Doe" },
  { "id": 2, "nome": "Jane Doe" }
]
```

---

## 🔄 Criando um **Service** para lógica de negócios  

O **Service** é responsável pela lógica da aplicação. Vamos criar um service para gerenciar usuários:  

```bash
nest generate service users
```

Isso cria `users.service.ts` dentro da pasta `src/users/`. Vamos editar ele:  

```ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class UsersService {
  private users = [{ id: 1, nome: 'John Doe' }];

  getUsers() {
    return this.users;
  }

  createUser(nome: string) {
    const novoUsuario = { id: this.users.length + 1, nome };
    this.users.push(novoUsuario);
    return novoUsuario;
  }
}
```

Agora, vamos conectar o **Service** ao nosso **Controller**.  

```ts
import { Controller, Get, Post, Body } from '@nestjs/common';
import { UsersService } from './users.service';

@Controller('users')
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  getAllUsers() {
    return this.usersService.getUsers();
  }

  @Post()
  createUser(@Body() body: { nome: string }) {
    return this.usersService.createUser(body.nome);
  }
}
```

Agora podemos **criar um usuário** enviando uma requisição **POST** para `http://localhost:3000/users` com JSON:  

```json
{
  "nome": "Alice"
}
```

E o serviço vai salvar esse usuário na memória! 🚀  

---

## 🛢️ Conectando ao Banco de Dados com **TypeORM**  

O NestJS suporta vários ORMs, mas o **TypeORM** é o mais usado. Vamos instalar ele:  

```bash
npm install @nestjs/typeorm typeorm mysql2
```

Agora, configuramos a conexão com MySQL no `app.module.ts`:  

```ts
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'mysql',
      host: 'localhost',
      port: 3306,
      username: 'root',
      password: 'senha',
      database: 'meubanco',
      autoLoadEntities: true,
      synchronize: true, // Não use em produção
    }),
  ],
})
export class AppModule {}
```

Agora, vamos criar uma **entidade de Usuário**:  

```bash
nest generate entity users
```

Isso cria `users.entity.ts`. Vamos definir os campos:  

```ts
import { Entity, PrimaryGeneratedColumn, Column } from 'typeorm';

@Entity()
export class User {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  nome: string;
}
```

Agora, adicionamos a entidade no **módulo de usuários**:

```ts
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';
import { User } from './users.entity';

@Module({
  imports: [TypeOrmModule.forFeature([User])],
  controllers: [UsersController],
  providers: [UsersService],
})
export class UsersModule {}
```

Agora, no **service**, podemos usar o TypeORM para buscar e salvar usuários:

```ts
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { User } from './users.entity';

@Injectable()
export class UsersService {
  constructor(@InjectRepository(User) private userRepo: Repository<User>) {}

  getUsers() {
    return this.userRepo.find();
  }

  createUser(nome: string) {
    const novoUsuario = this.userRepo.create({ nome });
    return this.userRepo.save(novoUsuario);
  }
}
```

Agora os usuários são **salvos no banco de dados**! 🔥  

---

## 🏁 Conclusão  

O **NestJS** é um framework **modular, robusto e organizado** para construir APIs escaláveis.  

✅ **Estrutura modular** (módulos, controllers, services)  
✅ **ORM integrado** (TypeORM, Prisma, Sequelize, etc.)  
✅ **Código limpo com decorators**  
✅ **Suporte a GraphQL, WebSockets e microservices**  
✅ **Perfeito para grandes projetos**  

Agora é só testar e aplicar no seu projeto! 🚀🔥