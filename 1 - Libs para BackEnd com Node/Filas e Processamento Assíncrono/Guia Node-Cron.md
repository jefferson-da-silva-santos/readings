# ⏰ Tudo sobre **node-cron** – Agendando Tarefas no Node.js

Se você precisa **executar tarefas automaticamente em horários programados** no seu projeto Node.js, o **node-cron** é a ferramenta ideal! Com ele, você pode **agendar execuções recorrentes**, como:

✅ **Enviar e-mails automaticamente** 📨  
✅ **Fazer backup do banco de dados** 💾  
✅ **Coletar dados de uma API periodicamente** 🔄  
✅ **Limpar logs e arquivos temporários** 🗑️  
✅ **Notificações diárias via WhatsApp ou SMS** 📲  

Aqui, vou te mostrar **como instalar, configurar e usar o `node-cron` na prática**! 🚀  

---

## 📦 Instalando o **node-cron**  

Para adicionar o `node-cron` ao seu projeto, basta rodar:  

```bash
npm install node-cron
```
ou, se estiver usando **yarn**:  
```bash
yarn add node-cron
```

Depois, importe no seu código:

```js
const cron = require('node-cron');
```

Agora bora criar nossas primeiras tarefas agendadas! ⏳✨  

---

## 🚀 Criando uma tarefa **agendada**  

Aqui está um exemplo básico que exibe `"Executando tarefa..."` **a cada minuto**:

```js
const cron = require('node-cron');

cron.schedule('* * * * *', () => {
  console.log('⏳ Executando tarefa a cada minuto:', new Date().toLocaleTimeString());
});
```

Agora, execute o script:

```bash
node index.js
```

Você verá a mensagem aparecendo **a cada minuto** no terminal! 😎  

---

## ⏳ Como funciona a sintaxe de agendamento?  

O formato do `node-cron` segue **cinco campos** que representam a frequência da execução:  

```
*  *  *  *  *  
|  |  |  |  |  
|  |  |  |  └── Dia da semana (0 - 7) → 0 e 7 = Domingo  
|  |  |  └──── Mês (1 - 12)  
|  |  └────── Dia do mês (1 - 31)  
|  └──────── Hora (0 - 23)  
└────────── Minuto (0 - 59)  
```

### 🔹 Exemplos de expressões cron  

| Expressão | Execução |
|-----------|---------|
| `* * * * *` | A cada **minuto** |
| `0 * * * *` | A cada **hora cheia** |
| `0 0 * * *` | **Todo dia** à meia-noite |
| `0 9 * * 1-5` | Segunda a sexta, **às 09:00** |
| `*/5 * * * *` | A cada **5 minutos** |
| `30 14 * * *` | Todo dia **às 14:30** |
| `0 0 1 * *` | Todo **primeiro dia do mês** |
| `0 0 * * 0` | Todo **domingo** à meia-noite |

Agora vamos usar isso na prática! 🔥  

---

## 🔵 Executando uma função **todo dia às 8h da manhã**  

```js
cron.schedule('0 8 * * *', () => {
  console.log('📅 Bom dia! Executando tarefa diária às 08:00 da manhã.');
});
```

Isso é útil para **relatórios diários, notificações, backups, etc.** 📩  

---

## 🔥 Enviando um **e-mail automaticamente** todo dia às 9h  

Se você quiser **enviar um e-mail automaticamente** usando `node-cron` + `nodemailer`, faça assim:  

```js
const nodemailer = require('nodemailer');
const cron = require('node-cron');

// Configurando o transporter do Nodemailer
const transporter = nodemailer.createTransport({
  service: 'gmail',
  auth: {
    user: 'seuemail@gmail.com',
    pass: 'suasenha'
  }
});

// Agendando o envio de e-mail diário às 09:00
cron.schedule('0 9 * * *', async () => {
  try {
    await transporter.sendMail({
      from: 'seuemail@gmail.com',
      to: 'destinatario@example.com',
      subject: 'Relatório Diário 📊',
      text: 'Bom dia! Aqui está o seu relatório diário. 📄'
    });

    console.log('✅ E-mail enviado com sucesso às 09:00!');
  } catch (error) {
    console.error('❌ Erro ao enviar e-mail:', error);
  }
});
```

Agora, todo dia às **09:00**, um e-mail será enviado! 🚀💌  

---

## 💾 **Fazendo Backup do Banco de Dados** toda meia-noite  

Se você quer **criar um backup diário do banco de dados**, pode rodar um script SQL automaticamente:  

```js
const exec = require('child_process').exec;
const cron = require('node-cron');

cron.schedule('0 0 * * *', () => {
  console.log('📂 Criando backup do banco de dados...');
  
  exec('mysqldump -u usuario -p senha meu_banco > backup.sql', (error) => {
    if (error) {
      console.error('❌ Erro ao criar backup:', error);
    } else {
      console.log('✅ Backup do banco concluído!');
    }
  });
});
```

Agora, todo dia à **meia-noite**, um **backup do banco será gerado automaticamente**! 🔥  

---

## 🗑️ **Limpando arquivos temporários** automaticamente  

Se você quer **excluir logs e arquivos temporários** periodicamente, pode usar este código:  

```js
const fs = require('fs');
const path = require('path');
const cron = require('node-cron');

cron.schedule('0 0 * * 0', () => { // Todo domingo à meia-noite
  console.log('🗑️ Limpando arquivos temporários...');

  fs.rmSync(path.join(__dirname, 'temp'), { recursive: true, force: true });

  console.log('✅ Arquivos temporários deletados com sucesso!');
});
```

Agora, todo **domingo à meia-noite**, a pasta `temp/` será apagada. 🔥  

---

## ⏸️ **Parando uma tarefa programada**  

Se precisar **iniciar e parar uma tarefa dinamicamente**, use `task.stop()` e `task.start()`:  

```js
const task = cron.schedule('*/10 * * * * *', () => {
  console.log('🔥 Esta tarefa executa a cada 10 segundos...');
}, { scheduled: false }); // Começa desativada

// Iniciar a tarefa após 5 segundos
setTimeout(() => {
  console.log('✅ Iniciando a tarefa...');
  task.start();
}, 5000);

// Parar a tarefa após 30 segundos
setTimeout(() => {
  console.log('🛑 Parando a tarefa...');
  task.stop();
}, 30000);
```

Isso **inicia a tarefa depois de 5 segundos** e **para depois de 30 segundos**.  

---

## 🏁 Conclusão  

O **node-cron** é uma ferramenta poderosa para **agendar tarefas recorrentes** no Node.js.  

### 🎯 Resumo:  
✅ **Executa tarefas automaticamente (por minuto, hora, dia, etc.)**  
✅ **Ideal para backups, e-mails, notificações e limpeza de dados**  
✅ **Suporte a logs e monitoramento**  
✅ **Simples e eficiente para rodar no backend**  

Agora é só testar no seu projeto! 🚀🔥