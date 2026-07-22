# Novo SGA — Docker

É necessário ter o Docker instalado no ambiente: https://docs.docker.com/engine/installation/

## Serviços

| Serviço    | Imagem                         | Porta |
|------------|--------------------------------|-------|
| `postgres` | postgres:18.4-trixie           | 15432 |
| `mercure`  | novosga/mercure:v0.11          | 13000 |
| `novosga`  | novosga/novosga:2.2-standalone | 18080 |
| `painel`   | novosga/panel-app:v2.1.1       | 18081 |
| `triagem`  | novosga/triage-app:v2.1.0      | 18082 |

## Recursos provisionados

| Serviço    | RAM         | CPU  |
|------------|-------------|------|
| `postgres` | 512m        | 0.5  |
| `mercure`  | 128m        | 0.25 |
| `novosga`  | 512m        | 1.0  |
| `painel`   | 86m         | 0.25 |
| `triagem`  | 86m         | 0.25 |
| **Total**  | **1,29 GB** | **2,25** |

## Variáveis de ambiente

| Variável                  | Obrigatório | Padrão        |
|---------------------------|-------------|---------------|
| `POSTGRES_DB`             | sim         | —             |
| `POSTGRES_USER`           | sim         | —             |
| `POSTGRES_PASSWORD`       | sim         | —             |
| `MERCURE_URL`             | sim         | —             |
| `MERCURE_PUBLIC_URL`      | sim         | —             |
| `MERCURE_JWT_SECRET`      | sim         | —             |
| `TZ`                      | sim         | —             |
| `LANGUAGE`                | sim         | —             |
| `NOVOSGA_ADMIN_USERNAME`  | sim         | —             |
| `NOVOSGA_ADMIN_PASSWORD`  | sim         | —             |
| `NOVOSGA_ADMIN_FIRSTNAME` | não         | Administrador |
| `NOVOSGA_ADMIN_LASTNAME`  | não         | Global        |

Gere senhas seguras com `openssl rand -hex 32`.

> Docker secrets ainda não é suportado pelo NovoSGA.

## Instalação

```bash
cp .env.example .env
# Edite o .env e preencha as variáveis obrigatórias
nano .env
docker compose up -d
```

> Em servidor remoto, altere `MERCURE_PUBLIC_URL` no `.env` para o IP/domínio público.

## Acesso

| URL                        | Serviço            |
|----------------------------|--------------------|
| http://IP_OU_DOMINIO:18080 | Aplicação          |
| http://IP_OU_DOMINIO:18081 | Painel de chamadas |
| http://IP_OU_DOMINIO:18082 | Triagem            |

## Volumes

| Volume            | Serviço    | Dados persistidos    |
|-------------------|------------|----------------------|
| `db_data`         | `postgres` | Banco de dados       |
| `novosga_uploads` | `novosga`  | Uploads da aplicação |
