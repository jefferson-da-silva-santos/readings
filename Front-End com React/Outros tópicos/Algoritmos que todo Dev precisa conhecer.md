# 📌 Guia Definitivo (e Divertido) para Algoritmos que Todo Programador Deve Conhecer 🚀

Ah, os **algoritmos**... Se você é programador, você vai acabar se deparando com eles mais cedo ou mais tarde. Seja para resolver problemas do dia a dia, otimizar seu código ou simplesmente impressionar seu chefe (ou sua mãe), conhecer alguns **algoritmos clássicos** é fundamental. Então, se você quer se tornar um mestre da lógica de programação, vem com a gente!

Aqui vão alguns **algoritmos essenciais**, com exemplos em JavaScript, para deixar seu cérebro afiado e pronto para qualquer desafio. 💥

---

## 🎯 Algoritmos que Todo Programador Deveria Conhecer

### 1️⃣ **Algoritmo de Busca Linear (Linear Search)** 🔍

Às vezes, a solução mais simples é a melhor. O algoritmo de **busca linear** é usado para procurar um valor dentro de uma lista, verificando cada item um a um. A complexidade é **O(n)**, o que significa que, no pior caso, você terá que verificar todos os itens da lista.

#### Como usar:

```js
function linearSearch(arr, target) {
  for (let i = 0; i < arr.length; i++) {
    if (arr[i] === target) {
      return i; // Retorna o índice onde o alvo foi encontrado
    }
  }
  return -1; // Retorna -1 se o alvo não for encontrado
}

const numbers = [10, 20, 30, 40, 50];
const target = 30;
console.log(linearSearch(numbers, target)); // Saída: 2
```

Aqui, o algoritmo percorre a lista até encontrar o número `30` na posição `2`.

---

### 2️⃣ **Algoritmo de Busca Binária (Binary Search)** 🏹

Se sua lista está **ordenada**, você pode usar a **busca binária**, que é bem mais eficiente que a linear, com **complexidade O(log n)**. Ele divide a lista ao meio repetidamente até encontrar o valor.

#### Como usar:

```js
function binarySearch(arr, target) {
  let left = 0;
  let right = arr.length - 1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2);

    if (arr[mid] === target) {
      return mid; // Retorna o índice do elemento encontrado
    }
    if (arr[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }

  return -1; // Retorna -1 se o alvo não for encontrado
}

const numbers = [10, 20, 30, 40, 50];
const target = 30;
console.log(binarySearch(numbers, target)); // Saída: 2
```

A **busca binária** só funciona em listas ordenadas, mas sua eficiência é incrível!

---

### 3️⃣ **Ordenação por Bolha (Bubble Sort)** 🧑‍🔬

A **ordenação por bolha** é um dos algoritmos de ordenação mais simples, embora não seja muito eficiente. Ele compara elementos adjacentes e os troca de lugar até que a lista esteja ordenada.

#### Como usar:

```js
function bubbleSort(arr) {
  const n = arr.length;
  for (let i = 0; i < n - 1; i++) {
    for (let j = 0; j < n - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]]; // Troca os elementos
      }
    }
  }
  return arr;
}

const numbers = [64, 34, 25, 12, 22, 11, 90];
console.log(bubbleSort(numbers)); // Saída: [ 11, 12, 22, 25, 34, 64, 90 ]
```

Embora seja fácil de entender e implementar, o **bubble sort** tem complexidade O(n²), então ele não é o melhor para listas grandes.

---

### 4️⃣ **Ordenação Rápida (Quick Sort)** ⚡

Se você precisa de uma **ordenação eficiente**, o **Quick Sort** é uma excelente escolha. Sua complexidade média é O(n log n), e é muito mais rápido que o bubble sort para listas grandes.

#### Como usar:

