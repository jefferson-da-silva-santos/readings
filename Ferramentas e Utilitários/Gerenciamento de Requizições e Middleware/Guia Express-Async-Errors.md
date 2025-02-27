# 🚀 Tudo sobre **express-async-errors** – Lidando com erros assíncronos no Express

Se você já trabalhou com **Express** e tentou capturar erros em funções **async**, sabe que pode ser meio chato. 😤 O Express não lida automaticamente com erros assíncronos, então você precisa usar `try/catch` em **todas as rotas** ou então um `next(error)`.  

O pacote **express-async-errors** resolve isso de um jeito muito mais **simples e automático**. Bora ver **o que ele faz, como instalar e como usar na prática**. 🔥

---

## ⚠️ O problema do Express com erros assíncronos
O Express, por padrão, **não trata exceções lançadas dentro de funções async**. Olha só:

```js
const express = require('express');
const app = express();

app.get('/erro', async (req, res) => {
  throw new Error('Algo deu errado!');
});

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

Agora, acessa `http://localhost:3000/erro` e... BOOM! 💥 O servidor trava e dá erro no console.

```
(node:12345) UnhandledPromiseRejectionWarning: Error: Algo deu errado!
```

Isso acontece porque o Express **não captura exceções de funções assíncronas automaticamente**.

A solução padrão seria usar um `try/catch`:

```js
app.get('/erro', async (req, res, next) => {
  try {
    throw new Error('Algo deu errado!');
  } catch (error) {
    next(error); // Passa o erro para o middleware de tratamento
  }
});
```

Funciona, mas **ninguém quer escrever isso em toda rota, né?** 😩

---

## 🛠 Instalando o **express-async-errors**
Agora entra o **express-async-errors**, que resolve esse problema **de forma automática**.

Instala no projeto com:

```bash
npm install express-async-errors
```

Depois, importa ele **logo no início do seu código**:

```js
require('express-async-errors');
```

Agora, qualquer erro assíncrono será capturado automaticamente. 😎

---

## ✅ Usando **express-async-errors** na prática
Depois de importar **express-async-errors**, o Express já vai capturar exceções assíncronas sem precisar de `try/catch`.

```js
const express = require('express');
require('express-async-errors'); // Importa ANTES das rotas

const app = express();

app.get('/erro', async (req, res) => {
  throw new Error('Algo deu errado!'); // Agora o Express captura automaticamente!
});

// Middleware global de erro
app.use((err, req, res, next) => {
  console.error('🔥 Erro capturado:', err.message);
  res.status(500).json({ erro: err.message });
});

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

Agora, se acessar `http://localhost:3000/erro`, o servidor **não vai mais travar** e retorna uma resposta JSON:

```json
{
  "erro": "Algo deu errado!"
}
```

---

## 🔄 Como funciona o **express-async-errors**?
Basicamente, esse pacote **"envolve" todas as rotas assíncronas** com um `try/catch` automaticamente.  

🔹 **Antes do pacote**:
```js
app.get('/rota', async (req, res, next) => {
  try {
    // Código que pode dar erro
  } catch (error) {
    next(error);
  }
});
```

🔹 **Depois do pacote**:
```js
app.get('/rota', async (req, res) => {
  // Se der erro, o Express captura automaticamente
});
```

O middleware de erro recebe qualquer exceção lançada e retorna uma resposta adequada.

---

## ⚡ Tratando erros específicos
Se quiser **personalizar as respostas de erro**, pode verificar o tipo do erro no middleware:

```js
app.use((err, req, res, next) => {
  if (err.name === 'ValidationError') {
    return res.status(400).json({ erro: 'Erro de validação!' });
  }

  console.error('🔥 Erro interno:', err.message);
  res.status(500).json({ erro: 'Erro interno do servidor' });
});
```

Agora, se um erro for do tipo **ValidationError**, ele retorna `400` em vez de `500`.

---

## 🤔 Preciso mesmo do **express-async-errors**?
Se você já está **usando Express com async/await**, **sim!**  
Caso contrário, **vai precisar usar `try/catch` em todas as rotas**, o que é chato e feio.  

**O que esse pacote faz?**  
✅ Captura automaticamente erros de **funções async**  
✅ Evita **crash do servidor** por erros não tratados  
✅ Funciona **sem precisar alterar nada no código**  

---

## 🏁 Conclusão
O **express-async-errors** é um pacotinho **pequeno, mas poderoso**, que **salva vidas** em APIs Express. Ele faz **o Express entender erros assíncronos sem precisar de `try/catch` em toda rota**.

### 🎯 Resumo:
✅ **Instalação simples:** `npm install express-async-errors`  
✅ **Importa no início:** `require('express-async-errors')`  
✅ **Sem precisar modificar nada no código**  
✅ **Evita crash do servidor**  

Agora é só testar no seu projeto! 🚀🔥