<h1 align="center">
  <img alt="GoBarber" src="https://ik.imagekit.io/antony/gobarber_rsw5Uki0X.png" />
</h1>

# 🔖 Sumário

- [Sobre](#%EF%B8%8F-sobre)
- [Tecnologias utilizadas](#-tecnologias-utilizadas)
- [Como baixar e executar?](#-como-baixar-e-executar)
  - [Baixando o projeto](#%EF%B8%8F-baixando-o-projeto)
  - [Preparando o ambiente](#-preparando-o-ambiente)
    - [Bancos de dados e tabelas](#-bancos-de-dados-e-tabelas)
    - [Variáveis de ambiente](#-variáveis-de-ambiente)
  - [Executando a API](#-executando-a-api)
- [Rotas da API](#-rotas-da-api)

## 🏷️ Sobre

O **GoBarber** é uma aplicação feita para o gerenciamento de serviços entre barbeiros e clientes. Nele é possível se registrar e marcar um horário/serviço com seu barbeiro e/ou se tornar um prestador desses serviços.

Esta é uma das aplicações que compõem todo o **sistema GoBarber**.

Para explorar as outras, acesse os links a seguir:

- [GoBarber Web](https://github.com/antonylemos/gobarber-web)
- [GoBarber Mobile](https://github.com/antonylemos/gobarber-mobile)

## 🚀 Tecnologias utilizadas

As principais tecnologias utilizadas para a construção dessa API foram:

- [Node.js](https://nodejs.org/en/)
- [Express](http://expressjs.com/)
- [PostgreSQL](https://www.postgresql.org/)
- [MongoDB](https://www.mongodb.com/)
- [Redis](https://redis.io/)

## 📦 Como baixar e executar?

**Antes de baixar e executar o projeto**, é necessário ter o **Node.js** já instalado e, em seguida, instalar as seguintes ferramentas:

- [Git](https://git-scm.com/)
- [Yarn](https://classic.yarnpkg.com/lang/en/)
- [Docker](https://www.docker.com/)

### ⬇️ Baixando o projeto

Abra o terminal do seu sistema operacional e execute os seguintes comandos:

```bash
  # Clonar o repositório
  git clone https://github.com/antonylemos/gobarber-api.git

  # Entrar no diretório
  cd gobarber-api

  # Instalar as dependências
  yarn install
```

### 🌎 Preparando o ambiente

Utilizando o **Docker**, iremos baixar as imagens dos bancos de dados necessários para a execução da API:

```bash
  # Baixar o PostgreSQL
  docker run --name gostack_postgres -e POSTGRES_PASSWORD=docker -p 5432:5432 -d postgres

  # Baixar o MongoDB
  docker run --name mongodb -p 27017:27017 -d -t mongo

  # Baixar o Redis
  docker run --name redis -p 6379:6379 -d -t redis:alpine
```

Para saber se os bancos de dados estão em execução, rode o seguinte comando:

```bash
  docker ps
```

Caso não estejam, execute o seguinte comando:

```bash
  docker start gostack_postgres mongodb redis
```

**Observação:** caso queira interromper a execução, substitua `start` por `stop`.

#### 🎲 Bancos de dados e tabelas

Para ajudar na visualização dos dados que estão sendo salvos nos bancos de dados, você poderá utilizar as seguintes ferramentas:

- [DBeaver](https://dbeaver.io/)
- [MongoDB Compass](https://www.mongodb.com/try/download/compass)

Com essas ferramentas, também é possível criar os _databases_ que serão utilizados na aplicação. Certifique-se de criar os seguintes _databases_ antes de executar a aplicação:

- **gostack_gobarber** (PostgreSQL)
- **gobarber** (MongoDB)

Para mais informações, verifique o arquivo `ormconfig.json`.

#### Variáveis de ambiente

Na raíz do projeto você encontrará o arquivo `.env.example`. A partir dele, crie um outro arquivo chamado `.env` utilizando a mesma estutura.

### 🏃 Executando a API

Com os bancos de dados em execução e estando no diretório da API, execute os seguintes comandos:

```bash
  # Criar as tabelas no PostgreSQL
  yarn typeorm migration:run

  # Executar o servidor
  yarn dev:server
```

## 📌 Rotas da API

Para testar as rotas, você poderá utilizar o [Insomnia](https://insomnia.rest/). O **_workspace_** completo dessa API está disponível para uso, basta clicar no botão abaixo:

<p align="center">
  <a href="https://insomnia.rest/run/?label=GoBarber%20API&uri=https%3A%2F%2Fgist.githubusercontent.com%2Fantonylemos%2F7c98f034d59d5f2a4ef0ddb6157385f6%2Fraw%2F3b9685b8d8968707a810e171bee8b6a8701791de%2Fgobarber-insomnia-workspace.json" target="_blank">
    <img src="https://insomnia.rest/images/run.svg" alt="Run in Insomnia">
  </a>
</p>

Caso deseje utilizar outra forma para realizar as requisições, as rotas disponíveis são:

⚠️ **Atenção! O _token_ necessário para realizar grande parte das requisições é gerado a partir da rota **`POST /sessions`** e deve ser enviado via _Bearer_ nas referidas rotas. No Insomnia, após realizar a autenticação, o _token_ é automaticamente compartilhado com as demais rotas.**

#### User

- **`POST /users`**: A rota deve receber os campos `name`, `email` e `password` no corpo da requisição para que se possa cadastrar um novo usuário.

- **`GET /profile`**: A rota deve receber o _token_ do usuário usuário autenticado e retorna as informações desse usuário.

- **`PATCH /users/avatar`**: A rota deve receber o _token_ do usuário autenticado e um arquivo de imagem para que se possa atualizar o avatar do usuário.

- **`PUT /profile`**: A rota deve receber o _token_ do usuário autenticado e pode receber os campos `name`, `email`, `old_password`, `password` e `password_confirmation` no corpo da requisição para que se possa atualizar os dados do usuário em questão.

  ##### Sessions

- **`POST /sessions`**: A rota deve receber os campos `email` e `password` no corpo da requisição para que se possa autenticar o usuário usuário.

  ##### Password

- **`POST /password/forgot`**: A rota deve receber o campo `email` no corpo da requisição. Isso fará com que um email seja enviado para que seja feita a recuperação da senha.

- **`POST /password/reset`**: A rota deve receber os campos `password`, `password_confirmation` e `token` no corpo da requisição para que se possa alterar a senha do usuário.

#### Appointments

- **`POST /appointments`**: A rota deve receber o _token_ do usuário autenticado e os campos `provider_id` e `date` no corpo da requisição para que se possa cadastrar um novo agendamento.

- **`POST /appointments/me`**: A rota deve receber o _token_ do usuário usuário autenticado e retorna os agendamentos que foram marcados com ele.

  ##### Providers

- **`GET /providers`**: A rota deve receber o _token_ do usuário usuário autenticado e retorna os provedores cadastrados no sistema.

- **`POST /providers/:provider_id/month-availability`**: A rota deve receber o _token_ do usuário usuário autenticado, o campo `provider_id` como parâmetro da rota e os campos `month` e `year` como query da requisição e retorna os dias disponíveis no mês informado.

- **`POST /providers/:provider_id/day-availability`**: A rota deve receber o _token_ do usuário usuário autenticado, o campo `provider_id` como parâmetro da rota e os campos `day`, `month` e `year` como query da requisição e retorna os horários disponíveis no dia informado.

---

Desenvolvido com 💜 por Antony Lemos 🧑🏽‍🚀


