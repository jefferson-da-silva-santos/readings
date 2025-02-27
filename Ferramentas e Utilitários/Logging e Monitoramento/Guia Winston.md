# 📜 Tudo sobre **Winston** – Logging Profissional no Node.js  

Se você tem uma aplicação **Node.js**, já deve ter percebido que **console.log() não é suficiente** para logs em produção. O **Winston** resolve isso, oferecendo um sistema de **logging profissional, estruturado e configurável**.

Aqui vamos ver **o que é, como usar e configurar Winston no seu projeto, incluindo logs com timestamps, níveis de severidade, arquivos de log, JSON, cores no console e muito mais!** 🚀  

---

## 🔥 O que é o **Winston**?  
O **Winston** é uma biblioteca de logging para **Node.js**, usada para **gerenciar logs de forma profissional e eficiente**. Ele permite:  

✅ Criar logs com **níveis de severidade** (`info`, `warn`, `error`, etc.)  
✅ **Salvar logs em arquivos**, banco de dados ou serviços externos  
✅ **Formatar logs em JSON** para facilitar análise  
✅ Criar **logs coloridos no console** para depuração  
✅ **Agrupar logs por categoria**  
✅ **Gerenciar múltiplos transports** (saídas de log)  

Se você quer **monitorar sua aplicação**, **debugar problemas** ou simplesmente **ter um histórico de eventos**, o Winston é a melhor opção! 🔥

---

## 📦 Instalando o Winston
Para adicionar ao seu projeto, basta rodar:  

```bash
npm install winston
```
ou  
```bash
yarn add winston
```

Agora bora configurar e ver o **poder do Winston**! 🚀  

---

## 🟢 Criando um logger básico
Para começar, criamos um **logger simples** que escreve logs no console:

```js
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info', // Nível mínimo de log que será registrado
  transports: [
    new winston.transports.Console() // Mostra logs no console
  ]
});

// Testando logs
logger.info('Isso é um log de INFO');
logger.warn('Isso é um log de WARN');
logger.error('Isso é um log de ERROR');
```

📝 **Saída esperada no console:**
```
info: Isso é um log de INFO
warn: Isso é um log de WARN
error: Isso é um log de ERROR
```

Por padrão, o **nível de log mínimo é `info`**, ou seja, logs de nível `debug` não aparecerão.

---

## 🔵 Adicionando timestamps e cores nos logs
Vamos melhorar a formatação dos logs, adicionando **timestamps e cores**.

```js
const winston = require('winston');

const logger = winston.createLogger({
  level: 'debug', // Agora logs de debug aparecem também
  format: winston.format.combine(
    winston.format.timestamp({ format: 'YYYY-MM-DD HH:mm:ss' }), // Adiciona timestamp
    winston.format.colorize(), // Adiciona cores
    winston.format.printf(({ timestamp, level, message }) => {
      return `${timestamp} [${level}]: ${message}`;
    })
  ),
  transports: [new winston.transports.Console()]
});

// Testando logs
logger.debug('Debug: Informações detalhadas para desenvolvimento');
logger.info('Info: Algo normal aconteceu');
logger.warn('Warning: Isso pode ser um problema');
logger.error('Error: Algo deu errado');
```

📝 **Saída colorida no console:**
```
2025-02-11 15:30:00 [debug]: Debug: Informações detalhadas para desenvolvimento
2025-02-11 15:30:00 [info]: Info: Algo normal aconteceu
2025-02-11 15:30:00 [warn]: Warning: Isso pode ser um problema
2025-02-11 15:30:00 [error]: Error: Algo deu errado
```

Agora fica **muito mais fácil visualizar os logs**! 🎨

---

## 📂 Salvando logs em arquivos
Muitas vezes, queremos **registrar logs em arquivos**, para análise futura.

Vamos configurar o **Winston para gravar logs em um arquivo**:

```js
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json() // Salva logs em JSON
  ),
  transports: [
    new winston.transports.File({ filename: 'logs/error.log', level: 'error' }), // Apenas erros
    new winston.transports.File({ filename: 'logs/combined.log' }) // Todos os logs
  ]
});

// Testando logs
logger.info('Info: Isso vai para combined.log');
logger.error('Error: Isso vai para error.log E combined.log');
```

Agora temos dois arquivos:
- **`logs/error.log`** → Apenas logs de erro  
- **`logs/combined.log`** → Todos os logs (`info`, `warn`, `error`, etc.)  

**Dica:** Para **rodar o servidor sem exibir logs no console**, remova o `Console()` dos `transports`.

---

## 🏎️ Melhorando logs para ambiente de produção
Quando estamos em **produção**, queremos **logs em JSON**, para que sejam fáceis de processar e armazenar.

```js
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.json() // Formato JSON para produção
  ),
  transports: [
    new winston.transports.File({ filename: 'logs/production.log' })
  ]
});

// Exemplo de log estruturado
logger.info('Usuário logado', { userId: 123, role: 'admin' });
```

📝 **Saída no `logs/production.log`:**
```json
{
  "level": "info",
  "message": "Usuário logado",
  "userId": 123,
  "role": "admin",
  "timestamp": "2025-02-11T15:45:00.000Z"
}
```

Isso facilita a integração com **Sistemas de Monitoramento (Logstash, Grafana, AWS CloudWatch, etc.)**.

---

## 🏗️ Criando um Logger Global
Se você precisa usar **logs em múltiplos arquivos**, pode criar um **módulo de logging**:

📁 **logger.js**
```js
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.combine(
    winston.format.timestamp({ format: 'YYYY-MM-DD HH:mm:ss' }),
    winston.format.printf(({ timestamp, level, message }) => {
      return `${timestamp} [${level.toUpperCase()}]: ${message}`;
    })
  ),
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({ filename: 'logs/app.log' })
  ]
});

module.exports = logger;
```

Agora podemos **importar e usar o logger em qualquer arquivo**:

📁 **app.js**
```js
const logger = require('./logger');

logger.info('Servidor iniciado com sucesso');
logger.warn('Aviso: Uso alto de memória');
logger.error('Erro crítico: Conexão com banco falhou');
```

Isso facilita o uso de logs **em todo o projeto**!

---

## 🚀 Integrando Winston com o Express.js
Se estiver usando **Express.js**, pode adicionar Winston para **logar requisições automaticamente**:

```js
const express = require('express');
const logger = require('./logger');

const app = express();

app.use((req, res, next) => {
  logger.info(`${req.method} ${req.url}`);
  next();
});

app.get('/', (req, res) => {
  res.send('API funcionando!');
});

app.listen(3000, () => logger.info('Servidor rodando na porta 3000'));
```

Agora, **todas as requisições serão logadas** no Winston! 🔥

---

## 🎯 Conclusão
O **Winston** é um sistema de logging **poderoso e flexível**, perfeito para aplicações Node.js.

✅ **Logs coloridos no console** 🎨  
✅ **Salvamento em arquivos para produção** 📝  
✅ **Formato JSON para integração com sistemas externos** 📊  
✅ **Logger global reutilizável** 🔥  
✅ **Integração com Express.js**  

Se sua aplicação precisa de **logs estruturados e profissionais**, o **Winston** é a escolha certa! 🚀🔥