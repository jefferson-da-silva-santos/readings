# 🛢️ Tudo sobre **Sequelize** – ORM para Node.js

Se você já precisou interagir com um banco de dados no **Node.js**, sabe que trabalhar direto com SQL pode ser meio chato e repetitivo. O **Sequelize** é um **ORM (Object-Relational Mapper)** que facilita essa comunicação, permitindo que você manipule bancos de dados **usando JavaScript em vez de escrever SQL puro**.

Vou te mostrar **o que é, como instalar, configurar e usar o Sequelize** com **MySQL**, **PostgreSQL** e outros bancos.

---

## 🚀 O que é o Sequelize?
O **Sequelize** é um ORM para **Node.js** que suporta **MySQL, PostgreSQL, MariaDB, SQLite e MSSQL**. Ele permite:

✅ Criar e manipular tabelas via código (migrations)  
✅ Escrever consultas **sem precisar usar SQL diretamente**  
✅ Relacionar tabelas como se fossem objetos  
✅ Fazer **validações e hooks** (gatilhos antes/salvar, deletar, etc.)  
✅ Melhor organização do código com **Models**  

Ele funciona basicamente transformando cada tabela do banco de dados em **uma classe JavaScript** que você pode manipular.

---

## 📦 Instalando o Sequelize
Antes de tudo, precisamos instalar o pacote principal do Sequelize e o driver do banco de dados.

### 🔹 Instalando o Sequelize:
```bash
npm install sequelize
```

### 🔹 Instalando o driver do banco de dados:
**Se estiver usando MySQL/MariaDB:**
```bash
npm install mysql2
```

**Se estiver usando PostgreSQL:**
```bash
npm install pg pg-hstore
```

**Se for SQLite (banco local, sem servidor):**
```bash
npm install sqlite3
```

Depois de instalar, vamos configurar a conexão com o banco.

---

## 🛠 Configurando a conexão com o banco de dados
Primeiro, criamos uma instância do Sequelize para conectar no banco:

```js
const { Sequelize } = require('sequelize');

// Configuração para MySQL
const sequelize = new Sequelize('meubanco', 'usuario', 'senha', {
  host: 'localhost',
  dialect: 'mysql'
});

// Testando conexão
(async () => {
  try {
    await sequelize.authenticate();
    console.log('🔥 Conexão com o banco de dados foi bem-sucedida!');
  } catch (error) {
    console.error('❌ Erro ao conectar com o banco:', error);
  }
})();
```

Se aparecer `🔥 Conexão com o banco de dados foi bem-sucedida!`, está tudo certo!

---

## 📌 Criando um **Model** (Tabela)
No Sequelize, uma **tabela** é representada por um **Model**. Vamos criar um model de **Usuário**:

```js
const { DataTypes } = require('sequelize');

const Usuario = sequelize.define('Usuario', {
  id: {
    type: DataTypes.INTEGER,
    primaryKey: true,
    autoIncrement: true
  },
  nome: {
    type: DataTypes.STRING,
    allowNull: false
  },
  email: {
    type: DataTypes.STRING,
    allowNull: false,
    unique: true
  },
  idade: {
    type: DataTypes.INTEGER
  }
}, {
  timestamps: true // Adiciona os campos createdAt e updatedAt automaticamente
});

// Sincronizar com o banco (criar tabela se não existir)
(async () => {
  await sequelize.sync();
  console.log('✅ Tabela de Usuários sincronizada com sucesso!');
})();
```

---

## 🟢 Criando um registro no banco
Para **inserir um usuário** no banco:

```js
(async () => {
  const novoUsuario = await Usuario.create({
    nome: 'John Doe',
    email: 'johndoe@example.com',
    idade: 30
  });

  console.log('Usuário criado:', novoUsuario.toJSON());
})();
```

Isso insere um novo registro na tabela **usuarios**.

---

## 🔵 Buscando dados no banco
Se quiser pegar **todos os usuários**:

