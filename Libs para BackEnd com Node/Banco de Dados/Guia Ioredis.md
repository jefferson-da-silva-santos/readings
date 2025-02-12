# 🔥 Tudo sobre **ioredis** – Cliente Redis para Node.js

Se você precisa usar o **Redis** no seu projeto Node.js, o **ioredis** é uma das melhores bibliotecas disponíveis. Ele permite trabalhar com **cache, filas, sessões, contadores e muito mais**, de forma rápida e eficiente.

Aqui vou te mostrar **como instalar, configurar e usar o ioredis** para melhorar a performance da sua aplicação. 🚀

---

## 🚀 O que é o **ioredis**?

O **ioredis** é um cliente para conectar com o **Redis** no Node.js. Ele suporta:

✅ **Conexão com Redis local e remoto**  
✅ **Suporte a clusters Redis (Alta Disponibilidade)**  
✅ **Pub/Sub para comunicação entre serviços**  
✅ **Pipelines e Transactions para otimizar operações**  
✅ **Streams para processar eventos em tempo real**  

Se você quer **armazenar dados temporários, criar cache, gerenciar sessões ou processar filas**, o Redis + ioredis é uma excelente escolha! 🔥

---

## 📦 Instalando o **ioredis**

Antes de instalar, **garanta que você tem o Redis rodando**. Se não tiver, instale com:

```bash
sudo apt install redis-server  # Linux
brew install redis            # MacOS
```

Agora, instale o **ioredis** no projeto:

```bash
npm install ioredis
```

Ou, se estiver usando **yarn**:

```bash
yarn add ioredis
```

Depois, importe no código:

```js
const Redis = require('ioredis');
```

Agora bora conectar no Redis! 🚀

---

## 🔌 Conectando ao Redis

Para criar uma conexão simples com o Redis local:

```js
const redis = new Redis();

redis.on('connect', () => console.log('🔥 Conectado ao Redis!'));
redis.on('error', (err) => console.error('❌ Erro no Redis:', err));
```

Se o Redis estiver rodando em um servidor remoto, use:

```js
const redis = new Redis({
  host: 'seu-redis-server.com',
  port: 6379,
  password: 'sua-senha'
});
```

Agora podemos armazenar e recuperar dados! 🏆

---

## 📥 Salvando e Recuperando Dados

### 🔹 **SET** – Armazena um valor

```js
(async () => {
  await redis.set('chave', 'Meu valor');
  console.log('✅ Valor salvo no Redis!');
})();
```

### 🔹 **GET** – Recupera um valor

```js
(async () => {
  const valor = await redis.get('chave');
  console.log('📥 Valor recuperado:', valor);
})();
```

Agora o **valor está salvo no Redis** e pode ser recuperado a qualquer momento!

---

## ⏳ Definindo Expiração (TTL)

Se quiser que um valor **expire automaticamente**, use `EX`:

```js
(async () => {
  await redis.set('temp', 'Dado temporário', 'EX', 10); // Expira em 10 segundos
  console.log('✅ Valor com expiração salvo!');
})();
```

Agora, após **10 segundos**, o dado será apagado do Redis.

---

## 📊 Contadores no Redis

O Redis é ótimo para **contadores e métricas**. Podemos incrementar valores facilmente:

### 🔹 **INCR** – Aumenta um contador

```js
(async () => {
  const contador = await redis.incr('visitas');
  console.log('👀 Visitas:', contador);
})();
```

### 🔹 **DECR** – Diminui um contador

```js
(async () => {
  const contador = await redis.decr('visitas');
  console.log('👀 Visitas:', contador);
})();
```

Isso é muito útil para **limites de acesso, contagem de usuários e estatísticas**.

---

## 🔄 Trabalhando com Listas

O Redis suporta **listas** que funcionam como filas (FIFO ou LIFO).

### 🔹 **LPUSH** – Adiciona itens no início

```js
(async () => {
  await redis.lpush('fila', 'Item 1', 'Item 2');
  console.log('✅ Itens adicionados à fila!');
})();
```

### 🔹 **RPUSH** – Adiciona no final

```js
(async () => {
  await redis.rpush('fila', 'Item 3');
})();
```

### 🔹 **LPOP** – Remove do início

```js
(async () => {
  const item = await redis.lpop('fila');
  console.log('📤 Item removido da fila:', item);
})();
```

### 🔹 **LRANGE** – Lista todos os itens

```js
(async () => {
  const itens = await redis.lrange('fila', 0, -1);
  console.log('📜 Itens na fila:', itens);
})();
```

Isso é **perfeito para filas de processamento e mensagens assíncronas!** 🔄

---

## 🔥 Publicação e Assinatura (Pub/Sub)

O Redis permite **comunicação entre serviços** usando Pub/Sub. Vamos criar um **publicador** e um **assinante**.

### 📌 **Subscriber** (Assinante)

```js
const subscriber = new Redis();

subscriber.subscribe('canal', (err, count) => {
  console.log(`📡 Assinado no canal: ${count}`);
});

subscriber.on('message', (canal, mensagem) => {
  console.log(`📩 Mensagem recebida em ${canal}:`, mensagem);
});
```

### 📌 **Publisher** (Publicador)

```js
(async () => {
  const publisher = new Redis();
  await publisher.publish('canal', 'Olá, Redis!');
})();
```

Agora, toda vez que algo for **publicado no canal**, o assinante recebe a mensagem! 📨

Isso é **ótimo para comunicação entre microsserviços** e **notificações em tempo real**.

---

## 🔗 Trabalhando com Hashes

Hashes são ótimos para **armazenar objetos**, como usuários.

### 🔹 **HSET** – Define campos em um objeto

```js
(async () => {
  await redis.hset('usuario:1', 'nome', 'Alice', 'idade', 25);
  console.log('✅ Usuário salvo!');
})();
```

### 🔹 **HGET** – Obtém um campo

```js
(async () => {
  const nome = await redis.hget('usuario:1', 'nome');
  console.log('📛 Nome:', nome);
})();
```

### 🔹 **HGETALL** – Obtém todos os campos

```js
(async () => {
  const usuario = await redis.hgetall('usuario:1');
  console.log('👤 Dados do usuário:', usuario);
})();
```

Hashes são ideais para **armazenar perfis de usuários, cache de dados e sessões.** 💾

---

## 🔥 Trabalhando com Redis Cluster

Se seu projeto precisa de **alta disponibilidade e escalabilidade**, você pode usar o **Redis Cluster**.

```js
const cluster = new Redis.Cluster([
  { host: 'redis1.example.com', port: 6379 },
  { host: 'redis2.example.com', port: 6379 }
]);

cluster.on('connect', () => console.log('🔥 Conectado ao Redis Cluster!'));
```

Isso distribui os dados **entre vários servidores**, garantindo mais **performance e segurança**. 🚀

---

## 🏁 Conclusão

O **ioredis** é uma biblioteca poderosa para usar o **Redis** no Node.js. Ele permite:

✅ **Armazenar e recuperar dados rapidamente**  
✅ **Gerenciar contadores e cache**  
✅ **Trabalhar com listas e filas**  
✅ **Usar Pub/Sub para eventos em tempo real**  
✅ **Gerenciar sessões e perfis com Hashes**  
✅ **Escalar com Redis Cluster**  

Se você precisa de **performance e eficiência**, o Redis + ioredis é a solução! Agora é só testar no seu projeto! 🚀🔥