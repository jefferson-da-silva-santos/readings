# 🌍 Tudo sobre **node-fetch** – Fazendo Requisições HTTP no Node.js  

Se você já precisou **fazer chamadas para APIs no Node.js**, provavelmente percebeu que o **fetch** nativo do navegador **não funciona no backend**.  

A solução? **node-fetch**, um pacote que traz o mesmo `fetch` do navegador para o Node.js. Com ele, você pode consumir APIs REST de forma **fácil e eficiente**.  

Aqui vou te mostrar **como instalar, configurar e usar o node-fetch** para fazer requisições HTTP no seu projeto. 🚀  

---

## 🚀 O que é o **node-fetch**?  
O **node-fetch** é um **cliente HTTP** para **Node.js** que permite:  

✅ **Fazer requisições HTTP (GET, POST, PUT, DELETE)**  
✅ **Baixar arquivos** e trabalhar com **streams**  
✅ **Enviar e receber JSON facilmente**  
✅ **Autenticação via Headers (JWT, Bearer, Basic Auth, etc.)**  
✅ **Timeouts e manipulação de erros**  

Se você já usou `fetch` no navegador, vai se sentir em casa! 😎  

---

## 📦 Instalando o node-fetch  

Para instalar no **Node.js**, basta rodar:  

```bash
npm install node-fetch
```
ou, se estiver usando **yarn**:  
```bash
yarn add node-fetch
```

Depois, importe no seu código:  

```js
const fetch = require('node-fetch');
```

Se estiver usando **ESModules** (Node.js 18+), importe assim:  
```js
import fetch from 'node-fetch';
```

Agora podemos fazer requisições HTTP! 🚀  

---

## 🔵 Fazendo uma requisição **GET**  

O exemplo mais básico de uso do `node-fetch` é buscar dados de uma API:

```js
const fetch = require('node-fetch');

const getUsers = async () => {
  try {
    const resposta = await fetch('https://jsonplaceholder.typicode.com/users');
    const dados = await resposta.json(); // Convertendo resposta para JSON
    console.log(dados);
  } catch (error) {
    console.error('❌ Erro na requisição:', error.message);
  }
};

getUsers();
```

Isso faz um **GET** na API e exibe os usuários no console.  

---

## 🟢 Fazendo uma requisição **POST** (Enviando dados)  

Para enviar dados (como criar um usuário), usamos **POST** com `body`:

```js
const fetch = require('node-fetch');

const criarUsuario = async () => {
  try {
    const resposta = await fetch('https://jsonplaceholder.typicode.com/users', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify({
        nome: 'John Doe',
        email: 'johndoe@example.com'
      })
    });

    const dados = await resposta.json();
    console.log('✅ Usuário criado:', dados);
  } catch (error) {
    console.error('❌ Erro ao criar usuário:', error.message);
  }
};

criarUsuario();
```

Isso envia um JSON para a API, criando um novo usuário. 📩  

---

## 🟡 Fazendo uma requisição **PUT e PATCH** (Atualizar dados)  

Se quiser **atualizar um recurso**, use **PUT** (para modificar tudo) ou **PATCH** (para modificar apenas alguns campos):

```js
const atualizarUsuario = async () => {
  try {
    const resposta = await fetch('https://jsonplaceholder.typicode.com/users/1', {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ nome: 'John Atualizado', email: 'novoemail@example.com' })
    });

    const dados = await resposta.json();
    console.log('✅ Usuário atualizado:', dados);
  } catch (error) {
    console.error('❌ Erro ao atualizar usuário:', error.message);
  }
};

atualizarUsuario();
```

---

## 🔴 Fazendo uma requisição **DELETE** (Remover dados)  

Para remover um usuário, basta fazer um **DELETE**:  

```js
const deletarUsuario = async () => {
  try {
    const resposta = await fetch('https://jsonplaceholder.typicode.com/users/1', {
      method: 'DELETE'
    });

    console.log('✅ Usuário deletado:', resposta.status);
  } catch (error) {
    console.error('❌ Erro ao deletar usuário:', error.message);
  }
};

deletarUsuario();
```

Se tudo der certo, a API retorna um `status 200` ou `204` indicando sucesso.

---

## 🔑 Adicionando **Headers** (Autenticação e JWT)  

Se a API exigir um **token JWT** (Bearer Token), basta passar nos headers:

```js
const getDadosProtegidos = async () => {
  try {
    const resposta = await fetch('https://api.exemplo.com/protegido', {
      headers: {
        'Authorization': 'Bearer MEU_TOKEN_AQUI'
      }
    });

    const dados = await resposta.json();
    console.log('✅ Dados protegidos:', dados);
  } catch (error) {
    console.error('❌ Erro ao acessar dados protegidos:', error.message);
  }
};

getDadosProtegidos();
```

Isso autentica a requisição e permite acessar **APIs protegidas**. 🔒  

---

## ⏳ Configurando **Timeout** (Evitando requisições presas)  

Por padrão, o `fetch` pode **ficar esperando indefinidamente**. Para evitar isso, podemos usar um timeout:

```js
const fetchComTimeout = async (url, options, timeout = 5000) => {
  const controller = new AbortController();
  const id = setTimeout(() => controller.abort(), timeout);

  const resposta = await fetch(url, { ...options, signal: controller.signal });
  clearTimeout(id);
  return resposta;
};

(async () => {
  try {
    const resposta = await fetchComTimeout('https://jsonplaceholder.typicode.com/users', {}, 3000);
    const dados = await resposta.json();
    console.log('✅ Dados recebidos:', dados);
  } catch (error) {
    console.error('❌ Timeout ou erro na requisição:', error.message);
  }
})();
```

Agora, se a API demorar mais de **3 segundos**, a requisição **será abortada automaticamente**. ⏳  

---

## 📥 Baixando Arquivos com node-fetch  

O `node-fetch` também permite **baixar arquivos**, como imagens e PDFs:

```js
const fs = require('fs');
const fetch = require('node-fetch');

const baixarImagem = async () => {
  const resposta = await fetch('https://via.placeholder.com/600x400.png');
  const stream = fs.createWriteStream('imagem.png');

  resposta.body.pipe(stream);
  console.log('✅ Imagem baixada com sucesso!');
};

baixarImagem();
```

Isso baixa e **salva a imagem no servidor**. 🖼️  

---

## 🔄 Trabalhando com **Streams**  

Se estiver lidando com **grandes volumes de dados**, pode usar streams:

```js
const fetchStream = async () => {
  const resposta = await fetch('https://jsonplaceholder.typicode.com/users');

  resposta.body.on('data', chunk => {
    console.log('📩 Recebendo dados:', chunk.toString());
  });

  resposta.body.on('end', () => {
    console.log('✅ Fim da transmissão.');
  });
};

fetchStream();
```

Isso melhora a performance ao **processar grandes respostas** em partes.  

---

## 🏁 Conclusão  

O **node-fetch** é um dos jeitos mais **simples e poderosos** de fazer requisições HTTP no Node.js.  

### 🎯 Resumo:  
✅ **GET, POST, PUT, DELETE** com facilidade  
✅ **Autenticação via Headers (JWT, Bearer Token)**  
✅ **Timeout para evitar requisições penduradas**  
✅ **Baixar arquivos e trabalhar com streams**  

Se você precisa consumir APIs no backend, **node-fetch é uma escolha perfeita!** 🚀🔥