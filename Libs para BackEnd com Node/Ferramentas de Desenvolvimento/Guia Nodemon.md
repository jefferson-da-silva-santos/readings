# 🔥 Tudo sobre **Nodemon** – Reiniciando o Servidor Automaticamente no Node.js  

Se você já desenvolveu um projeto com **Node.js**, sabe como é **chato** ter que reiniciar o servidor manualmente toda vez que faz uma alteração no código. 😩  

O **Nodemon** resolve esse problema monitorando os arquivos do projeto e **reiniciando automaticamente** sempre que houver uma mudança.  

Aqui vou te mostrar **como instalar, configurar e usar o Nodemon da melhor forma possível**. 🚀  

---

## 🚀 O que é o **Nodemon**?  

O **Nodemon** é uma ferramenta que **monitora mudanças nos arquivos do seu projeto** e **reinicia automaticamente o servidor** toda vez que você salva uma alteração.  

✅ **Evita reiniciar o servidor manualmente**  
✅ **Facilita o desenvolvimento e aumenta a produtividade**  
✅ **Monitora arquivos automaticamente**  
✅ **Compatível com qualquer aplicação Node.js**  
✅ **Totalmente configurável (ignorar arquivos, mudar extensões, etc.)**  

Agora bora instalar e usar! 🎯  

---

## 📦 Instalando o Nodemon  

O Nodemon pode ser instalado de duas formas: **globalmente** ou **localmente no projeto**.  

### 🔹 Instalar globalmente (para usar em qualquer projeto)  
```bash
npm install -g nodemon
```
Agora você pode rodar **qualquer script Node.js** com o comando:  
```bash
nodemon server.js
```

### 🔹 Instalar localmente no projeto  
```bash
npm install --save-dev nodemon
```
Isso adiciona o Nodemon como **dependência de desenvolvimento** (`devDependencies`).  

Agora, edite o `package.json` e adicione um **script de inicialização**:

```json
"scripts": {
  "start": "node server.js",
  "dev": "nodemon server.js"
}
```

Agora, para rodar o servidor com **Nodemon**, use:  
```bash
npm run dev
```

---

## 🔥 Usando o Nodemon para monitorar o servidor  

Agora, sempre que você alterar o código, o Nodemon **reinicia o servidor automaticamente**.  

Crie um **servidor básico** com Express:  

```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('🔥 Servidor rodando com Nodemon!');
});

app.listen(3000, () => console.log('🚀 Servidor rodando na porta 3000'));
```

Agora, inicie o servidor com:
```bash
nodemon server.js
```

Agora, **edite o código e salve**. O Nodemon detecta a alteração e **reinicia automaticamente**! 🎯  

---

## ⚙️ Configurando o Nodemon  

O Nodemon permite criar um arquivo **nodemon.json** para configurações personalizadas.  

Crie o arquivo `nodemon.json` na raiz do projeto:  

```json
{
  "watch": ["src"],  
  "ext": "js,json",  
  "ignore": ["node_modules"],  
  "exec": "node server.js"
}
```

🔹 **watch** → Monitora mudanças apenas dentro da pasta `src`  
🔹 **ext** → Monitora apenas arquivos `.js` e `.json`  
🔹 **ignore** → Ignora a pasta `node_modules`  
🔹 **exec** → Define o comando para rodar o servidor  

Agora, rode:
```bash
nodemon
```

Agora o Nodemon só reinicia quando houver mudanças dentro da **pasta src** e ignora arquivos desnecessários! 🚀  

---

## ⏳ Definindo tempo de reinício  

Se quiser que o Nodemon **espere um pouco antes de reiniciar**, use o `--delay`:  

```bash
nodemon --delay 2 server.js
```

Isso faz com que o Nodemon **aguarde 2 segundos** antes de reiniciar o servidor.

---

## 🚀 Rodando scripts diferentes com Nodemon  

Você pode usar Nodemon para rodar **outros comandos além de Node.js**.  

```bash
nodemon --exec "npm test"
```

Isso faz com que os testes sejam executados automaticamente sempre que um arquivo for alterado.

---

## 📜 Verificando logs do Nodemon  

Se quiser ver logs detalhados sobre **o que o Nodemon está fazendo**, use:  

```bash
nodemon --verbose server.js
```

Isso mostra **cada arquivo monitorado** e **o motivo dos reinícios**.

---

## 🏁 Conclusão  

O **Nodemon** é uma ferramenta essencial para qualquer desenvolvedor **Node.js**, pois **automatiza o restart do servidor** e **aumenta a produtividade**.  

### 🎯 Resumo:
✅ **Monitora alterações no código e reinicia automaticamente**  
✅ **Fácil de configurar e personalizar**  
✅ **Evita o trabalho manual de reiniciar o servidor**  
✅ **Compatível com qualquer aplicação Node.js**  

Agora, é só instalar e usar no seu projeto! 🚀🔥