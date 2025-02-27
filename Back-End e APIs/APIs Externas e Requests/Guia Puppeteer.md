# 🤖 Tudo sobre **Puppeteer** – Automação de Navegador no Node.js  

Se você precisa **automatizar ações no navegador** como **scraping de dados, geração de PDFs ou testes automatizados**, o **Puppeteer** é uma das ferramentas mais poderosas para isso.  

Aqui, vou te mostrar **como instalar, configurar e usar Puppeteer na prática**, desde acessar páginas e preencher formulários até tirar screenshots e gerar PDFs! 🚀  

---

## 🚀 O que é o **Puppeteer**?  
O **Puppeteer** é uma biblioteca do **Node.js** que permite **controlar o Google Chrome** ou **Chromium** programaticamente.  

✅ **Automação de tarefas no navegador** (acessar sites, clicar, preencher formulários)  
✅ **Web Scraping** (coletar dados de sites)  
✅ **Geração de PDFs** a partir de páginas HTML  
✅ **Testes automatizados** em aplicações web  
✅ **Captura de screenshots de sites**  

Bora instalar e ver como funciona! 🔥  

---

## 📦 Instalando o Puppeteer  

Para instalar o Puppeteer no seu projeto, rode:  

```bash
npm install puppeteer
```
ou, se estiver usando **yarn**:  
```bash
yarn add puppeteer
```

⚠️ **Obs:** O Puppeteer já instala uma versão do Chromium por padrão, o que pode **demorar** um pouco no primeiro download.  

---

## 🌐 Abrindo um navegador com Puppeteer  

Vamos abrir o **Chromium** com Puppeteer e acessar um site:  

```js
const puppeteer = require('puppeteer');

(async () => {
  // Inicia o navegador
  const browser = await puppeteer.launch({ headless: false });

  // Abre uma nova aba
  const page = await browser.newPage();

  // Navega até um site
  await page.goto('https://google.com');

  console.log('✅ Página carregada com sucesso!');

  // Fecha o navegador após 5 segundos
  setTimeout(() => browser.close(), 5000);
})();
```

🔹 **O que acontece aqui?**  
1️⃣ O **Chromium** é aberto (**não headless**, ou seja, visível)  
2️⃣ Abre uma aba e acessa o **Google**  
3️⃣ Aguarda 5 segundos e fecha o navegador  

💡 Se quiser rodar o navegador em **modo headless** (sem interface gráfica), use:  
```js
await puppeteer.launch({ headless: true });
```

---

## 📸 Tirando um Screenshot de um site  

Se quiser **capturar a tela** de um site, basta fazer assim:  

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();

  await page.goto('https://google.com');
  await page.screenshot({ path: 'screenshot.png', fullPage: true });

  console.log('✅ Screenshot salvo como screenshot.png');

  await browser.close();
})();
```

Isso salva um **arquivo PNG** com a captura de tela do site! 📸  

---

## 📝 Extraindo texto de um site (**Web Scraping**)  

Se quiser **pegar dados de um site automaticamente**, use este código:  

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();

  await page.goto('https://example.com');

  // Extraindo o texto do título da página
  const titulo = await page.evaluate(() => document.querySelector('h1').innerText);

  console.log('📄 Título da página:', titulo);

  await browser.close();
})();
```

Isso extrai o **título do site** e exibe no console! 🔥  

---

## 🖱️ Clicando em Botões  

Se quiser **clicar em um botão** automaticamente, use `.click()`:

```js
await page.click('button#meuBotao');
```

Exemplo completo:  

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch({ headless: false });
  const page = await browser.newPage();

  await page.goto('https://example.com');

  // Clica no botão com id "meuBotao"
  await page.click('#meuBotao');

  console.log('✅ Botão clicado!');

  await browser.close();
})();
```

Isso simula um clique no botão **#meuBotao**. 🖱️  

---

## ⌨️ Preenchendo Formulários  

Se quiser **preencher inputs e enviar formulários**, use `.type()` e `.click()`:

```js
await page.type('input#email', 'meuemail@example.com');
await page.type('input#senha', 'minhaSenha123');
await page.click('button#enviar');
```

Exemplo completo:  

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch({ headless: false });
  const page = await browser.newPage();

  await page.goto('https://example.com/login');

  // Preenche o formulário de login
  await page.type('#email', 'meuemail@example.com');
  await page.type('#senha', 'minhaSenha123');

  // Clica no botão de login
  await page.click('#btnLogin');

  console.log('✅ Login enviado!');

  await browser.close();
})();
```

Isso **simula um login** em um site! 🚀  

---

## 📂 Gerando um PDF de um site  

Se quiser **converter uma página em PDF**, use `.pdf()`:  

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();

  await page.goto('https://example.com');

  await page.pdf({ path: 'pagina.pdf', format: 'A4' });

  console.log('✅ PDF gerado com sucesso!');

  await browser.close();
})();
```

Isso salva um **PDF da página** com formato **A4**! 📄✨  

---

## ⚡ Executando Código JavaScript na Página  

Se quiser rodar **código JavaScript** dentro da página, use `.evaluate()`:

```js
const titulo = await page.evaluate(() => {
  return document.title;
});

console.log('📄 Título da página:', titulo);
```

Exemplo completo:  

```js
const puppeteer = require('puppeteer');

(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage();

  await page.goto('https://example.com');

  const titulo = await page.evaluate(() => document.title);

  console.log('📄 Título da página:', titulo);

  await browser.close();
})();
```

Isso retorna o **título da página** no console.  

---

## 🏎️ Dicas de Performance  

Se quiser **melhorar a performance** ao carregar páginas, siga essas dicas:  

### 🔹 **Desativar imagens e CSS** para scraping mais rápido  
```js
await page.setRequestInterception(true);
page.on('request', (req) => {
  if (req.resourceType() === 'image' || req.resourceType() === 'stylesheet') {
    req.abort();
  } else {
    req.continue();
  }
});
```

### 🔹 **Rodar em headless para maior velocidade**  
```js
const browser = await puppeteer.launch({ headless: true });
```

### 🔹 **Esperar elementos carregarem antes de interagir**  
```js
await page.waitForSelector('#meuElemento');
```

---

## 🎯 Resumo Final  

O **Puppeteer** é uma ferramenta incrível para **automação de navegador** no Node.js.  

✅ **Acessar páginas automaticamente**  
✅ **Tirar screenshots e gerar PDFs**  
✅ **Fazer Web Scraping de sites**  
✅ **Preencher formulários e clicar em botões**  
✅ **Rodar JavaScript dentro da página**  

Se precisar de **testes automatizados, scraping ou geração de PDFs**, **Puppeteer** é a escolha certa! Agora é só testar no seu projeto! 🚀🔥