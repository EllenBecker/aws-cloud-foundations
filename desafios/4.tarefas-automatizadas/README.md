<div align="center">
  <h1>🚀 Desafio Tarefas automatizadas</h1>
  <strong> Utilizando AWS Lambda, DynamoDB e S3</strong>
  </p>
</div>

O desafio consiste em criar uma **arquitetura automatizada** que processa arquivos enviados para o **Amazon S3**, utilizando o **AWS Lambda** para orquestrar o fluxo e o **Amazon DynamoDB** como banco de dados para registro dos resultados.

## ⚙️ Fluxo da Solução

1. **Upload de Arquivo:**  
   O usuário realiza o upload de um arquivo em um bucket S3.

2. **Trigger Lambda:**  
   Esse evento dispara automaticamente uma função **AWS Lambda** escrita em **Node.js**.

3. **Processamento:**  
   A função Lambda processa o conteúdo do arquivo (por exemplo: leitura, limpeza ou transformação dos dados).

4. **Registro no DynamoDB:**  
   As informações processadas são gravadas em uma tabela do **Amazon DynamoDB**.

## 🧠 Visão Geral do Fluxo

| Etapa      | Serviço          | Descrição                                                    |
| :--------- | :--------------- | :----------------------------------------------------------- |
| **Step 1** | 🗂️ S3            | O arquivo é armazenado no bucket.                            |
| **Step 2** | 🔍 Step Function | Verifica se há arquivos válidos para processamento.          |
| **Step 3** | ⚡ Lambda        | Processa o conteúdo do arquivo e extrai os dados relevantes. |
| **Step 4** | 🧾 DynamoDB      | Armazena o resultado do processamento em uma tabela NoSQL.   |

## 🧰 Ambiente de Desenvolvimento

Para desenvolvimento e testes, foi utilizado o **[LocalStack](https://localstack.cloud/)**, uma ferramenta que simula os serviços da AWS localmente.  
Isso permite validar toda a arquitetura sem custos na nuvem, garantindo que:

- As funções Lambda funcionem corretamente;
- As interações com o S3 e DynamoDB ocorram como esperado;
- O fluxo completo seja testado antes da implantação real na AWS.

## 🪣 Criação do Bucket S3

Crie o bucket S3 no LocalStack:

```bash
awslocal s3api create-bucket --bucket notas-fiscais-upload
```

Para verificar se o bucket foi criado, execute:

```bash
awslocal s3api list-buckets
```

## 🗄️ Criação da Tabela no DynamoDB

Crie uma tabela com a chave primária id no LocalStack:

```bash
aws dynamodb create-table \
  --endpoint-url=http://localhost:4566 \
  --table-name NotasFiscais \
  --attribute-definitions AttributeName=id,AttributeType=S \
  --key-schema AttributeName=id,KeyType=HASH \
  --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5

```

Para verificar se a tabela foi criada, execute:

```bash
awslocal dynamodb list-tables
```
