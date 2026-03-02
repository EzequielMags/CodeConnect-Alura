## SPA de Blog com React e React Router

Aplicação de estudo construída com **React** e **Vite**, focada em criar uma **Single Page Application (SPA)** utilizando o **React Router** para navegação entre páginas, autenticação simulada e proteção de rotas.

### Tecnologias principais

- **React 19**  
- **Vite** para bundling e ambiente de desenvolvimento  
- **React Router 7 (`react-router`)** para gerenciamento de rotas  
- **React Markdown (`react-markdown`)** para renderização de conteúdo em markdown  

### Funcionalidades

- **Fluxo de autenticação simples**:
  - Cadastro de usuário (armazenado em `localStorage`)
  - Login e logout
  - Estado de autenticação gerenciado pelo hook `useAuth`
- **Rotas protegidas**:
  - Feed de posts acessível apenas para usuários autenticados
  - Página de post de blog individual com comentários e likes
- **Layout de SPA**:
  - Navegação sem recarregar a página
  - Separação entre rotas de autenticação (`/auth/*`) e rotas da aplicação logada (`/` e derivados)

---

## Como o React Router é utilizado neste projeto

### Configuração básica das rotas

As rotas da aplicação são configuradas em `src/main.jsx` usando os componentes do **React Router**:

- `BrowserRouter` envolve toda a aplicação
- `Routes` agrupa todas as definições de rotas
- `Route` define cada caminho da aplicação

Estrutura principal:

- **Rotas de autenticação (`/auth`)**
  - `/auth/register` → página de cadastro (`Register`)
  - `/auth/login` → página de login (`Login`)
  - `/auth/logout` → rota que executa o logout e redireciona o usuário

- **Rotas da aplicação logada (`/`)**
  - `/` → página de feed (`Feed`), **protegida**
  - `/blog-post` → página de um post específico (`BlogPost`), **protegida**

### Rotas aninhadas

O React Router é usado com **rotas aninhadas**, aproveitando o agrupamento por prefixos:

- Rota pai `/auth` agrupa:
  - `register`, `login`, `logout`
- Rota pai `/` agrupa:
  - rota raiz (`""`) para o feed
  - `blog-post` para detalhes de um post

Isso deixa a declaração de rotas mais organizada e espelha a estrutura de URLs da aplicação.

### Rotas protegidas (`ProtectedRoute`)

Para evitar que usuários não autenticados acessem o feed ou o post do blog, é utilizado um componente `ProtectedRoute` (em `src/components/ProtectedRoute`), que envolve as páginas que precisam de autenticação:

- Na configuração das rotas:
  - `/` renderiza `<ProtectedRoute><Feed /></ProtectedRoute>`
  - `/blog-post` renderiza `<ProtectedRoute><BlogPost /></ProtectedRoute>`

O `ProtectedRoute` normalmente:

- Consome o estado de autenticação (ex.: via `useAuth`)
- Verifica se o usuário está logado
  - se **não estiver**, redireciona para `/auth/login`
  - se **estiver**, renderiza o componente filho (ex.: `Feed`, `BlogPost`)

### Hook de navegação (`useNavigate`)

Para navegar programaticamente entre rotas, o projeto usa o hook `useNavigate` do React Router:

- **Na tela de Login (`Login`)**:
  - Após um login bem-sucedido: `navigate("/")` leva o usuário ao feed
  - Em caso de erro, o usuário permanece na mesma página e recebe um alerta

- **No componente `Logout`**:
  - Ao montar, o componente executa:
    - `logout()` (limpa o usuário do `localStorage` e do estado)
    - `navigate("/auth/login")` (redireciona para a tela de login)

Isso permite fluxos de navegação baseados em ações do usuário (login/logout) sem depender de links visíveis na interface.

### Linkagem entre páginas

Além da navegação programática, a aplicação utiliza um componente de `Link` personalizado (`src/components/Link`), que provavelmente encapsula o `Link` do React Router para manter o estilo visual consistente.  

Exemplo importante:

- Na tela de Login, há um link para a rota de cadastro:
  - `href="/auth/register"` → leva o usuário para a página de criação de conta.

---

## Autenticação e integração com as rotas

### Hook `useAuth`

O hook `useAuth` centraliza a lógica de autenticação:

- **Armazena o usuário logado** em estado React
- Persiste dados no `localStorage`:
  - `auth_users`: lista de usuários cadastrados
  - `auth_user`: usuário atualmente autenticado
- Expõe funções:
  - `register(name, email, password)`
  - `login(email, password)`
  - `logout()`
  - `isAuthenticated` (booleano)

Esse hook é utilizado por:

- **`Login`**: para autenticar e redirecionar
- **`Logout`**: para encerrar a sessão
- **`ProtectedRoute`**: para verificar se o usuário pode acessar rotas privadas

---

## Como rodar o projeto

### Pré-requisitos

- **Node.js** (versão LTS recomendada)
- **npm** (ou outro gerenciador de pacotes compatível)

### Instalação

```bash
npm install
```

### Ambiente de desenvolvimento

```bash
npm run dev
```

O Vite exibirá no terminal a URL local (geralmente `http://localhost:5173`).

### Build para produção

```bash
npm run build
```

### Preview do build

```bash
npm run preview
```

---

## Resumo do uso do React Router

- **`BrowserRouter`** envolve a aplicação inteira.
- **`Routes` e `Route`** definem a estrutura de navegação:
  - Rotas de autenticação em `/auth/*`
  - Rotas privadas protegidas em `/` e `/blog-post`
- **`ProtectedRoute`** garante que apenas usuários autenticados acessem determinadas telas.
- **`useNavigate`** gerencia redirecionamentos após login e logout.
- **Links** (via componente `Link`) permitem navegação declarativa entre páginas.

Com isso, o projeto demonstra na prática como estruturar uma **SPA com React Router**, incluindo **rotas públicas, protegidas, navegação programática e controle de autenticação**.
