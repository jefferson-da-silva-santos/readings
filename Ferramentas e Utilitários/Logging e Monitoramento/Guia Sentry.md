# 🛑 Tudo sobre **Sentry** – Monitoramento de Erros no Node.js  

Se você já teve problemas tentando encontrar **erros em produção**, sabe que pode ser um pesadelo. 😩 O **Sentry** é a ferramenta perfeita para **monitorar erros em tempo real** no seu projeto **Node.js**, ajudando a entender **o que quebrou, quando quebrou e por que quebrou**.  

Aqui, vou te mostrar **como instalar, configurar e capturar erros com o Sentry** no seu projeto Express/Fastify. 🚀  

---

## 🚀 O que é o **Sentry**?  
O **Sentry** é um serviço de **monitoramento de erros e performance** para aplicações **backend e frontend**. Ele permite:  

✅ **Capturar exceções e erros automaticamente**  
✅ **Ver o stack trace (onde o erro aconteceu)**  
✅ **Receber alertas em tempo real (Slack, email, etc.)**  
✅ **Monitorar performance e gargalos no código**  
✅ **Filtrar e categorizar erros de acordo com gravidade**  

Se você quer **saber imediatamente quando algo quebra em produção**, o Sentry é essencial! 🔥  

---

## 📦 Criando uma conta no Sentry  