```js
function quickSort(arr) {
  if (arr.length <= 1) {
    return arr;
  }

  const pivot = arr[0];
  const left = [];
  const right = [];

  for (let i = 1; i < arr.length; i++) {
    if (arr[i] < pivot) {
      left.push(arr[i]);
    } else {
      right.push(arr[i]);
    }
  }

  return [...quickSort(left), pivot, ...quickSort(right)];
}

const numbers = [64, 34, 25, 12, 22, 11, 90];
console.log(quickSort(numbers)); // Saída: [ 11, 12, 22, 25, 34, 64, 90 ]
```

Aqui, escolhemos um "pivô", dividimos os elementos menores e maiores e recursivamente ordenamos as partes. **Quick Sort** é muito rápido, mas tem a desvantagem de ser mais complexo de entender e implementar.

---

### 5️⃣ **Algoritmo de Fatorial (Factorial)** 🧮

O cálculo de **fatorial** é um exemplo clássico de recursão. O fatorial de um número é o produto de todos os números inteiros de 1 até esse número.

#### Como usar:

```js
function factorial(n) {
  if (n === 0) {
    return 1;
  }
  return n * factorial(n - 1);
}

console.log(factorial(5)); // Saída: 120
```

Aqui, usamos **recursão** para multiplicar o número `n` pelo fatorial de `n - 1`, até chegarmos a `0`, onde a função retorna `1`.

---

### 6️⃣ **Algoritmo de Fibonacci (Fibonacci)** 🔢

A **sequência de Fibonacci** é outra maravilha da recursão! Cada número é a soma dos dois números anteriores (com 0 e 1 como os dois primeiros números). Esse algoritmo é muito usado para otimizar problemas de programação dinâmica.

#### Como usar:

```js
function fibonacci(n) {
  if (n <= 1) {
    return n;
  }
  return fibonacci(n - 1) + fibonacci(n - 2);
}

console.log(fibonacci(6)); // Saída: 8
```

Esse código simples calcula o enésimo número da sequência de Fibonacci, mas **não é eficiente** para valores grandes de `n`. Há maneiras mais rápidas de fazer isso, como o uso de **memorização**.

---

### 7️⃣ **Algoritmo de Dijkstra (Shortest Path)** 🚦

O **algoritmo de Dijkstra** é usado para encontrar o caminho mais curto entre dois pontos em um grafo ponderado. Ele é muito utilizado em sistemas de navegação, como o Google Maps.

#### Como usar (exemplo simplificado):

```js
function dijkstra(graph, start) {
  const distances = {};
  const visited = new Set();

  // Inicialize as distâncias
  for (let node in graph) {
    distances[node] = Infinity;
  }
  distances[start] = 0;

  while (visited.size < Object.keys(graph).length) {
    // Pega o nó com a menor distância
    let closestNode = null;
    for (let node in graph) {
      if (!visited.has(node)) {
        if (closestNode === null || distances[node] < distances[closestNode]) {
          closestNode = node;
        }
      }
    }

    // Atualize as distâncias dos vizinhos
    for (let neighbor in graph[closestNode]) {
      const newDist = distances[closestNode] + graph[closestNode][neighbor];
      if (newDist < distances[neighbor]) {
        distances[neighbor] = newDist;
      }
    }

    visited.add(closestNode);
  }

  return distances;
}

const graph = {
  A: { B: 1, C: 4 },
  B: { A: 1, C: 2, D: 5 },
  C: { A: 4, B: 2, D: 1 },
  D: { B: 5, C: 1 }
};

console.log(dijkstra(graph, 'A')); // Saída: { A: 0, B: 1, C: 3, D: 4 }
```

O algoritmo de **Dijkstra** vai calcular a distância mínima para todos os nós a partir do nó de origem. É muito útil para resolver problemas de redes e otimização.

### 8️⃣ **Algoritmo de Pesquisa em Profundidade (DFS)** 🌳

A **pesquisa em profundidade (DFS)** é usada para explorar todos os nós de um grafo ou árvore, indo o mais fundo possível em cada ramo antes de retroceder. Essa abordagem é útil em problemas de busca, como jogos ou gráficos.

