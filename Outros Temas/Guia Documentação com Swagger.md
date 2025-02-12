# Guia Definitivo e Divertido do Swagger para Documentação de APIs em Node.js

Fala, dev! Se você já construiu uma API e precisou explicar pra alguém como ela funciona, provavelmente bateu aquela preguiça de escrever tudo na mão. E se te falar que tem um jeito de documentar a API automaticamente, bonitinho, interativo e sem dor de cabeça? Sim, é ele: o **Swagger**! Bora entender tudo sobre essa maravilha?

---

## 1. O que diabos é o Swagger?

Swagger é uma ferramenta poderosa para documentar APIs REST. Hoje em dia ele faz parte da OpenAPI Initiative, então o nome mais chique pra ele é **OpenAPI Specification (OAS)**. Mas no fim, a ideia é a mesma: gerar documentação bonita, interativa e fácil de entender.

### Por que usar?
- **Facilidade de documentação:** esquece escrever tudo na unha.
- **Interatividade:** dá pra testar os endpoints direto da documentação.
- **Padrão de mercado:** se você trabalha com APIs, cedo ou tarde vai se deparar com ele.
- **Geração automática de clientes:** quer um SDK em outra linguagem? Ele gera pra você.

---

## 2. Instalando o Swagger no seu projeto Node.js

Aqui vamos usar **swagger-jsdoc** (para escrever documentação direto no código) e **swagger-ui-express** (para expor a documentação em uma rota bonitona). Bora instalar:

```sh
npm install swagger-jsdoc swagger-ui-express
```

---

## 3. Configurando o Swagger no seu projeto Express

Agora, vamos adicionar o Swagger ao nosso servidor Node.js com Express.

### Criando o arquivo de configuração
Crie um arquivo `swagger.js` na raiz do projeto e cole isso:

```javascript
const swaggerJSDoc = require('swagger-jsdoc');
const swaggerUi = require('swagger-ui-express');

const options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'Minha API Super Doc',
      version: '1.0.0',
      description: 'Documentação da minha API usando Swagger',
    },
    servers: [
      {
        url: 'http://localhost:3000',
        description: 'Servidor local',
      },
    ],
  },
  apis: ['./routes/*.js'], // Apontando para os arquivos onde documentamos as rotas
};

const swaggerSpec = swaggerJSDoc(options);

const swaggerDocs = (app) => {
  app.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));
};

module.exports = swaggerDocs;
```

### Integrando com o servidor
No seu `server.js` ou `app.js`, importe e chame o `swaggerDocs`:

```javascript
const express = require('express');
const swaggerDocs = require('./swagger');

const app = express();

swaggerDocs(app);

app.listen(3000, () => {
  console.log('Servidor rodando em http://localhost:3000');
  console.log('Swagger rodando em http://localhost:3000/api-docs');
});
```

Agora é só rodar `node server.js` e abrir o navegador em `http://localhost:3000/api-docs`. Tcharaaaan! 🎉

---

## 4. Documentando as Rotas

O Swagger permite que você documente suas rotas usando **JSDoc comments**. Vamos ver como fazer isso em um arquivo de rota:

```javascript
/**
 * @swagger
 * /users:
 *   get:
 *     summary: Retorna a lista de usuários
 *     description: Endpoint que lista todos os usuários cadastrados.
 *     responses:
 *       200:
 *         description: Sucesso!
 *         content:
 *           application/json:
 *             schema:
 *               type: array
 *               items:
 *                 type: object
 *                 properties:
 *                   id:
 *                     type: integer
 *                     example: 1
 *                   nome:
 *                     type: string
 *                     example: João Silva
 */
app.get('/users', (req, res) => {
  res.json([
    { id: 1, nome: 'João Silva' },
    { id: 2, nome: 'Maria Souza' },
  ]);
});
```

Basta reiniciar o servidor e conferir no Swagger! 😎

---

## 5. Adicionando Parâmetros e Corpo de Requisição

### Parâmetros na URL

```javascript
/**
 * @swagger
 * /users/{id}:
 *   get:
 *     summary: Retorna um usuário pelo ID
 *     parameters:
 *       - name: id
 *         in: path
 *         required: true
 *         schema:
 *           type: integer
 *     responses:
 *       200:
 *         description: Retorno com sucesso
 */
```

### Corpo da Requisição (POST)

```javascript
/**
 * @swagger
 * /users:
 *   post:
 *     summary: Cria um novo usuário
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             type: object
 *             properties:
 *               nome:
 *                 type: string
 *                 example: Carlos Henrique
 *     responses:
 *       201:
 *         description: Criado com sucesso
 */
```

---

## 6. Conclusão

Swagger é essencial para documentar APIs REST. Ele melhora a comunicação entre devs, facilita testes e dá aquele ar de profissionalismo pro seu projeto. Agora é só botar a mão na massa e deixar suas APIs documentadas e lindas! 🚀