1️⃣ Acesse **[https://sentry.io](https://sentry.io/)** e crie uma conta gratuita.  
2️⃣ Crie um novo **projeto para Node.js**.  
3️⃣ Pegue sua **DSN (Data Source Name)** no painel do Sentry.  

Agora podemos instalar o SDK no Node.js! 🎯  

---

## 📦 Instalando o Sentry no Node.js  

Para instalar o SDK do Sentry no seu projeto, rode:  

```bash
npm install @sentry/node
```
ou, se estiver usando **yarn**:  
```bash
yarn add @sentry/node
```

Agora, bora configurar o monitoramento de erros! 🚀  

---

## 🔌 Configurando o Sentry no Express  

Se você usa **Express**, basta inicializar o **Sentry** como middleware:

```js
const express = require('express');
const Sentry = require('@sentry/node');

const app = express();

// 🔥 Inicializa o Sentry com sua DSN
Sentry.init({
  dsn: 'SEU_DSN_AQUI',
  tracesSampleRate: 1.0, // Coleta 100% das requisições (ajuste se necessário)
});

// 📌 Middleware para capturar requisições
app.use(Sentry.Handlers.requestHandler());
app.use(Sentry.Handlers.tracingHandler());

app.get('/', (req, res) => {
  res.send('🚀 API rodando com Sentry!');
});

// 💥 Rota que gera um erro proposital
app.get('/erro', (req, res) => {
  throw new Error('Algo deu muito errado! 💥');
});

// 🛑 Middleware para capturar erros e enviar para o Sentry
app.use(Sentry.Handlers.errorHandler());

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

Agora, quando acessar `http://localhost:3000/erro`, o erro será **automaticamente capturado pelo Sentry**, e você pode vê-lo no painel online.  

💡 **Dica:** O Sentry captura **todas as exceções e rejeições de Promises** automaticamente. 📡  

---

## ⚡ Capturando erros manualmente  

Se quiser **capturar um erro específico manualmente**, use `Sentry.captureException()`:

```js
app.get('/falha', (req, res) => {
  try {
    throw new Error('Erro manual no sistema!');
  } catch (error) {
    Sentry.captureException(error);
    res.status(500).send('😵 Algo deu errado, e estamos investigando!');
  }
});
```

Agora, sempre que essa rota for acessada, o erro será enviado para o Sentry.

---

## 🏎️ Configurando Sentry no **Fastify**  

Se você usa **Fastify**, a configuração é bem parecida:

```js
const fastify = require('fastify')();
const Sentry = require('@sentry/node');

Sentry.init({
  dsn: 'SEU_DSN_AQUI',
  tracesSampleRate: 1.0,
});

// 🔹 Capturando requisições automaticamente
fastify.addHook('onRequest', (req, res, done) => {
  Sentry.Handlers.requestHandler()(req, res, done);
});

fastify.get('/', async (req, res) => {
  return { message: '🚀 Fastify rodando com Sentry!' };
});

// 💥 Capturando erros automaticamente
fastify.setErrorHandler((error, req, res) => {
  Sentry.captureException(error);
  res.status(500).send({ error: 'Erro capturado pelo Sentry' });
});

fastify.listen({ port: 3000 }, () => console.log('🔥 Servidor Fastify rodando na porta 3000'));
```

Agora, qualquer erro no **Fastify** será automaticamente enviado ao **Sentry**.

---

## 📊 Monitorando **performance e tempos de resposta**  

O Sentry pode monitorar **tempos de resposta de requisições** para detectar lentidão na API.  

Para ativar isso, basta adicionar `tracesSampleRate: 1.0` na configuração:

```js
Sentry.init({
  dsn: 'SEU_DSN_AQUI',
  tracesSampleRate: 1.0, // Habilita monitoramento de performance
});
```

Agora, no painel do Sentry, você pode ver **quais rotas são mais lentas** e **quais requisições demoram mais**! 🔥  

---

## 🛠️ Capturando exceções globais  

Se você quiser capturar **qualquer erro não tratado**, pode usar:

```js
process.on('uncaughtException', (err) => {
  console.error('❌ Erro crítico não tratado:', err);
  Sentry.captureException(err);
});

process.on('unhandledRejection', (reason) => {
  console.error('❌ Rejeição de Promise não tratada:', reason);
  Sentry.captureException(reason);
});
```

Agora, mesmo **erros inesperados** serão enviados ao **Sentry**, garantindo que **nada passe despercebido**. 🔥

---

## 📩 Recebendo **notificações de erro** no Slack/Email  

No painel do Sentry, vá em:  
**Settings → Alerts → Configure Slack**  

Agora, **sempre que um erro novo aparecer**, você recebe um alerta no Slack! 📢  

Também pode configurar para **receber e-mails de erro críticos**. 📬  

---

## 🔄 Filtrando erros desnecessários  

Se quiser **ignorar alguns erros específicos**, adicione `ignoreErrors`:

```js
Sentry.init({
  dsn: 'SEU_DSN_AQUI',
  ignoreErrors: [
    'ValidationError', // Ignora erros de validação
    'NotFoundError' // Ignora erros 404
  ]
});
```

Isso evita **poluir o painel com erros irrelevantes**. 🧹  

---

## 🔥 Exemplo real: Capturando erro de banco de dados  

Imagine que temos uma conexão MySQL e queremos capturar falhas de banco:

```js
const mysql = require('mysql2');
const Sentry = require('@sentry/node');

Sentry.init({ dsn: 'SEU_DSN_AQUI' });

const conexao = mysql.createConnection({
  host: 'localhost',
  user: 'root',
  password: 'senha_errada', // Simulação de erro
  database: 'meubanco'
});

conexao.connect((erro) => {
  if (erro) {
    console.error('❌ Erro ao conectar no banco:', erro.message);
    Sentry.captureException(erro); // Envia para o Sentry
  }
});
```

Agora, **se a conexão falhar**, o erro será enviado **automaticamente para o Sentry**! 🚀  

---

## 🎯 Resumo Final  

O **Sentry** é uma ferramenta essencial para **monitoramento de erros** em **Node.js**, ajudando a detectar e resolver problemas rapidamente.

✅ **Captura erros em tempo real (crashes, falhas, etc.)**  
✅ **Stack trace completo para entender a causa do erro**  
✅ **Monitoramento de performance e latência da API**  
✅ **Integração com Slack e email para alertas**  
✅ **Filtro de erros irrelevantes (ex: 404)**  

Se você quer **dormir tranquilo sabendo que sua API está monitorada**, **use o Sentry**! 🔥🚀  

Agora é só testar no seu projeto! 🎯