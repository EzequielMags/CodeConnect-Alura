## CodeConnect - API com Docker e Front-end React

Este repositório contém **dois projetos**:

- **Backend (`4870--api-com-docker`)**: API construída com NestJS, usando Postgres via Docker.
- **Frontend (`CodeConnect-FrontEnd`)**: Aplicação web em React + Vite que consome essa API.

### Estrutura de pastas

- `4870--api-com-docker/` → código da API (NestJS, Prisma, Docker).
- `CodeConnect-FrontEnd/` → código do front-end (React, Vite, React Router).

---

## Pré‑requisitos

- **Node.js** LTS instalado.
- **npm** (ou yarn/pnpm, se preferir).
- **Docker** e **Docker Compose** instalados para rodar o banco de dados.

---

## Backend – 4870--api-com-docker

Entre na pasta do backend:

```bash
cd 4870--api-com-docker
```

### Variáveis de ambiente

O arquivo `.env` já está preparado com:

- `DATABASE_URL="postgresql://postgres:postgres@localhost:5432/code_connect"`  
- `JWT_SECRET="seu-jwt-secret-super-secreto"`  
- `JWT_EXPIRES_IN="7d"`  
- `PORT=3000` → a API sobe em `http://localhost:3000`.

### Subir o Postgres com Docker

O arquivo `docker-compose.yml` sobe um container Postgres na porta **5432**:

```bash
docker compose up -d
```

Isso cria o banco `code_connect` com usuário e senha `postgres/postgres`.

### Instalar dependências da API

```bash
npm install
```

### Rodar a API em desenvolvimento

Com o banco já rodando via Docker:

```bash
npm run start:dev
```

A API ficará disponível em: `http://localhost:3000`.

---

## Frontend – CodeConnect-FrontEnd

Em outro terminal, entre na pasta do front-end:

```bash
cd CodeConnect-FrontEnd
```

### Instalar dependências

```bash
npm install
```

### Rodar em modo desenvolvimento

```bash
npm run dev
```

Por padrão, o Vite usa uma porta como `http://localhost:5173`.  
Abra o navegador nessa URL para acessar o site.

Se o front-end precisar da URL da API, configure em um arquivo `.env` na pasta `CodeConnect-FrontEnd`, por exemplo:

```bash
VITE_API_URL="http://localhost:3000"
```

---

## Scripts úteis

### Backend (`4870--api-com-docker`)

- `npm run start:dev` – inicia a API em modo desenvolvimento (watch).
- `npm run build` – build da aplicação NestJS.
- `npm run test` / `npm run test:e2e` – testes unitários / e2e.

### Frontend (`CodeConnect-FrontEnd`)

- `npm run dev` – inicia o servidor de desenvolvimento do Vite.
- `npm run build` – gera build de produção.
- `npm run preview` – sobe um servidor para pré-visualizar o build.

---

## Como versionar no Git / enviar para o GitHub

1. Confirme que o arquivo `.gitignore` está na **raiz** deste diretório (mesmo nível de `4870--api-com-docker` e `CodeConnect-FrontEnd`).
2. No terminal, na raiz do projeto, rode:

```bash
git add .
git commit -m "chore: inicializa projeto CodeConnect (API + front)"
git branch -M main
git remote add origin <URL_DO_SEU_REPOSITORIO>
git push -u origin main
```

Substitua `<URL_DO_SEU_REPOSITORIO>` pela URL do repositório que você criou no GitHub.

