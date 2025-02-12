# 🛢️ Tudo sobre **Knex.js** – O SQL Query Builder para Node.js  

Se você precisa trabalhar com **bancos de dados relacionais** no **Node.js**, provavelmente já viu duas abordagens comuns:  

1. **Escrever SQL puro** – Mais flexível, mas chato de manter 😩  
2. **Usar um ORM (Sequelize, Prisma)** – Facilita, mas pode ser pesado 🚀  

O **Knex.js** é um meio-termo perfeito! Ele é um **Query Builder** que permite escrever consultas **SQL estruturadas em JavaScript**, sem a complexidade de um ORM e sem precisar escrever SQL puro o tempo todo.  

---

## 🚀 O que é o **Knex.js**?  
O **Knex.js** é um **query builder** para **Node.js**, compatível com vários bancos de dados SQL, incluindo:  

✅ **MySQL**  
✅ **PostgreSQL**  
✅ **SQLite**  
✅ **MSSQL**  

Com ele, você pode criar **queries estruturadas e reutilizáveis** sem precisar escrever SQL puro manualmente.

💡 **Knex não é um ORM!** Ele apenas **gera SQL de forma programática** e permite trabalhar com **migrations, transactions e queries encadeadas**.

---

## 📦 Instalando o Knex e um driver de banco  

Para instalar o **Knex.js**, rode:  

```bash
npm install knex
```
ou, se estiver usando **yarn**:  
```bash
yarn add knex
```

Além do Knex, você precisa instalar o **driver do banco** que vai usar.  

### 🔹 Para MySQL/MariaDB  
```bash
npm install mysql2
```

### 🔹 Para PostgreSQL  
```bash
npm install pg
```

### 🔹 Para SQLite  
```bash
npm install sqlite3
```

Agora que temos tudo pronto, bora conectar ao banco. 🚀  

---

## 🔌 Configurando a conexão no Knex  

Crie um arquivo `knexfile.js` e configure sua conexão:  

```js
module.exports = {
  development: {
    client: 'mysql2', // ou 'pg', 'sqlite3', etc.
    connection: {
      host: 'localhost',
      user: 'root',
      password: 'senha',
      database: 'meubanco'
    },
    migrations: {
      tableName: 'knex_migrations',
      directory: './migrations'
    }
  }
};
```

Agora, crie uma instância do Knex no seu código:

```js
const knex = require('knex')(require('./knexfile').development);
```

Pronto! Agora podemos começar a criar e manipular dados. 🔥

---

## 🔨 Criando Migrations (Estrutura do Banco)  

O **Knex.js** tem suporte a **migrations**, que são scripts para criar e atualizar tabelas.  

### 📌 Criando uma migration
```bash
npx knex migrate:make criar_usuarios
```

Isso cria um arquivo dentro de `migrations/`, tipo:
```
migrations/
  20240211_criar_usuarios.js
```

Agora, edite esse arquivo e defina a estrutura da tabela:

```js
exports.up = function(knex) {
  return knex.schema.createTable('usuarios', table => {
    table.increments('id').primary();
    table.string('nome').notNullable();
    table.string('email').notNullable().unique();
    table.integer('idade');
    table.timestamps(true, true);
  });
};

exports.down = function(knex) {
  return knex.schema.dropTable('usuarios');
};
```

Agora, execute a migration para criar a tabela:  

```bash
npx knex migrate:latest
```

Isso cria a tabela **usuarios** no banco! 😎  

Se quiser **reverter uma migration**, faça:  

```bash
npx knex migrate:rollback
```

---

## 🟢 Inserindo Dados (INSERT)  

Para adicionar registros na tabela, usamos `.insert()`:

```js
(async () => {
  const novoUsuario = await knex('usuarios').insert({
    nome: 'John Doe',
    email: 'johndoe@example.com',
    idade: 30
  });

  console.log('Usuário criado com ID:', novoUsuario);
})();
```

Isso insere um usuário no banco e retorna o **ID** gerado.

---

## 🔵 Buscando Dados (SELECT)  

### 🔹 Buscar todos os usuários  
```js
(async () => {
  const usuarios = await knex('usuarios').select('*');
  console.log(usuarios);
})();
```

### 🔹 Buscar um usuário pelo email  
```js
(async () => {
  const usuario = await knex('usuarios').where({ email: 'johndoe@example.com' }).first();
  console.log(usuario);
})();
```

### 🔹 Filtrar usuários maiores de 18 anos  
```js
(async () => {
  const adultos = await knex('usuarios').where('idade', '>=', 18);
  console.log(adultos);
})();
```

---

## 🟡 Atualizando Dados (UPDATE)  

Para atualizar um usuário, usamos `.update()`:

```js
(async () => {
  await knex('usuarios').where({ email: 'johndoe@example.com' }).update({ idade: 35 });
  console.log('Usuário atualizado!');
})();
```

Isso altera a idade do usuário com o email `johndoe@example.com`.

---

## 🔴 Deletando Dados (DELETE)  

Para excluir um usuário, usamos `.delete()`:

```js
(async () => {
  await knex('usuarios').where({ email: 'johndoe@example.com' }).delete();
  console.log('Usuário deletado!');
})();
```

Isso remove o usuário do banco.

---

## 🔗 Relacionamentos (JOIN)  

O Knex permite **fazer JOINs** facilmente. Vamos supor que temos uma tabela `posts` relacionada com `usuarios`:

```js
(async () => {
  const posts = await knex('posts')
    .join('usuarios', 'posts.usuario_id', '=', 'usuarios.id')
    .select('posts.*', 'usuarios.nome as autor');

  console.log(posts);
})();
```

Isso retorna todos os posts com o **nome do autor**.

---

## 🔥 Transações no Knex  

Se precisar garantir que várias operações sejam executadas **juntas ou nenhuma seja salva**, usamos **transações**:

```js
(async () => {
  const trx = await knex.transaction();

  try {
    const novoUsuario = await trx('usuarios').insert({
      nome: 'Alice',
      email: 'alice@example.com',
      idade: 28
    });

    await trx('posts').insert({
      titulo: 'Meu primeiro post',
      conteudo: 'Este é um post de teste',
      usuario_id: novoUsuario[0]
    });

    await trx.commit();
    console.log('Usuário e post criados com sucesso!');
  } catch (error) {
    await trx.rollback();
    console.error('Erro na transação:', error);
  }
})();
```

Se alguma das queries falhar, **nada será salvo** no banco.

---

## 🎯 Resumo Final  

O **Knex.js** é um ótimo **query builder** para quem quer uma abordagem mais flexível do que um ORM, mas ainda quer evitar SQL puro.

✅ **Suporte a múltiplos bancos (MySQL, PostgreSQL, SQLite, etc.)**  
✅ **Migrations para versionamento do banco**  
✅ **Queries estruturadas e reutilizáveis**  
✅ **JOINs, Transactions e Performance Otimizada**  

Se você quer **mais controle sobre suas queries**, mas **não quer a complexidade de um ORM como o Sequelize**, o Knex.js é uma **excelente escolha**! 🚀🔥