# ⚡ Tudo sobre **Express.js** – O Framework Web mais Popular do Node.js

Se você está desenvolvendo uma aplicação web com **Node.js**, provavelmente já ouviu falar do **Express.js**. Ele é **rápido, minimalista e flexível**, sendo uma das opções mais populares para criar **APIs e servidores web**.

Aqui, vou te mostrar **o que é, como instalar e como usar o Express para criar APIs REST, middlewares, rotas e muito mais!**

---

## 🚀 O que é o Express?
O **Express.js** é um **framework para Node.js** que facilita a criação de **servidores web** e **APIs**.

✅ **Leve e rápido** – Apenas um fino wrapper sobre o HTTP do Node.js  
✅ **Facilidade para criar rotas** – Definição simples de **GET, POST, PUT, DELETE**  
✅ **Suporte a middlewares** – Permite processar requisições antes de enviá-las  
✅ **Ótima integração com bancos de dados** – Funciona bem com **MySQL, PostgreSQL, MongoDB**  
✅ **Suporte a templates** – Possível usar **EJS, Pug, Handlebars** para renderizar HTML  

---

## 📦 Instalando o Express
Para começar, você precisa ter o **Node.js** instalado. Depois, crie uma pasta para o projeto e instale o Express:

```bash
mkdir meu-servidor && cd meu-servidor
npm init -y
npm install express
```

Agora podemos criar um **servidor web básico**.

---

## 🔥 Criando um Servidor com Express
No seu projeto, crie um arquivo **`server.js`** e adicione:

```js
const express = require('express');
const app = express();

// Rota simples
app.get('/', (req, res) => {
  res.send('🔥 Servidor Express rodando com sucesso!');
});

// Inicia o servidor na porta 3000
app.listen(3000, () => {
  console.log('🚀 Servidor rodando em http://localhost:3000');
});
```

Agora, execute:

```bash
node server.js
```

Se tudo deu certo, você verá **"🔥 Servidor Express rodando com sucesso!"** ao acessar `http://localhost:3000` no navegador. 🎉

---

## 🔄 Criando uma API REST com Express
Agora, bora criar um **CRUD** completo para uma API de usuários.

### 📌 Configuração inicial:
Crie um arquivo **`index.js`** e configure o Express com suporte a JSON:

```js
const express = require('express');
const app = express();

app.use(express.json()); // Habilita suporte a JSON nas requisições

const PORT = 3000;
app.listen(PORT, () => console.log(`🚀 Servidor rodando em http://localhost:${PORT}`));
```

Agora podemos criar rotas para **listar, criar, atualizar e deletar usuários**.

---

### 🟢 Criando Rotas para a API

#### 📌 Array temporário para armazenar usuários:
```js
let usuarios = [
  { id: 1, nome: 'Alice' },
  { id: 2, nome: 'Bob' }
];
```

#### 🔵 **Rota GET** – Buscar todos os usuários:
```js
app.get('/usuarios', (req, res) => {
  res.json(usuarios);
});
```

#### 🟢 **Rota POST** – Criar um novo usuário:
```js
app.post('/usuarios', (req, res) => {
  const novoUsuario = { id: usuarios.length + 1, nome: req.body.nome };
  usuarios.push(novoUsuario);
  res.status(201).json(novoUsuario);
});
```

#### 🟡 **Rota PUT** – Atualizar um usuário:
```js
app.put('/usuarios/:id', (req, res) => {
  const id = parseInt(req.params.id);
  const usuario = usuarios.find(u => u.id === id);
  
  if (!usuario) return res.status(404).json({ erro: 'Usuário não encontrado' });

  usuario.nome = req.body.nome;
  res.json(usuario);
});
```

#### 🔴 **Rota DELETE** – Remover um usuário:
```js
app.delete('/usuarios/:id', (req, res) => {
  const id = parseInt(req.params.id);
  usuarios = usuarios.filter(u => u.id !== id);
  res.status(204).send(); // Sem conteúdo
});
```

Agora temos um **CRUD funcional com Express!** 🚀

---

## 🛠️ Middlewares no Express
Os **middlewares** são funções que processam as requisições **antes de chegarem às rotas**.

### 🔹 **Exemplo de middleware para logs**
```js
app.use((req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
  next(); // Continua para a próxima função
});
```

Agora, **todas as requisições serão registradas no console**.

---

## 🔒 Lidando com CORS
Se o front-end estiver em um domínio diferente, o navegador pode bloquear requisições. Para resolver isso, usamos o **CORS**:

```js
const cors = require('cors');
app.use(cors());
```

Se quiser **permitir apenas um domínio específico**, faça assim:

```js
app.use(cors({ origin: 'http://meusite.com' }));
```

---

## ⚡ Servindo Arquivos Estáticos (HTML, CSS, JS)
Se quiser servir páginas HTML, CSS e JavaScript direto pelo Express, coloque os arquivos numa pasta e adicione:

```js
app.use(express.static('public'));
```

Agora, qualquer arquivo dentro da pasta **`public/`** pode ser acessado via navegador.

---

## 🗄️ Conectando com um Banco de Dados
O Express pode ser integrado com **bancos SQL ou NoSQL**.

### 🔹 **Com MySQL (via Sequelize)**
```js
const { Sequelize } = require('sequelize');

const sequelize = new Sequelize('meubanco', 'usuario', 'senha', {
  host: 'localhost',
  dialect: 'mysql'
});

sequelize.authenticate()
  .then(() => console.log('✅ Conectado ao MySQL'))
  .catch(err => console.error('❌ Erro ao conectar:', err));
```

### 🔹 **Com MongoDB (via Mongoose)**
```js
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/meubanco', {
  useNewUrlParser: true,
  useUnifiedTopology: true
})
  .then(() => console.log('✅ Conectado ao MongoDB'))
  .catch(err => console.error('❌ Erro ao conectar:', err));
```

---

## 📜 Lidando com Erros no Express
Uma boa prática é criar um **middleware de erro** para capturar exceções e evitar travamentos no servidor.

```js
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ erro: 'Algo deu errado!' });
});
```

Agora, qualquer erro será tratado e retornado ao cliente.

---

## 🎯 Resumo Final
O **Express.js** é o melhor amigo dos desenvolvedores **Node.js** quando o assunto é construir APIs e servidores web.

### ✅ O que aprendemos:
✔ Criar um **servidor Express**  
✔ Criar uma **API REST completa (CRUD)**  
✔ Usar **middlewares** para logs e tratamento de erros  
✔ Permitir requisições de outros domínios com **CORS**  
✔ Servir arquivos estáticos (HTML, CSS, JS)  
✔ Conectar a **bancos SQL e NoSQL**  
✔ Melhorar segurança e performance  

Agora é só testar no seu projeto! 🚀🔥