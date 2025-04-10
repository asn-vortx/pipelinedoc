# Conectando ao MongoDB

# mongodb_connect
Aqui está um exemplo de conexão com o MongoDB usando `pymongo`:

```python
from contextlib import contextmanager
import logging
from pymongo import MongoClient

@contextmanager
def mongodb_connect(conn_string: str, **kwargs):
    """Gerenciador de contexto para conexão MongoDB.

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

Temos tambem a funcao que faz extracao de documentos de um mongoDB a partir de um client:

```Python
import logging
from typing import Optional, Dict, Any, List
from pymongo import MongoClient

def extract_from_mongo(
    client: MongoClient,
    database: str,
    collection: str,
    query_filter: Optional[Dict[str, Any]] = None
) -> List[Dict[str, Any]]:
    """
    Extrai documentos de uma coleção MongoDB e os retorna como lista de dicionários.

    Args:
        client (MongoClient): Instância do MongoClient já conectada.
        database (str): Nome do banco de dados no MongoDB.
        collection (str): Nome da coleção a ser consultada.
        query_filter (dict, optional): Filtro para o find(). Defaults to {}.

    Returns:
        List[Dict[str, Any]]: Lista de documentos retornados pela query.
    """
    
    query_filter = query_filter or {}
    col = client[database][collection]
    
    try:
        logging.info(f"Iniciando extração da coleção '{collection}' do banco '{database}'")
        
        docs = list(col.find(query_filter))
        
        logging.info(f"Extração concluída: {len(docs)} documentos extraídos da coleção '{collection}'.")
        
        return docs
    
    except Exception as e:
        logging.error(f"Erro ao extrair documentos da coleção '{collection}': {e}", exc_info=True)
        raise
```


## Exemplos de uso

```Python
with mongodb_connect(credentials['CONN_STRING'], uuidRepresentation='pythonLegacy') as client:
    
    query_filter = {"RegisterDate": {"$gte": last_date, "$lt": end_date}}

    docs = extract_from_mongo(client, DATABASE, collection_name, query_filter)
```