# Gerador de QR Code


Uma aplicação Spring Boot que gera códigos QR e os armazena na AWS S3. Este projeto demonstra a integração da biblioteca ZXing do Google para geração de QR Codes e do S3 da AWS para armazenamento.

## Índice

- [Como Usar](#como-usar)
    - [Pré-requisitos](#pré-requisitos)
    - [Variáveis de Ambiente](#variáveis-de-ambiente)
    - [Executando a Aplicação](#executando-a-aplicação)
        - [Desenvolvimento Local](#desenvolvimento-local)
        - [Implantação com Docker](#implantação-com-docker)
    - [Configuração do AWS S3](#configuração-do-aws-s3)
- [Endpoints da API](#endpoints-da-api)

## Como Usar

Esta seção fornece instruções completas para configurar e executar a aplicação Geradora de QR Code.

### Pré-requisitos

- JDK Java 21
- Maven
- Docker
- Conta AWS com acesso ao S3
- AWS CLI configurado com as credenciais apropriadas

### Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto com as seguintes variáveis:

```env
AWS_ACCESS_KEY_ID=sua_chave_de_acesso
AWS_SECRET_ACCESS_KEY=sua_chave_secreta
AWS_REGION=sua_regiao
AWS_BUCKET_NAME=nome_do_seu_bucket
```

### Executando a Aplicação

#### Desenvolvimento Local

1. Crie o arquivo `.env` conforme descrito acima
2. Compile o projeto:

   ```bash
   mvn clean package
   ```

3. Execute a aplicação:

   ```bash
   mvn spring-boot:run
   ```

#### Implantação com Docker

1. Construa a imagem Docker:

   ```bash
   docker build -t qrcode-generator:X.X .
   ```

   > Lembre-se de substituir o nome e a versão da imagem, se desejar.

2. Execute o contêiner:

   ```bash
   docker run --env-file .env -p 8080:8080 qrcode-generator:X.X
   ```

   > Lembre-se de ajustar o caminho do arquivo `.env` se ele estiver em outro local.

### Configuração do AWS S3

1. Crie um bucket S3 na sua conta AWS
2. Atualize a variável `AWS_BUCKET_NAME` no seu arquivo `.env` ou no comando Docker
3. Certifique-se de que suas credenciais da AWS tenham permissões adequadas para acessar o bucket S3


## Endpoints da API

### POST /qrcode

Gera um QR Code a partir do texto fornecido e o armazena na AWS S3. O QR Code será gerado como uma imagem PNG com dimensões de 200x200 pixels.

**Parâmetros**

| Nome   | Obrigatório | Tipo   | Descrição |
|--------|-------------|--------|-----------|
| `text` | sim         | string | O conteúdo de texto a ser codificado no QR Code. Pode ser qualquer string que você deseje converter. |

**Resposta**

```json
{
  "url": "https://seu-bucket.s3.sua-regiao.amazonaws.com/uuid-aleatorio"
}
```

**Resposta de Erro**

Se ocorrer algum erro durante a geração do QR Code ou upload para o S3, a API retornará um erro 500 (Internal Server Error).

**Exemplo de Uso**

```bash
curl -X POST http://localhost:8080/qrcode      -H "Content-Type: application/json"      -d '{"text": "https://example.com"}'
```
