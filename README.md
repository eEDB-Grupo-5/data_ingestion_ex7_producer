# ex7-producer

This project is a Python-based producer that reads CSV files from an AWS S3 bucket, encodes the data into Avro format, and sends it as messages to an AWS SQS queue.

## Features

- **CSV Processing:** Reads CSV files from a specified S3 bucket and prefix.
- **Avro Encoding:** Encodes data into Avro format using a predefined schema.
- **SQS Messaging:** Sends Avro-encoded messages to an SQS queue.
- **Continuous Polling:** Continuously polls the S3 bucket for new files.
- **Configuration:** Easily configurable through environment variables.
- **Containerization:** Docker support for easy deployment.

## Prerequisites

- Python 3.10+
- Poetry
- AWS Account with S3 and SQS access
- Docker (optional)

## Installation

1.  Clone the repository:
    ```bash
    git clone <repository-url>
    cd ex7-producer
    ```
2.  Install the dependencies using Poetry:
    ```bash
    poetry install
    ```

## Configuration

1.  Create a `.env` file in the root of the project:
    ```bash
    cp .env.example .env
    ```
2.  Update the `.env` file with your AWS credentials and other settings:
    ```
    AWS_DEFAULT_REGION=<your-aws-region>
    AWS_ACCESS_KEY_ID=<your-aws-access-key-id>
    AWS_SECRET_ACCESS_KEY=<your-aws-secret-access-key>
    AWS_ENDPOINT_URL=<your-aws-endpoint-url> # e.g., http://localhost:4566 for localstack
    S3_BUCKET_NAME=<your-s3-bucket-name>
    S3_PATH_PREFIX=<your-s3-path-prefix> # optional
    SQS_QUEUE_URL=<your-sqs-queue-url>
    ```

## Usage

To run the producer, use the following command:

```bash
poetry run python src/ex7_producer/app.py
```

The application will start polling the S3 bucket for new CSV files and send the data to the SQS queue.

## Data Schema

The project uses an Avro schema to structure the data. The schema is defined in `src/ex7_producer/schemas/reclamacoes.avsc`.

The following fields are defined in the schema:

| Field                                               | Type     | Description                               |
| --------------------------------------------------- | -------- | ----------------------------------------- |
| `ano`                                               | `string` | Year of the complaint.                    |
| `trimestre`                                         | `string` | Quarter of the complaint.                 |
| `categoria`                                         | `string` | Category of the complaint.                |
| `tipo`                                              | `string` | Type of the complaint.                    |
| `cnpj_if`                                           | `string` | CNPJ of the financial institution.        |
| `instituicao_financeira`                            | `string` | Name of the financial institution.        |
| `indice`                                            | `string` | Index of the complaint.                   |
| `quantidade_de_reclamacoes_reguladas_procedentes`   | `string` | Number of valid regulated complaints.     |
| `quantidade_de_reclamacoes_reguladas_outras`        | `string` | Number of other regulated complaints.     |
| `quantidade_de_reclamacoes_nao_reguladas`           | `string` | Number of non-regulated complaints.       |
| `quantidade_total_de_reclamacoes`                   | `string` | Total number of complaints.               |
| `quantidade_total_de_clientes_ccs_e_scr`            | `string` | Total number of clients (CCS and SCR).    |
| `quantidade_de_clientes_ccs`                        | `string` | Number of clients (CCS).                  |
| `quantidade_de_clientes_scr`                        | `string` | Number of clients (SCR).                  |

## Docker

To build and run the project with Docker, use the following commands:

1.  Build the Docker image:
    ```bash
    docker build -t ex7-producer .
    ```
2.  Run the Docker container:
    ```bash
    docker run --env-file .env ex7-producer
    ```
