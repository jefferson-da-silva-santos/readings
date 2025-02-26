# 📌 Guia Definitivo (e Descomplicado) para Implementar Clean Architecture com Node.js e Express 🚀

Você já se perguntou como evitar o caos nos seus projetos e fazer eles se manterem organizados e escaláveis à medida que crescem? Bem-vindo ao universo da **Clean Architecture**! 😎 Vamos aprender a aplicar essa abordagem para criar sistemas robustos e de fácil manutenção, sem perder a cabeça no meio do caminho.

---

## 🎯 Regras de Ouro para uma Clean Architecture com Node.js e Express

### 1️⃣ **Divida suas responsabilidades em camadas bem definidas**

A Clean Architecture é toda sobre **separação de responsabilidades**. Cada parte do seu código deve ter um único motivo para mudar. Essa abordagem ajuda a manter o código mais modular, fácil de testar e, acima de tudo, **organizado**. Aqui estão as camadas principais:

- **Entities (Entidades):** Representam os dados do domínio (ex: usuários, pedidos).
- **Use Cases (Casos de uso):** Contêm a lógica de negócio (ex: registrar um usuário, calcular preço).
- **Interface Adapters (Adaptadores de interface):** São responsáveis por interagir com as interfaces externas (ex: controllers, routes).
- **Frameworks e Drivers:** Onde você configura seu servidor Express, banco de dados, etc.

Essa separação mantém as **entidades e lógica de negócio independentes de detalhes de implementação**, como o banco de dados ou a API.

--- 

### 2️⃣ **Use Dependências de Inversão (Inversion of Control - IoC)**

Em vez de deixar seus controllers e casos de uso conhecerem diretamente a implementação dos repositórios ou serviços externos (como bancos de dados ou APIs), use **injeção de dependência** para "inverter" esse fluxo. Isso facilita a substituição de componentes sem afetar a lógica de negócio.

🚫 **Errado:**
```js
const userController = new UserController(new MySQLUserRepository());
```

✅ **Certo:**
```js
// Em vez disso, use um injetor de dependência
const userController = new UserController(userRepository);
```

Com isso, você pode facilmente trocar `MySQLUserRepository` por um `MongoUserRepository` sem tocar em nenhum outro lugar do código. 💪

---

### 3️⃣ **Mantenha o código limpo com Serviços e Repositórios**

Não coloque toda a lógica dentro de seus controllers. **Use casos de uso e serviços** devem ser responsáveis pela lógica de negócio, enquanto os controllers devem apenas receber as requisições, chamar os serviços e devolver as respostas.

#### Exemplo:

❌ **Errado:**
```js
app.post('/register', (req, res) => {
    const user = req.body;
    const userExists = db.findUser(user.email);
    if (userExists) {
        return res.status(400).send('User already exists');
    }
    db.saveUser(user);
    res.status(200).send('User created');
});
```

✅ **Certo:**
```js
// Controller
app.post('/register', userController.register);

// Caso de uso
class RegisterUser {
    constructor(userRepository) {
        this.userRepository = userRepository;
    }

    execute(user) {
        if (this.userRepository.findUserByEmail(user.email)) {
            throw new Error('User already exists');
        }
        this.userRepository.save(user);
    }
}

// Repositório
class UserRepository {
    constructor(db) {
        this.db = db;
    }

    findUserByEmail(email) {
        return this.db.find({ email });
    }

    save(user) {
        this.db.insert(user);
    }
}
```

Viu a diferença? Agora, o controller é simplificado e apenas delega o trabalho para o caso de uso, e a lógica de acesso ao banco está isolada no repositório. **Códigos mais fáceis de entender e testar!**

---

### 4️⃣ **Evite dependências diretas com o Framework (Express, por exemplo)**

O Express deve ser apenas uma ferramenta para lidar com as requisições HTTP, não deve ditar toda a lógica do seu sistema. **Não acople demais seu código ao Express!** Ao invés disso, use **controllers e roteadores** para abstrair a comunicação com o cliente.

#### Exemplo:

❌ **Errado:**
```js
app.get('/user', (req, res) => {
    userService.getUserById(req.query.id).then(user => {
        res.json(user);
    });
});
```

✅ **Certo:**
```js
// Use um controlador para tratar a requisição
app.get('/user', userController.getUser);

// Dentro do controller
class UserController {
    constructor(userService) {
        this.userService = userService;
    }

    async getUser(req, res) {
        try {
            const user = await this.userService.getUserById(req.query.id);
            res.json(user);
        } catch (error) {
            res.status(500).send(error.message);
        }
    }
}
```

Aqui, você evita que seu código se prenda às peculiaridades do Express, facilitando mudanças futuras (como trocar o Express por outro framework).

---

### 5️⃣ **Utilize DTOs (Data Transfer Objects)**

DTOs ajudam a isolar o formato dos dados entre diferentes camadas do sistema, garantindo que mudanças no banco de dados não afetem diretamente a camada de apresentação (o que os clientes da API recebem).

❌ **Errado:**
```js
app.get('/user', (req, res) => {
    db.find({ id: req.query.id }).then(user => {
        res.json(user);  // Retorna todos os dados do usuário
    });
});
```

✅ **Certo:**
```js
// DTO para transferir dados específicos
class UserDTO {
    constructor(user) {
        this.id = user.id;
        this.name = user.name;
        this.email = user.email;
    }
}

// Controller
app.get('/user', (req, res) => {
    db.find({ id: req.query.id }).then(user => {
        const userDTO = new UserDTO(user);
        res.json(userDTO);
    });
});
```

Agora, você tem controle sobre os dados que está expondo e pode facilmente mudar o formato sem afetar outras camadas.

---

### 6️⃣ **Facilite os Testes com Mocks e Stubs**

Testar o código fica muito mais fácil quando você desacopla a lógica de negócio do Express e de outras dependências externas. Use mocks e stubs para simular o comportamento de componentes externos (como bancos de dados) durante os testes.

```js
// Exemplo de teste com Jest
test('should return user info', async () => {
    const mockUserRepository = {
        findUserById: jest.fn().mockReturnValue({ id: 1, name: 'John' })
    };
    const userController = new UserController(mockUserRepository);
    const req = { query: { id: 1 } };
    const res = { json: jest.fn() };

    await userController.getUser(req, res);

    expect(res.json).toHaveBeenCalledWith({ id: 1, name: 'John' });
});
```

---

## 🎉 Conclusão: Clean Architecture para o Futuro 💡

Seguindo a Clean Architecture, você estará no caminho certo para criar aplicações escaláveis, de fácil manutenção e testáveis. **Separar responsabilidades**, **inverter dependências**, **usar DTOs** e **manter o framework fora da lógica de negócios** são apenas alguns dos conceitos-chave dessa abordagem.

Lembre-se: arquitetura limpa não é só sobre como organizar o código, mas como **facilitar o crescimento e manutenção do seu sistema** ao longo do tempo. Seu eu do futuro vai te agradecer por seguir essas práticas! 🥳

Agora é hora de colocar as mãos no código e criar um sistema que vai brilhar no futuro! 🚀✨