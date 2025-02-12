# 🚀 Tudo sobre **Pino** – O Logger Super Rápido para Node.js  

Se você está desenvolvendo uma aplicação **Node.js** e quer um **sistema de logs eficiente, rápido e flexível**, **Pino** é a solução perfeita! Ele é um **logger de alta performance**, muito mais rápido do que `console.log()` e até do que bibliotecas como **Winston**.

Aqui vou te mostrar **o que é, como instalar, configurar e usar o Pino no seu projeto**. Bora lá! 🔥  

---

## 🚀 O que é o **Pino**?  

O **Pino** é um **logger JSON** para **Node.js**, focado em **performance e baixo consumo de recursos**.  

🔹 **Por que usar Pino em vez de console.log()?**  

✅ **Super rápido** – Mais de **5x mais rápido** que `console.log()` 🚀  
✅ **Baixo consumo de CPU e memória**  
✅ **Formato JSON estruturado** – Melhor para logs em produção  
✅ **Suporte a streams** – Pode salvar logs em arquivos ou enviar para sistemas externos  
✅ **Níveis de log** – Permite configurar logs como `info`, `warn`, `error`, etc.  
✅ **Extensível** – Suporte para plugins e integração com ferramentas como **ElasticSearch, Loki e Datadog**  

Se sua aplicação Node.js precisa de **logs eficientes e bem organizados**, o **Pino** é a melhor escolha! 🎯  

---

## 📦 Instalando o Pino  

Para instalar no seu projeto, basta rodar:  

```bash
npm install pino
```
ou, se estiver usando **yarn**:  
```bash
yarn add pino
```

Agora, bora configurar o logger. 🚀  

---

## 🔥 Criando um Logger Básico com Pino  

A configuração mais simples do Pino é assim:  

```js
const pino = require('pino');

// Criando um logger básico
const logger = pino();

logger.info('🚀 Aplicação iniciada!');
logger.warn('⚠️ Cuidado, algo pode estar errado!');
logger.error('❌ Erro fatal no sistema!');
```

Quando você executa o código (`node app.js`), ele gera uma saída **JSON estruturada**, como esta:  

```json
{"level":30,"time":1700000000000,"pid":1234,"hostname":"meu-servidor","msg":"🚀 Aplicação iniciada!"}
```

💡 **Isso facilita a análise automática dos logs em servidores e sistemas de monitoramento.**  

---

## 🎛️ Configurando **Níveis de Log**  

O **Pino** suporta diferentes níveis de log para melhor organização:  

| Nível  | Código | Descrição |
|--------|--------|------------|
| `trace` | 10  | Log de depuração detalhado |
| `debug` | 20  | Logs para desenvolvedores |
| `info`  | 30  | Informações gerais (padrão) |
| `warn`  | 40  | Alertas de possíveis problemas |
| `error` | 50  | Erros que precisam de atenção |
| `fatal` | 60  | Erros críticos que derrubam o app |

Para definir um nível mínimo de log:  

```js
const logger = pino({ level: 'warn' });

logger.info('🔵 Isso não será exibido'); // Ignorado (nível info < warn)
logger.warn('⚠️ Isso será logado');
logger.error('❌ Erro detectado');
```

Agora, apenas logs **warn, error e fatal** serão exibidos.  

---

## 📁 Salvando Logs em Arquivo  

Por padrão, o **Pino** imprime os logs no console, mas podemos redirecionar para um arquivo:  

```bash
node app.js | pino-pretty > logs.txt
```

Agora, os logs ficam salvos no arquivo `logs.txt`.  

Se quiser salvar diretamente no código:  

```js
const fs = require('fs');
const stream = fs.createWriteStream('./logs.txt', { flags: 'a' });

const logger = pino({}, stream);

logger.info('✅ Log salvo no arquivo!');
```

Agora, os logs são **armazenados diretamente** no arquivo **logs.txt**! 📂  

---

## 🎨 Formatando Logs para Leitura  

Por padrão, os logs do **Pino** são formatados em JSON. Se quiser algo mais legível para humanos, use o **pino-pretty**:

### 📌 Instalando o **pino-pretty**  
```bash
npm install pino-pretty
```

Agora, rode seu app formatando a saída:  

```bash
node app.js | pino-pretty
```

Isso deixa os logs mais bonitos:  

```plaintext
[12:34:56] INFO 🚀 Aplicação iniciada!
[12:34:57] WARN ⚠️ Cuidado, algo pode estar errado!
[12:34:58] ERROR ❌ Erro fatal no sistema!
```

Se quiser configurar diretamente no código:

```js
const logger = pino({
  transport: {
    target: 'pino-pretty'
  }
});

logger.info('📝 Log formatado e bonito!');
```

Agora os logs aparecem coloridos no terminal! 🌈  

---

## ⚡ Integrando o Pino com **Express**  

Se sua aplicação usa **Express**, você pode integrar o **Pino** para logar requisições automaticamente.  

### 📌 Instale o middleware do Pino  
```bash
npm install pino-http
```

### 🔹 Adicione o middleware no Express  
```js
const express = require('express');
const pino = require('pino');
const pinoHttp = require('pino-http');

const app = express();
const logger = pino({ level: 'info' });

// Middleware para logar todas as requisições
app.use(pinoHttp({ logger }));

app.get('/', (req, res) => {
  req.log.info('Rota / acessada');
  res.send('🚀 Fast API with Pino!');
});

app.listen(3000, () => logger.info('🔥 Servidor rodando na porta 3000'));
```

Agora, toda requisição será **logada automaticamente** no formato JSON! 📡  

---

## 🔗 Enviando Logs para Monitoramento (Elastic, Loki, Datadog)  

Se você quiser **enviar logs para um sistema de monitoramento**, pode integrar o **Pino** com serviços como:

- **ElasticSearch** → Para armazenar logs e visualizar no Kibana  
- **Grafana Loki** → Para logs em tempo real  
- **Datadog** → Para análise de logs e métricas  

### 📌 Enviando logs para um serviço externo via HTTP  
```js
const stream = require('stream');
const axios = require('axios');

const transport = new stream.Writable({
  write(chunk, encoding, callback) {
    axios.post('https://meu-monitoramento.com/logs', chunk.toString())
      .catch(err => console.error('Erro ao enviar log:', err));
    callback();
  }
});

const logger = pino({}, transport);

logger.info('🔗 Log enviado para um serviço externo!');
```

Isso manda todos os logs diretamente para um **servidor de monitoramento**! 🚀  

---

## 🏁 Conclusão  

O **Pino** é um dos loggers **mais rápidos e eficientes** para **Node.js**. Ele é **ideal para aplicações que precisam de logs estruturados, rápido processamento e integração com ferramentas de monitoramento**.  

### 🎯 Resumo:  
✅ **Mais rápido que console.log() e Winston**  
✅ **Logs JSON estruturados**  
✅ **Salvamento automático em arquivos**  
✅ **Integração com Express**  
✅ **Compatível com ElasticSearch, Loki, Datadog e mais**  

Se você quer **logs rápidos, organizados e fáceis de analisar**, **Pino** é a solução perfeita! Agora é só testar no seu projeto! 🚀🔥