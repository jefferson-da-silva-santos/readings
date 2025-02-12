# 📧 Tudo sobre **Nodemailer** – Enviando E-mails no Node.js  

Se você precisa **enviar e-mails no seu projeto Node.js**, o **Nodemailer** é a ferramenta perfeita! Com ele, você pode enviar e-mails via **Gmail, Outlook, SMTP e até serviços como AWS SES e Mailgun**.  

Aqui vou te mostrar **como instalar, configurar e enviar e-mails com anexos e templates**. Bora lá! 🚀  

---

## 🚀 O que é o **Nodemailer**?  
O **Nodemailer** é um módulo do **Node.js** que permite enviar **e-mails de forma simples e flexível**. Ele suporta:  

✅ **Envio via SMTP (Gmail, Outlook, Yahoo, etc.)**  
✅ **Envio autenticado (usuário e senha ou OAuth2)**  
✅ **Suporte a HTML e templates**  
✅ **Anexos (imagens, PDFs, etc.)**  
✅ **Envio em massa (com rate limit para evitar bloqueios)**  

Agora bora instalar e usar. 🔥  

---

## 📦 Instalando o Nodemailer  

Para instalar no seu projeto, rode:  

```bash
npm install nodemailer
```
ou, se estiver usando **yarn**:  
```bash
yarn add nodemailer
```

Depois, importe no seu código:

```js
const nodemailer = require('nodemailer');
```

Agora podemos configurar o envio! 🚀  

---

## ✉️ Configurando um serviço SMTP (Exemplo com Gmail)  

O jeito mais comum de enviar e-mails é usando **SMTP**, um protocolo que permite que e-mails sejam enviados de forma segura.  

### 📌 Criando um **transporter** (Gmail)
```js
const transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: 'seuemail@gmail.com',
    pass: 'suasenha'
  }
});
```

Isso cria um **transporter** para envio de e-mails via **Gmail**.  

