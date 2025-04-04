# 🛡️ Guia de Rotas Privadas com React Router (v6+)  
> Porque nem todo mundo pode entrar no camarote da sua aplicação.

---

## 🧠 Conceito Rápido: o que é uma Rota Privada?

Em uma SPA (Single Page Application), **Rota Privada** é aquela que **só pode ser acessada se o usuário estiver autenticado**. Caso contrário, o usuário é redirecionado para outra rota, geralmente o `/login`.

---

## 📦 Pacotes necessários

Você precisa do React Router v6+ instalado. Se ainda não tiver:

```bash
npm install react-router-dom
```

---

## 🔑 Passo 1: Simulando um sistema de autenticação

Vamos criar uma função para verificar se o usuário está logado. Isso pode vir de um contexto, localStorage, cookie, etc.

```js
// auth.js (ou authService.js)
export const isAuthenticated = () => {
  return !!localStorage.getItem("token");
};
```

---

## 🚧 Passo 2: Criando o Componente de Rota Privada

```jsx
// PrivateRoute.jsx
import { Navigate, Outlet } from "react-router-dom";
import { isAuthenticated } from "./auth";

const PrivateRoute = () => {
  return isAuthenticated() ? <Outlet /> : <Navigate to="/login" />;
};

export default PrivateRoute;
```

> 🤓 **O que é o Outlet?**  
É como se fosse um “buraco” onde o conteúdo da rota protegida vai ser renderizado, **caso a permissão esteja ok**.

---

## 🗺️ Passo 3: Organizando suas rotas

```jsx
// App.jsx ou Routes.jsx
import { BrowserRouter, Routes, Route } from "react-router-dom";
import PrivateRoute from "./PrivateRoute";

import Home from "./pages/Home";
import Dashboard from "./pages/Dashboard";
import Login from "./pages/Login";
import NotFound from "./pages/NotFound";

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />

        <Route path="/login" element={<Login />} />

        {/* ROTAS PRIVADAS */}
        <Route element={<PrivateRoute />}>
          <Route path="/dashboard" element={<Dashboard />} />
          {/* Adicione mais rotas privadas aqui */}
        </Route>

        {/* ROTA CURINGA */}
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

---

## 🔁 Bônus: Criando um `AuthContext` pra centralizar o controle

Se quiser melhorar ainda mais, você pode usar um contexto para controlar o login:

```jsx
// AuthContext.jsx
import { createContext, useContext, useState } from "react";

const AuthContext = createContext();

export const AuthProvider = ({ children }) => {
  const [auth, setAuth] = useState(!!localStorage.getItem("token"));

  const login = (token) => {
    localStorage.setItem("token", token);
    setAuth(true);
  };

  const logout = () => {
    localStorage.removeItem("token");
    setAuth(false);
  };

  return (
    <AuthContext.Provider value={{ auth, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
};

export const useAuth = () => useContext(AuthContext);
```

E a `PrivateRoute` usando esse contexto:

```jsx
import { Navigate, Outlet } from "react-router-dom";
import { useAuth } from "./AuthContext";

const PrivateRoute = () => {
  const { auth } = useAuth();
  return auth ? <Outlet /> : <Navigate to="/login" />;
};
```

---

## 🧪 Teste como um dev esperto:

1. Tente acessar `/dashboard` sem estar logado → Deve ser redirecionado para `/login`.
2. Faça login e armazene o token → Agora `/dashboard` funciona.
3. Limpe o token (deslogado) → Voltará a proteger a rota.

---

## 🧠 Recapitulando:

✅ Use `Outlet` dentro de uma rota pai para proteger várias páginas.  
✅ Use `<Navigate to="/login" />` para redirecionar usuários não autenticados.  
✅ Use `AuthContext` se quiser mais organização e controle global.  
✅ Seja malandro: proteja também a API no backend (porque frontend é só o porteiro).

---

Se quiser, posso montar um template completo com login fake + rota privada + logout. É só pedir! 🚀  
Ou quer que gere uma versão em TypeScript?