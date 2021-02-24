![elasticsearch](https://miro.medium.com/max/2400/1*BmvPfSSm2G8C-khX1rhCGg.png)
# Búsquedas para un E-Commerce en ElasticSearch

## Paso 1: Creación del Indíce

Lo primero que haremos será crear un CURL request a elasticsearch con el siguiente formato:
El nombre de *omicron-products* puede ser modificado por cualquiera que uno desee. Este nombre es con fines practicos

```
$> curl -XPUT http://127.0.0.1:9200/omicron-products -H "Content-Type: application/json" --data @index-elastic.txt
```

La salida que obtendremos es la siguiente:


```
{
  "acknowledged" : true,
  "shards_acknowledged" : true,
  "index" : "omicron-products"
}
```

## Paso 2:

Ahora insertaremos los articulos que tenemos dentro del archivo de texto. Para ello podemos ejecutar el siguiente comando:

```
$> curl -XPOST http://127.0.0.1:9200/_bulk -H "Content-Type: application/json" --data-binary @articulos-elastic.txt
```

Si el archivo no llegara a funcionar, puedes hacer la misma peticion utilizando el siguiente comando:

```
$> curl -X POST "http://127.0.0.1:9200/_bulk?pretty" -H 'Content-Type: application/json' -d'
{"index" : {"_index" : "omicron-products"}}
{"id" : "001", "description" : "Laptop Lenovo Thinkpad T490 14 Intel Core i5 8265U Disco duro 256 GB SSD Ram 8 GB Windows 10 Pro Color Negro", "category" : "Computadoras", "subcategory" : "Portátiles / Tablets"}
{"index" : {"_index" : "omicron-products"}}
{"id" : "001", "description" : "Laptop HP EliteBook 1040 G4 14 Intel Core i5 7200U Disco duro 256 GB SSD Ram 8 GB Windows 10 Pro Color Plata", "category" : "Computadoras", "subcategory" : "Portátiles / Tablets"}
{"index" : {"_index" : "omicron-products"}}
{"id" : "001", "description" : "NOTEBOOK LENOVO 330-14AST AMD A6-9225 8GB 1TB W10H AZUL 81D5001ELM A4600HU", "category" : "Computadoras", "subcategory" : "Portátiles / Tablets"}
{"index" : {"_index" : "omicron-products"}}
{"id" : "001", "description" : "Desktop Lenovo Thinkcenter M720q Intel Core i7 8700T Disco duro 1 TB Ram 8 GB Windows 10 Pro Color Negro", "category" : "Computadoras", "subcategory" : "Escritorio"}
'
```


La salida que obtenemos de ejecutar la línea anterior es igual a esta:

```
{
  "took" : 127,
  "errors" : false,
  "items" : [
    {
      "index" : {
        "_index" : "omicron-products",
        "_type" : "_doc",
        "_id" : "vNwq1XcBbQCAO4Zs89ab",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 0,
        "_primary_term" : 1,
        "status" : 201
      }
    },
    {
      "index" : {
        "_index" : "omicron-products",
        "_type" : "_doc",
        "_id" : "vdwq1XcBbQCAO4Zs89au",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 1,
        "_primary_term" : 1,
        "status" : 201
      }
    },
    {
      "index" : {
        "_index" : "omicron-products",
        "_type" : "_doc",
        "_id" : "vtwq1XcBbQCAO4Zs89au",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 2,
        "_primary_term" : 1,
        "status" : 201
      }
    },
    {
      "index" : {
        "_index" : "omicron-products",
        "_type" : "_doc",
        "_id" : "v9wq1XcBbQCAO4Zs89au",
        "_version" : 1,
        "result" : "created",
        "_shards" : {
          "total" : 2,
          "successful" : 1,
          "failed" : 0
        },
        "_seq_no" : 3,
        "_primary_term" : 1,
        "status" : 201
      }
    }
  ]
}
```

## Paso 3: Búsquedas

Para realizar las búsquedas, en nuestro archivo de busquedas tenemos el siguiente query:

```
{
    "query" : {
        "multi-match" : {
            "query" : "Laptop",
            "fields" : ["description", "category", "subcategory"]
            "analyzer" : "elastic-synonym-analyzer"
        }
    }
}
```

Utilizaremos el siguiente comando para realizar las busquedas en elasticsearch:

```
$> curl -XPOST http://127.0.0.1:9200/omicron-products/_search?pretty -H "Content-Type: application/json" --data @search-elastic.txt
```

Obtendremos la salida de los documentos que hemos indexado, como se muestra a continuación:

```
{
  "took" : 2,
  "timed_out" : false,
  "_shards" : {
    "total" : 1,
    "successful" : 1,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
      "value" : 3,
      "relation" : "eq"
    },
    "max_score" : 1.4599355,
    "hits" : [
      {
        "_index" : "omicron-products",
        "_type" : "_doc",
        "_id" : "x9w21XcBbQCAO4ZstNZ9",
        "_score" : 1.4599355,
        "_source" : {
          "id" : "001",
          "description" : "Desktop Lenovo Thinkcenter M720q Intel Core i7 8700T Disco duro 1 TB Ram 8 GB Windows 10 Pro Color Negro",
          "category" : "Computadoras",
          "subcategory" : "Escritorio"
        }
      },
      {
        "_index" : "omicron-products",
        "_type" : "_doc",
        "_id" : "xNw21XcBbQCAO4ZstNZ9",
        "_score" : 0.6586049,
        "_source" : {
          "id" : "001",
          "description" : "Laptop Lenovo Thinkpad T490 14\" Intel Core i5 8265U Disco duro 256 GB SSD Ram 8 GB Windows 10 Pro Color Negro",
          "category" : "Computadoras",
          "subcategory" : "Portátiles / Tablets"
        }
      },
      {
        "_index" : "omicron-products",
        "_type" : "_doc",
        "_id" : "xdw21XcBbQCAO4ZstNZ9",
        "_score" : 0.6457332,
        "_source" : {
          "id" : "001",
          "description" : "Laptop HP EliteBook 1040 G4 14\" Intel Core i5 7200U Disco duro 256 GB SSD Ram 8 GB Windows 10 Pro Color Plata",
          "category" : "Computadoras",
          "subcategory" : "Portátiles / Tablets"
        }
      }
    ]
  }
}

```