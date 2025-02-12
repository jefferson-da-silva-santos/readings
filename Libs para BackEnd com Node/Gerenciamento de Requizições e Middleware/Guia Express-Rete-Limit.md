# 🚀 Tudo sobre **express-rate-limit** – Protegendo APIs contra Ataques  

Se você tem uma **API em Express**, uma das preocupações é **segurança**. Ataques como **DDoS (negação de serviço)** ou **brute-force (tentativa de senha)** podem sobrecarregar seu servidor.  

A solução? **express-rate-limit**, um middleware que **limita o número de requisições** de um IP em um determinado período.  

Aqui, vou te mostrar **como instalar, configurar e usar no seu projeto Express**! 🔥  

---

## 🛡️ O que é o **express-rate-limit**?
O **express-rate-limit** é um middleware para **Express.js** que ajuda a prevenir abuso e ataques definindo um **limite de requisições**.  

✅ **Evita ataques DDoS e brute-force**  
✅ **Melhora a segurança da API**  
✅ **Evita sobrecarga do servidor**  
✅ **Personalizável (mensagens, tempo, IPs bloqueados, etc.)**  
✅ **Fácil de integrar com Redis para escalabilidade**  

Se sua API tem **rotas sensíveis** (como login, cadastro ou pagamentos), você precisa usar isso! 🛑  

---

## 📦 Instalando o express-rate-limit
Para adicionar ao seu projeto Express, basta rodar:  

```bash
npm install express-rate-limit
```
ou, se estiver usando **yarn**:  
```bash
yarn add express-rate-limit
```

Depois, basta importar no seu código:

```js
const rateLimit = require('express-rate-limit');
```

---

## 🔥 Como usar **express-rate-limit** no Express
Agora, bora configurar um **limitador de requisições** básico:

```js
const express = require('express');
const rateLimit = require('express-rate-limit');

const app = express();

// Criando um limitador de 100 requisições por IP a cada 15 minutos
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutos
  max: 100, // Máximo de 100 requisições por IP
  message: '⛔ Limite de requisições excedido. Tente novamente mais tarde.',
  headers: true, // Mostra nos headers o tempo restante para resetar o limite
});

// Aplicando o limitador globalmente em todas as rotas
app.use(limiter);

app.get('/', (req, res) => {
  res.send('🚀 API rodando normalmente!');
});

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

Agora, **se um IP fizer mais de 100 requisições em 15 minutos**, ele verá essa resposta:  

```json
{
  "message": "⛔ Limite de requisições excedido. Tente novamente mais tarde."
}
```

---

## 🔵 Aplicando o rate-limit **apenas em rotas específicas**
Se quiser aplicar **apenas em uma rota**, como `/login`, faça assim:

```js
const loginLimiter = rateLimit({
  windowMs: 10 * 60 * 1000, // 10 minutos
  max: 5, // Máximo de 5 tentativas
  message: '⛔ Muitas tentativas de login. Tente novamente em 10 minutos.'
});

app.post('/login', loginLimiter, (req, res) => {
  res.send('🛡️ Tentando logar...');
});
```

Isso impede **ataques de brute-force**, onde alguém tenta descobrir senhas testando várias combinações. 🔐  

---

## ⚡ Personalizando as mensagens e ações
Podemos **personalizar a resposta quando um IP for bloqueado**.  

```js
const customLimiter = rateLimit({
  windowMs: 5 * 60 * 1000, // 5 minutos
  max: 3, // Máximo de 3 requisições
  handler: (req, res, next) => {
    res.status(429).json({
      error: '⛔ Você fez muitas requisições. Espere 5 minutos!',
      retryAfter: Math.ceil(req.rateLimit.resetTime / 1000) + ' segundos'
    });
  }
});

app.get('/custom', customLimiter, (req, res) => {
  res.send('🛡️ Esta é uma rota protegida.');
});
```

Se o usuário ultrapassar o limite, ele recebe:

```json
{
  "error": "⛔ Você fez muitas requisições. Espere 5 minutos!",
  "retryAfter": "300 segundos"
}
```

---

## 🏎️ Aumentando escalabilidade com **Redis**
Se sua API roda em **várias instâncias** (como em containers ou Kubernetes), usar **Redis** para armazenar os limites de requisição é uma boa ideia.

Para isso, use o `rate-limit-redis`:

```bash
npm install ioredis rate-limit-redis
```

E configure assim:

```js
const RedisStore = require('rate-limit-redis');
const Redis = require('ioredis');

const redisClient = new Redis({
  host: 'localhost',
  port: 6379
});

const limiterRedis = rateLimit({
  store: new RedisStore({ client: redisClient }),
  windowMs: 15 * 60 * 1000,
  max: 100,
  message: '⛔ Muitas requisições. Espere um pouco!'
});

app.use(limiterRedis);
```

Agora, os limites são armazenados no **Redis**, funcionando corretamente mesmo em várias instâncias da API.

---

## 🎛️ Configurações avançadas
### 1️⃣ **Diferenciar limite por tipo de usuário**
Se quiser **definir limites diferentes para usuários autenticados e anônimos**, pode fazer assim:

```js
const userLimiter = (req, res, next) => {
  if (req.user && req.user.isAdmin) {
    return next(); // Admins não têm limite
  }

  return rateLimit({
    windowMs: 10 * 60 * 1000,
    max: 50 // Usuários comuns podem fazer 50 requisições
  })(req, res, next);
};

app.use('/api/usuarios', userLimiter);
```

Agora, **admins não têm limite** e usuários comuns podem fazer até **50 requisições**.

---

### 2️⃣ **Resetar o limite manualmente**
Se um usuário for **desbloqueado manualmente**, podemos resetar o limite dele chamando `req.rateLimit.resetKey(ip)`:

```js
app.post('/admin/reset-limit', (req, res) => {
  req.rateLimit.resetKey(req.body.ip);
  res.send('✅ Limite resetado para o IP ' + req.body.ip);
});
```

Isso é útil quando queremos liberar um usuário **antes do tempo acabar**.

---

## 🏁 Conclusão
O **express-rate-limit** é um middleware essencial para **segurança de APIs**, ajudando a evitar abuso, ataques DDoS e brute-force.

### 🎯 Resumo:
✅ **Impede ataques de força bruta** 🔐  
✅ **Protege APIs de sobrecarga e DDoS** 🛡️  
✅ **Personalizável (mensagens, tempo, IPs bloqueados)** 🛠️  
✅ **Pode usar Redis para escalabilidade** 🚀  

Se sua API lida com **login, cadastros ou qualquer recurso sensível**, **use o express-rate-limit** para garantir segurança! 🔥🚀