#### Como usar:

```js
function dfs(graph, start, visited = new Set()) {
  visited.add(start);
  console.log(start); // Imprime o nó visitado

  for (let neighbor of graph[start]) {
    if (!visited.has(neighbor)) {
      dfs(graph, neighbor, visited);
    }
  }
}

const graph = {
  A: ['B', 'C'],
  B: ['A', 'D', 'E'],
  C: ['A', 'F'],
  D: ['B'],
  E: ['B', 'F'],
  F: ['C', 'E']
};

dfs(graph, 'A'); // Saída: A B D E F C
```

Aqui, a função `dfs` vai percorrer o grafo começando do nó `A` e explorando profundamente antes de voltar.

---

### 9️⃣ **Algoritmo de Pesquisa em Largura (BFS)** 🌐

A **pesquisa em largura (BFS)** explora todos os nós de uma árvore ou grafo nível por nível. Ao contrário do DFS, que vai fundo, o BFS explora os nós mais próximos primeiro. É útil para encontrar o caminho mais curto em um grafo não ponderado.

#### Como usar:

```js
function bfs(graph, start) {
  const visited = new Set();
  const queue = [start];

  visited.add(start);

  while (queue.length > 0) {
    const node = queue.shift();
    console.log(node); // Imprime o nó visitado

    for (let neighbor of graph[node]) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push(neighbor);
      }
    }
  }
}

const graph = {
  A: ['B', 'C'],
  B: ['A', 'D', 'E'],
  C: ['A', 'F'],
  D: ['B'],
  E: ['B', 'F'],
  F: ['C', 'E']
};

bfs(graph, 'A'); // Saída: A B C D E F
```

Aqui, o algoritmo de **BFS** percorre todos os nós em largura, explorando os vizinhos de cada nó antes de ir mais fundo.

---

### 🔟 **Algoritmo de Ordenação por Inserção (Insertion Sort)** 🔄

A **ordenação por inserção** é um algoritmo simples onde os elementos são comparados e inseridos na posição correta dentro de uma lista ordenada. Sua complexidade é O(n²) no pior caso, mas é eficiente para listas pequenas ou quase ordenadas.

#### Como usar:

```js
function insertionSort(arr) {
  for (let i = 1; i < arr.length; i++) {
    let current = arr[i];
    let j = i - 1;

    // Move os elementos de arr[0..i-1] que são maiores que current
    while (j >= 0 && arr[j] > current) {
      arr[j + 1] = arr[j];
      j--;
    }

    // Insere current na posição correta
    arr[j + 1] = current;
  }
  return arr;
}

const numbers = [12, 11, 13, 5, 6];
console.log(insertionSort(numbers)); // Saída: [ 5, 6, 11, 12, 13 ]
```

Embora seja simples, o **insertion sort** é eficiente quando a lista já está quase ordenada. Ele pode ser uma boa escolha em situações específicas, mas para grandes volumes de dados, algoritmos como **merge sort** ou **quick sort** são melhores.

---

### 1️⃣1️⃣ **Algoritmo de Merge Sort** 🧩

O **Merge Sort** é um algoritmo de ordenação baseado na técnica de **dividir e conquistar**. Ele divide recursivamente a lista em partes menores, ordena cada parte e depois combina as partes ordenadas. Sua complexidade é **O(n log n)**, tornando-o muito eficiente para grandes volumes de dados.

#### Como usar:

```js
function mergeSort(arr) {
  if (arr.length <= 1) return arr;

  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));

  return merge(left, right);
}

function merge(left, right) {
  const result = [];
  let leftIndex = 0;
  let rightIndex = 0;

  while (leftIndex < left.length && rightIndex < right.length) {
    if (left[leftIndex] < right[rightIndex]) {
      result.push(left[leftIndex]);
      leftIndex++;
    } else {
      result.push(right[rightIndex]);
      rightIndex++;
    }
  }

  return result.concat(left.slice(leftIndex), right.slice(rightIndex));
}

const numbers = [38, 27, 43, 3, 9, 82, 10];
console.log(mergeSort(numbers)); // Saída: [ 3, 9, 10, 27, 38, 43, 82 ]
```

