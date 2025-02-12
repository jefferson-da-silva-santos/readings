# 🐢 Tudo sobre **express-slow-down** – Protegendo sua API contra abuso e DDoS  

Se você tem uma API pública no **Express**, sabe que tem sempre aquele espertinho que **tenta fazer milhares de requisições por segundo**. O **express-slow-down** ajuda a **diminuir a velocidade** das requisições quando um usuário faz muitas em um curto período de tempo.

Aqui eu vou te mostrar **o que é, como instalar, configurar e boas práticas** para usar o `express-slow-down` no seu projeto.

---

## 🚀 O que é o **express-slow-down**?
O `express-slow-down` é um **middleware** para Express que funciona como um **rate limiter "suave"**. Em vez de **bloquear o usuário** quando ele faz muitas requisições (como o `express-rate-limit` faz), ele **aumenta o tempo de resposta** gradativamente. Isso pode ser útil para:

✅ **Evitar ataques DDoS**  
✅ **Proteger sua API contra bots e scraping**  
✅ **Desestimular spam sem bloquear o usuário imediatamente**  
✅ **Melhorar a experiência, permitindo requisições esporádicas**  

🔹 **Qual a diferença entre `express-slow-down` e `express-rate-limit`?**  
- **express-rate-limit** → Bloqueia requisições quando o limite é atingido  
- **express-slow-down** → Apenas **diminui a velocidade de resposta** das requisições  

Se quiser ser menos agressivo no bloqueio, o **express-slow-down** é uma ótima escolha.

---

## 📦 Instalando o express-slow-down
Para instalar no seu projeto **Express**, basta rodar:

```bash
npm install express-slow-down
```
ou
```bash
yarn add express-slow-down
```

Depois, importe no seu código:

```js
const slowDown = require('express-slow-down');
```

---

## ⚙️ Como configurar o **express-slow-down**
Vamos adicionar o middleware para **limitar o número de requisições**.

### 🟢 Exemplo básico:
```js
const express = require('express');
const slowDown = require('express-slow-down');

const app = express();

// Configurando o slow-down
const speedLimiter = slowDown({
  windowMs: 1 * 60 * 1000, // 1 minuto
  delayAfter: 5, // Após 5 requisições...
  delayMs: 500 // Adiciona 500ms de delay para cada requisição extra
});

app.use('/api', speedLimiter);

app.get('/api/teste', (req, res) => {
  res.send('Requisição recebida!');
});

app.listen(3000, () => {
  console.log('🔥 Servidor rodando na porta 3000');
});
```

Agora, se um usuário fizer **mais de 5 requisições por minuto**, ele **ainda pode continuar acessando**, mas cada nova requisição **ficará mais lenta**.

---

## 🔥 Como funciona o `express-slow-down`?
### 🔹 Configuração explicada:
```js
const speedLimiter = slowDown({
  windowMs: 1 * 60 * 1000, // Período de tempo (1 minuto)
  delayAfter: 5, // Número de requisições antes de começar a atrasar
  delayMs: 500 // Atraso em milissegundos para cada requisição extra
});
```

### 🕒 O que acontece na prática?
- **Primeiras 5 requisições** → São processadas normalmente  
- **A partir da 6ª requisição** → Começa a atrasar **500ms**  
- **Na 7ª requisição** → O atraso aumenta para **1000ms**  
- **Na 8ª requisição** → O atraso sobe para **1500ms**  
- ... e assim por diante  

Isso desestimula **bots e ataques**, porque eles começam a esperar cada vez mais tempo.

---

## 🚀 Aplicando `express-slow-down` em rotas específicas
Se quiser aplicar o **slow down apenas em algumas rotas**, pode fazer assim:

```js
app.get('/api/protegida', speedLimiter, (req, res) => {
  res.send('Esta rota está protegida contra abuso!');
});
```

Agora, apenas a rota `/api/protegida` será afetada pelo limite.

---

## 🔗 Usando com `express-rate-limit`
Você pode combinar `express-slow-down` com `express-rate-limit` para criar um sistema **completo** de proteção.

### 🔹 Instalando o rate limit:
```bash
npm install express-rate-limit
```

### 🔹 Configurando juntos:
```js
const rateLimit = require('express-rate-limit');

const speedLimiter = slowDown({
  windowMs: 1 * 60 * 1000,
  delayAfter: 5,
  delayMs: 500
});

const limiter = rateLimit({
  windowMs: 1 * 60 * 1000,
  max: 10, // Bloqueia após 10 requisições
  message: '❌ Você excedeu o limite de requisições!'
});

app.use('/api', speedLimiter, limiter);
```

Agora:
✅ O usuário pode fazer até **5 requisições sem delay**  
✅ Depois, o tempo de resposta **fica mais lento**  
✅ Se ele chegar em **10 requisições no minuto**, **é bloqueado**  

Isso cria um **sistema de proteção mais robusto** contra abuso.

---

## 📜 Personalizando mensagens de erro
Se quiser retornar uma **mensagem personalizada** quando uma requisição é atrasada:

```js
const speedLimiter = slowDown({
  windowMs: 1 * 60 * 1000,
  delayAfter: 5,
  delayMs: 500,
  handler: (req, res, next) => {
    res.status(429).json({
      message: '⚠️ Você está fazendo muitas requisições! Tente novamente mais tarde.',
      delay: req.slowDown.currentDelay // Mostra o tempo de delay
    });
  }
});
```

Agora, quando o usuário exceder o limite, ele recebe:
```json
{
  "message": "⚠️ Você está fazendo muitas requisições! Tente novamente mais tarde.",
  "delay": 1000
}
```

Isso melhora a **experiência do usuário**, avisando que ele deve esperar.

---

## 🛠 Aplicando limites diferentes para rotas específicas
Você pode aplicar **limites diferentes para cada rota**, dependendo da importância delas.

```js
const loginLimiter = slowDown({
  windowMs: 5 * 60 * 1000, // 5 minutos
  delayAfter: 3, // Após 3 tentativas...
  delayMs: 1000 // Adiciona 1 segundo de delay
});

const apiLimiter = slowDown({
  windowMs: 1 * 60 * 1000, // 1 minuto
  delayAfter: 10, // Após 10 requisições...
  delayMs: 500 // Adiciona 500ms de delay
});

app.post('/login', loginLimiter, (req, res) => {
  res.send('Tentativa de login recebida.');
});

app.use('/api', apiLimiter);
```

Agora:
✅ O **login** começa a demorar **após 3 tentativas**  
✅ A **API** começa a demorar **após 10 requisições**  

Isso é ótimo para **proteger endpoints sensíveis como login**.

---

## 🏁 Conclusão
O **express-slow-down** é uma ferramenta **poderosa** para evitar **abuso e ataques DDoS leves**, sem bloquear usuários imediatamente.

### 🎯 Resumo:
✅ **Atraso progressivo** em requisições excessivas  
✅ **Mais amigável que um bloqueio total**  
✅ **Configuração simples e flexível**  
✅ **Pode ser combinado com `express-rate-limit` para máxima proteção**  
✅ **Personalização de mensagens e tempo de delay**  

Agora é só testar no seu servidor Express e **proteger sua API contra abuso**! 🚀🐢