# Lista de Exercícios sobre Context Api (Api de Contexto) do React. 🚀

### 🚀 **Exercício 1: Criando um Tema Global**
**Objetivo:** Criar um tema global utilizando a Context API para permitir a alternância entre "light" e "dark" em toda a aplicação.

**Enunciado:**
Você foi contratado para implementar um sistema de tema global para um aplicativo React. O tema pode ser "light" ou "dark", e deve ser compartilhado por todos os componentes. Utilize a **Context API** para criar um contexto de tema e implementar a alternância de tema.

1. Crie o contexto de tema com `React.createContext()`, com o valor inicial sendo "light".
2. Crie um provedor `ThemeContext.Provider` no seu componente `App`.
3. Crie um botão para alternar entre "light" e "dark" e passe o estado do tema via contexto.
4. Crie um componente `ThemedComponent` que consuma o contexto de tema e altere sua aparência conforme o valor do tema.

**Resultado Esperado:**
- A troca de tema deve ser refletida em todos os componentes que consomem o contexto.

---

### 🚀 **Exercício 2: Compartilhando Informações do Usuário**
**Objetivo:** Criar um contexto para armazenar as informações do usuário logado e compartilhá-las entre diferentes componentes.

**Enunciado:**
Você está criando um sistema de autenticação para um site de comércio eletrônico. Quando o usuário fizer login, você deve armazenar informações como nome e email, e essas informações devem ser acessíveis por qualquer parte da aplicação.

1. Crie o contexto `UserContext` com valores iniciais de `null` para o nome e email.
2. Implemente um provedor que armazene essas informações.
3. Crie um componente `UserProfile` que consome esse contexto e exiba o nome e email do usuário.
4. Adicione uma funcionalidade para atualizar o nome e email do usuário a partir de um formulário.

**Resultado Esperado:**
- O perfil do usuário deve ser mostrado em qualquer componente que consome o contexto, e qualquer mudança de nome ou email deve ser refletida imediatamente.

---

### 🚀 **Exercício 3: Controle de Carrinho de Compras**
**Objetivo:** Criar um contexto para gerenciar o carrinho de compras, com funcionalidades para adicionar e remover itens.

**Enunciado:**
Você está desenvolvendo um sistema de carrinho de compras para um e-commerce. Utilize a Context API para compartilhar o estado do carrinho entre vários componentes, permitindo adicionar e remover produtos.

1. Crie o contexto `CartContext` que armazene um array de itens no carrinho.
2. Crie uma função `addItem` para adicionar itens ao carrinho e uma função `removeItem` para remover.
3. Crie um componente `CartSummary` que consome o contexto e exibe os itens no carrinho.
4. Adicione um botão de "Adicionar ao Carrinho" em um componente `Product` que, ao ser clicado, adicione o produto ao carrinho.

**Resultado Esperado:**
- O carrinho deve ser compartilhado entre os componentes, e a adição ou remoção de produtos deve ser refletida na interface do usuário.

---

### 🚀 **Exercício 4: Configurações de Preferências do Usuário**
**Objetivo:** Implementar um sistema de preferências de idioma global utilizando a Context API.

**Enunciado:**
Você está criando uma aplicação multilíngue e precisa implementar um sistema de preferências de idioma. O idioma selecionado deve ser acessível em toda a aplicação e permitir a troca entre idiomas.

1. Crie um contexto `LanguageContext` com um valor inicial de `en` (inglês).
2. Implemente um provedor que permita alterar o idioma global da aplicação.
3. Crie um componente `LanguageSelector` que permita ao usuário selecionar entre `en` (inglês) e `pt` (português).
4. Crie um componente `WelcomeMessage` que consuma o contexto de idioma e exiba uma saudação ("Hello" ou "Olá") com base no idioma selecionado.

**Resultado Esperado:**
- O idioma da aplicação deve mudar dinamicamente, e todos os componentes que consomem o contexto de idioma devem exibir as traduções corretas.

---

### 🚀 **Exercício 5: Controle de Acesso com Permissões**
**Objetivo:** Utilizar a Context API para controlar o acesso a diferentes áreas de um sistema, com base nas permissões do usuário.

**Enunciado:**
Você está criando um painel administrativo onde diferentes usuários têm diferentes níveis de permissão (admin, usuário comum, etc.). Utilize a Context API para controlar quem tem acesso a qual área do sistema.

1. Crie um contexto `AuthContext` com informações sobre o usuário, como `role` (admin, user).
2. Implemente um provedor que forneça o estado de autenticação e as permissões do usuário.
3. Crie um componente `AdminDashboard` que só deve ser exibido se o usuário for um "admin".
4. Crie um componente `UserDashboard` que deve ser exibido para qualquer usuário logado, independentemente do tipo.

**Resultado Esperado:**
- O acesso às áreas do sistema deve ser controlado de acordo com as permissões definidas no contexto, e os componentes devem ser exibidos ou ocultados com base nas permissões.

---

### 🚀 **Exercício 6: Gerenciamento de Estado de Formulário Global**
**Objetivo:** Gerenciar o estado de um formulário (nome, email e mensagem) utilizando a Context API, para que os dados possam ser acessados e alterados por qualquer parte da aplicação.

**Enunciado:**
Você está criando um formulário de contato em um site e precisa de uma maneira de compartilhar e gerenciar os dados do formulário em toda a aplicação. Utilize a Context API para armazenar e atualizar os dados do formulário.

1. Crie um contexto `FormContext` para armazenar os campos do formulário (`name`, `email`, `message`).
2. Implemente um provedor para compartilhar o estado do formulário.
3. Crie um componente `Form` que consome o contexto e exibe um formulário com os campos de nome, email e mensagem.
4. Crie um componente `FormSummary` que consome o contexto e exibe os dados inseridos no formulário.

**Resultado Esperado:**
- O estado do formulário deve ser acessível e atualizado globalmente, permitindo que qualquer componente que consome o contexto veja as mudanças em tempo real.

---

Esses exercícios devem ajudá-lo a dominar o uso da **Context API do React** em diferentes cenários! Cada um aborda uma aplicação prática, ajudando você a entender como essa poderosa ferramenta pode ser usada em várias situações. 😎🚀
