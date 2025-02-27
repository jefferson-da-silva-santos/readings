# 📜 Tudo sobre **Morgan** – Logger HTTP para Express e Node.js

Se você já trabalhou com **Node.js** e **Express**, sabe que **logar as requisições** é essencial para debugar erros, monitorar o tráfego e até mesmo entender como a API está sendo usada. O **Morgan** é a ferramenta perfeita para isso!

Aqui vou te mostrar **o que é, como instalar, configurar e personalizar logs HTTP com o Morgan**.

---

## 🚀 O que é o **Morgan**?
O **Morgan** é um **logger de requisições HTTP para Node.js** que funciona como um **middleware do Express**. Ele registra **detalhes das requisições**, como:

✅ Método da requisição (`GET`, `POST`, etc.)  
✅ URL acessada  
✅ Código de resposta (`200`, `404`, `500`, etc.)  
✅ Tempo de resposta do servidor  
✅ Endereço IP do cliente  

Isso ajuda a entender **quem está acessando sua API e como ela está respondendo**.

---

## 📦 Instalando o Morgan
Para usar no seu projeto **Node.js + Express**, instale com:

```bash
npm install morgan
```
ou
```bash
yarn add morgan
```

Depois, basta importar no seu código:

```js
const morgan = require('morgan');
```

---

## ⚙️ Como usar o Morgan no Express?
Para ativar os logs no seu servidor **Express**, basta adicionar o **Morgan como middleware**:

```js
const express = require('express');
const morgan = require('morgan');

const app = express();

// Ativando o Morgan no modo "dev"
app.use(morgan('dev'));

app.get('/', (req, res) => {
  res.send('Olá, mundo!');
});

app.listen(3000, () => {
  console.log('🔥 Servidor rodando na porta 3000');
});
```

Agora, sempre que alguém fizer uma requisição, você verá um log como este no terminal:

```
GET / 200 5.123 ms - 12
```

Isso significa:  
🔹 **GET** → Método da requisição  
🔹 **/** → Rota acessada  
🔹 **200** → Código de status da resposta  
🔹 **5.123 ms** → Tempo que o servidor levou para responder  
🔹 **12** → Tamanho da resposta (em bytes)  

---

## 📜 Modos de logging do Morgan
O Morgan tem **vários formatos de log**. Aqui estão os principais:

### 🟢 `dev` (Recomendado para desenvolvimento)
```js
app.use(morgan('dev'));
```
Loga as requisições de forma colorida e enxuta, incluindo **método, status e tempo de resposta**.

**Exemplo de saída:**
```
GET /api/users 200 5.123 ms - 34
POST /api/login 401 8.345 ms - 15
```

### 🔵 `combined` (Recomendado para produção)
```js
app.use(morgan('combined'));
```
Este formato segue o **padrão Apache/Nginx**, incluindo **IP do cliente, timestamp, método, status, user-agent, etc.**.

**Exemplo de saída:**
```
192.168.0.1 - - [10/Feb/2025:10:00:00 +0000] "GET /api/users HTTP/1.1" 200 34 "-" "Mozilla/5.0"
```

### 🟡 `common` (Parecido com `combined`, mas sem user-agent)
```js
app.use(morgan('common'));
```
**Exemplo de saída:**
```
192.168.0.1 - - [10/Feb/2025:10:00:00 +0000] "GET /api/users HTTP/1.1" 200 34
```

### 🟠 `short` (Formato reduzido)
```js
app.use(morgan('short'));
```
**Exemplo de saída:**
```
GET /api/users 200 5.123 ms
```

### 🔴 `tiny` (Mínimo possível)
```js
app.use(morgan('tiny'));
```
**Exemplo de saída:**
```
GET /api/users 200 - 5.123 ms
```

---

## 📂 Salvando logs em um arquivo
Se você quiser salvar os logs em um **arquivo**, pode usar o `fs.createWriteStream`:

```js
const fs = require('fs');
const path = require('path');

const accessLogStream = fs.createWriteStream(path.join(__dirname, 'access.log'), { flags: 'a' });

app.use(morgan('combined', { stream: accessLogStream }));
```

Agora, todas as requisições serão **salvas no arquivo `access.log`**, e você pode analisar depois.

---

## 🎯 Criando logs **personalizados**
Se precisar personalizar o formato dos logs, pode criar um **template customizado**:

```js
morgan.token('corpo', (req) => JSON.stringify(req.body));

app.use(morgan(':method :url :status :response-time ms - :corpo'));
```

**Exemplo de saída:**
```
POST /api/login 401 10.123 ms - {"email":"teste@example.com","senha":"123456"}
```

Isso ajuda a **registrar o corpo da requisição**, útil para debug.

---

## 🔥 Filtrando logs apenas para determinadas rotas
Se você quiser logar **somente certas rotas**, pode fazer assim:

```js
app.use('/api', morgan('dev')); // Apenas para rotas que começam com /api
```

Ou para **ignorar logs de arquivos estáticos**:
```js
app.use(morgan('dev', { skip: (req) => req.url.includes('.css') || req.url.includes('.js') }));
```

---

## 🔄 Usando Morgan com Winston (Logs mais avançados)
Se quiser **logs mais robustos**, pode combinar o Morgan com o **Winston**, que permite salvar logs no banco, enviar para monitoramento, etc.

### 🔹 Instalar Winston:
```bash
npm install winston
```

### 🔹 Configurar integração:
```js
const winston = require('winston');

const logger = winston.createLogger({
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({ filename: 'logs.log' })
  ]
});

// Usando Morgan para logar com Winston
app.use(morgan('combined', {
  stream: { write: (message) => logger.info(message.trim()) }
}));
```

Agora, os logs aparecem **no console e no arquivo logs.log**!

---

## 🏁 Conclusão
O **Morgan** é essencial para **monitoramento e debug** no Express. Ele te dá **visibilidade sobre as requisições** e pode ser integrado com **arquivos de log ou serviços externos**.

### 🎯 Resumo:
✅ **Registra requisições automaticamente**  
✅ **Vários formatos de log (`dev`, `combined`, `tiny`, etc.)**  
✅ **Pode salvar logs em arquivos**  
✅ **Logs customizáveis**  
✅ **Integração com Winston para logs avançados**  

Agora é só testar e **monitorar seu servidor Express** com Morgan! 🚀🔥