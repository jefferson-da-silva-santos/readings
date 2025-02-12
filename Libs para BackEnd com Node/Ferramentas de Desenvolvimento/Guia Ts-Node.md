# 🚀 Tudo sobre **ts-node** – Executando TypeScript no Node.js

Se você está desenvolvendo em **TypeScript**, sabe que normalmente é necessário **transpilar o código para JavaScript** antes de rodar no Node.js. Mas e se você pudesse **rodar TypeScript diretamente no Node** sem precisar compilar antes? 🎯  

O **ts-node** resolve exatamente esse problema! Ele permite executar arquivos **TypeScript diretamente**, sem necessidade de um build manual.  

Aqui vou te mostrar **como instalar, configurar e usar o `ts-node` na prática**. 🔥  

---

## 📌 O que é o **ts-node**?
O `ts-node` é um interpretador para **TypeScript** no **Node.js**, permitindo rodar `.ts` sem precisar compilar manualmente para `.js`.  

✅ **Roda arquivos TypeScript diretamente no Node**  
✅ **Ótimo para desenvolvimento e scripts rápidos**  
✅ **Funciona com ESM (ECMAScript Modules) e CommonJS**  
✅ **Integração com `nodemon` para recarregamento automático**  
✅ **Ótimo para rodar testes e CLIs**  

Agora bora instalar e testar! 🚀  

---

## 📦 Instalando o **ts-node**  

Para instalar globalmente (pode rodar em qualquer lugar):  
```bash
npm install -g ts-node
```

Ou instalar localmente no seu projeto:  
```bash
npm install --save-dev ts-node
```

Se estiver usando **yarn**:  
```bash
yarn add -D ts-node
```

Agora, crie um arquivo **`index.ts`** e coloque um código TypeScript básico:

```ts
const nome: string = 'TypeScript';
console.log(`🚀 Olá, ${nome}!`);
```

Para rodar esse código sem precisar compilar manualmente, basta executar:  

```bash
ts-node index.ts
```

E pronto! Você rodou TypeScript diretamente no Node! 🔥  

---

## ⚡ Usando **ts-node** no modo watch (`--watch`)
Se quiser que o código **se atualize automaticamente** quando fizer mudanças, use a flag `--watch`:  

```bash
ts-node --watch index.ts
```

Agora, toda vez que salvar o arquivo, ele **vai recarregar automaticamente**!  

💡 **Dica:** Para um recarregamento ainda mais eficiente, use **`nodemon`** junto com `ts-node`:
```bash
npx nodemon --exec ts-node index.ts
```
Isso deixa o ambiente **ainda mais dinâmico**! 🚀

---

## 🔥 Rodando um projeto TypeScript inteiro (`tsconfig.json`)
Se o seu projeto já tem um **`tsconfig.json`**, o `ts-node` vai **respeitar suas configurações** automaticamente.  

Basta rodar:
```bash
ts-node src/index.ts
```

Se precisar passar um `tsconfig.json` diferente:
```bash
ts-node --project tsconfig.prod.json src/index.ts
```

Isso ajuda a **rodar diferentes configurações** dependendo do ambiente (desenvolvimento, produção, testes, etc.).

---

## 🔵 Habilitando ES Modules (`--loader ts-node/esm`)
Se estiver usando **ES Modules (ECMAScript Modules)** no TypeScript, adicione `"type": "module"` no `package.json`:

```json
{
  "type": "module"
}
```

E rode o `ts-node` com a flag `--loader ts-node/esm`:
```bash
node --loader ts-node/esm index.ts
```

Agora você pode usar `import` e `export` normalmente! 🔥  

---

## 🟢 Executando um script TypeScript sem instalar localmente
Se quiser rodar um arquivo `.ts` sem precisar instalar `ts-node` no projeto, use `npx`:

```bash
npx ts-node script.ts
```

Isso é útil para **testar rapidamente códigos TypeScript sem precisar configurar um projeto inteiro**.

---

## 🔄 Integração com Jest (Rodando testes em TypeScript)
Se você usa **Jest** para testes e quer rodar TypeScript sem compilar antes, instale:

```bash
npm install --save-dev ts-jest @types/jest
```

Depois, configure o Jest para rodar com `ts-node`:

```bash
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node'
};
```

Agora você pode rodar os testes **diretamente em TypeScript**! 🚀  

---

## 🏎️ Melhorando a performance (`--transpile-only`)
O `ts-node` normalmente **verifica erros de tipo** antes de rodar o código, o que pode deixar a execução mais lenta.  

Se quiser rodar **mais rápido**, use a flag `--transpile-only`:

```bash
ts-node --transpile-only index.ts
```

Isso **ignora a verificação de tipos** e só transpila o código, rodando mais rápido.

---

## 🏁 Conclusão  

O `ts-node` é essencial para quem **trabalha com TypeScript** e quer rodar código **sem precisar compilar antes**. Ele deixa o fluxo de desenvolvimento **muito mais ágil**.  

### 🎯 Resumo:
✅ **Roda TypeScript diretamente no Node.js**  
✅ **Suporte a ES Modules e CommonJS**  
✅ **Modo watch (`--watch`) para recarregamento automático**  
✅ **Rodando projetos inteiros (`tsconfig.json`)**  
✅ **Mais rápido com `--transpile-only`**  
✅ **Ótimo para rodar testes e scripts**  

Se você usa **TypeScript no dia a dia**, o `ts-node` vai facilitar sua vida! 🚀🔥