```js
(async () => {
  const usuarios = await Usuario.findAll();
  console.log(usuarios.map(u => u.toJSON()));
})();
```

Para **buscar um usuário específico**:

```js
(async () => {
  const usuario = await Usuario.findOne({ where: { email: 'johndoe@example.com' } });
  console.log(usuario.toJSON());
})();
```

Para buscar um **usuário pelo ID**:

```js
(async () => {
  const usuario = await Usuario.findByPk(1);
  console.log(usuario?.toJSON() || 'Usuário não encontrado');
})();
```

---

## 🟡 Atualizando um registro
Se precisar alterar os dados de um usuário:

```js
(async () => {
  await Usuario.update(
    { idade: 31 },
    { where: { email: 'johndoe@example.com' } }
  );
  console.log('Usuário atualizado com sucesso!');
})();
```

---

## 🔴 Deletando um registro
Para remover um usuário:

```js
(async () => {
  await Usuario.destroy({ where: { email: 'johndoe@example.com' } });
  console.log('Usuário deletado com sucesso!');
})();
```

---

## 🔗 Relacionamentos no Sequelize
O Sequelize permite **criar relações** entre tabelas, como **1 para N** (one-to-many) e **N para N** (many-to-many).

### 🏠 Criando Model de Posts (Exemplo de relacionamento 1:N)
Vamos criar um modelo de **Post**, onde um usuário pode ter **vários posts**.

```js
const Post = sequelize.define('Post', {
  titulo: {
    type: DataTypes.STRING,
    allowNull: false
  },
  conteudo: {
    type: DataTypes.TEXT,
    allowNull: false
  }
});

// Relacionamento: Um usuário pode ter vários posts
Usuario.hasMany(Post, { onDelete: 'CASCADE' });
Post.belongsTo(Usuario);

// Sincronizar as tabelas
(async () => {
  await sequelize.sync();
  console.log('✅ Tabelas de Usuários e Posts sincronizadas!');
})();
```

Agora, se quiser **criar um post para um usuário**:

```js
(async () => {
  const usuario = await Usuario.findOne({ where: { email: 'johndoe@example.com' } });

  if (usuario) {
    const novoPost = await usuario.createPost({
      titulo: 'Meu primeiro post!',
      conteudo: 'Este é um post de teste no Sequelize.'
    });

    console.log('Post criado:', novoPost.toJSON());
  }
})();
```

E para buscar os posts de um usuário:

```js
(async () => {
  const usuario = await Usuario.findOne({
    where: { email: 'johndoe@example.com' },
    include: Post
  });

  console.log(usuario.Posts.map(p => p.toJSON()));
})();
```

---

## 📜 Migrations e Seeders
Se você quiser usar **migrations** (para versionamento do banco), o Sequelize tem a **CLI**.

1️⃣ Instale a CLI:
```bash
npm install --save-dev sequelize-cli
```

2️⃣ Inicie o Sequelize no projeto:
```bash
npx sequelize-cli init
```

Isso cria as pastas:
```
📂 migrations/
📂 seeders/
📂 models/
📂 config/
```

3️⃣ Criar uma migration:
```bash
npx sequelize-cli model:generate --name Usuario --attributes nome:string,email:string,idade:integer
```

4️⃣ Rodar as migrations:
```bash
npx sequelize-cli db:migrate
```

Isso permite **manter o banco atualizado sem precisar recriar tabelas manualmente**.

---

## 🏁 Conclusão
O Sequelize é um ORM **poderoso e flexível** para Node.js, facilitando a interação com bancos de dados sem precisar escrever SQL manualmente.

### 🎯 Resumo:
✅ **Conexão com banco**  
✅ **Criação de Models (Tabelas)**  
✅ **CRUD (Criar, Buscar, Atualizar, Deletar)**  
✅ **Relacionamentos (1:N, N:N, etc.)**  
✅ **Migrations para versionamento do banco**  

Agora é só testar no seu projeto! 🚀🔥