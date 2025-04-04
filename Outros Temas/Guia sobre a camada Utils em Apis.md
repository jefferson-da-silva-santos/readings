# 🧰 Guia Definitivo (e Divertido) sobre a Camada **Utils**: O Canivete Suíço da Sua API 🛠️

Sabe aquelas funções que não se encaixam em lugar nenhum, mas são úteis em TODO lugar?  
Então, essas belezinhas vão morar na **camada `utils`**.

---

## 🧙‍♂️ O que é a camada Utils?

É uma **coleção de funções, helpers e ferramentas genéricas**, que:

✅ Não têm dependência de regra de negócio  
✅ Podem ser reutilizadas por qualquer parte da aplicação  
✅ Tornam seu código mais limpo, DRY e organizado

> É tipo aquele amigo que sempre tem um cabo USB na mochila: você nunca sabe quando vai precisar, mas quando precisa, salva sua vida.

---

## 📌 Exemplos clássicos de coisas que vão pra `utils`

- Formatadores de datas  
- Validação de CPF, CNPJ, e-mails  
- Geração de tokens  
- Criptografia de senhas  
- Cálculos genéricos  
- Conversões de unidades  
- Geração de códigos aleatórios  
- Slugs, normalização de strings  
- Verificações simples (`isEmpty`, `isValidUUID`)

---

## 📁 Onde ela mora na arquitetura?

```bash
src/
├── controllers/
├── services/
├── repositories/
├── routes/
├── dtos/
└── utils/
     ├── dateUtils.js
     ├── stringUtils.js
     ├── validateCPF.js
     └── ...
```

---

## 📘 Exemplo em Node.js

### 📄 `utils/dateUtils.js`
```js
function formatDateToBR(date) {
  return new Intl.DateTimeFormat('pt-BR').format(new Date(date));
}

function getCurrentYear() {
  return new Date().getFullYear();
}

module.exports = { formatDateToBR, getCurrentYear };
```

### Usando em qualquer parte da app:
```js
const { formatDateToBR } = require('../utils/dateUtils');

console.log(formatDateToBR('2025-04-04')); // "04/04/2025"
```

---

## ☕ Exemplo em Java

### 📄 `utils/StringUtils.java`
```java
public class StringUtils {

    public static boolean isNullOrEmpty(String s) {
        return s == null || s.trim().isEmpty();
    }

    public static String slugify(String input) {
        return input.toLowerCase().replaceAll("[^a-z0-9]", "-");
    }
}
```

### Usando em qualquer lugar:
```java
String name = "Java é Lindo!";
String slug = StringUtils.slugify(name); // java-é-lindo
```

---

## 🤓 Boas práticas

### 1️⃣ **Não misture regras de negócio**

❌ Utils não deve acessar banco de dados  
❌ Utils não deve chamar services ou controllers  
✅ Utils deve ser genérico e independente

---

### 2️⃣ **Reutilize com gosto**

Se você repetir o mesmo código em mais de 2 arquivos… **você tem um candidato a utilidade**!

---

### 3️⃣ **Nomeie com clareza**

Diga o que o utilitário faz. Nada de `helper1`, `funcoes.js`, `utilsStuff`. 😒

✅ `dateUtils.js`  
✅ `generateToken.js`  
✅ `validateCPF.js`

---

### 4️⃣ **Organize por tipo**

Evite colocar tudo num arquivo só. Prefira separar por categoria (stringUtils, mathUtils, cryptoUtils, etc.)

---

### 5️⃣ **Teste seus utilitários!**

Eles são pequenos, mas podem ser usados em toda a aplicação. Um errinho aqui vira efeito borboleta no sistema. 🦋

---

## 📦 Bônus: Dica pra exportação em Node.js

Se tiver muitos arquivos, você pode fazer um `index.js` dentro de `/utils`:

### 📄 `utils/index.js`
```js
module.exports = {
  ...require('./dateUtils'),
  ...require('./stringUtils'),
  ...require('./validateCPF')
};
```

Agora no código:
```js
const { formatDateToBR, isValidCPF } = require('../utils');
```

Fica chique, né? 😎

---

## 🏁 Conclusão: `Utils` são seus ajudantes de confiança 💼

Pense na camada utils como sua **biblioteca pessoal** de soluções. Bem feita, ela te poupa **tempo**, **repetição** e **código feio**.

> "Quando a lógica não sabe onde morar, mas você sabe que vai usar muito… ela vai pra camada `utils`." 💡

---

Se quiser, posso gerar um mini pacote de utilitários prontos com testes unitários pra você começar a usar no seu projeto. Só pedir! 🚀