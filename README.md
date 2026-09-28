# Biblioteca — Sistema de Reservas de Livros

Aplicação para cadastrar livros e usuários e registrar reservas e devoluções.
Trabalho avaliativo de **Computação em Nuvem** (FAG — Engenharia de Software): containerização com Docker e Docker Compose.

| Serviço    | Tecnologia                     | Porta no host |
|------------|--------------------------------|---------------|
| `frontend` | React 19 + Vite                | 5173          |
| `backend`  | Node.js + Express 5 (API REST) | 3000          |
| `db`       | PostgreSQL 16                  | — (só na rede interna) |

## Pré-requisitos

- [Docker](https://docs.docker.com/get-docker/) com Docker Compose (v2 ou superior)
- Git

## Como executar

```bash
git clone <url-deste-repositorio> biblioteca
cd biblioteca
cp .env.example .env
docker compose up -d
```

Na primeira execução o banco é criado automaticamente com as tabelas (`backend/schema.sql`)
e com dados de exemplo (`backend/dados.sql`).

## Como acessar

- Interface web: <http://localhost:5173>
- API: <http://localhost:3000> (ex.: <http://localhost:3000/livros>)

### Rotas da API

| Método | Rota | Descrição |
|--------|------|-----------|
| GET/POST | `/livros` | Lista / cria livros |
| GET/PUT/DELETE | `/livros/:id` | Busca / atualiza / remove um livro |
| GET/POST | `/usuarios` | Lista / cria usuários |
| GET/PUT/DELETE | `/usuarios/:id` | Busca / atualiza / remove um usuário |
| GET/POST | `/reservas` | Lista / cria reservas |
| GET | `/reservas/usuario/:id` e `/reservas/livro/:id` | Reservas de um usuário / livro |
| PUT | `/reservas/:id/devolver` | Registra a devolução |
| DELETE | `/reservas/:id` | Remove uma reserva |

## Comandos úteis

```bash
docker compose ps              # situação dos serviços
docker compose logs -f backend # logs da API
docker compose down            # para tudo, mantendo os dados (volume "dados")
docker compose down -v         # para tudo e apaga o banco (volta aos dados de exemplo)
```

## Configuração

Todas as configurações vêm do arquivo `.env` (que **não** é versionado). O `.env.example`
mostra as variáveis necessárias com valores fictícios:

| Variável | Uso |
|----------|-----|
| `DB_USER`, `DB_PASSWORD`, `DB_NAME` | Credenciais do PostgreSQL (usadas pelo `db` e pelo `backend`) |
| `API_PORT` | Porta da API no host |
| `WEB_PORT` | Porta da interface web no host |

Se a porta 3000 ou 5173 já estiver em uso, altere `API_PORT`/`WEB_PORT` no `.env` e rode
`docker compose up -d --build` (o frontend precisa ser reconstruído para apontar para a nova porta da API).

## Estrutura

```
.
├── backend/            # API (Dockerfile, .dockerignore, schema.sql, dados.sql)
├── frontend/           # Interface web (Dockerfile, .dockerignore)
├── docker-compose.yml
├── .env.example
└── .gitignore
```
