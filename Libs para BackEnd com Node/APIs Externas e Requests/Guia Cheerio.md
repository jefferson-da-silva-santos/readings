# 🕷️ Tudo sobre **Cheerio** – Web Scraping com Node.js  

Se você já precisou **extrair informações de páginas da web** no **Node.js**, o **Cheerio** é a ferramenta perfeita! Com ele, você pode manipular **HTML como se estivesse usando jQuery**, sem precisar de um navegador.  

Aqui, vou te mostrar **como instalar, configurar e extrair dados de sites** usando o **Cheerio**. Vamos explorar tudo desde **seleção de elementos, atributos, scraping avançado e boas práticas**. 🚀  

---

## 🚀 O que é o **Cheerio**?  
O **Cheerio** é uma biblioteca para **parsear e manipular HTML no Node.js**, parecido com o que fazemos no **jQuery** no front-end.  

✅ **Leve e rápido** – Não precisa carregar um navegador (diferente do Puppeteer)  
✅ **Seleciona elementos como jQuery** (`$('h1').text()`)  
✅ **Suporte a busca por classes, IDs, atributos e filhos**  
✅ **Manipulação fácil de DOM (HTML, texto, atributos, etc.)**  
✅ **Ideal para Web Scraping, parsing de HTML e extração de dados**  

Se você precisa **pegar títulos, imagens, links ou qualquer outra informação de uma página**, o Cheerio é a escolha certa! 🕵️‍♂️  

---

## 📦 Instalando o **Cheerio**  

Para instalar o **Cheerio**, rode:  

```bash
npm install cheerio axios
```

Além do **Cheerio**, instalamos o **Axios**, que ajuda a **buscar o HTML das páginas**.

Agora podemos começar o scraping! 🔥  

---

## 🔎 Carregando um HTML no Cheerio  

Antes de fazer scraping real, vamos ver **como carregar um HTML e manipular ele**.  

```js
const cheerio = require('cheerio');

const html = `
  <html>
    <body>
      <h1>Olá, Mundo!</h1>
      <p class="mensagem">Bem-vindo ao Cheerio!</p>
      <a href="https://google.com">Google</a>
    </body>
  </html>
`;

// Carrega o HTML no Cheerio
const $ = cheerio.load(html);

// Pegando o texto da <h1>
console.log($('h1').text()); // "Olá, Mundo!"

// Pegando o texto da <p class="mensagem">
console.log($('.mensagem').text()); // "Bem-vindo ao Cheerio!"

// Pegando o link do <a>
console.log($('a').attr('href')); // "https://google.com"
```

Aqui estamos **carregando um HTML manualmente** e pegando elementos como faríamos no jQuery.

Agora bora aplicar isso para **extrair dados de sites reais!** 🔥  

---

## 🌐 Fazendo Web Scraping de um site real  

Agora vamos **buscar o HTML de um site real** e extrair informações dele.  

### 🔹 Exemplo: Pegar títulos e links de notícias  
```js
const axios = require('axios');
const cheerio = require('cheerio');

const URL = 'https://www.bbc.com/news';

const pegarNoticias = async () => {
  try {
    const { data } = await axios.get(URL);
    const $ = cheerio.load(data);

    const noticias = [];

    $('.gs-c-promo-heading').each((index, element) => {
      const titulo = $(element).text();
      const link = $(element).attr('href');

      noticias.push({
        titulo,
        link: link.startsWith('http') ? link : `https://www.bbc.com${link}`
      });
    });

    console.log(noticias);
  } catch (error) {
    console.error('❌ Erro ao buscar notícias:', error.message);
  }
};

pegarNoticias();
```

### 🔥 O que esse código faz?  
1️⃣ **Baixa o HTML da página da BBC** usando Axios  
2️⃣ **Carrega o HTML no Cheerio**  
3️⃣ **Busca todos os elementos** com `.gs-c-promo-heading` (classe dos títulos)  
4️⃣ **Extrai o título e o link da notícia**  
5️⃣ **Exibe um array com as notícias extraídas**  

---

## 🏷️ Selecionando elementos no Cheerio  

O **Cheerio** permite buscar elementos HTML usando **seletores como jQuery**. Aqui estão algumas formas de buscar dados:  

### 🔹 Selecionando por **tag**  
```js
$('h1').text(); // Pega o texto do <h1>
$('a').attr('href'); // Pega o link do primeiro <a>
```

### 🔹 Selecionando por **classe**  
```js
$('.titulo').text(); // Pega o texto da classe .titulo
$('.mensagem').html(); // Pega o HTML dentro da classe .mensagem
```

### 🔹 Selecionando por **ID**  
```js
$('#meu-id').text(); // Pega o conteúdo do elemento com ID
```

### 🔹 Pegando **vários elementos** (loop)  
Se houver **vários elementos**, usamos `.each()` para iterar:  

```js
$('.noticia').each((index, element) => {
  console.log($(element).text());
});
```

Isso percorre **todos os elementos** com a classe `.noticia` e exibe seus textos.  

---

## 📎 Extraindo atributos (imagens, links, etc.)  

Além do texto, podemos extrair **links, imagens e outros atributos**.  

### 🔹 Pegando **todos os links** da página  
```js
$('a').each((i, el) => {
  console.log($(el).attr('href'));
});
```

### 🔹 Pegando **todas as imagens** da página  
```js
$('img').each((i, el) => {
  console.log($(el).attr('src'));
});
```

Agora temos um **scraper de imagens**! 📸  

---

## 🔄 Extraindo dados de tabelas  

Se precisar pegar informações de **tabelas HTML**, o Cheerio facilita:

```js
$('table tbody tr').each((i, row) => {
  const coluna1 = $(row).find('td').eq(0).text();
  const coluna2 = $(row).find('td').eq(1).text();

  console.log(`Coluna 1: ${coluna1} | Coluna 2: ${coluna2}`);
});
```

Isso percorre todas as **linhas de uma tabela** e pega os valores das colunas.  

---

## ⚡ Boas práticas para Web Scraping  

✔ **Use um User-Agent** para evitar bloqueios:  

```js
const { data } = await axios.get(URL, {
  headers: { 'User-Agent': 'Mozilla/5.0' }
});
```

✔ **Respeite as regras do site** (`robots.txt`):  
Antes de fazer scraping, veja se o site permite acessando `https://site.com/robots.txt`.

✔ **Não faça muitas requisições de uma vez**  
Se for coletar **muitos dados**, use um intervalo entre as requisições:

```js
await new Promise(r => setTimeout(r, 3000)); // Espera 3s entre requisições
```

✔ **Use proxies se necessário**  
Se um site bloquear seu IP, use proxies como **BrightData ou ScraperAPI**.

---

## 🏁 Conclusão  

O **Cheerio** é uma ferramenta **super leve e rápida** para Web Scraping no Node.js, permitindo **extrair e manipular HTML** facilmente.  

### 🎯 Resumo:  
✅ **Carrega HTML no Node.js**  
✅ **Busca e extrai dados como jQuery**  
✅ **Pega textos, links, imagens e tabelas**  
✅ **Ótimo para scraping de notícias, preços e dados da web**  

Se você precisa **coletar informações de sites**, o **Cheerio** é a solução ideal! Agora é só testar no seu projeto. 🚀🔥