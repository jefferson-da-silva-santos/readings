# 📌 Guia Definitivo (e Divertido) para Fazer Requisições para APIs com Fetch no JavaScript 🚀

Ah, o **Fetch API**... Essa maravilha do JavaScript que permite fazer requisições HTTP de forma elegante (e sem precisar de bibliotecas externas como o Axios). Mas, se você não souber usá-la direito, pode acabar preso em um limbo de **promises não resolvidas, respostas maltratadas e erros misteriosos**. 😆

Então, bora aprender **tudo** sobre o `fetch` e como usá-lo de forma eficiente, explorando seus parâmetros, opções, formas de tratar dados e lidar com erros. Se prepara, porque esse guia vai ser uma viagem bem detalhada! 🏄‍♂️

---

## 🎯 O que é o Fetch API?

O **Fetch API** é uma interface moderna para fazer requisições HTTP no JavaScript. Ele substitui o antigo `XMLHttpRequest` e torna mais fácil buscar, enviar e manipular dados de servidores.

A estrutura básica de uma requisição com `fetch` é essa:

```javascript
fetch(url, options)
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Erro na requisição:', error));
```

Simples, né? Mas essa simplicidade esconde **muitos detalhes** que vamos explorar!

---

## 1️⃣ Fazendo Requisições GET com Fetch

O método mais básico de `fetch` é uma requisição **GET**, usada para obter dados de um servidor. Como padrão, `fetch` já faz uma requisição GET se você não passar o segundo parâmetro (`options`).

### Exemplo de requisição GET:

```javascript
fetch('https://jsonplaceholder.typicode.com/posts')
  .then(response => response.json())
  .then(data => console.log('Dados recebidos:', data))
  .catch(error => console.error('Erro na requisição:', error));
```

### 🛠 Explicando o código:
- `fetch(url)`: Faz a requisição HTTP.
- `.then(response => response.json())`: Converte a resposta para JSON.
- `.then(data => console.log(data))`: Exibe os dados no console.
- `.catch(error => console.error(error))`: Captura erros, caso a requisição falhe.

---

## 2️⃣ Como Funciona a Resposta do Fetch?

A resposta do `fetch` **não é automaticamente convertida em JSON**. Ele retorna um objeto `Response`, e você precisa chamar `.json()` para obter os dados.

### Inspecionando a resposta:

```javascript
fetch('https://jsonplaceholder.typicode.com/posts/1')
  .then(response => {
    console.log('Objeto Response:', response);
    return response.json();
  })
  .then(data => console.log('Dados tratados:', data))
  .catch(error => console.error('Erro:', error));
```

### 🔍 Estrutura do Objeto Response:

```json
{
  "type": "cors",
  "status": 200,
  "ok": true,
  "headers": { ... },
  "body": "..."
}
```

- **`status`**: Código HTTP (200, 404, 500...).
- **`ok`**: Booleano indicando sucesso (`true` se o status for entre 200 e 299).
- **`headers`**: Cabeçalhos da resposta.
- **`body`**: Corpo da resposta (precisa ser tratado com `.json()`, `.text()`, etc.).

---

## 3️⃣ Tratando Erros no Fetch

O `fetch` **não lança erro para status HTTP de erro** (como `404` ou `500`). Ele só lança erro se a conexão falhar (ex: sem internet).

### Como verificar se a resposta tem erro:

```javascript
fetch('https://jsonplaceholder.typicode.com/404')
  .then(response => {
    if (!response.ok) {
      throw new Error(`Erro HTTP! Status: ${response.status}`);
    }
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('Erro tratado:', error));
```

### 🛑 Melhor prática:
Sempre verifique `response.ok` antes de processar os dados!

---

## 4️⃣ Fazendo Requisições POST com Fetch

Para enviar dados para uma API, usamos **método POST** e passamos um objeto `options`.

### Exemplo de requisição POST:

