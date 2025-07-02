HEAD
# 🧠 RH System

Sistema completo de Gestão de RH com controle de funcionários, férias, avaliações, recrutamento, comunicados e relatórios.



## 📦 Tecnologias Utilizadas

- **Backend:** NestJS + Prisma + PostgreSQL
- **Frontend:** React + Vite + TypeScript
- **ORM:** Prisma
- **Banco de dados:** PostgreSQL


## 🔧 Pré-requisitos

Antes de rodar o projeto, certifique-se de ter instalado:

- Node.js (v18 ou superior)
- PostgreSQL (crie um banco vazio com nome `rh_db`)
- Git
- npm ou yarn

---

## 🚀 Como rodar o projeto localmente

### 📁 1. Clone o repositório

```bash
git clone https://github.com/seu-usuario/selene-rh-system.git
cd selene-rh-system

## ⚙️ 2. Backend (NestJS)

cd backend
npm install

### ✅ Configure o .env
### Crie o arquivo .env com o conteúdo abaixo:

DATABASE_URL="postgresql://usuario:senha@localhost:5432/rh_db"
JWT_SECRET=seusecretkey

### ⚙️ Execute as migrations e gere o client do Prisma:

npx prisma migrate dev
npx prisma generate

### 🚀 Inicie o servidor:

npm run start:dev

Acesse a API via: http://localhost:3000/api

### 💻 3. Frontend (React + Vite)

cd ../frontend
npm install
npm run dev

Acesse o frontend via: http://localhost:5173



## 🐳 Como rodar com Docker (opcional)

Se preferir rodar o sistema com Docker, siga os passos abaixo:

### 📁 1. Estrutura esperada
selene-rh-system/
├── backend/
│   ├── Dockerfile
│   ├── docker-compose.yml
│   ├── .env
├── frontend/
│   ├── Dockerfile

### 🧩 2. Arquivos necessários

📄 backend/Dockerfile  



FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

RUN npx prisma generate

EXPOSE 3000

CMD ["npm", "run", "start:dev"]


📄 frontend/Dockerfile


FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 5173

CMD ["npm", "run", "dev"]



📄 backend/docker-compose.yml


version: '3.8'

services:
  db:
    image: postgres
    restart: always
    container_name: rh_postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: rh_db
    ports:
      - "5432:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data

  backend:
    build: .
    container_name: rh_backend
    depends_on:
      - db
    ports:
      - "3000:3000"
    environment:
      DATABASE_URL: postgresql://postgres:postgres@db:5432/rh_db
      JWT_SECRET: supersecret

  frontend:
    build:
      context: ../frontend
    container_name: rh_frontend
    depends_on:
      - backend
    ports:
      - "5173:5173"
    environment:
      VITE_API_URL: http://localhost:3000

volumes:
  pgdata:


### ▶️ 3. Subir os containers
Dentro da pasta backend, rode:

docker-compose up --build

### ✅ Acesso
Frontend: http://localhost:5173

Backend (Swagger): http://localhost:3000/api

Banco de dados PostgreSQL: localhost:5432
Usuário: postgres
Senha: postgres
Banco: rh_db

### 🧠 Dica extra: Migrations no Docker

### O Prisma precisa rodar as migrations dentro do container backend:

docker exec -it rh_backend sh
npx prisma migrate dev

## 🛠 Funcionalidades

✅ CRUD de Funcionários

✅ CRUD de Férias

✅ CRUD de Avaliações

✅ CRUD de Vagas e Candidaturas

✅ CRUD de Comunicados

✅ Relatórios dinâmicos (aniversariantes, férias próximas, etc.)

✅ Autenticação com JWT (temporariamente desabilitada para desenvolvimento)

### 🧪 Testando com Swagger

http://localhost:3000/api

Você pode autenticar com o token gerado no login (/auth/login) e testar as rotas protegidas.


### 🔐 Como realizar o login no sistema
Para acessar o sistema, siga os passos abaixo:

1. Acesse o Swagger para gerar o token JWT
Abra o Swagger do backend no navegador:

http://localhost:3000/api
Localize a rota de autenticação:

POST /auth/login
Clique em "Try it out", preencha com as credenciais válidas e execute:

{
  "email": "admin@selene.com",
  "password": "123456"
}
Você receberá uma resposta como esta:

{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR..."
}
2. Copie o token e insira no botão Authorize
No topo do Swagger, clique no botão Authorize.

Cole o token no seguinte formato:

Bearer SEU_TOKEN_AQUI
Exemplo:

Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Clique em Authorize e depois em Close.

3. Acesse o sistema no navegador
Abra o frontend:

http://localhost:5173
Você verá a tela de login do sistema.

Preencha com o mesmo e-mail e senha usados no Swagger:

E-mail: admin@selene.com

Senha: 123456

4. Pronto! 🎉
Se as credenciais estiverem corretas, você será redirecionado automaticamente para o Dashboard do sistema.
As próximas requisições autenticadas ao backend utilizarão o token armazenado localmente.
---
### 🗂 Estrutura do Projeto

selene-rh-system/
├── backend/         # NestJS + Prisma
│   ├── src/
│   └── prisma/
├── frontend/        # React + Vite
│   ├── src/
│   └── public/
---

## 📫 Contato
# Sistema-de-RH
54a796f97535631e45ddf678b2b5c5b98b467be4
