# ⚡ Tudo sobre **Fastify** – O Framework Node.js Rápido e Leve  

Se você já mexeu com **Express**, sabe que ele é um dos frameworks mais populares para criar APIs no Node.js. Mas, se você precisa de **mais performance**, **melhor gerenciamento de requests** e uma API mais otimizada, **Fastify** é uma excelente opção! 🚀  

Aqui vou te mostrar **o que é, como instalar e usar, suas vantagens e diferenças para o Express**.  

---

## 🚀 O que é o **Fastify**?
O **Fastify** é um framework web para Node.js **focado em velocidade** e **baixo consumo de recursos**. Ele foi projetado para ser **mais rápido que o Express** e oferecer uma API otimizada para trabalhar com JSON.  

🔹 **Por que usar o Fastify em vez do Express?**  

✅ **Performance insana** – Até **4x mais rápido que o Express** 🚀  
✅ **Baixo consumo de memória** – Ideal para sistemas escaláveis  
✅ **Rotas assíncronas** – Melhor controle sobre requisições  
✅ **Validação de JSON nativa** – Sem precisar de bibliotecas extras  
✅ **Plugin-based** – Modularidade poderosa  
✅ **Compatível com Express Middleware** – Você pode migrar facilmente  

Se sua API precisa de **alta performance** e **baixo uso de CPU/memória**, o **Fastify** é uma excelente escolha! 💡  

---

## 📦 Instalando o Fastify
Para começar, instala ele no seu projeto:  

```bash
npm install fastify
```
ou, se estiver usando **yarn**:  
```bash
yarn add fastify
```

Depois, cria um arquivo `server.js` e importa ele:

```js
const fastify = require('fastify')();
```

Agora bora criar um servidor básico!  

---

## 🔥 Criando um servidor HTTP com Fastify
A forma mais simples de rodar um servidor com **Fastify** é assim:

```js
const fastify = require('fastify')({ logger: true });

fastify.get('/', async (request, reply) => {
  return { message: '🔥 Fastify rodando!' };
});

// Inicia o servidor
const start = async () => {
  try {
    await fastify.listen({ port: 3000 });
    console.log('🚀 Servidor rodando em http://localhost:3000');
  } catch (err) {
    fastify.log.error(err);
    process.exit(1);
  }
};

start();
```

Agora, executa o servidor:
```bash
node server.js
```

Acesse `http://localhost:3000` no navegador e veja a resposta:  

```json
{
  "message": "🔥 Fastify rodando!"
}
```

Simples assim! 😎

---

## 🔵 Criando Rotas no Fastify
Assim como no Express, o Fastify permite definir rotas para **GET, POST, PUT, DELETE**.  

### 🟢 Criando uma rota **GET**
```js
fastify.get('/api/usuarios', async (request, reply) => {
  return [
    { id: 1, nome: 'Alice' },
    { id: 2, nome: 'Bob' }
  ];
});
```

### 🔵 Criando uma rota **POST**
```js
fastify.post('/api/usuarios', async (request, reply) => {
  const { nome } = request.body;
  return { message: `Usuário ${nome} criado!` };
});
```

### 🟡 Criando uma rota **PUT**
```js
fastify.put('/api/usuarios/:id', async (request, reply) => {
  const { id } = request.params;
  const { nome } = request.body;
  return { message: `Usuário ${id} atualizado para ${nome}` };
});
```

### 🔴 Criando uma rota **DELETE**
```js
fastify.delete('/api/usuarios/:id', async (request, reply) => {
  const { id } = request.params;
  return { message: `Usuário ${id} deletado!` };
});
```

Essas rotas são **totalmente assíncronas**, garantindo **melhor performance**! 🚀  

---

## ⚡ Diferença entre **Fastify** e **Express**
| 🔥 Fastify  | 🏎️ Express  |
|------------|------------|
| **+ Rápido** (4x mais rápido) | Mais lento |
| **Menos consumo de memória** | Maior consumo de memória |
| **Validação de JSON embutida** | Precisa de `express-validator` |
| **Totalmente assíncrono** | Suporta sync e async |
| **Arquitetura modular** | Mais monolítico |

Se você precisa de **velocidade**, **eficiência** e **baixa latência**, **Fastify** é a melhor escolha. 🚀🔥  

---

## 📜 Trabalhando com **Request Params e Body**
Assim como no Express, podemos acessar **parâmetros da URL** e **corpo da requisição**.

### 🏷️ Pegando parâmetros da URL
```js
fastify.get('/api/usuarios/:id', async (request, reply) => {
  const { id } = request.params;
  return { message: `Usuário ${id} encontrado!` };
});
```

### 📝 Pegando dados do **corpo da requisição** (Body)
```js
fastify.post('/api/usuarios', async (request, reply) => {
  const { nome, email } = request.body;
  return { message: `Usuário ${nome} (${email}) criado!` };
});
```

---

## 🔐 Validação de Dados (Schema Validation)
Uma das melhores partes do Fastify é a **validação automática de JSON**! 🏆  

Em vez de usar bibliotecas externas (`express-validator`, `joi`), no Fastify você **define um schema diretamente na rota**:

```js
fastify.post('/api/usuarios', {
  schema: {
    body: {
      type: 'object',
      required: ['nome', 'email'],
      properties: {
        nome: { type: 'string' },
        email: { type: 'string', format: 'email' }
      }
    }
  }
}, async (request, reply) => {
  const { nome, email } = request.body;
  return { message: `Usuário ${nome} criado!` };
});
```

Se o usuário tentar enviar um JSON inválido, o Fastify **automaticamente retorna erro 400**:

```json
{
  "statusCode": 400,
  "error": "Bad Request",
  "message": "body should have required property 'nome'"
}
```

Isso **melhora a segurança** e reduz código desnecessário! 🔥

---

## 🛠 Criando Plugins (Middleware no Fastify)
No Fastify, **middlewares são chamados de Plugins**. Podemos criar um **plugin global** para logs, autenticação ou qualquer outro recurso.

### 🔹 Criando um plugin para log de requisições
```js
fastify.addHook('onRequest', async (request, reply) => {
  console.log(`📝 [${request.method}] ${request.url}`);
});
```

Agora, **toda requisição será logada** no terminal! 📄✅

---

## 🏁 Conclusão
O **Fastify** é um framework **super rápido e leve** para APIs no Node.js. Se você já usou Express, a adaptação é muito fácil!

### 🎯 Resumo:
✅ **Mais rápido e eficiente que o Express** 🚀  
✅ **Validação automática de JSON**  
✅ **Rotas assíncronas por padrão**  
✅ **Menor consumo de memória**  
✅ **Compatível com middleware do Express**  

Se você está buscando **alta performance** para APIs, **Fastify é a escolha certa**! Agora é só testar no seu projeto. 🚀🔥