```javascript
fetch('https://jsonplaceholder.typicode.com/posts', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    title: 'Meu Post',
    body: 'Esse é um post de exemplo.',
    userId: 1
  })
})
  .then(response => response.json())
  .then(data => console.log('Resposta da API:', data))
  .catch(error => console.error('Erro no POST:', error));
```

### 🔍 Explicando:
- `method: 'POST'`: Define o método HTTP.
- `headers: { 'Content-Type': 'application/json' }`: Informa que estamos enviando JSON.
- `body: JSON.stringify(...)`: Converte o objeto para JSON.

---

## 5️⃣ Lidando com Timeouts

O `fetch` não tem suporte nativo para timeouts, mas podemos criar um timeout manualmente com `Promise.race()`.

```javascript
const fetchWithTimeout = (url, options, timeout = 5000) => {
  return Promise.race([
    fetch(url, options),
    new Promise((_, reject) => setTimeout(() => reject(new Error('Timeout!')), timeout))
  ]);
};
```

---

## 6️⃣ Trabalhando com Autenticação

Se a API exigir um **token de autenticação**, basta adicioná-lo nos cabeçalhos:

```javascript
fetch('https://api.exemplo.com/dados', {
  headers: {
    'Authorization': 'Bearer SEU_TOKEN_AQUI'
  }
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Erro:', error));
```

---

## 7️⃣ Manipulando Cabeçalhos de Resposta

Os cabeçalhos da resposta HTTP podem conter informações importantes, como tipo de conteúdo, permissões e cache.

### Exemplo:

```javascript
fetch('https://jsonplaceholder.typicode.com/posts')
  .then(response => {
    console.log('Cabeçalhos:', response.headers);
    return response.json();
  })
  .then(data => console.log('Dados recebidos:', data))
  .catch(error => console.error('Erro:', error));
```

---

## 8️⃣ Enviando Formulários com Fetch

Para enviar dados de formulário, utilizamos o `FormData`.

### Exemplo:

```javascript
const formData = new FormData();
formData.append('username', 'exemplo');
formData.append('password', '123456');

fetch('https://api.exemplo.com/login', {
  method: 'POST',
  body: formData
})
  .then(response => response.json())
  .then(data => console.log('Resposta:', data))
  .catch(error => console.error('Erro:', error));
```

---

## 9️⃣ Trabalhando com Cookies

Algumas APIs requerem cookies para autenticação. Podemos incluí-los na requisição com `credentials`.

### Exemplo:

```javascript
fetch('https://api.exemplo.com/dados', {
  credentials: 'include'
})
  .then(response => response.json())
  .then(data => console.log('Dados:', data))
  .catch(error => console.error('Erro:', error));
```

---

## 🔟 Cancelando uma Requisição com AbortController

O `AbortController` permite cancelar requisições em andamento.

### Exemplo:

```javascript
const controller = new AbortController();
const signal = controller.signal;

fetch('https://jsonplaceholder.typicode.com/posts', { signal })
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Requisição cancelada:', error));

setTimeout(() => controller.abort(), 2000); // Cancela após 2 segundos
```
---

## 1️⃣1️⃣ Trabalhando com JSON e Respostas Textuais

Nem todas as respostas são JSON. Algumas APIs retornam texto puro ou HTML.

```javascript
fetch('https://api.exemplo.com/texto')
  .then(response => response.text())
  .then(data => console.log('Texto recebido:', data))
  .catch(error => console.error('Erro:', error));
```

---

## 1️⃣2️⃣ Usando Fetch com Async/Await

Podemos usar `async/await` para tornar o código mais legível:

```javascript
async function fetchData() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts');
    const data = await response.json();
    console.log('Dados:', data);
  } catch (error) {
    console.error('Erro na requisição:', error);
  }
}

fetchData();
```

---

## 1️⃣3️⃣ Tratando Diferentes Tipos de Erros

Podemos capturar erros específicos para melhorar a experiência do usuário:

