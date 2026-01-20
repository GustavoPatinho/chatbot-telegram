# 🤖 Chatbot de Clima no Telegram com n8n

Este projeto é um **estudo prático** de automação utilizando **n8n**, **Telegram Bot API** e **OpenWeather API**, executado em ambiente **Docker** com **PostgreSQL**, **Redis** e **ngrok**.

O fluxo permite que um usuário envie o nome de uma cidade via Telegram e receba, em tempo real, a **temperatura atual** da localidade informada.

---

## 🧠 Visão Geral da Arquitetura

- **Telegram Bot**: interface de entrada e saída com o usuário
- **n8n (Editor + Worker)**: orquestração do workflow em modo fila (queue)
- **OpenWeather API**: fonte de dados meteorológicos
- **PostgreSQL**: persistência de dados do n8n
- **Redis**: gerenciamento de filas de execução
- **ngrok**: exposição pública do webhook do Telegram em ambiente local
- **Docker Compose**: orquestração de todos os serviços

---

## 📁 Estrutura do Repositório
.
├── workflow-chatbot-telegram.json   # Workflow do n8n exportado
├── docker-compose.yml               # Infraestrutura local com Docker
└── README.md                        # Documentação do projeto

## 🔄 Funcionamento do Workflow

O workflow Weather Check segue os seguintes passos:

1. Recebe mensagens do Telegram

- O usuário envia o nome da cidade no formato Cidade,UF
- Exemplo: São Paulo,SP
- Normaliza o texto
- Remove acentos
- Converte para lowercase
- Ajusta espaçamento e vírgulas

2. Consulta a OpenWeather API

- Unidades métricas
- Resposta em português (pt_br)
- Tratamento de resposta
- Retorna cidade e temperatura arredondada
- Trata erros de cidade não encontrada (404)
- Trata erros inesperados

3. Responde ao usuário no Telegram

- ✅ Sucesso: temperatura atual
- ❌ Cidade inválida
- ⚠️ Erro interno

## 🚀 Como Executar o Projeto

1. Pré-requisitos

- Docker
- Docker Compose
- Conta no Telegram (para criar o bot)
- Conta no OpenWeather
- Conta no ngrok

2. Configurações Necessárias

Antes de subir os containers, edite o arquivo docker-compose.yml e substitua os valores abaixo:

N8N_ENCRYPTION_KEY=REPLACE_WITH_RANDOM_KEY
WEBHOOK_URL=https://REPLACE_WITH_NGROK_DOMAIN
N8N_EDITOR_BASE_URL=https://REPLACE_WITH_NGROK_DOMAIN
NGROK_AUTHTOKEN=REPLACE_WITH_NGROK_AUTH_TOKEN

3. Subir o Ambiente

docker compose up -d

Após subir:
- ngrok irá gerar uma URL pública (ver logs do container)

4. Importar o Workflow

- Acesse o n8n pelo navegador
- Vá em Import workflow
- Selecione o arquivo workflow-chatbot-telegram.json
- Configure as credenciais:
    - Telegram API
    - OpenWeather API (substitua OPENWEATHER_API_KEY no node HTTP Request)

5. Configurar o Bot do Telegram

- Crie um bot via @BotFather
- Copie o token
- Configure nas credenciais do n8n
- Envie mensagens diretamente para o bot

## 💬 Exemplo de Uso

Mensagem enviada no Telegram:
Curitiba,PR

Resposta do bot:
🌤️ A temperatura em Curitiba é de 18°C.

## 🔐 Observações de Segurança

- Este projeto é apenas para estudo
- Não utilize essas configurações em produção sem:
    - HTTPS fixo
    - Secrets seguros
    - Autenticação no n8n
- O uso do ngrok é apenas para ambiente local

## 📌 Objetivo do Projeto

Este repositório tem como objetivo:
- Demonstrar uso prático do n8n em modo fila
- Integrar APIs externas
- Trabalhar com webhooks do Telegram
- Orquestrar serviços com Docker Compose
- Aplicar tratamento de erros e normalização de dados

## 🛠️ Tecnologias Utilizadas

- n8n
- Docker & Docker Compose
- Telegram Bot API
- OpenWeather API
- PostgreSQL
- Redis
- ngrok

## 📄 Licença

Projeto de uso educacional e livre para estudo e experimentação.