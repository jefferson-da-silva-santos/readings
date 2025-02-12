# 🔒 Tudo sobre **bcryptjs** – Hash de Senhas no Node.js

Se você já trabalhou com autenticação de usuários no **Node.js**, sabe que **guardar senhas puras no banco é um erro grave**. A solução? **Hashear as senhas!** E é aí que entra o **bcryptjs**.

Neste guia, vou te mostrar **o que é, como funciona, como usar e boas práticas** para **hashing de senhas e comparação segura** no seu projeto.

---

## 🚀 O que é o **bcryptjs**?
O **bcryptjs** é uma biblioteca para **criptografar senhas usando o algoritmo bcrypt**. Ele transforma a senha em um **hash seguro e irreversível**, protegendo os dados dos usuários caso o banco de dados seja comprometido.

🔹 **Por que não guardar senhas puras no banco?**  
Se um hacker invadir seu banco e pegar as senhas **sem criptografia**, ele pode logar como qualquer usuário. **Já era!** 😨  
Por isso, a **boa prática** é guardar **apenas o hash da senha**.

🔹 **Por que usar bcryptjs e não outro método?**  
✅ O bcrypt foi projetado para ser **lento**, dificultando ataques de força bruta  
✅ Ele usa **salting**, adicionando uma camada extra de proteção  
✅ Mesmo que duas pessoas tenham a mesma senha, os hashes serão diferentes  
✅ Tem suporte para **Node.js** e **JavaScript puro** (não depende de C++ como o `bcrypt`)  

Agora, bora ver como **usar o bcryptjs na prática**! 💪

---

## 📦 Instalando o **bcryptjs**
Para usar no seu projeto Node.js, instala com:

```bash
npm install bcryptjs
```
ou, se estiver usando **yarn**:
```bash
yarn add bcryptjs
```

Depois, importa no seu código:

```js
const bcrypt = require('bcryptjs');
```
Se estiver usando **ESModules**, faz assim:
```js
import bcrypt from 'bcryptjs';
```

---

## 🔐 Gerando um hash de senha
O primeiro passo é transformar a senha do usuário em um **hash seguro** antes de salvar no banco.

```js
const bcrypt = require('bcryptjs');

const senha = 'minhaSenhaSuperSecreta';

(async () => {
  const salt = await bcrypt.genSalt(10); // Gera um salt
  const hash = await bcrypt.hash(senha, salt); // Gera o hash

  console.log('Senha original:', senha);
  console.log('Hash gerado:', hash);
})();
```

🔹 **O que acontece aqui?**  
1️⃣ `genSalt(10)` cria um **salt aleatório** com custo 10  
2️⃣ `hash(senha, salt)` aplica a senha no algoritmo bcrypt + salt  
3️⃣ O hash gerado é salvo no banco, e **não dá para revertê-lo!**  

**Exemplo de hash gerado:**
```
$2a$10$J8UkmZfFZJ8UkmZfFZy5OuMfR8EexT8uoN35Hoz9QJzQX8uP9y5Q2
```
Esse hash é o que será salvo no banco.

---

## 🔄 Comparando a senha inserida com o hash salvo
Na hora do **login**, o usuário digita a senha e precisamos comparar com o hash salvo no banco.

```js
const bcrypt = require('bcryptjs');

const senhaDigitada = 'minhaSenhaSuperSecreta';
const hashSalvoNoBanco = '$2a$10$J8UkmZfFZJ8UkmZfFZy5OuMfR8EexT8uoN35Hoz9QJzQX8uP9y5Q2'; // Exemplo

(async () => {
  const senhaCorreta = await bcrypt.compare(senhaDigitada, hashSalvoNoBanco);

  if (senhaCorreta) {
    console.log('✅ Senha correta! Usuário autenticado.');
  } else {
    console.log('❌ Senha incorreta! Acesso negado.');
  }
})();
```

🔹 **O que acontece aqui?**  
1️⃣ `compare(senhaDigitada, hashSalvoNoBanco)` pega a senha e compara com o hash  
2️⃣ Se a senha estiver correta, retorna **true**, senão retorna **false**  
3️⃣ **Mesmo que o hash seja diferente da senha original, bcrypt sabe como validar!**  

---

## 🧂 O que é esse tal de **Salt**?
O **Salt** é um valor aleatório adicionado à senha antes de ser hasheada. Isso impede ataques como **Rainbow Table**, onde hackers usam dicionários de hashes prontos.

Sem **salt**, se duas pessoas tivessem a mesma senha, o hash delas seria igual. Com o **salt**, os hashes são diferentes, mesmo que a senha seja igual!

🔹 **Exemplo sem salt** (hashes iguais para a mesma senha):
```
Senha: "123456"
Hash1: abcdefg123...
Hash2: abcdefg123...
```

🔹 **Exemplo com salt** (hashes diferentes para a mesma senha):
```
Senha: "123456"
Hash1: a1b2c3d4e5...
Hash2: f6g7h8i9j0...
```

Por isso, **SEMPRE** use **salts** ao gerar hashes!

---

## ⚡ Qual valor de **Salt Rounds** devo usar?
O `bcrypt.genSalt(10)` usa um número de **rounds** para gerar o salt. Quanto maior, mais seguro, mas também mais lento.

**Recomendações:**
🔹 **8** – Rápido, mas menos seguro  
🔹 **10** – Melhor equilíbrio entre segurança e performance (padrão recomendado)  
🔹 **12 ou mais** – Segurança maior, mas pode deixar o login lento  

Exemplo de salt mais forte:
```js
const salt = await bcrypt.genSalt(12);
```
**Se for um sistema muito acessado, um `salt` alto pode deixar a autenticação lenta.**

---

## 🏎️ Hashing **síncrono** vs **assíncrono**
O bcrypt permite gerar hashes de forma **síncrona** e **assíncrona**.

### 🕒 Método assíncrono (recomendado para produção):
```js
const hash = await bcrypt.hash('senha', 10);
```
✅ **Não trava o event loop**  
✅ **Ideal para servidores**  
✅ **Mais rápido em sistemas escaláveis**  

### 🕑 Método síncrono (não recomendado para servidores):
```js
const hash = bcrypt.hashSync('senha', 10);
```
❌ **Pode travar o event loop**  
❌ **Ruim para aplicações que precisam ser rápidas**  
❌ **Só use se for um script rodando isolado**  

---

## 🏁 Conclusão
O **bcryptjs** é essencial para segurança de senhas no Node.js. Ele garante que **mesmo se o banco de dados for invadido, as senhas não possam ser facilmente descobertas**.

### 🎯 Resumo:
✅ **Gera hashes seguros** para senhas  
✅ **Usa salt para evitar ataques de dicionário**  
✅ **Protege contra ataques de força bruta**  
✅ **Permite comparar senha inserida com hash armazenado**  
✅ **Roda tanto no Node.js quanto no navegador**  

### 🔥 Exemplo completo de uso:
```js
const bcrypt = require('bcryptjs');

(async () => {
  const senha = 'superSegura123';

  // Gerando hash
  const salt = await bcrypt.genSalt(10);
  const hash = await bcrypt.hash(senha, salt);
  console.log('Hash da senha:', hash);

  // Comparando senha digitada
  const senhaCorreta = await bcrypt.compare('superSegura123', hash);
  console.log('Senha correta?', senhaCorreta);
})();
```

Agora você pode usar o `bcryptjs` no seu sistema para proteger senhas com segurança! 🚀🔒