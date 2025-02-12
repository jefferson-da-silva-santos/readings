## 🕒 Tudo sobre **date-fns** – Manipulação de Datas no Node.js e JavaScript

Se você já tentou mexer com datas em JavaScript, sabe que a API nativa pode ser meio chata. O `date-fns` é um dos melhores jeitos de **formatar, manipular e trabalhar com datas** no **Node.js e no front-end**. Vamos ver **por que ele é útil, como instalar e explorar suas funcionalidades principais**.

---

## 🛠 Por que usar o `date-fns`?
O `date-fns` é uma **biblioteca moderna e eficiente para trabalhar com datas**. Ele traz uma abordagem **funcional**, onde você importa só o que precisa, sem carregar um monstro de código (como o Moment.js fazia).

✅ **Leve** (só importa os módulos que você usa)  
✅ **Sem mutabilidade** (não modifica o objeto original, sempre retorna um novo)  
✅ **Fácil de usar** (funções com nomes descritivos)  
✅ **Suporte a internacionalização** (formatos de datas de vários países)  
✅ **Compatível com TypeScript**  

---

## 📦 Instalando o `date-fns`
Para usar no seu projeto Node.js ou no front, basta instalar:

```bash
npm install date-fns
```
ou
```bash
yarn add date-fns
```

Depois, importa no código:

```js
const { format } = require('date-fns');
```
Se estiver usando **ESModules**, faz assim:
```js
import { format } from 'date-fns';
```

---

## ⏳ Como pegar a data atual?
Vamos começar pelo básico: pegar a data de **agora**.

```js
const { format } = require('date-fns');

const agora = new Date();
console.log(agora); // Retorna um objeto Date
```

Isso retorna algo como:
```
2025-02-11T15:20:00.000Z
```

---

## 📅 Formatando datas
O `date-fns` tem a função **`format`** que te permite exibir a data do jeito que quiser.

```js
const { format } = require('date-fns');

const agora = new Date();
console.log(format(agora, 'dd/MM/yyyy')); // 11/02/2025
console.log(format(agora, 'dd-MM-yyyy HH:mm:ss')); // 11-02-2025 15:20:45
console.log(format(agora, 'EEEE, dd MMMM yyyy')); // Tuesday, 11 February 2025
```

### 📖 Alguns padrões de formatação úteis:
| Formato | Saída |
|---------|-------|
| `yyyy`  | 2025  |
| `yy`    | 25    |
| `MMMM`  | February |
| `MMM`   | Feb   |
| `MM`    | 02    |
| `dd`    | 11    |
| `EEEE`  | Tuesday |
| `EEE`   | Tue    |
| `HH`    | 15 (horário 24h) |
| `hh`    | 03 (horário 12h) |
| `mm`    | 20 (minutos) |
| `ss`    | 45 (segundos) |
| `a`     | PM ou AM |

Se precisar exibir no formato brasileiro:
```js
console.log(format(new Date(), "dd 'de' MMMM 'de' yyyy", { locale: require('date-fns/locale/pt-BR') }));
// 11 de fevereiro de 2025
```

---

## 📆 Adicionando ou removendo tempo de uma data
Às vezes, você precisa calcular prazos, vencimentos, etc.

### 🟢 Adicionar tempo
```js
const { addDays, addMonths, addYears } = require('date-fns');

const hoje = new Date();

console.log(addDays(hoje, 10)); // +10 dias
console.log(addMonths(hoje, 3)); // +3 meses
console.log(addYears(hoje, 1)); // +1 ano
```

### 🔴 Remover tempo
```js
const { subDays, subMonths, subYears } = require('date-fns');

console.log(subDays(hoje, 5)); // -5 dias
console.log(subMonths(hoje, 2)); // -2 meses
console.log(subYears(hoje, 1)); // -1 ano
```

---

## 📊 Diferença entre datas
Quer saber quantos dias faltam para um evento? Usa `differenceInDays`:

```js
const { differenceInDays, differenceInHours } = require('date-fns');

const data1 = new Date(2025, 2, 20);
const data2 = new Date(2025, 2, 11);

console.log(differenceInDays(data1, data2)); // 9 dias
console.log(differenceInHours(data1, data2)); // 216 horas
```

Isso é ótimo para calcular **tempo restante para vencimentos**!

---

## 📅 Comparando datas
Precisa saber se uma data é antes ou depois de outra?

```js
const { isBefore, isAfter, isEqual } = require('date-fns');

const dataA = new Date(2025, 2, 11);
const dataB = new Date(2025, 2, 20);

console.log(isBefore(dataA, dataB)); // true
console.log(isAfter(dataA, dataB)); // false
console.log(isEqual(dataA, dataB)); // false
```

Isso ajuda muito em **validações**, tipo verificar se um boleto já venceu.

---

## ⏳ Verificando se uma data está dentro de um intervalo
Se precisar ver se uma data está entre duas outras:

```js
const { isWithinInterval } = require('date-fns');

const hoje = new Date();
const inicio = new Date(2025, 1, 1);
const fim = new Date(2025, 2, 15);

console.log(isWithinInterval(hoje, { start: inicio, end: fim }));
```

Isso retorna `true` se `hoje` estiver no intervalo.

---

## 📆 Pegando o início ou o fim do dia, mês ou ano
Às vezes você quer zerar o horário de uma data:

```js
const { startOfDay, endOfDay, startOfMonth, endOfMonth, startOfYear, endOfYear } = require('date-fns');

const agora = new Date();

console.log(startOfDay(agora));  // 2025-02-11T00:00:00.000Z
console.log(endOfDay(agora));    // 2025-02-11T23:59:59.999Z

console.log(startOfMonth(agora)); // Primeiro dia do mês
console.log(endOfMonth(agora));   // Último dia do mês

console.log(startOfYear(agora)); // 1º de Janeiro do ano
console.log(endOfYear(agora));   // 31 de Dezembro do ano
```

Isso é útil para **relatórios e filtros por períodos**.

---

## 🌍 Trabalhando com fusos horários (`tz`)
Se precisar lidar com **fusos horários**, instale o pacote extra:

```bash
npm install date-fns-tz
```

E use:

```js
const { format } = require('date-fns-tz');

const agora = new Date();
const fusoSP = 'America/Sao_Paulo';

console.log(format(agora, 'yyyy-MM-dd HH:mm:ssXXX', { timeZone: fusoSP }));
// 2025-02-11 12:30:00-03:00
```

Isso resolve problemas com **horário de verão e conversões de fuso**.

---

## 🏁 Conclusão
O `date-fns` facilita **formatar, manipular e comparar datas** no Node.js e no front. Ele é **rápido, modular e melhor que soluções antigas como o Moment.js**.

### 🎯 Resumo rápido:
✅ **Formatar** → `format(new Date(), 'dd/MM/yyyy')`  
✅ **Adicionar/remover tempo** → `addDays()`, `subMonths()`  
✅ **Diferença entre datas** → `differenceInDays()`  
✅ **Comparação de datas** → `isBefore()`, `isAfter()`  
✅ **Fusos horários** → `date-fns-tz`  

Agora é só testar no seu projeto! 🚀🔥