# 🛡️ Tudo sobre **Helmet** – Protegendo sua API Express

Se você está rodando um **servidor Express**, provavelmente já ouviu falar de **segurança** e **boas práticas** para evitar ataques. O **Helmet** é um **middleware de segurança para Express** que ajuda a proteger sua aplicação contra **várias vulnerabilidades conhecidas**.

Bora ver **o que é, como funciona e como configurar o Helmet** no seu servidor Express! 🚀🔥

---

## 🚀 O que é o **Helmet**?
O **Helmet** é um pacote que adiciona automaticamente **vários headers HTTP de segurança** para proteger sua aplicação contra ataques comuns, como:

✅ **Cross-Site Scripting (XSS)** – Injeção de scripts maliciosos  
✅ **Clickjacking** – Quando um site mal-intencionado tenta enganar o usuário para clicar em algo indesejado  
✅ **Sniffing de MIME-Type** – Protege contra arquivos sendo carregados incorretamente  
✅ **Injeção de código em headers** – Protege contra manipulação maliciosa de headers  
✅ **Outras vulnerabilidades conhecidas**  

Ele **não é um firewall**, mas adiciona **proteções importantes** na resposta HTTP do seu servidor.

---

## 📦 Instalando o Helmet
Antes de tudo, você precisa instalar o **Helmet** no seu projeto:

```bash
npm install helmet
```

Depois, importa no código:

```js
const helmet = require('helmet');
```

Se estiver usando **ESModules**, faz assim:
```js
import helmet from 'helmet';
```

Agora, bora configurar!

---

## ⚙️ Usando o Helmet no Express
O Helmet é **muito fácil de usar**. Basta adicionar ele como um **middleware** no seu servidor Express:

```js
const express = require('express');
const helmet = require('helmet');

const app = express();

// Aplica o Helmet para todas as rotas
app.use(helmet());

app.get('/', (req, res) => {
  res.send('🛡️ Express protegido com Helmet!');
});

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

Isso já adiciona **vários headers de segurança** automaticamente! 🚀

---

## 🔍 Quais headers o Helmet adiciona?
O Helmet adiciona **diversos headers** de segurança por padrão:

| Header | O que faz? |
|--------|-----------|
| `X-DNS-Prefetch-Control` | Impede que navegadores façam pré-busca de DNS automaticamente. |
| `X-Frame-Options` | Protege contra **Clickjacking**, impedindo que seu site seja carregado dentro de iframes. |
| `X-Content-Type-Options` | Bloqueia sniffing de MIME-Type (protege contra arquivos sendo interpretados como scripts maliciosos). |
| `Strict-Transport-Security (HSTS)` | Obriga o uso de HTTPS. |
| `Referrer-Policy` | Controla quais informações são enviadas no cabeçalho Referer. |
| `Content-Security-Policy (CSP)` | Protege contra **XSS (Cross-Site Scripting)**. |

Vamos explorar **cada um desses headers** e como configurar!

---

## 🛠 Configurando o Helmet

O Helmet permite **customizar** quais proteções você quer ativar ou desativar.

### 🔹 Configurando cada header separadamente
```js
app.use(helmet({
  contentSecurityPolicy: false, // Desativa CSP (caso precise carregar scripts externos)
  frameguard: { action: 'deny' }, // Bloqueia iframes completamente
  referrerPolicy: { policy: 'no-referrer' }, // Impede envio do cabeçalho Referer
}));
```

### 🔹 Protegendo contra Clickjacking (Frameguard)
O **clickjacking** acontece quando um site mal-intencionado coloca seu site dentro de um **iframe** para enganar usuários a clicarem em algo.

```js
app.use(helmet.frameguard({ action: 'deny' }));
```

Isso impede que seu site seja carregado em um iframe.

Se precisar permitir **iframes apenas do seu domínio**:
```js
app.use(helmet.frameguard({ action: 'sameorigin' }));
```

---

### 🔹 Segurança no **HTTPS** (HSTS)
Se seu site usa **HTTPS**, você pode obrigar os navegadores a usarem apenas HTTPS, mesmo se alguém tentar acessar via HTTP:

```js
app.use(helmet.hsts({
  maxAge: 31536000, // Tempo máximo de 1 ano em segundos
  includeSubDomains: true, // Aplica a subdomínios
  preload: true // Permite pré-carregar nos navegadores
}));
```

Isso ajuda a evitar ataques **Man-in-the-Middle (MitM)**.

---

### 🔹 Protegendo contra **Sniffing de MIME-Type**
Isso impede que navegadores tentem interpretar arquivos como scripts maliciosos:

```js
app.use(helmet.noSniff());
```

Agora, se alguém tentar forçar um **arquivo de imagem a ser lido como um script**, isso será bloqueado.

---

### 🔹 Configurando **Content Security Policy (CSP)**
O **CSP** protege contra **XSS (Cross-Site Scripting)** bloqueando scripts externos não autorizados.

```js
app.use(helmet.contentSecurityPolicy({
  directives: {
    defaultSrc: ["'self'"], // Permite apenas recursos do próprio site
    scriptSrc: ["'self'", "https://apis.google.com"], // Permite scripts do próprio site + Google APIs
    styleSrc: ["'self'", "'unsafe-inline'"], // Permite estilos internos
  }
}));
```

Isso evita que um hacker injete um **script malicioso** via `<script>` e roube dados do usuário.

---

### 🔹 Controlando Referer (Referrer Policy)
O **Referrer Policy** controla **quais informações** seu site envia para outros domínios ao seguir links.

```js
app.use(helmet.referrerPolicy({ policy: 'no-referrer' }));
```

Isso impede que sites externos saibam de onde veio o tráfego.

Se quiser permitir que apenas sites HTTPS recebam o Referer:
```js
app.use(helmet.referrerPolicy({ policy: 'strict-origin-when-cross-origin' }));
```

---

## 📊 Exibindo os Headers no Navegador
Depois de configurar o Helmet, você pode checar os headers no navegador:

1️⃣ **Abra o seu site no navegador**  
2️⃣ **Clique com o botão direito > Inspecionar**  
3️⃣ **Vá até a aba "Network" e clique na requisição do site**  
4️⃣ **Vá até "Headers" e veja os headers adicionados pelo Helmet**  

Você verá algo assim:

```
strict-transport-security: max-age=31536000; includeSubDomains
x-frame-options: DENY
x-content-type-options: nosniff
content-security-policy: default-src 'self'
```

Agora seu servidor Express está **muito mais seguro**! 🔥🔥

---

## 🏁 Conclusão
O **Helmet** é uma ferramenta essencial para **proteger seu servidor Express** contra ataques comuns na web.

### 🎯 Resumo:
✅ **Bloqueia Clickjacking (x-frame-options)**  
✅ **Protege contra XSS com Content-Security-Policy**  
✅ **Força HTTPS com HSTS**  
✅ **Impede sniffing de MIME-Type**  
✅ **Controla Referer-Policy**  
✅ **Fácil de integrar no Express**  

### 🔥 Exemplo completo de uso:
```js
const express = require('express');
const helmet = require('helmet');

const app = express();

// Aplica proteção com Helmet
app.use(helmet());

app.get('/', (req, res) => {
  res.send('🛡️ Express protegido com Helmet!');
});

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

Agora seu servidor Express está **mais seguro** contra ataques comuns! 🚀🔥