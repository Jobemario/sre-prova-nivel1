# sre-prova-nivel1

Projeto SRE (Docker + CI/CD + Healthcheck + Rollback)

📌 Visão Geral

Este projeto demonstra uma implementação prática de princípios de SRE (Site Reliability Engineering) aplicados a uma aplicação web simples.

A solução cobre:

Containerização com Docker
Pipeline CI/CD com testes automatizados
Monitoramento de saúde da aplicação
Estratégia de rollback manual
Documentação operacional para times

🧱 Arquitetura da Solução
Usuário → Aplicação Web (Docker)
             ↓
        Healthcheck (/health)
             ↓
GitHub Actions (CI/CD)
   ├── Build
   ├── Test
   └── Deploy
             ↓
Rollback Manual (versão anterior)

🚀 Tecnologias Utilizadas
Containerização: Docker
CI/CD: GitHub Actions
Linguagem: Python (Flask)
Monitoramento: Healthcheck HTTP
Versionamento: Git

📦 Estrutura do Projeto
.
├── app/
│   ├── app.py
│   ├── requirements.txt
│
├── tests/
│   ├── test_app.py
│
├── Dockerfile
├── docker-compose.yml
├── .github/
│   └── workflows/
│       └── ci.yml
└── README.md

🐳 Containerização com Docker
Dockerfile
FROM python:3.11-slim

WORKDIR /app

COPY app/ .

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 8080

CMD ["python", "app.py"]

▶️ Como Executar Localmente

# Build da imagem
docker build -t sre-app .

# Rodar container
docker run -p 8080:8080 sre-app

# Testar aplicação
curl http://localhost:8080/
curl http://localhost:8080/health
🧪 Testes Automatizados

Exemplo com pytest:

def test_health():
    response = client.get("/health")
    assert response.status_code == 200
    
🔄 Pipeline CI/CD (GitHub Actions)
Fluxo
Checkout do código
Build do container
Execução dos testes
Deploy (simulado ou local)
Exemplo .github/workflows/ci.yml
name: CI Pipeline

on:
  push:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Build Docker
        run: docker build -t sre-app .

      - name: Run Tests
        run: docker run sre-app pytest
        
❤️ Monitoramento de Saúde
Endpoint
GET /health
Exemplo de resposta
{
  "status": "ok"
}
Uso operacional
curl http://localhost:8080/health

🔁 Estratégia de Rollback Manual
Cenário

Falha após deploy → voltar versão anterior

Processo
# Listar imagens
docker images

# Rodar versão anterior
docker run -p 8080:8080 sre-app:<versao-anterior>
Boa prática
Utilizar tags versionadas:
sre-app:v1
sre-app:v2
