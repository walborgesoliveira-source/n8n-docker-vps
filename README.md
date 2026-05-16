# n8n Docker VPS

Ambiente Docker do n8n usado na VPS da IA Guru.

## Estrutura

- `docker-compose.yml`: serviços `n8n` e PostgreSQL.
- `.env`: credenciais e variáveis do ambiente local. Não versionar.

## Executar

```bash
docker compose up -d
```

## Acesso

- URL pública: `https://n8n.iaguru.com.br`
- Porta local: `127.0.0.1:5678`

## Redes

O serviço `n8n` usa:

- `n8n_network`: rede do banco PostgreSQL do n8n.
- `proxy_net`: rede compartilhada para comunicação interna com outros serviços, como `coreps_api`.

## Segurança

Não versionar `.env`, dumps, backups ou exports de workflows com segredos.

## Notificações Telegram

O workflow de agendamento do Massoterapia RJ pode enviar alerta para Telegram quando estas variáveis existirem no `.env`:

```env
TELEGRAM_BOT_TOKEN=
TELEGRAM_CHAT_ID=
```

Quando elas ficam vazias, o workflow apenas registra o agendamento e responde ao site.
