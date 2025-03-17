# Conectando ao MongoDB

# mongodb_connect
Aqui está um exemplo de conexão com o MongoDB usando `pymongo`:

```python
from pymongo import MongoClient
import logging
from contextlib import contextmanager

@contextmanager
def mongodb_connect(conn_string: str, **kwargs):
    """Gerenciador de contexto para conexão MongoDB flexível.

    Args:
        conn_string (str): String de conexão do MongoDB.
        **kwargs: Parâmetros opcionais para o MongoClient.

    Yield:
        MongoClient: Conexão ativa com o MongoDB.
    """

    try:
        logging.info("Creating MongoDB connection")
        conn = MongoClient(conn_string, **kwargs)
        yield conn
    except Exception as e:
        logging.error(f"Database connection error: {e}", exc_info=True)
        raise
    finally:
        if conn:
            logging.info("Closing MongoDB connection")
            conn.close()
```

## Exemplos de uso

```Python
conn_string = "mongodb://localhost:27017"
with mongodb_connect(conn_string) as client:
    db = client["meu_banco"]
    print(db.list_collection_names())
```

!!! warning "Cuidado!"
    Certifique-se de passar um `conn_string` válido para evitar falhas de conexão.