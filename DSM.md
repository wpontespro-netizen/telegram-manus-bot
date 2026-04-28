# DSM — Documento de Solução Mínima

## 1) Objetivo
Criar um bot de Telegram simples, com webhook via FastAPI, capaz de:
- receber mensagens enviadas por usuários;
- processar o conteúdo recebido;
- responder no mesmo chat.

## 2) Escopo da primeira versão
A versão atual cobre:
- endpoint `POST /webhook` para receber updates do Telegram;
- endpoint `GET /` para health check (`{"status": "rodando"}`);
- envio de resposta para o usuário usando `sendMessage` da API do Telegram;
- resposta inicial em modo eco: `"Você escreveu: {texto}"`.

## 3) Arquitetura
Componentes:
1. **Telegram** envia updates para o webhook configurado.
2. **FastAPI (`main.py`)** recebe o payload e extrai `chat_id` e `text`.
3. **Camada de envio** (`send_message`) chama a API do Telegram com o token via variável de ambiente `TELEGRAM_TOKEN`.

Fluxo resumido:
1. Usuário envia mensagem no Telegram.
2. Telegram chama `POST /webhook`.
3. Aplicação valida `chat_id`.
4. Aplicação monta resposta.
5. Aplicação envia mensagem de volta ao chat.

## 4) Requisitos funcionais
- RF01: Receber updates via webhook.
- RF02: Identificar o chat de origem pelo `chat_id`.
- RF03: Enviar resposta de texto ao usuário.
- RF04: Expor endpoint raiz para verificação de disponibilidade.

## 5) Requisitos não funcionais
- RNF01: Configuração por variável de ambiente (`TELEGRAM_TOKEN`).
- RNF02: Implementação assíncrona para chamadas HTTP externas.
- RNF03: Estrutura enxuta para facilitar evolução com integração Manus.

## 6) Critérios de aceite
- CA01: `GET /` retorna `200` com `{"status": "rodando"}`.
- CA02: Ao receber payload válido no webhook, o bot responde no mesmo chat.
- CA03: Sem `chat_id`, o webhook retorna `{"ok": true}` sem erro.

## 7) Próximos passos
- Substituir resposta de eco por chamada real ao Manus.
- Adicionar logs estruturados e tratamento de falhas da API do Telegram.
- Incluir autenticação/validação do webhook.
- Criar testes automatizados para endpoint e integração externa.