O **merge sort** é especialmente útil quando se lida com grandes quantidades de dados e deseja-se garantir uma performance eficiente.

---

### 1️⃣2️⃣ **Algoritmo de Knapsack (Mochila)** 🎒

O problema da **mochila** é um exemplo clássico de otimização e programação dinâmica. Dado um conjunto de itens com pesos e valores, você deve escolher um conjunto de itens cuja soma de pesos não ultrapasse uma capacidade máxima e que maximize o valor total.

#### Como usar:

```js
function knapsack(weights, values, capacity) {
  const n = weights.length;
  const dp = Array(n + 1).fill().map(() => Array(capacity + 1).fill(0));

  for (let i = 1; i <= n; i++) {
    for (let w = 1; w <= capacity; w++) {
      if (weights[i - 1] <= w) {
        dp[i][w] = Math.max(values[i - 1] + dp[i - 1][w - weights[i - 1]], dp[i - 1][w]);
      } else {
        dp[i][w] = dp[i - 1][w];
      }
    }
  }

  return dp[n][capacity];
}

const weights = [1, 2, 3, 8, 4, 5];
const values = [20, 5, 10, 40, 15, 25];
const capacity = 10;

console.log(knapsack(weights, values, capacity)); // Saída: 60
```

Aqui, o algoritmo calcula o valor máximo que pode ser transportado, dada uma capacidade limitada de carga.

---

### 1️⃣3️⃣ **Algoritmo de Floyd-Warshall** 🌍

O algoritmo de **Floyd-Warshall** é um algoritmo clássico para encontrar os **caminhos mais curtos entre todos os pares de nós em um grafo**. Ele é útil quando você precisa de uma solução geral para todos os caminhos possíveis em um grafo ponderado.

#### Como usar:

```js
function floydWarshall(graph) {
  const dist = [];
  
  // Inicializa a matriz de distâncias
  for (let i = 0; i < graph.length; i++) {
    dist[i] = [];
    for (let j = 0; j < graph.length; j++) {
      dist[i][j] = graph[i][j] !== Infinity ? graph[i][j] : Infinity;
    }
  }

  // Aplica o algoritmo de Floyd-Warshall
  for (let k = 0; k < graph.length; k++) {
    for (let i = 0; i < graph.length; i++) {
      for (let j = 0; j < graph.length; j++) {
        dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
      }
    }
  }

  return dist;
}

const graph = [
  [0, 3, Infinity, Infinity, Infinity, Infinity],
  [3, 0, 1, Infinity, Infinity, Infinity],
  [Infinity, 1, 0, 7, Infinity, 2],
  [Infinity, Infinity, 7, 0, 2, 3],
  [Infinity, Infinity, Infinity, 2, 0, 1],
  [Infinity, Infinity, 2, 3, 1, 0]
];

console.log(floydWarshall(graph));
```

Esse algoritmo pode ser útil para resolver problemas de otimização em grafos complexos, como os encontrados em redes de comunicação, logística, ou transporte.

---

## 🎉 Conclusão: Mais Algoritmos, Mais Poder! 💪

Agora você tem uma gama ainda maior de algoritmos essenciais para resolver uma variedade de problemas em programação! Do **DFS e BFS** para grafos até algoritmos de otimização como **Knapsack** e **Floyd-Warshall**, você tem as ferramentas para resolver problemas mais complexos e aumentar a eficiência do seu código.

Continue praticando e implementando esses algoritmos, e logo você será o mestre das técnicas de programação e da resolução de problemas! 😎

Boa sorte e que seu código seja sempre **rápido e eficiente**! 🚀