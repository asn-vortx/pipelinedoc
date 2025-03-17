# ETL Taxa Selic

Este ETL coleta a taxa Selic diária, processa e armazena na AWS.

## 🔹 Tecnologias Usadas:
- **AWS Lambda** para processamento
- **Step Functions** para orquestração
- **Athena/S3** para armazenamento

## 📌 Fluxo do ETL:
1. Consulta a API do Bacen.
2. Processa os dados e salva no S3.
3. Executa consultas no Athena.

## 📌 Código de Exemplo:
```python
import requests

url = "https://api.bcb.gov.br/dados/serie/bcdata.sgs.432/dados?formato=json"
dados = requests.get(url).json()
print(dados)
```