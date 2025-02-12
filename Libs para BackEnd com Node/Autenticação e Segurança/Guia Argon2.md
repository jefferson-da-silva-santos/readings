# 🔒 Tudo sobre **Argon2** – O Melhor Algoritmo para Hash de Senhas no Node.js

Se você precisa armazenar senhas de forma **segura** no seu sistema, usar apenas `bcrypt` já não é suficiente. O **Argon2** é atualmente o **melhor algoritmo de hash para senhas**, sendo o vencedor da competição de hash de senhas do **PHC (Password Hashing Competition)**.

Aqui, vou te mostrar **o que é o Argon2, como instalar, como usar e por que ele é mais seguro que o bcrypt**. 🚀

---

## 🛡️ O que é o **Argon2**?
O **Argon2** é um **algoritmo de hash de senhas** projetado para ser **seguro contra ataques de força bruta e GPUs**, ao contrário de hashes simples como `SHA-256`. Ele tem três variações:

1. **Argon2d** – Focado em resistência contra **ataques de hardware (GPUs e ASICs)**  
2. **Argon2i** – Mais seguro contra **ataques de força bruta baseados em tempo (side-channel)**  
3. **Argon2id** – Uma mistura dos dois (recomendado para senhas)  

🔥 **Argon2id** é a melhor opção para armazenar senhas!

---

## 📦 Instalando o Argon2 no Node.js

Para instalar o **pacote oficial do Argon2** no seu projeto, basta rodar:

```bash
npm install argon2
```
ou, se estiver usando **yarn**:
```bash
yarn add argon2
```

Agora podemos importar e começar a usar! 🚀

```js
const argon2 = require('argon2');
```

---

## 🔐 Gerando um hash seguro de senha

A primeira coisa que precisamos fazer é **hashear** uma senha antes de salvar no banco.

```js
const argon2 = require('argon2');

const gerarHash = async () => {
  try {
    const hash = await argon2.hash('minhaSenhaSuperSegura');
    console.log('🔐 Hash gerado:', hash);
  } catch (err) {
    console.error('❌ Erro ao gerar hash:', err);
  }
};

gerarHash();
```

### 🔹 O que acontece aqui?
1️⃣ `argon2.hash('senha')` gera um hash seguro automaticamente  
2️⃣ O hash gerado já contém **salt embutido**, então não precisamos passar manualmente  
3️⃣ Esse hash será salvo no banco de dados para comparação futura  

🛡️ **Exemplo de hash gerado (Argon2id)**:
```
$argon2id$v=19$m=65536,t=3,p=4$WmJraGFtaHRhTnhL$4EuIQw...
```

---

## ✅ Comparando a senha com o hash salvo (Login)

Na hora do **login**, o usuário digita a senha e precisamos compará-la com o hash salvo no banco.

```js
const verificarSenha = async () => {
  const senhaDigitada = 'minhaSenhaSuperSegura';
  const hashSalvoNoBanco = '$argon2id$v=19$m=65536,t=3,p=4$WmJraGFtaHRhTnhL$4EuIQw...';

  try {
    if (await argon2.verify(hashSalvoNoBanco, senhaDigitada)) {
      console.log('✅ Senha correta! Usuário autenticado.');
    } else {
      console.log('❌ Senha incorreta! Acesso negado.');
    }
  } catch (err) {
    console.error('❌ Erro ao verificar senha:', err);
  }
};

verificarSenha();
```

### 🔹 O que acontece aqui?
1️⃣ O usuário digita a senha  
2️⃣ `argon2.verify(hashSalvoNoBanco, senhaDigitada)` compara a senha digitada com o hash  
3️⃣ Se for igual, retorna **true** e o usuário pode logar  
4️⃣ Se for diferente, retorna **false** e bloqueia o acesso  

Isso torna impossível um hacker **descriptografar** a senha salva no banco. 🔐

---

## ⚙️ Configurando parâmetros do Argon2

Por padrão, `argon2.hash()` usa parâmetros seguros, mas podemos **personalizar** para ajustar performance e segurança.

```js
const hashCustomizado = async () => {
  try {
    const hash = await argon2.hash('senhaForte', {
      type: argon2.argon2id, // Modo recomendado (id)
      memoryCost: 2 ** 16,   // Consumo de memória (64MB)
      timeCost: 5,           // Quantidade de iterações
      parallelism: 2         // Paralelismo (quantidade de threads)
    });

    console.log('🔐 Hash com configuração customizada:', hash);
  } catch (err) {
    console.error('❌ Erro ao gerar hash:', err);
  }
};

hashCustomizado();
```

🔹 **Explicação dos parâmetros:**  
✅ `type: argon2.argon2id` – Escolhe a versão **mais segura**  
✅ `memoryCost: 2 ** 16` – Usa **64MB de RAM** para aumentar resistência  
✅ `timeCost: 5` – Aumenta o tempo de execução (padrão é **3**)  
✅ `parallelism: 2` – Usa **2 threads** para melhorar performance  

Se quiser mais segurança, aumente `timeCost` e `memoryCost`. Mas **não exagere**, pois senhas precisam ser verificadas rapidamente no login. 🔥

---

## 🔄 Quando **re-hashear** uma senha?

Senhas podem ficar **desatualizadas** conforme novas técnicas de ataque surgem. O Argon2 permite verificar **se um hash precisa ser atualizado** com `argon2.needsRehash()`:

```js
const checarSePrecisaAtualizar = async () => {
  const hashAntigo = '$argon2id$v=19$m=4096,t=3,p=1$oldHash123';

  if (argon2.needsRehash(hashAntigo, { memoryCost: 2 ** 16, timeCost: 5, parallelism: 2 })) {
    console.log('⚠️ O hash da senha precisa ser atualizado!');
  } else {
    console.log('✅ O hash ainda é seguro.');
  }
};

checarSePrecisaAtualizar();
```

Se `needsRehash()` retornar **true**, gere um novo hash e atualize no banco. 🔄

---

## 🔥 Argon2 vs Bcrypt – Qual é melhor?  

| 🔥 **Argon2** | ⚡ **Bcrypt** |
|--------------|--------------|
| **Mais seguro** (resistente a ataques de hardware) | Segurança menor contra GPUs/ASICs |
| Usa **memória RAM** para dificultar ataques | Apenas aumenta o tempo de cálculo |
| **Recomendado por especialistas** | Criado nos anos 90 (mais antigo) |
| Mais lento, mas **mais seguro** 🔒 | Mais rápido, mas menos seguro |

⚠️ **Se estiver começando um novo projeto, use Argon2!**  
Mas se seu sistema já usa **bcrypt**, pode continuar usando, pois ele ainda é seguro (mas menos que Argon2).

---

## 🏁 Conclusão

O **Argon2** é a **melhor opção para hash de senhas em 2024**. Ele é **seguro, resistente a ataques e otimizado para hardware moderno**.  

### 🎯 Resumo:
✅ **Melhor que bcrypt para segurança de senhas**  
✅ **Resistente a ataques por GPU e força bruta**  
✅ **Já inclui salt embutido (não precisa adicionar manualmente)**  
✅ **Pode configurar memória, tempo e threads para ajuste de performance**  
✅ **Permite re-hash automático quando necessário**  

Se quer segurança máxima para senhas no seu sistema, **use Argon2**! 🔥🔐  

Agora é só testar no seu projeto! 🚀🔥