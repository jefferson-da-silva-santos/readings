# 🛠️ Tudo sobre **dotenv** – Gerenciando Variáveis de Ambiente no Node.js  

Se você trabalha com **Node.js**, já deve ter visto código assim:  

```js
const API_KEY = 'meu-segredo-super-seguro';
```
O **problema**? ❌ Nunca devemos expor **segredos e credenciais** diretamente no código!  

A solução? **dotenv**!  

Com o **dotenv**, podemos armazenar variáveis sensíveis em um **arquivo `.env`** e carregá-las no código **sem riscos de vazamento**.  

---

## 🚀 O que é o **dotenv**?  
O **dotenv** é um pacote que **lê variáveis de ambiente** de um arquivo `.env` e as carrega no `process.env`.  

✅ **Esconde senhas e tokens** do código-fonte  
✅ **Facilita a configuração do ambiente (dev, staging, produção)**  
✅ **Mantém o código seguro e organizado**  
✅ **100% compatível com qualquer projeto Node.js**  

Agora, bora instalar e configurar! 🔥  

---

## 📦 Instalando o dotenv  

No seu projeto Node.js, instale o dotenv:  

```bash
npm install dotenv
```
ou, se estiver usando **yarn**:  
```bash
yarn add dotenv
```

Depois, crie um arquivo **`.env`** na raiz do projeto:  

```ini
PORT=3000
DB_HOST=localhost
DB_USER=root
DB_PASS=minhasenha123
API_KEY=meu-segredo-super-seguro
```

---

## 🔌 Configurando o dotenv no Node.js  

No seu arquivo principal (`server.js` ou `index.js`), importe o **dotenv** logo no início:  

```js
require('dotenv').config();

console.log('🔑 API_KEY:', process.env.API_KEY);
console.log('📡 Conectando ao banco:', process.env.DB_HOST);
```

Agora, quando rodamos:  

```bash
node server.js
```

A saída será:
```
🔑 API_KEY: meu-segredo-super-seguro
📡 Conectando ao banco: localhost
```

✅ **Agora suas credenciais estão protegidas e não aparecem no código-fonte!**  

---

## ⚡ Usando dotenv com Express  

Se você tem um servidor **Express**, pode carregar as variáveis no seu código:  

```js
require('dotenv').config();
const express = require('express');

const app = express();
const PORT = process.env.PORT || 3000; // Usa a porta do .env ou a padrão (3000)

app.get('/', (req, res) => {
  res.send('🚀 Servidor rodando na porta ' + PORT);
});

app.listen(PORT, () => console.log(`🔥 Servidor iniciado na porta ${PORT}`));
```

Agora, ao rodar `node server.js`, ele usa **a porta definida no `.env`**!  

---

## 🛑 Como garantir que o `.env` NÃO vá para o GitHub?  

Nunca devemos enviar o `.env` para o repositório! Para evitar isso, adicione ele no **`.gitignore`**:  

```ini
# Ignorar o arquivo de variáveis de ambiente
.env
```

Agora, mesmo que você use `git add .`, o **.env não será enviado** para o repositório. ✅  

---

## 🔄 Criando um `.env.example` para compartilhar configurações  

Se outras pessoas do time precisam das mesmas variáveis, crie um **`.env.example`** sem valores sensíveis:  

```ini
PORT=3000
DB_HOST=localhost
DB_USER=root
DB_PASS= # Defina sua senha aqui
API_KEY=
```

Isso ajuda outros devs a saberem quais variáveis precisam ser configuradas **sem expor segredos**.

---

## 📜 Carregando múltiplos arquivos `.env`  

Se você quer diferentes arquivos para **desenvolvimento e produção**, use o `dotenv-flow`:  

```bash
npm install dotenv-flow
```

E no código, importe:  

```js
require('dotenv-flow').config();
```

Agora, crie **`.env.development`** e **`.env.production`**.  

Quando rodamos:  
```bash
NODE_ENV=development node server.js
```
Ele carrega **`.env.development`** automaticamente. 🔥  

---

## ⚡ Usando dotenv no Knex.js, Sequelize e Mongoose  

Se você usa bancos de dados como **MySQL, PostgreSQL ou MongoDB**, pode carregar credenciais do `.env`.  

### 🔹 **Knex.js**
```js
require('dotenv').config();
const knex = require('knex')({
  client: 'mysql2',
  connection: {
    host: process.env.DB_HOST,
    user: process.env.DB_USER,
    password: process.env.DB_PASS,
    database: 'meubanco'
  }
});
```

### 🔹 **Sequelize**
```js
require('dotenv').config();
const { Sequelize } = require('sequelize');

const sequelize = new Sequelize(process.env.DB_NAME, process.env.DB_USER, process.env.DB_PASS, {
  host: process.env.DB_HOST,
  dialect: 'mysql'
});
```

### 🔹 **Mongoose (MongoDB)**
```js
require('dotenv').config();
const mongoose = require('mongoose');

mongoose.connect(process.env.MONGO_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true
});
```

Agora, todas as credenciais são **seguras e configuráveis** sem precisar alterar o código! 🔥  

---

## 🔥 Dicas avançadas do dotenv  

### 🟢 1. Definir variáveis padrão no código  
Se uma variável não estiver definida no `.env`, podemos definir um **valor padrão** no código:  

```js
const PORT = process.env.PORT || 3000;
```

Agora, se `PORT` não estiver no `.env`, ele assume **3000** como padrão. ✅  

---

### 🔵 2. Verificar se todas as variáveis estão carregadas  
Se quiser garantir que todas as variáveis estão definidas, pode criar uma função de validação:  

```js
const requiredEnv = ['DB_HOST', 'DB_USER', 'DB_PASS', 'API_KEY'];

requiredEnv.forEach((env) => {
  if (!process.env[env]) {
    console.error(`❌ ERRO: A variável ${env} não está definida no .env`);
    process.exit(1);
  }
});
```

Agora, se alguma variável estiver **faltando**, o servidor não inicia! 🔥  

---

### 🟡 3. Usando dotenv com ESModules (`import`)  

Se estiver usando **ESModules (import/export)** no Node.js, precisa importar o dotenv assim:  

```js
import 'dotenv/config';
```

Isso já carrega as variáveis de ambiente sem precisar de `require()`.  

---

## 🏁 Conclusão  

O **dotenv** é essencial para manter **segurança e organização** no Node.js.  

### 🎯 Resumo:  
✅ **Esconde senhas e chaves da API do código-fonte**  
✅ **Facilita configuração entre ambientes (dev/staging/prod)**  
✅ **Evita envio de credenciais para o GitHub**  
✅ **Funciona com qualquer projeto Node.js**  

Agora é só testar no seu projeto! 🚀🔥