# Desafio 1 - instâncias EC2 na AWS

## 📌 Visão Geral

Este projeto apresenta uma arquitetura hipotética de processamento de arquivos utilizando serviços da AWS. O objetivo é demonstrar como integrar armazenamento, computação serverless e máquinas virtuais para construir um pipeline escalável de processamento de dados.

Quando um arquivo é enviado para um bucket do Amazon S3, um evento é disparado automaticamente para uma função AWS Lambda. A Lambda é responsável por validar o evento e iniciar o processamento em uma instância Amazon EC2. Durante a execução, a aplicação utiliza um volume Amazon EBS para armazenar arquivos temporários e dados intermediários. Após o processamento, o resultado é enviado para um bucket de saída no Amazon S3.

---

# 🏗 Arquitetura

```text
Usuário
   │
   ▼
Amazon S3 (Entrada)
   │
Evento ObjectCreated
   ▼
AWS Lambda
   │
Inicia processamento
   ▼
Amazon EC2
   │
Leitura/Gravação
   ▼
Amazon EBS
   │
Resultado Final
   ▼
Amazon S3 (Saída)
```

---

## 🎯 Objetivos

- Automatizar o processamento de arquivos.
- Reduzir intervenção manual.
- Demonstrar integração entre serviços AWS.
- Utilizar arquitetura orientada a eventos (Event-Driven).
- Separar responsabilidades entre armazenamento, processamento e persistência.

---

## ⚙ Fluxo de Funcionamento

### 1. Upload do Arquivo

O usuário realiza o upload de um arquivo para o bucket de entrada do Amazon S3.

Exemplos de arquivos suportados:

- CSV
- JSON
- XML
- TXT

---

### 2. Evento do Amazon S3

Após o upload, o evento **ObjectCreated** é disparado automaticamente.

Esse evento invoca uma função AWS Lambda responsável por iniciar o fluxo de processamento.

---

### 3. Execução da AWS Lambda

A função Lambda executa tarefas iniciais como:

- Validação do evento.
- Validação do nome do arquivo.
- Registro de logs.
- Identificação do bucket.
- Inicialização do processamento na instância EC2.

---

### 4. Processamento na Amazon EC2

A instância EC2 executa a lógica principal da aplicação.

Atividades executadas:

- Leitura do arquivo.
- Limpeza dos dados.
- Transformações.
- Validações.
- Consolidação.
- Geração de novos arquivos.

A aplicação pode ser desenvolvida utilizando tecnologias como:

- Python
- PySpark
- Java
- Node.js

---

### 5. Utilização do Amazon EBS

Durante o processamento, o volume Amazon EBS é utilizado para armazenar:

- Arquivos temporários.
- Logs locais.
- Arquivos intermediários.
- Cache.
- Dados processados antes da publicação.

Esse armazenamento garante melhor desempenho para operações intensivas de leitura e escrita.

---

### 6. Publicação dos Resultados

Após o processamento, os arquivos resultantes são enviados para um bucket de saída no Amazon S3.

Exemplo:

```text
processed/
├── vendas.csv
├── clientes.csv
└── relatorio_final.json
```

---

# ☁ Serviços AWS Utilizados

| Serviço | Responsabilidade |
|----------|------------------|
| Amazon S3 | Armazenamento dos arquivos de entrada e saída |
| AWS Lambda | Processamento inicial e orquestração |
| Amazon EC2 | Execução do processamento principal |
| Amazon EBS | Armazenamento persistente utilizado pela EC2 |

---

# 📁 Estrutura do Projeto

```text
aws-data-pipeline/
│
├── diagrams/
│   └── architecture.drawio
│
├── lambda/
│   └── lambda_function.py
│
├── ec2/
│   ├── processor.py
│   ├── requirements.txt
│   └── config.py
│
├── scripts/
│   └── bootstrap.sh
│
├── docs/
│   └── architecture.md
│
└── README.md
```

---

# 🔒 Boas Práticas

- Aplicação do princípio do menor privilégio (IAM Least Privilege).
- Versionamento dos buckets S3.
- Criptografia dos objetos utilizando SSE-S3 ou SSE-KMS.
- Monitoramento com Amazon CloudWatch.
- Tratamento de exceções e erros.
- Logs estruturados.
- Separação entre ambientes de Desenvolvimento, Homologação e Produção.

---

# 🛠 Tecnologias

- Amazon S3
- AWS Lambda
- Amazon EC2
- Amazon EBS
- Python (exemplo de processamento)
- IAM
- CloudWatch

---