```javascript
fetch('https://api.exemplo.com/dados')
  .then(response => {
    if (!response.ok) {
      throw new Error(`Erro HTTP: ${response.status}`);
    }
    return response.json();
  })
  .catch(error => {
    if (error.message.includes('Failed to fetch')) {
      console.error('Erro de conexão. Verifique sua internet.');
    } else {
      console.error('Erro inesperado:', error);
    }
  });
```

---

## 1️⃣4️⃣ Utilizando Fetch em Loop para Múltiplas Requisições

```javascript
const urls = ['https://jsonplaceholder.typicode.com/posts/1', 'https://jsonplaceholder.typicode.com/posts/2'];

Promise.all(urls.map(url => fetch(url).then(res => res.json())))
  .then(results => console.log('Dados:', results))
  .catch(error => console.error('Erro:', error));
```

---

## 1️⃣5️⃣ Lidando com Retentativas de Requisição (Retry)

Se a requisição falhar, podemos tentar novamente algumas vezes:

```javascript
async function fetchWithRetry(url, retries = 3) {
  for (let i = 0; i < retries; i++) {
    try {
      const response = await fetch(url);
      if (!response.ok) throw new Error('Erro HTTP');
      return await response.json();
    } catch (error) {
      console.warn(`Tentativa ${i + 1} falhou. Retentando...`);
    }
  }
  throw new Error('Falha após múltiplas tentativas');
}
```

---

## 1️⃣6️⃣ Cacheando Respostas para Melhor Desempenho

Para evitar múltiplas requisições desnecessárias, podemos armazenar respostas no cache:

```javascript
const cache = new Map();

async function fetchWithCache(url) {
  if (cache.has(url)) {
    console.log('Retornando do cache');
    return cache.get(url);
  }
  const response = await fetch(url);
  const data = await response.json();
  cache.set(url, data);
  return data;
}
```

---

## 1️⃣7️⃣ Monitorando o Progresso de uma Requisição

Se a API suporta `ReadableStream`, podemos monitorar o progresso da resposta:

```javascript
fetch('https://api.exemplo.com/dados')
  .then(response => {
    const reader = response.body.getReader();
    return reader.read();
  })
  .then(({ done, value }) => console.log('Progresso:', done, value))
  .catch(error => console.error('Erro:', error));
```

---

## 1️⃣8️⃣ Enviando Arquivos com Fetch

Para fazer upload de arquivos, usamos `FormData`.

```javascript
const formData = new FormData();
formData.append('file', fileInput.files[0]);

fetch('https://api.exemplo.com/upload', {
  method: 'POST',
  body: formData
})
  .then(response => response.json())
  .then(data => console.log('Upload concluído:', data))
  .catch(error => console.error('Erro:', error));
```

---

## 1️⃣9️⃣ Requisições Paralelas com Fetch

Se precisarmos buscar múltiplos recursos ao mesmo tempo, podemos usar `Promise.all()`.

```javascript
const urls = ['https://api.exemplo.com/dados1', 'https://api.exemplo.com/dados2'];

Promise.all(urls.map(url => fetch(url).then(res => res.json())))
  .then(results => console.log('Dados recebidos:', results))
  .catch(error => console.error('Erro:', error));
```

---

## 2️⃣0️⃣ Requisições Condicionais com Cabeçalhos ETag

Podemos evitar downloads desnecessários verificando o cabeçalho `ETag` da resposta anterior:

```javascript
fetch('https://api.exemplo.com/dados', {
  headers: {
    'If-None-Match': 'etag-anterior'
  }
})
  .then(response => {
    if (response.status === 304) {
      console.log('Dados não modificados');
      return;
    }
    return response.json();
  })
  .then(data => console.log('Dados:', data))
  .catch(error => console.error('Erro:', error));
```

---

🎉 **Conclusão:** Agora você domina o `fetch` e sabe como fazer requisições de forma eficiente. Use essas técnicas e nunca mais sofra com APIs no JavaScript! 🚀

