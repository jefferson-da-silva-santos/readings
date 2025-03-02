# 🌐 Tudo sobre **Manipulação de URLs com Node.js Puro**

Se você já precisou trabalhar com URLs no **Node.js**, sabe que lidar com manipulação de caminhos, parâmetros de consulta e a estrutura das URLs pode ser um desafio. No entanto, o Node.js tem várias ferramentas nativas para facilitar esse processo.

Vou te mostrar **o que é, como usar e manipular URLs no Node.js** de maneira simples e prática.

---

## 🚀 O que são URLs e por que manipulá-las no Node.js?
Uma **URL** (Uniform Resource Locator) é a referência que usamos para acessar recursos na web. O Node.js fornece funções e módulos para manipulá-las de forma eficiente e fácil.

Quando você manipula URLs, você pode:

✅ Analisar e extrair componentes como caminho, parâmetros e âncoras  
✅ Criar URLs dinamicamente  
✅ Resolver URLs relativas e absolutas  
✅ Manipular parâmetros de consulta (query strings)  
✅ Garantir que a URL seja bem formatada

Tudo isso pode ser feito com a ajuda de alguns módulos internos do Node.js.

---

## 📦 Trabalhando com o módulo `url` do Node.js
O módulo `url` do Node.js fornece funções e utilitários que tornam a manipulação de URLs uma tarefa fácil e rápida.

### 🔹 Exemplo básico de uso do `url`:
```js
const url = require('url');

const minhaUrl = 'https://www.exemplo.com:8080/pagina?nome=joao&idade=25#topo';

// Parseando a URL
const parsedUrl = url.parse(minhaUrl);

console.log(parsedUrl);
```

Isso irá exibir todos os componentes da URL, como protocolo, host, caminho, parâmetros e hash.

---

## 🛠 Como criar uma URL com o módulo `url`
Você também pode **criar URLs** a partir de componentes específicos, o que é útil quando você precisa gerar URLs dinamicamente em seu aplicativo.

### 🔹 Criando uma URL com `url.format`:
```js
const url = require('url');

const urlComponents = {
  protocol: 'https:',
  host: 'www.exemplo.com',
  pathname: '/pagina',
  search: '?nome=joao&idade=25',
  hash: '#topo'
};

const minhaUrl = url.format(urlComponents);

console.log(minhaUrl); // https://www.exemplo.com/pagina?nome=joao&idade=25#topo
```

Aqui, você passou os componentes da URL como um objeto e o método `url.format` os juntou corretamente.

---

## 🔗 Manipulando os parâmetros de consulta
Os parâmetros de consulta (query parameters) são essenciais para passar informações através de URLs. O Node.js facilita a manipulação desses parâmetros.

### 🔹 Extraindo parâmetros de consulta:
```js
const url = require('url');

const minhaUrl = 'https://www.exemplo.com/pagina?nome=joao&idade=25';
const parsedUrl = url.parse(minhaUrl, true); // O 'true' converte os parâmetros em objeto

console.log(parsedUrl.query); // { nome: 'joao', idade: '25' }
```

Isso permite que você extraia facilmente os parâmetros e trabalhe com eles como um objeto.

---

## 🟢 Como resolver URLs relativas
Às vezes, você vai precisar resolver uma URL relativa a uma URL base. O Node.js também oferece uma maneira simples de fazer isso.

### 🔹 Resolvendo uma URL relativa:
```js
const url = require('url');

const baseUrl = 'https://www.exemplo.com/pagina';
const relativeUrl = '/outro-caminho?id=123';

const resolvedUrl = url.resolve(baseUrl, relativeUrl);

console.log(resolvedUrl); // https://www.exemplo.com/outro-caminho?id=123
```

Aqui, a função `url.resolve` combina a URL base com a URL relativa e retorna o caminho completo.

---

## 🔵 Manipulando as partes da URL com `URL` (API mais moderna)
O Node.js também possui uma API mais moderna chamada `URL`, que funciona de forma semelhante ao módulo `url`, mas com uma abordagem mais orientada a objetos.

### 🔹 Criando um objeto URL:
```js
const { URL } = require('url');

const minhaUrl = new URL('https://www.exemplo.com/pagina?nome=joao&idade=25');

console.log(minhaUrl.hostname); // www.exemplo.com
console.log(minhaUrl.pathname); // /pagina
console.log(minhaUrl.searchParams.get('nome')); // joao
```

A classe `URL` permite que você acesse diretamente as partes da URL como `hostname`, `pathname` e `searchParams`.

---

## 🟡 Manipulando os parâmetros de consulta com `URLSearchParams`
Para manipular os parâmetros de consulta de forma mais flexível, você pode usar o `URLSearchParams`, que é parte da API `URL`.

### 🔹 Adicionando e removendo parâmetros:
```js
const { URL, URLSearchParams } = require('url');

const minhaUrl = new URL('https://www.exemplo.com/pagina?nome=joao&idade=25');

// Adicionando um novo parâmetro
minhaUrl.searchParams.append('cidade', 'rio');

// Removendo um parâmetro
minhaUrl.searchParams.delete('idade');

console.log(minhaUrl.toString()); // https://www.exemplo.com/pagina?nome=joao&cidade=rio
```

O `URLSearchParams` facilita a manipulação de parâmetros de consulta, permitindo adicionar, remover e alterar parâmetros de maneira simples.

---

## 🔴 Trabalhando com URL no contexto de APIs
No contexto de APIs REST, é comum manipular URLs para criar rotas dinâmicas, como obter dados específicos de um recurso. Vamos ver um exemplo básico de como isso pode ser feito em um servidor com **Node.js puro**.

### 🔹 Criando uma API simples com URLs dinâmicas:
```js
const http = require('http');
const url = require('url');

const server = http.createServer((req, res) => {
  const parsedUrl = url.parse(req.url, true);
  
  if (parsedUrl.pathname === '/usuario' && parsedUrl.query.id) {
    res.writeHead(200, { 'Content-Type': 'application/json' });
    res.end(JSON.stringify({ id: parsedUrl.query.id, nome: 'João' }));
  } else {
    res.writeHead(404);
    res.end('Página não encontrada');
  }
});

server.listen(3000, () => {
  console.log('Servidor rodando na porta 3000');
});
```

Este código cria uma API simples que retorna um objeto JSON com dados do usuário, usando o parâmetro `id` da URL.

---

## 📜 Conclusão
Manipular URLs no Node.js é uma tarefa simples e eficiente, com ferramentas nativas como o módulo `url` e a API `URL`.

### 🎯 Resumo:
✅ **Análise e formatação de URLs**  
✅ **Extração de parâmetros de consulta**  
✅ **Resolução de URLs relativas**  
✅ **Manipulação flexível com `URLSearchParams`**  
✅ **Criação de APIs com URLs dinâmicas**  

Agora é só aplicar esses conceitos no seu projeto! 🚀🌐
