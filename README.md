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
TELEGRAM_CHAT_IDS=
```

Use `TELEGRAM_CHAT_IDS` para enviar para mais de um destino, separados por virgula. Exemplo atual do Massoterapia RJ: `1399833142,-1003952424161`.

Ao trocar IDs no `.env`, recrie o container com `docker compose up -d n8n`. Apenas `docker compose restart n8n` nao recarrega variaveis de ambiente ja gravadas no container.

Quando elas ficam vazias, o workflow apenas registra o agendamento e responde ao site.

## WhatsApp automático

O workflow também pode enviar uma mensagem inicial ao cliente por WhatsApp depois que o pedido for registrado no CORE PS. Configure no `.env`:

```env
WHATSAPP_API_URL=
WHATSAPP_API_TOKEN=
WHATSAPP_TO_FIELD=to
WHATSAPP_MESSAGE_FIELD=message
```

`WHATSAPP_API_URL` deve apontar para o endpoint HTTP do provedor de WhatsApp. O workflow envia `Authorization: Bearer WHATSAPP_API_TOKEN` e um JSON com os campos definidos em `WHATSAPP_TO_FIELD` e `WHATSAPP_MESSAGE_FIELD`. Se alguma variável obrigatória estiver vazia, o envio por WhatsApp é ignorado e o agendamento continua funcionando.
