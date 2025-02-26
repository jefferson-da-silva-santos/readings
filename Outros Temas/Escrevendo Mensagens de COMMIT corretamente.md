# 📌 Guia Definitivo (e Divertido) para Escrever Mensagens de Commit no Seu Projeto 🚀

Ah, o **commit**... Uma das partes mais importantes do seu fluxo de trabalho de desenvolvimento, mas também um campo minado se você não prestar atenção! Escrever boas mensagens de commit é como deixar um bilhete carinhoso para o futuro (e para seus colegas de equipe). Você vai querer que aquele bilhete te diga: "Aqui, olha o que foi feito!", e não: "Não faça isso novamente". 😆 Então, bora aprender a escrever mensagens de commit de forma clara, objetiva e engraçada (porque programador também tem senso de humor)! 

---

## 🎯 Regras de Ouro para Escrever Mensagens de Commit

### 1️⃣ **Seja Claro e Objetivo: Nada de Enrolação!** 🗣️

Uma boa mensagem de commit deve explicar **o que foi feito**, de forma concisa e sem rodeios. Não escreva um romance, mas também não seja vago demais. Lembre-se: o commit deve ser útil tanto para você no futuro quanto para qualquer outro programador que venha a ler.

❌ **Errado:**
```
Alterações feitas
```

✅ **Certo:**
```
Corrige erro ao carregar dados do usuário na tela inicial
```

Aqui, não estamos só dizendo que algo foi alterado, estamos explicando o que foi alterado e por quê! Isso facilita a vida de todos, especialmente quando a história do código fica mais longa.

---

### 2️⃣ **Use o Presente do Indicativo: Ação, Baby!** ✨

Mensagens de commit devem ser escritas no tempo verbal presente. Imagine que você está contando o que está acontecendo **agora**, no momento em que o commit é feito. Então, use **verbos de ação** para descrever o que está sendo feito.

❌ **Errado:**
```
Corrigido erro ao tentar salvar o formulário
```

✅ **Certo:**
```
Corrige erro ao tentar salvar o formulário
```

No tempo presente, fica claro que o commit refere-se a uma ação em andamento. Isso ajuda a manter a consistência no histórico do projeto.

---

### 3️⃣ **Comece com um Verbão Forte (e não um Fraco)!** 💪

Comece suas mensagens com um verbo no imperativo que transmita a ação claramente. Evite iniciar com palavras vagas ou com frases no passado. 

❌ **Errado:**
```
Foi adicionada funcionalidade de login
```

✅ **Certo:**
```
Adiciona funcionalidade de login
```

Assim, a ação fica mais direta, mais objetiva e mais fácil de entender. Como se fosse uma instrução direta, como: "Ei, eu fiz isso agora!"

---

### 4️⃣ **Separando a Introdução do Detalhe** 📝

Se o commit for complexo e exigir mais explicações, use a **primeira linha** para descrever o que foi feito de maneira rápida e depois, no **corpo**, explique o porquê e os detalhes técnicos. Não transforme a mensagem em um texto gigante, só dê mais contexto quando necessário.

❌ **Errado:**
```
Implementa sistema de login, validação, integração com banco de dados, e cache.
A implementação foi necessária para melhorar o desempenho e fornecer segurança ao acessar o sistema.
```

✅ **Certo:**
```
Implementa sistema de login

- Adiciona validação de dados de entrada
- Integra com banco de dados para autenticação
- Adiciona cache para otimizar desempenho
```

A primeira linha explica rapidamente o que foi feito, e as linhas subsequentes dão detalhes, organizadas de maneira clara e estruturada.

---

### 5️⃣ **Evite Mensagens de Commit Abstratas** 🕵️‍♂️

O **commit** não é um lugar para mensagens misteriosas. "Stuff", "algumas mudanças", "atualização", são mensagens que não dizem absolutamente nada. Seja **específico**!

❌ **Errado:**
```
Stuff
```

✅ **Certo:**
```
Refatora função de cálculo de imposto para melhorar precisão
```

Aqui, não há mistério, sabemos exatamente o que foi feito, e quem vai ler o commit também vai saber.

---

### 6️⃣ **Não Deixe de Informar o Problema** 🚨

Se o commit resolve um problema ou bug, **explique qual problema foi resolvido**. Isso ajuda a rastrear a origem de erros, além de ser útil quando alguém precisar fazer um "revert" ou investigar a mudança mais tarde.

❌ **Errado:**
```
Ajustes feitos
```

✅ **Certo:**
```
Corrige bug de layout no formulário de cadastro em telas pequenas
```

Aqui, estamos explicando qual erro foi corrigido e onde ele estava acontecendo. Isso facilita a manutenção do código no futuro e ajuda a entender a evolução do projeto.

---

### 7️⃣ **Evite Usar Gírias e Abreviações Confusas** 🕵️‍♀️

Embora você tenha todo o direito de ser criativo na hora de escrever suas mensagens de commit, tenha em mente que outros desenvolvedores vão ler isso no futuro. Usar gírias, abreviações ou termos internos pode confundir, então evite.

❌ **Errado:**
```
Fixes bug in the log-in, pls no more bugs.
```

✅ **Certo:**
```
Corrige erro de autenticação no login, previne falhas na validação do usuário
```

Aqui, a mensagem é clara, profissional e, o mais importante, útil para os outros. Diga o que fez de forma direta, sem apelidos, sem gírias!

---

### 8️⃣ **Use Mensagens de Commit para Contar Histórias** 📖

Pense nas mensagens de commit como uma maneira de contar a história do seu código. Se você estava em uma missão (ou em um ciclo de debug), descreva isso de forma envolvente. O commit é o ponto de virada onde você salva a "história" do que aconteceu no código.

❌ **Errado:**
```
Bug fix
```

✅ **Certo:**
```
Corrige falha que causava o travamento ao abrir a tela de perfil
```

O commit aqui não é só "um fix". Ele narra a falha e o que foi feito para corrigir o problema, facilitando a vida de quem for lidar com isso no futuro.

---

## 🎉 Conclusão: Mensagens de Commit Bem Feitas São um Abraço no Futuro! 🤗

Se você ainda acha que escrever mensagens de commit não é importante, pense no seguinte: daqui a alguns meses, você (ou alguém da sua equipe) terá que entender o que foi feito naquele commit. Escrever de maneira clara, objetiva e específica vai evitar que você se perca no meio de um mar de commits vagos e confusos.

Lembre-se: **comprometa-se com clareza e a comunicação é tudo**! Bom código começa com bons commits, e bons commits começam com boas mensagens. Então, vai lá e espalha a arte de escrever commits perfeitos por aí! 🚀✨

Agora, vai lá e **commite** com estilo! 😎