# Automação Serverless (AWS S3, Lambda e DynamoDB)

Este projeto demonstra como construir uma **máquina de automação** que usa serviços da AWS (Amazon Web Services) que não precisam de servidores.

A ideia principal é: **"Se um arquivo é adicionado, ele é processado automaticamente."**

## A Missão do Projeto

1.  **Automatizar a Tarefa:** Fazer com que o upload de um arquivo acione um código automaticamente.
2.  **Organizar os Dados:** Salvar o resultado desse processamento em um banco de dados rápido e escalável.
3.  **Criar uma API:** Tornar os dados processados acessíveis por meio de um link web simples.

## Como Funciona (A Arquitetura)

O sistema tem dois caminhos: a **Escrita** (quando os dados entram) e a **Leitura** (quando os dados são consultados).

### 1. Fluxo de Escrita (A Automação)

| Passo | Serviço | O Que Acontece |
| :--- | :--- | :--- |
| **1. Upload** | **Amazon S3**  | Você coloca um arquivo (ex: um JSON) no "balde" do S3. |
| **2. Gatilho** | **S3 Trigger** | O S3 **avisa imediatamente** o nosso código sobre o novo arquivo. |
| **3. Processamento** | **AWS Lambda (1ª Função)** | Nosso código **acorda**, lê o arquivo, transforma as informações e enriquece os dados. |
| **4. Salvar** | **DynamoDB**  | O código salva o resultado final no banco de dados. |

### 2. Fluxo de Leitura (A Consulta)

| Passo | Serviço | O Que Acontece |
| :--- | :--- | :--- |
| **1. Requisição** | **Postman / Cliente** | Alguém pede os dados por meio de um **endereço web (URL)**. |
| **2. Roteamento** | **API Gateway** | Ele funciona como um **porteiro**, recebe o pedido e o encaminha para o código certo. |
| **3. Consulta** | **AWS Lambda (2ª Função)** | Este código **acorda**, vai até o DynamoDB e busca o item específico. |
| **4. Resposta** | **API Gateway** | O porteiro entrega o resultado de volta para quem pediu. |

## Por Que Usar Estes Serviços?

| Serviço | Motivo da Escolha |
| :--- | :--- |
| **S3** | **Armazenamento de Início.** É o local mais seguro e barato para guardar arquivos antes de serem processados. |
| **Lambda** | **Pague Pelo Uso.** Só rodamos e pagamos o código quando o arquivo é *realmente* enviado ou quando a API é *realmente* chamada. Não há servidores ligados 24/7. |
| **DynamoDB** | **Rápido e Forte.** É um banco de dados que consegue lidar com milhões de consultas por segundo. Ideal para a API. |
| **API Gateway** | **Segurança e Organização.** É o único ponto de entrada para o sistema, nos permitindo controlar quem acessa e como. |

