# 📧 Tudo sobre **Mailgun.js** – Enviando E-mails no Node.js  

Se você precisa enviar e-mails no **Node.js**, uma das melhores opções é o **Mailgun**. Ele é um **serviço de e-mail transacional** que permite enviar **e-mails automáticos, notificações e newsletters** de forma fácil e confiável.  

Aqui vou te mostrar **o que é, como configurar e como enviar e-mails no seu projeto Node.js usando Mailgun.js**! 🚀  

---

## 📌 O que é o **Mailgun.js**?  
O **Mailgun** é um serviço de e-mails baseado na nuvem, focado em **entrega de alta performance**. Ele permite enviar **e-mails transacionais e marketing** com uma API simples e poderosa.

### 🔹 Por que usar o Mailgun.js?  
✅ **Fácil de integrar** – API simples baseada em REST  
✅ **Alta taxa de entrega** – Evita cair no spam  
✅ **Suporte a templates** – E-mails HTML prontos  
✅ **Monitoramento de envios** – Logs e estatísticas  
✅ **Escalável** – Perfeito para sistemas com grande volume de e-mails  

Se você precisa enviar **e-mails automáticos, boletins ou notificações**, o Mailgun é uma excelente opção!  

---

## 📦 Instalando o **Mailgun.js**  
Para começar, instale o SDK do Mailgun:  

```bash
npm install mailgun.js form-data
```

Depois, importe no seu código:  

```js
const Mailgun = require('mailgun.js');
const formData = require('form-data');
```

Agora, bora configurar a API. 🚀  

---

## 🔑 Configurando o Mailgun  
Para usar o Mailgun, você precisa de **duas informações principais**:  

1️⃣ **API Key** – Pegue no painel do Mailgun  
2️⃣ **Domínio** – Pode ser um domínio próprio ou o sandbox do Mailgun  

### 🔹 Configurando o Mailgun.js  

```js
const Mailgun = require('mailgun.js');
const formData = require('form-data');

const mailgun = new Mailgun(formData);
const client = mailgun.client({
  username: 'api',
  key: 'SUA_API_KEY_AQUI' // Pegue no Mailgun
});
```

Agora estamos prontos para enviar e-mails! 🚀  

---

## ✉️ Enviando um e-mail básico  
A forma mais simples de enviar um e-mail:

```js
(async () => {
  try {
    const message = await client.messages.create('SEU_DOMINIO_AQUI', {
      from: 'Seu Nome <seuemail@seudominio.com>',
      to: ['destinatario@example.com'],
      subject: '🚀 Teste de E-mail com Mailgun.js',
      text: 'Este é um e-mail enviado pelo Mailgun.js no Node.js!'
    });

    console.log('📨 E-mail enviado!', message);
  } catch (error) {
    console.error('❌ Erro ao enviar e-mail:', error);
  }
})();
```

🔹 **Explicação do código:**  
- `from`: O remetente do e-mail  
- `to`: Lista de destinatários  
- `subject`: Assunto do e-mail  
- `text`: Corpo do e-mail (somente texto)  

Se rodar esse código e tudo estiver certo, o e-mail será enviado com sucesso! ✅  

---

## 🎨 Enviando um e-mail com **HTML**  
Se quiser enviar um **e-mail formatado**, use `html` em vez de `text`:  

```js
(async () => {
  try {
    const message = await client.messages.create('SEU_DOMINIO_AQUI', {
      from: 'Seu Nome <seuemail@seudominio.com>',
      to: ['destinatario@example.com'],
      subject: '💡 Teste de E-mail com HTML',
      html: '<h1>Olá!</h1><p>Este é um <strong>e-mail HTML</strong> enviado pelo Mailgun.js!</p>'
    });

    console.log('📨 E-mail enviado!', message);
  } catch (error) {
    console.error('❌ Erro ao enviar e-mail:', error);
  }
})();
```

Agora o e-mail será enviado com **formatação HTML**. 📩  

---

## 📎 Enviando e-mails com anexos  
Se precisar **anexar arquivos**, pode fazer assim:

```js
(async () => {
  try {
    const message = await client.messages.create('SEU_DOMINIO_AQUI', {
      from: 'Seu Nome <seuemail@seudominio.com>',
      to: ['destinatario@example.com'],
      subject: '📎 Teste de Anexos',
      text: 'Veja o arquivo em anexo.',
      attachment: ['/caminho/para/arquivo.pdf'] // Caminho do arquivo no servidor
    });

    console.log('📨 E-mail enviado com anexo!', message);
  } catch (error) {
    console.error('❌ Erro ao enviar e-mail:', error);
  }
})();
```

Agora o destinatário receberá um **e-mail com anexo**. 📝  

---

## 📊 Obtendo logs de envios  
Se quiser ver o **status dos e-mails enviados**, pode usar `client.messages.get`:  

```js
(async () => {
  try {
    const logs = await client.messages.list('SEU_DOMINIO_AQUI');
    console.log('📊 Logs de envio:', logs);
  } catch (error) {
    console.error('❌ Erro ao buscar logs:', error);
  }
})();
```

Isso retorna informações sobre os e-mails enviados, como status e falhas.

---

## ⚡ Enviando e-mails em massa  
Se precisar **enviar o mesmo e-mail para várias pessoas**, basta adicionar **múltiplos destinatários** na lista `to`:  

```js
(async () => {
  try {
    const message = await client.messages.create('SEU_DOMINIO_AQUI', {
      from: 'Seu Nome <seuemail@seudominio.com>',
      to: ['usuario1@example.com', 'usuario2@example.com', 'usuario3@example.com'],
      subject: '📢 E-mail para múltiplos destinatários',
      text: 'Este e-mail foi enviado para várias pessoas ao mesmo tempo!'
    });

    console.log('📨 E-mails enviados!', message);
  } catch (error) {
    console.error('❌ Erro ao enviar e-mails:', error);
  }
})();
```

Agora todos os destinatários receberão o mesmo e-mail. 📬  

---

## 🛡️ Melhorando segurança com **domínios verificados**  
Se você estiver usando o Mailgun em produção, o ideal é **usar um domínio verificado** em vez do **sandbox**.

1️⃣ **Vá até o painel do Mailgun**  
2️⃣ **Adicione um domínio próprio**  
3️⃣ **Configure os registros DNS (SPF, DKIM, MX)**  
4️⃣ **Espere a verificação e pronto!**  

Isso melhora a **entregabilidade** dos e-mails e evita cair no spam. 📢  

---

## 🏁 Conclusão  
O **Mailgun.js** é uma ferramenta poderosa para enviar **e-mails automáticos, notificações e campanhas** no Node.js.  

### 🎯 Resumo:  
✅ **Fácil de usar** – API REST simples  
✅ **Alta taxa de entrega** – Evita spam  
✅ **Suporte a e-mails HTML e anexos**  
✅ **Escalável para grandes volumes**  
✅ **Monitoramento de envios com logs**  

Se você precisa **enviar e-mails de forma confiável**, o **Mailgun.js** é uma excelente escolha! 🚀📧