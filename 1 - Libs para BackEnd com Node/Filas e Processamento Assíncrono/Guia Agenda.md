# ⏳ Tudo sobre **Agenda.js** – Agendando Tarefas no Node.js  

Se você precisa **executar tarefas agendadas** no **Node.js**, o **Agenda.js** é uma das melhores opções! Com ele, você pode criar **cron jobs**, **tarefas recorrentes** e até **jobs em filas**, tudo isso integrado com **MongoDB**.  

Aqui vou te mostrar **como instalar, configurar e usar o Agenda.js** para criar agendamentos no seu projeto. 🚀  

---

## 🚀 O que é o **Agenda.js**?  
O **Agenda.js** é uma biblioteca para **agendamento de tarefas** no **Node.js**, baseada no **MongoDB**. Ele permite:  

✅ **Agendar tarefas recorrentes (cron jobs)**  
✅ **Executar tarefas assíncronas**  
✅ **Definir jobs únicos ou repetitivos**  
✅ **Gerenciar jobs em filas**  
✅ **Reexecutar jobs falhos automaticamente**  

Se sua aplicação precisa de **notificações automáticas, envio de e-mails programados, geração de relatórios ou qualquer ação recorrente**, o **Agenda.js** é uma excelente escolha! 🔥  

---

## 📦 Instalando o Agenda.js  

Para usar no seu projeto, instale com:  

```bash
npm install agenda
```
ou, se estiver usando **yarn**:  
```bash
yarn add agenda
```

Agora, bora conectar ao **MongoDB** e rodar nossos jobs! 🚀  

---

## 🔌 Configurando o **Agenda.js** com MongoDB  

Antes de começar, você precisa de um **banco de dados MongoDB** rodando localmente ou no **MongoDB Atlas**.  

Crie um arquivo `agenda.js` e adicione:  

```js
const Agenda = require('agenda');

const agenda = new Agenda({ db: { address: 'mongodb://localhost:27017/agendaDB' } });

agenda.on('ready', () => console.log('✅ Agenda conectada ao MongoDB!'));
agenda.on('error', err => console.error('❌ Erro na Agenda:', err));

module.exports = agenda;
```

Agora que temos a conexão configurada, bora criar nossos **jobs**! 💪  

---

## ⏰ Criando um Job Básico  

Vamos criar um **job que roda a cada minuto** e imprime uma mensagem no console:  

```js
const agenda = require('./agenda');

agenda.define('meu job', async job => {
  console.log(`🔥 Executando tarefa: ${job.attrs.name} - ${new Date()}`);
});

// Iniciando o Agenda
(async () => {
  await agenda.start();
  await agenda.every('1 minute', 'meu job');
})();
```

Agora, execute o script:  
```bash
node agenda.js
```

E veja a mágica acontecer! ✨ O **job será executado a cada minuto**.

---

## 🕒 Agendando Tarefas Recorrentes  

Podemos agendar tarefas de várias formas. Aqui estão alguns exemplos:

### 🔹 Executar **a cada minuto**  
```js
await agenda.every('1 minute', 'meu job');
```

### 🔹 Executar **todo dia às 9h**  
```js
await agenda.every('0 9 * * *', 'enviar email');
```

### 🔹 Executar **toda segunda-feira às 8h**  
```js
await agenda.every('0 8 * * 1', 'gerar relatório semanal');
```

Esses padrões seguem a **sintaxe de cron**. Se não sabe usar, teste seu cron [aqui](https://crontab.guru/)!

---

## 🟢 Agendando Jobs para Executar **Uma Única Vez**  

Se quiser executar um job apenas **uma vez em um horário específico**, use `.schedule()`:  

```js
await agenda.schedule('in 5 minutes', 'enviar lembrete');
```

Isso agenda o job para rodar **daqui a 5 minutos**. ⏳  

Se quiser rodar em um horário exato:  

```js
await agenda.schedule('2025-02-12T10:00:00', 'ligar para cliente');
```

Isso roda o job **exatamente às 10h do dia 12 de fevereiro de 2025**.

---

## 🔵 Passando **dados para um Job**  

Se precisar passar **informações extras** para o job, use `data()`:  

```js
agenda.define('enviar email', async job => {
  const { email, mensagem } = job.attrs.data;
  console.log(`📩 Enviando e-mail para ${email}: ${mensagem}`);
});

await agenda.schedule('in 1 minute', 'enviar email', {
  email: 'usuario@example.com',
  mensagem: 'Olá! Este é um e-mail de teste do Agenda.js!'
});
```

Agora, quando o job rodar, ele **terá os dados** que passamos!

---

## 🔥 Executando um Job **manual**  

Se quiser rodar um job **imediatamente**, sem precisar agendar:  

```js
await agenda.now('meu job');
```

Isso executa o job na hora. 🚀  

---

## 🟡 Listando Jobs Agendados  

Se quiser ver **todos os jobs pendentes**, faça:  

```js
const jobs = await agenda.jobs({});
console.log(jobs);
```

Se quiser filtrar por nome:  

```js
const jobs = await agenda.jobs({ name: 'meu job' });
```

---

## 🔴 Cancelando ou Deletando um Job  

Se precisar **cancelar um job agendado** antes de ele rodar:  

```js
await agenda.cancel({ name: 'enviar email' });
console.log('🚫 Job cancelado!');
```

Isso remove **todos os jobs com esse nome** do banco.

Se quiser **deletar apenas um job específico**, use o `_id` do job:  

```js
await agenda.cancel({ _id: '65abcd1234ef567890123456' });
console.log('🚫 Job deletado!');
```

---

## ⚡ Lidando com Falhas e Reexecução  

Se um job **falhar**, você pode definir **quantas vezes ele pode ser reexecutado** antes de desistir:  

```js
agenda.define('processar pedido', async job => {
  throw new Error('Algo deu errado! 😢'); // Simulando erro
}, { lockLifetime: 10000 });

await agenda.every('1 minute', 'processar pedido');

// Se falhar, ele tenta novamente até 3 vezes
agenda.on('fail:processar pedido', async job => {
  console.log(`❌ Job falhou! Tentativa: ${job.attrs.failCount}`);

  if (job.attrs.failCount < 3) {
    await job.save(); // Reagendar o job automaticamente
  }
});
```

Agora, se um job falhar, ele **tenta novamente até 3 vezes** antes de desistir. 🔄  

---

## 🎯 Resumo Final  

O **Agenda.js** é uma excelente ferramenta para **agendamento de tarefas** no Node.js. Ele é ideal para **notificações automáticas, envio de e-mails, geração de relatórios e tarefas recorrentes**.  

✅ **Fácil integração com MongoDB**  
✅ **Agendamento com cron jobs ou horários específicos**  
✅ **Passagem de dados personalizados para os jobs**  
✅ **Reexecução automática de jobs falhos**  
✅ **Listagem, cancelamento e gerenciamento de jobs**  

Agora é só testar no seu projeto! 🚀🔥