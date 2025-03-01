# 📌 Guia Definitivo sobre Middlewares no Express.js 🚀

Se você trabalha com **Node.js** e **Express.js**, já deve ter ouvido falar dos **middlewares**. Eles são um dos pilares do framework, permitindo manipular requisições e respostas de maneira modular e reutilizável. Mas você sabe realmente como usá-los de forma eficiente? 🤔

Neste guia, vamos nos aprofundar nos **middlewares**, abordando:
- O que são e como funcionam.
- Tipos de middlewares.
- Como criar e utilizar middlewares personalizados.
- Estratégias para reaproveitamento e organização.
- Melhores práticas e otimizações.
- Tratamento de erros avançado.
- Middlewares assíncronos e promessas.

Então, bora mergulhar no mundo dos **middlewares no Express**! 🚀

---

## 🎯 O que são Middlewares no Express.js?

Middlewares são **funções intermediárias** executadas no ciclo de requisição-resposta do Express. Eles podem **modificar a requisição ou a resposta**, **encaminhar para o próximo middleware** ou até mesmo **interromper a execução**.

### 🔄 Como funciona um middleware?

Todo middleware recebe três parâmetros:

```javascript
(req, res, next) => {
  // Código do middleware
  next(); // Chama o próximo middleware
}
```

- `req` → Objeto da requisição.
- `res` → Objeto da resposta.
- `next` → Função que chama o próximo middleware na fila.

**Exemplo básico:**

```javascript
app.use((req, res, next) => {
  console.log('Middleware executado!');
  next();
});
```

Este middleware será executado em **todas** as requisições.

---

## 🔍 Tipos de Middlewares

O Express permite diferentes tipos de middlewares:

### 1️⃣ **Middlewares Globais (Aplicação Inteira)**
Executados em **todas** as requisições da aplicação.

```javascript
app.use((req, res, next) => {
  console.log(`Requisição recebida: ${req.method} ${req.url}`);
  next();
});
```

### 2️⃣ **Middlewares de Rota**
São aplicados a **rotas específicas**.

```javascript
const verificarAutenticacao = (req, res, next) => {
  if (!req.headers.authorization) {
    return res.status(401).json({ error: 'Acesso negado!' });
  }
  next();
};

app.get('/dados', verificarAutenticacao, (req, res) => {
  res.json({ mensagem: 'Acesso permitido!' });
});
```

### 3️⃣ **Middlewares Incorporados do Express**
O Express já traz middlewares úteis embutidos:

```javascript
app.use(express.json()); // Processa JSON no corpo da requisição
app.use(express.urlencoded({ extended: true })); // Processa formulários
app.use(express.static('public')); // Serve arquivos estáticos
```

### 4️⃣ **Middlewares de Terceiros**
Podemos instalar pacotes externos para funcionalidades específicas:

- `morgan` → Geração de logs detalhados.
- `cors` → Permite requisições entre domínios.
- `helmet` → Aumenta a segurança contra ataques.

```javascript
const morgan = require('morgan');
const cors = require('cors');

app.use(morgan('dev'));
app.use(cors());
```

---

## 🛠 Criando e Reutilizando Middlewares

### **Middleware de Log Personalizado**

```javascript
const logger = (req, res, next) => {
  console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
  next();
};

module.exports = logger;
```

Uso:

```javascript
const logger = require('./middlewares/logger');
app.use(logger);
```

### **Middleware de Autenticação Simples**

```javascript
const autenticar = (req, res, next) => {
  if (!req.headers.authorization) {
    return res.status(403).json({ error: 'Token não fornecido!' });
  }
  next();
};

app.use('/api', autenticar);
```

### **Middleware de Tratamento de Erros Avançado**

```javascript
app.use((err, req, res, next) => {
  console.error('Erro:', err.stack);
  res.status(err.status || 500).json({ error: err.message });
});
```

---

## ⚡ Middlewares Assíncronos e Promessas

Podemos usar `async/await` para middlewares assíncronos, evitando problemas com **promises não tratadas**.

```javascript
const buscarDados = async (req, res, next) => {
  try {
    const dados = await fetch('https://api.exemplo.com/dados');
    req.dados = await dados.json();
    next();
  } catch (error) {
    next(error);
  }
};

app.get('/dados', buscarDados, (req, res) => {
  res.json(req.dados);
});
```

---

## 📌 Melhores Práticas e Otimizações

✔ **Organize seus middlewares em arquivos separados**

```
📂 meu-projeto
 ┣ 📂 middlewares
 ┃ ┣ 📜 logger.js
 ┃ ┣ 📜 autenticacao.js
 ┃ ┗ 📜 erros.js
 ┣ 📂 routes
 ┃ ┣ 📜 usuarios.js
 ┃ ┗ 📜 produtos.js
 ┣ 📜 server.js
```

✔ **Evite middlewares desnecessários** para não comprometer a performance.
✔ **Use middlewares reutilizáveis** para modularizar a aplicação.
✔ **Combine middlewares** de forma eficiente, usando `app.use()` somente quando necessário.
✔ **Sempre trate erros** para evitar que exceções quebrem sua API.

---

## 🎉 Conclusão

Agora você domina **middlewares no Express.js**! 🏆

✔ **Entendeu como middlewares funcionam**.
✔ **Aprendeu diferentes tipos e quando usá-los**.
✔ **Criou middlewares reutilizáveis e organizados**.
✔ **Implementou middlewares assíncronos**.
✔ **Aplicou boas práticas para otimização**.

Os middlewares são essenciais para **segurança, organização e performance** da sua API. Agora é sua vez: **crie seus próprios middlewares e otimize suas aplicações Express!** 🚀