⚠️ **IMPORTANTE!** Gmail pode bloquear o envio por **senha**. Para evitar isso:  
1️⃣ **Habilite "Acesso a apps menos seguros"** [aqui](https://myaccount.google.com/lesssecureapps) *(Não recomendado para produção)*  
2️⃣ **Use OAuth2** (mais seguro) – veremos isso depois  

Agora bora enviar um e-mail! 🚀  

---

## 📤 Enviando um e-mail básico  

```js
const mailOptions = {
  from: 'seuemail@gmail.com',
  to: 'destinatario@example.com',
  subject: 'Olá! 🚀',
  text: 'Este é um e-mail enviado pelo Nodemailer!',
};

transporter.sendMail(mailOptions, (error, info) => {
  if (error) {
    console.log('❌ Erro ao enviar e-mail:', error);
  } else {
    console.log('✅ E-mail enviado com sucesso!', info.response);
  }
});
```

Se tudo deu certo, o destinatário recebe o e-mail **quase instantaneamente!** 💌  

---

## 🎨 Enviando e-mail com HTML  

Se quiser formatar o e-mail bonitinho, basta usar **HTML** no lugar do `text`:  

```js
const mailOptions = {
  from: 'seuemail@gmail.com',
  to: 'destinatario@example.com',
  subject: 'E-mail com HTML',
  html: '<h1>Olá! 🚀</h1><p>Este e-mail contém <b>HTML</b>!</p>'
};

transporter.sendMail(mailOptions, (error, info) => {
  if (error) {
    console.log('❌ Erro ao enviar e-mail:', error);
  } else {
    console.log('✅ E-mail enviado com sucesso!', info.response);
  }
});
```

Agora o destinatário verá um **e-mail estilizado**! 📩✨  

---

## 📎 Enviando **anexos** (Imagens, PDFs, etc.)  

O Nodemailer permite anexar **qualquer tipo de arquivo** no e-mail.  

```js
const mailOptions = {
  from: 'seuemail@gmail.com',
  to: 'destinatario@example.com',
  subject: 'E-mail com Anexo 📎',
  text: 'Aqui está o arquivo que você pediu!',
  attachments: [
    {
      filename: 'arquivo.pdf',
      path: './arquivo.pdf' // Caminho do arquivo no servidor
    }
  ]
};

transporter.sendMail(mailOptions, (error, info) => {
  if (error) {
    console.log('❌ Erro ao enviar e-mail:', error);
  } else {
    console.log('✅ E-mail enviado com anexo!', info.response);
  }
});
```

Isso envia um **PDF anexado** no e-mail! 🎯  

---

## 🔑 Usando **OAuth2** para Gmail (Mais seguro)  

Se não quiser expor sua senha, use **OAuth2** para autenticar no Gmail.  

### 📌 Instale o pacote OAuth2  
```bash
npm install googleapis
```

### 🔹 Configure o transporter com OAuth2  
```js
const { google } = require('googleapis');
const OAuth2 = google.auth.OAuth2;

const oauth2Client = new OAuth2(
  'SEU_CLIENT_ID',
  'SEU_CLIENT_SECRET',
  'https://developers.google.com/oauthplayground'
);

oauth2Client.setCredentials({ refresh_token: 'SEU_REFRESH_TOKEN' });

const accessToken = await oauth2Client.getAccessToken();

const transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    type: 'OAuth2',
    user: 'seuemail@gmail.com',
    clientId: 'SEU_CLIENT_ID',
    clientSecret: 'SEU_CLIENT_SECRET',
    refreshToken: 'SEU_REFRESH_TOKEN',
    accessToken: accessToken
  }
});
```

Agora você pode enviar e-mails **sem expor sua senha**! 🔥  

---

## 📩 Enviando e-mails **em massa**  

Se quiser enviar um e-mail para **vários destinatários**, basta separar os e-mails por `,`:

```js
const mailOptions = {
  from: 'seuemail@gmail.com',
  to: 'email1@example.com, email2@example.com, email3@example.com',
  subject: 'E-mail em Massa',
  text: 'Este é um e-mail enviado para múltiplos destinatários!'
};

transporter.sendMail(mailOptions, (error, info) => {
  if (error) {
    console.log('❌ Erro ao enviar e-mail:', error);
  } else {
    console.log('✅ E-mails enviados!', info.response);
  }
});
```

⚠️ **Dica:** Se for enviar **muitos e-mails**, use serviços como **Mailgun, SendGrid ou AWS SES**, para evitar bloqueios.

---

## 📜 Usando Templates de E-mail  

Se quiser usar **templates de e-mail dinâmicos**, podemos usar **handlebars**:

### 📌 Instale a lib handlebars  
```bash
npm install nodemailer-express-handlebars
```

### 🔹 Configure o handlebars no transporter  
```js
const hbs = require('nodemailer-express-handlebars');
const path = require('path');

transporter.use('compile', hbs({
  viewEngine: {
    extName: '.hbs',
    partialsDir: path.join(__dirname, 'views'),
    layoutsDir: path.join(__dirname, 'views'),
    defaultLayout: '',
  },
  viewPath: path.join(__dirname, 'views'),
  extName: '.hbs'
}));
```

Agora crie um arquivo de template `views/email.hbs`:
```html
<h1>Olá, {{nome}}! 🚀</h1>
<p>Seu código de verificação é <b>{{codigo}}</b></p>
```

### 🔹 Enviando e-mail com template  
```js
const mailOptions = {
  from: 'seuemail@gmail.com',
  to: 'destinatario@example.com',
  subject: 'Seu Código de Verificação',
  template: 'email',
  context: { nome: 'João', codigo: '123456' }
};

transporter.sendMail(mailOptions);
```

Isso envia um e-mail **dinâmico**, substituindo `{{nome}}` e `{{codigo}}` pelos valores! 🎯  

---

## 🏁 Conclusão  

O **Nodemailer** é a melhor opção para **enviar e-mails no Node.js**.  

### 🎯 Resumo:  
✅ **Envio via SMTP (Gmail, Outlook, Yahoo, etc.)**  
✅ **Suporte a HTML e templates**  
✅ **Anexos e imagens**  
✅ **E-mails em massa**  
✅ **Suporte a OAuth2 para mais segurança**  

Agora é só testar no seu projeto! 🚀🔥