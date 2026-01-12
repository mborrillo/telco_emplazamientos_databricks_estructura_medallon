03_gold_analytics.py`  
  - Construye una tabla maestra por `codigo_unico` combinando la información de los tres módulos.  
  - Calcula métricas temporales como `dias_desde_ultima_modificacion`.  
  - Genera tablas agregadas mensuales por módulo, cabecera y `areaOYM`, y calcula ratios de disponibilidad de emplazamientos por área.
[03_gold_analytics.py](https://github.com/user-attachments/files/24573013/03_gold_analytics.py)
    "## CAPA GOLD"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "8aad912f-948b-4be1-9b56-76105e00c8fc",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "Se  evaluan los estados de cada modulo previamente (antes de definir las agrupaciones de estados general)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "5bf82f04-0cf4-4703-8d1b-f97d8a2786ea",
     "showTitle": false,
     "tableResultSettingsMap": {
      "0": {
       "dataGridStateBlob": "{\"version\":1,\"tableState\":{\"columnPinning\":{\"left\":[\"#row_number#\"],\"right\":[]},\"columnSizing\":{\"estado\":155},\"columnVisibility\":{}},\"settings\":{\"columns\":{}},\"syncTimestamp\":1768142065313}",
       "filterBlob": null,
       "queryPlanFiltersBlob": null,
       "tableResultIndex": 0
      }
     },
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>modulo</th><th>estado</th><th>num_filas</th></tr></thead><tbody><tr><td>Centros TX</td><td>activo</td><td>11234</td></tr><tr><td>Centros TX</td><td>baja</td><td>30</td></tr><tr><td>Infraestructura</td><td>A Restituir</td><td>104</td></tr><tr><td>Infraestructura</td><td>Candidato</td><td>30</td></tr><tr><td>Infraestructura</td><td>Liberado</td><td>6468</td></tr><tr><td>Infraestructura</td><td>No Utilizado</td><td>3949</td></tr><tr><td>Infraestructura</td><td>Preliberado</td><td>4</td></tr><tr><td>Infraestructura</td><td>Restituido</td><td>1626</td></tr><tr><td>Sitios RAN</td><td>baja</td><td>342</td></tr><tr><td>Sitios RAN</td><td>desdefinido</td><td>1136</td></tr><tr><td>Sitios RAN</td><td>liberado</td><td>8682</td></tr><tr><td>Sitios RAN</td><td>planificado</td><td>1900</td></tr></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [
        [
         "Centros TX",
         "activo",
         11234
        ],
        [
         "Centros TX",
         "baja",
         30
        ],
        [
         "Infraestructura",
         "A Restituir",
         104
        ],
        [
         "Infraestructura",
         "Candidato",
         30
        ],
        [
         "Infraestructura",
         "Liberado",
         6468
        ],
        [
         "Infraestructura",
         "No Utilizado",
         3949
        ],
        [
         "Infraestructura",
         "Preliberado",
         4
        ],
        [
         "Infraestructura",
         "Restituido",
         1626
        ],
        [
         "Sitios RAN",
         "baja",
         342
        ],
        [
         "Sitios RAN",
         "desdefinido",
         1136
        ],
        [
         "Sitios RAN",
         "liberado",
         8682
        ],
        [
         "Sitios RAN",
         "planificado",
         1900
        ]
       ],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "modulo",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "estado",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "num_filas",
            "nullable": false,
            "type": "long"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 8
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "modulo",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "estado",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "num_filas",
         "type": "\"long\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "--Listo todos los estados de cada modulo, a fin de evaluar y decidir agrupaciones de estado comunes\n",
    "SELECT\n",
    "  modulo,\n",
    "  estado,\n",
    "  COUNT(*) AS num_filas\n",
    "FROM silver_emplazamientos_modulo\n",
    "GROUP BY modulo, estado\n",
    "ORDER BY modulo, estado;"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "90b96500-c579-4b6c-a196-f2a70f36d3fe",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "Se crea una tabla maestra por codigo_unico con estados y fechas por módulo, y tablas agregadas para analizar disponibilidad por área, región y evolución temporal. Luego creamos varias tablas, cada una de ellas responde a preguntas relevantes del negocio/operacion."
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "9a19a4a6-7f42-4eb0-9108-592e9586ca64",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "1. Backlog de pendientes por módulo\n",
    " \n",
    "“¿Qué centros están pendientes en cada módulo, con su última fecha y localización OYM?”"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "f4f5d756-ce79-428f-98d0-a7f30dade45f",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>num_affected_rows</th><th>num_inserted_rows</th></tr></thead><tbody></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "num_affected_rows",
            "nullable": true,
            "type": "long"
           },
           {
            "metadata": {},
            "name": "num_inserted_rows",
            "nullable": true,
            "type": "long"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 9
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "num_affected_rows",
         "type": "\"long\""
        },
        {
         "metadata": "{}",
         "name": "num_inserted_rows",
         "type": "\"long\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "DROP TABLE IF EXISTS gold_centros_pendientes_por_modulo;\n",
    "\n",
    "CREATE OR REPLACE TABLE gold_centros_pendientes_por_modulo\n",
    "USING DELTA\n",
    "AS\n",
    "SELECT\n",
    "  modulo,\n",
    "  codigo_unico,\n",
    "  nombre_sitio,\n",
    "  estado,\n",
    "  fecha_modificacion,\n",
    "  cabeceraOYM,\n",
    "  areaOYM\n",
    "FROM silver_emplazamientos_modulo\n",
    "WHERE UPPER(estado) IN ('PLANIFICADO', 'LIBERADO', 'DESDEFINIDO', 'BAJA', 'ACTIVO', 'NO UTILIZADO', 'RESTITUIDO', 'A RESTITUIR'); \n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "2f4edae1-96fe-41e0-9832-cd93158f50fd",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>modulo</th><th>codigo_unico</th><th>nombre_sitio</th><th>estado</th><th>fecha_modificacion</th><th>cabeceraOYM</th><th>areaOYM</th></tr></thead><tbody><tr><td>Centros TX</td><td>ARBA10144</td><td>ARBA10144</td><td>activo</td><td>2016-01-25T00:00:00.000Z</td><td>SUR</td><td>BAHIA BLANCA</td></tr><tr><td>Centros TX</td><td>ARER0519</td><td>ARER0519</td><td>activo</td><td>2016-04-28T00:00:00.000Z</td><td>NEA</td><td>ENTRE RIOS_ZONA 02</td></tr><tr><td>Centros TX</td><td>ARBA13732</td><td>ARBA13732</td><td>activo</td><td>2016-04-29T00:00:00.000Z</td><td>BUENOS AIRES</td><td>LA PLATA</td></tr><tr><td>Centros TX</td><td>ARBA13713</td><td>ARBA13713</td><td>activo</td><td>2016-04-29T00:00:00.000Z</td><td>BUENOS AIRES</td><td>LA PLATA</td></tr><tr><td>Centros TX</td><td>ARBA12749</td><td>ARBA12749</td><td>activo</td><td>2016-04-29T00:00:00.000Z</td><td>BUENOS AIRES</td><td>LA PLATA</td></tr></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [
        [
         "Centros TX",
         "ARBA10144",
         "ARBA10144",
         "activo",
         "2016-01-25T00:00:00.000Z",
         "SUR",
         "BAHIA BLANCA"
        ],
        [
         "Centros TX",
         "ARER0519",
         "ARER0519",
         "activo",
         "2016-04-28T00:00:00.000Z",
         "NEA",
         "ENTRE RIOS_ZONA 02"
        ],
        [
         "Centros TX",
         "ARBA13732",
         "ARBA13732",
         "activo",
         "2016-04-29T00:00:00.000Z",
         "BUENOS AIRES",
         "LA PLATA"
        ],
        [
         "Centros TX",
         "ARBA13713",
         "ARBA13713",
         "activo",
         "2016-04-29T00:00:00.000Z",
         "BUENOS AIRES",
         "LA PLATA"
        ],
        [
         "Centros TX",
         "ARBA12749",
         "ARBA12749",
         "activo",
         "2016-04-29T00:00:00.000Z",
         "BUENOS AIRES",
         "LA PLATA"
        ]
       ],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "modulo",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "codigo_unico",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "nombre_sitio",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "estado",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "fecha_modificacion",
            "nullable": true,
            "type": "timestamp"
           },
           {
            "metadata": {},
            "name": "cabeceraOYM",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "areaOYM",
            "nullable": true,
            "type": "string"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 10
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "modulo",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "codigo_unico",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "nombre_sitio",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "estado",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "fecha_modificacion",
         "type": "\"timestamp\""
        },
        {
         "metadata": "{}",
         "name": "cabeceraOYM",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "areaOYM",
         "type": "\"string\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "select *\n",
    "from gold_centros_pendientes_por_modulo\n",
    "limit 5"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "95dbd903-0e18-440d-97a5-5b493864d067",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "2. Resumen de backlog por región / cabecera\n",
    "\n",
    "¿Cuántos centros pendientes tiene cada región / cabecera por módulo?"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "518c378f-1e34-4128-86fd-d6bf5efc8630",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>num_affected_rows</th><th>num_inserted_rows</th></tr></thead><tbody></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "num_affected_rows",
            "nullable": true,
            "type": "long"
           },
           {
            "metadata": {},
            "name": "num_inserted_rows",
            "nullable": true,
            "type": "long"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 11
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "num_affected_rows",
         "type": "\"long\""
        },
        {
         "metadata": "{}",
         "name": "num_inserted_rows",
         "type": "\"long\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "DROP TABLE IF EXISTS gold_backlog_resumen_oym;\n",
    "\n",
    "CREATE OR REPLACE TABLE gold_backlog_resumen_oym\n",
    "USING DELTA\n",
    "AS\n",
    "SELECT\n",
    "  modulo,\n",
    "  cabeceraOYM,\n",
    "  areaOYM,\n",
    "  COUNT(*) AS num_centros_pendientes\n",
    "FROM gold_centros_pendientes_por_modulo\n",
    "GROUP BY modulo, cabeceraOYM, areaOYM;\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "e5b4da78-f515-43fa-95a2-ce2626512fa0",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>modulo</th><th>cabeceraOYM</th><th>areaOYM</th><th>num_centros_pendientes</th></tr></thead><tbody><tr><td>Sitios RAN</td><td>CUYO</td><td>INT</td><td>906</td></tr><tr><td>Centros TX</td><td>NEA</td><td>MISIONES_ZONA 02</td><td>13</td></tr><tr><td>Centros TX</td><td>NEA</td><td>ROSARIO_ZONA 04</td><td>67</td></tr><tr><td>Infraestructura</td><td>BUENOS AIRES</td><td>LA PLATA</td><td>229</td></tr><tr><td>Infraestructura</td><td>NEA</td><td>ESTE</td><td>3</td></tr></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [
        [
         "Sitios RAN",
         "CUYO",
         "INT",
         906
        ],
        [
         "Centros TX",
         "NEA",
         "MISIONES_ZONA 02",
         13
        ],
        [
         "Centros TX",
         "NEA",
         "ROSARIO_ZONA 04",
         67
        ],
        [
         "Infraestructura",
         "BUENOS AIRES",
         "LA PLATA",
         229
        ],
        [
         "Infraestructura",
         "NEA",
         "ESTE",
         3
        ]
       ],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "modulo",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "cabeceraOYM",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "areaOYM",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "num_centros_pendientes",
            "nullable": true,
            "type": "long"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 12
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "modulo",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "cabeceraOYM",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "areaOYM",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "num_centros_pendientes",
         "type": "\"long\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "select *\n",
    "from gold_backlog_resumen_oym\n",
    "limit 5 "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "80f98716-6eae-48b6-9c21-74b3960269f2",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "3. Estado consolidado por emplazamiento (cross-módulo)\n",
    "\n",
    "¿Qué estado tiene cada emplazamiento en cada módulo, con fecha de última modificación?"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "2f77bcd4-d3f7-43b3-8bc8-3a12f92db93c",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>num_affected_rows</th><th>num_inserted_rows</th></tr></thead><tbody></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "num_affected_rows",
            "nullable": true,
            "type": "long"
           },
           {
            "metadata": {},
            "name": "num_inserted_rows",
            "nullable": true,
            "type": "long"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 13
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "num_affected_rows",
         "type": "\"long\""
        },
        {
         "metadata": "{}",
         "name": "num_inserted_rows",
         "type": "\"long\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "DROP TABLE IF EXISTS gold_emplazamientos_estado_cross_modulo;\n",
    "\n",
    "CREATE OR REPLACE TABLE gold_emplazamientos_estado_cross_modulo\n",
    "USING DELTA\n",
    "AS\n",
    "SELECT\n",
    "  codigo_unico,\n",
    "\n",
    "  MAX(CASE WHEN modulo = 'Centros TX'      THEN nombre_sitio END) AS nombre_sitio_ref,\n",
    "\n",
    "  MAX(CASE WHEN modulo = 'Centros TX'      THEN estado END) AS estado_tx,\n",
    "  MAX(CASE WHEN modulo = 'Sitios RAN'      THEN estado END) AS estado_ran,\n",
    "  MAX(CASE WHEN modulo = 'Infraestructura' THEN estado END) AS estado_infra,\n",
    "\n",
    "  MAX(CASE WHEN modulo = 'Centros TX'      THEN fecha_modificacion END) AS fecha_tx,\n",
    "  MAX(CASE WHEN modulo = 'Sitios RAN'      THEN fecha_modificacion END) AS fecha_ran,\n",
    "  MAX(CASE WHEN modulo = 'Infraestructura' THEN fecha_modificacion END) AS fecha_infra\n",
    "FROM silver_emplazamientos_modulo\n",
    "GROUP BY codigo_unico;\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "5af9d3f7-5b84-415e-b450-d6f889fa9a76",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>codigo_unico</th><th>nombre_sitio_ref</th><th>estado_tx</th><th>estado_ran</th><th>estado_infra</th><th>fecha_tx</th><th>fecha_ran</th><th>fecha_infra</th></tr></thead><tbody><tr><td>ARBA9652</td><td>ARBA9652</td><td>activo</td><td>liberado</td><td>Liberado</td><td>2015-12-01T00:00:00.000Z</td><td>2022-09-22T00:00:00.000Z</td><td>2017-11-24T20:20:09.000Z</td></tr><tr><td>ARLR0030</td><td>ARLR0030</td><td>activo</td><td>liberado</td><td>null</td><td>2015-12-01T00:00:00.000Z</td><td>2024-09-03T00:00:00.000Z</td><td>null</td></tr><tr><td>ARBA1778</td><td>ARBA1778</td><td>activo</td><td>liberado</td><td>Liberado</td><td>2015-12-01T00:00:00.000Z</td><td>2024-07-10T00:00:00.000Z</td><td>2017-11-24T20:20:09.000Z</td></tr><tr><td>ARSJ0206</td><td>ARSJ0206</td><td>activo</td><td>liberado</td><td>Liberado</td><td>2015-12-01T00:00:00.000Z</td><td>2024-06-13T00:00:00.000Z</td><td>2017-11-24T20:20:09.000Z</td></tr><tr><td>ARBA7224</td><td>ARBA7224</td><td>activo</td><td>liberado</td><td>Liberado</td><td>2015-12-01T00:00:00.000Z</td><td>2024-03-18T00:00:00.000Z</td><td>2017-11-24T20:20:09.000Z</td></tr></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [
        [
         "ARBA9652",
         "ARBA9652",
         "activo",
         "liberado",
         "Liberado",
         "2015-12-01T00:00:00.000Z",
         "2022-09-22T00:00:00.000Z",
         "2017-11-24T20:20:09.000Z"
        ],
        [
         "ARLR0030",
         "ARLR0030",
         "activo",
         "liberado",
         null,
         "2015-12-01T00:00:00.000Z",
         "2024-09-03T00:00:00.000Z",
         null
        ],
        [
         "ARBA1778",
         "ARBA1778",
         "activo",
         "liberado",
         "Liberado",
         "2015-12-01T00:00:00.000Z",
         "2024-07-10T00:00:00.000Z",
         "2017-11-24T20:20:09.000Z"
        ],
        [
         "ARSJ0206",
         "ARSJ0206",
         "activo",
         "liberado",
         "Liberado",
         "2015-12-01T00:00:00.000Z",
         "2024-06-13T00:00:00.000Z",
         "2017-11-24T20:20:09.000Z"
        ],
        [
         "ARBA7224",
         "ARBA7224",
         "activo",
         "liberado",
         "Liberado",
         "2015-12-01T00:00:00.000Z",
         "2024-03-18T00:00:00.000Z",
         "2017-11-24T20:20:09.000Z"
        ]
       ],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "codigo_unico",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "nombre_sitio_ref",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "estado_tx",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "estado_ran",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "estado_infra",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "fecha_tx",
            "nullable": true,
            "type": "timestamp"
           },
           {
            "metadata": {},
            "name": "fecha_ran",
            "nullable": true,
            "type": "timestamp"
           },
           {
            "metadata": {},
            "name": "fecha_infra",
            "nullable": true,
            "type": "timestamp"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 14
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "codigo_unico",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "nombre_sitio_ref",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "estado_tx",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "estado_ran",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "estado_infra",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "fecha_tx",
         "type": "\"timestamp\""
        },
        {
         "metadata": "{}",
         "name": "fecha_ran",
         "type": "\"timestamp\""
        },
        {
         "metadata": "{}",
         "name": "fecha_infra",
         "type": "\"timestamp\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "select *\n",
    "from gold_emplazamientos_estado_cross_modulo\n",
    "limit 5"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "d136c099-3409-495b-860c-7ae571df35ca",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "4. Emplazamientos con inconsistencias de estado entre módulos\n",
    "\n",
    "¿Qué centros tienen estados distintos entre módulos (por ejemplo, cerrado en uno y pendiente en otro)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "c7f9ea54-294f-4114-b5a9-45f314647498",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>num_affected_rows</th><th>num_inserted_rows</th></tr></thead><tbody></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "num_affected_rows",
            "nullable": true,
            "type": "long"
           },
           {
            "metadata": {},
            "name": "num_inserted_rows",
            "nullable": true,
            "type": "long"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 15
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "num_affected_rows",
         "type": "\"long\""
        },
        {
         "metadata": "{}",
         "name": "num_inserted_rows",
         "type": "\"long\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "DROP TABLE IF EXISTS gold_emplazamientos_inconsistencias_estado;\n",
    "\n",
    "CREATE OR REPLACE TABLE gold_emplazamientos_inconsistencias_estado\n",
    "USING DELTA\n",
    "AS\n",
    "WITH base AS (\n",
    "  SELECT *\n",
    "  FROM gold_emplazamientos_estado_cross_modulo\n",
    "),\n",
    "norm AS (\n",
    "  SELECT\n",
    "    codigo_unico,\n",
    "    UPPER(COALESCE(estado_tx, 'SIN_DATO'))   AS estado_tx,\n",
    "    UPPER(COALESCE(estado_ran, 'SIN_DATO'))  AS estado_ran,\n",
    "    UPPER(COALESCE(estado_infra, 'SIN_DATO')) AS estado_infra\n",
    "  FROM base\n",
    ")\n",
    "SELECT *\n",
    "FROM norm\n",
    "WHERE NOT (estado_tx = estado_ran AND estado_ran = estado_infra);\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "3387c8e3-d27e-4a6e-a3e6-5e6fb3faa762",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>codigo_unico</th><th>estado_tx</th><th>estado_ran</th><th>estado_infra</th></tr></thead><tbody><tr><td>ARBA9652</td><td>ACTIVO</td><td>LIBERADO</td><td>LIBERADO</td></tr><tr><td>ARLR0030</td><td>ACTIVO</td><td>LIBERADO</td><td>SIN_DATO</td></tr><tr><td>ARBA1778</td><td>ACTIVO</td><td>LIBERADO</td><td>LIBERADO</td></tr><tr><td>ARSJ0206</td><td>ACTIVO</td><td>LIBERADO</td><td>LIBERADO</td></tr><tr><td>ARBA7224</td><td>ACTIVO</td><td>LIBERADO</td><td>LIBERADO</td></tr></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [
        [
         "ARBA9652",
         "ACTIVO",
         "LIBERADO",
         "LIBERADO"
        ],
        [
         "ARLR0030",
         "ACTIVO",
         "LIBERADO",
         "SIN_DATO"
        ],
        [
         "ARBA1778",
         "ACTIVO",
         "LIBERADO",
         "LIBERADO"
        ],
        [
         "ARSJ0206",
         "ACTIVO",
         "LIBERADO",
         "LIBERADO"
        ],
        [
         "ARBA7224",
         "ACTIVO",
         "LIBERADO",
         "LIBERADO"
        ]
       ],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "codigo_unico",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "estado_tx",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "estado_ran",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "estado_infra",
            "nullable": true,
            "type": "string"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 16
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "codigo_unico",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "estado_tx",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "estado_ran",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "estado_infra",
         "type": "\"string\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "select *\n",
    "from gold_emplazamientos_inconsistencias_estado\n",
    "limit 5"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "537c0f24-d884-4cde-889a-b44d89ec4bf4",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "5. Emplazamientos inactivos / obsoletos por antigüedad\n",
    "\n",
    "¿Qué emplazamientos no se han tocado desde hace X días y podrían revisarse o depurarse?"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "b68d4692-6bf5-41b3-9707-b5435a3877e9",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>num_affected_rows</th><th>num_inserted_rows</th></tr></thead><tbody></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "num_affected_rows",
            "nullable": true,
            "type": "long"
           },
           {
            "metadata": {},
            "name": "num_inserted_rows",
            "nullable": true,
            "type": "long"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 17
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "num_affected_rows",
         "type": "\"long\""
        },
        {
         "metadata": "{}",
         "name": "num_inserted_rows",
         "type": "\"long\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "DROP TABLE IF EXISTS gold_emplazamientos_sin_actualizacion;\n",
    "\n",
    "CREATE OR REPLACE TABLE gold_emplazamientos_sin_actualizacion\n",
    "USING DELTA\n",
    "AS\n",
    "SELECT\n",
    "  modulo,\n",
    "  codigo_unico,\n",
    "  nombre_sitio,\n",
    "  estado,\n",
    "  fecha_modificacion,\n",
    "  cabeceraOYM,\n",
    "  areaOYM,\n",
    "  datediff(current_date(), CAST(fecha_modificacion AS DATE)) AS dias_desde_ultima_modificacion\n",
    "FROM silver_emplazamientos_modulo\n",
    "WHERE fecha_modificacion IS NOT NULL\n",
    "  AND datediff(current_date(), CAST(fecha_modificacion AS DATE)) > 180;\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "587df605-8c16-4ea7-b73c-dbacd1c01459",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>modulo</th><th>codigo_unico</th><th>nombre_sitio</th><th>estado</th><th>fecha_modificacion</th><th>cabeceraOYM</th><th>areaOYM</th><th>dias_desde_ultima_modificacion</th></tr></thead><tbody><tr><td>Centros TX</td><td>ARBA10144</td><td>ARBA10144</td><td>activo</td><td>2016-01-25T00:00:00.000Z</td><td>SUR</td><td>BAHIA BLANCA</td><td>3640</td></tr><tr><td>Centros TX</td><td>ARER0519</td><td>ARER0519</td><td>activo</td><td>2016-04-28T00:00:00.000Z</td><td>NEA</td><td>ENTRE RIOS_ZONA 02</td><td>3546</td></tr><tr><td>Centros TX</td><td>ARBA13732</td><td>ARBA13732</td><td>activo</td><td>2016-04-29T00:00:00.000Z</td><td>BUENOS AIRES</td><td>LA PLATA</td><td>3545</td></tr><tr><td>Centros TX</td><td>ARBA13713</td><td>ARBA13713</td><td>activo</td><td>2016-04-29T00:00:00.000Z</td><td>BUENOS AIRES</td><td>LA PLATA</td><td>3545</td></tr><tr><td>Centros TX</td><td>ARBA12749</td><td>ARBA12749</td><td>activo</td><td>2016-04-29T00:00:00.000Z</td><td>BUENOS AIRES</td><td>LA PLATA</td><td>3545</td></tr></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [
        [
         "Centros TX",
         "ARBA10144",
         "ARBA10144",
         "activo",
         "2016-01-25T00:00:00.000Z",
         "SUR",
         "BAHIA BLANCA",
         3640
        ],
        [
         "Centros TX",
         "ARER0519",
         "ARER0519",
         "activo",
         "2016-04-28T00:00:00.000Z",
         "NEA",
         "ENTRE RIOS_ZONA 02",
         3546
        ],
        [
         "Centros TX",
         "ARBA13732",
         "ARBA13732",
         "activo",
         "2016-04-29T00:00:00.000Z",
         "BUENOS AIRES",
         "LA PLATA",
         3545
        ],
        [
         "Centros TX",
         "ARBA13713",
         "ARBA13713",
         "activo",
         "2016-04-29T00:00:00.000Z",
         "BUENOS AIRES",
         "LA PLATA",
         3545
        ],
        [
         "Centros TX",
         "ARBA12749",
         "ARBA12749",
         "activo",
         "2016-04-29T00:00:00.000Z",
         "BUENOS AIRES",
         "LA PLATA",
         3545
        ]
       ],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "modulo",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "codigo_unico",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "nombre_sitio",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "estado",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "fecha_modificacion",
            "nullable": true,
            "type": "timestamp"
           },
           {
            "metadata": {},
            "name": "cabeceraOYM",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "areaOYM",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "dias_desde_ultima_modificacion",
            "nullable": true,
            "type": "integer"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 18
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "modulo",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "codigo_unico",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "nombre_sitio",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "estado",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "fecha_modificacion",
         "type": "\"timestamp\""
        },
        {
         "metadata": "{}",
         "name": "cabeceraOYM",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "areaOYM",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "dias_desde_ultima_modificacion",
         "type": "\"integer\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "select *\n",
    "from gold_emplazamientos_sin_actualizacion \n",
    "limit 5"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "23b5a7ce-532f-471f-af5a-534bbfd6f046",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "6. Evolución Mensual de Altas/Bajas por Módulo"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "6e0189be-f26c-4d3a-aa2b-289fc54518c9",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>modulo</th><th>yearMonth</th><th>estado</th><th>areaOYM</th><th>cabeceraOYM</th><th>num_emplazamientos</th></tr></thead><tbody><tr><td>Centros TX</td><td>2025-05</td><td>activo</td><td></td><td></td><td>5</td></tr><tr><td>Centros TX</td><td>2025-05</td><td>activo</td><td></td><td>NEA</td><td>8</td></tr><tr><td>Centros TX</td><td>2025-05</td><td>activo</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-05</td><td>activo</td><td></td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-05</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2025-05</td><td>activo</td><td></td><td>NOA</td><td>10</td></tr><tr><td>Centros TX</td><td>2025-05</td><td>activo</td><td></td><td>SUR</td><td>5</td></tr><tr><td>Centros TX</td><td>2025-05</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2025-05</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2025-04</td><td>Liberado</td><td></td><td></td><td>4</td></tr><tr><td>Centros TX</td><td>2025-04</td><td>activo</td><td></td><td>NOA</td><td>6</td></tr><tr><td>Centros TX</td><td>2025-04</td><td>activo</td><td>PEHUAJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-04</td><td>activo</td><td></td><td></td><td>5</td></tr><tr><td>Centros TX</td><td>2025-04</td><td>activo</td><td>MISIONES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-04</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-04</td><td>activo</td><td></td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-04</td><td>activo</td><td>LA RIOJA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-04</td><td>activo</td><td></td><td>NEA</td><td>7</td></tr><tr><td>Centros TX</td><td>2025-04</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>18</td></tr><tr><td>Centros TX</td><td>2025-04</td><td>activo</td><td></td><td>SUR</td><td>12</td></tr><tr><td>Centros TX</td><td>2025-04</td><td>activo</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-03</td><td>activo</td><td></td><td></td><td>2</td></tr><tr><td>Centros TX</td><td>2025-03</td><td>activo</td><td></td><td>NEA</td><td>22</td></tr><tr><td>Centros TX</td><td>2025-03</td><td>activo</td><td></td><td>NOA</td><td>9</td></tr><tr><td>Centros TX</td><td>2025-03</td><td>activo</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-03</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Infraestructura</td><td>2025-02</td><td>Liberado</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2025-02</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-02</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-02</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2025-01</td><td>Liberado</td><td></td><td></td><td>5</td></tr><tr><td>Infraestructura</td><td>2025-01</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-01</td><td>activo</td><td></td><td></td><td>2</td></tr><tr><td>Centros TX</td><td>2025-01</td><td>activo</td><td></td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2025-01</td><td>activo</td><td></td><td>NEA</td><td>10</td></tr><tr><td>Centros TX</td><td>2025-01</td><td>activo</td><td></td><td>NOA</td><td>8</td></tr><tr><td>Centros TX</td><td>2025-01</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-01</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2025-01</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-12</td><td>Liberado</td><td></td><td></td><td>6</td></tr><tr><td>Centros TX</td><td>2024-12</td><td>activo</td><td></td><td>NEA</td><td>7</td></tr><tr><td>Centros TX</td><td>2024-12</td><td>activo</td><td></td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2024-12</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-12</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-12</td><td>activo</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-11</td><td>Liberado</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-11</td><td>Liberado</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-11</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-11</td><td>Liberado</td><td></td><td></td><td>6</td></tr><tr><td>Centros TX</td><td>2024-11</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-11</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-11</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-10</td><td>Liberado</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-10</td><td>Liberado</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-10</td><td>activo</td><td></td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-10</td><td>activo</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-10</td><td>activo</td><td></td><td>NEA</td><td>13</td></tr><tr><td>Centros TX</td><td>2024-10</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-09</td><td>Liberado</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-09</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-09</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-09</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-09</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-09</td><td>activo</td><td></td><td></td><td>2</td></tr><tr><td>Centros TX</td><td>2024-09</td><td>activo</td><td></td><td>NOA</td><td>10</td></tr><tr><td>Centros TX</td><td>2024-09</td><td>activo</td><td></td><td>NEA</td><td>20</td></tr><tr><td>Centros TX</td><td>2024-09</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-08</td><td>Liberado</td><td></td><td></td><td>2</td></tr><tr><td>Centros TX</td><td>2024-08</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-08</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-08</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-08</td><td>activo</td><td>CATAMARCA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-08</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2024-08</td><td>activo</td><td></td><td>NOA</td><td>10</td></tr><tr><td>Centros TX</td><td>2024-08</td><td>activo</td><td></td><td>NEA</td><td>10</td></tr><tr><td>Infraestructura</td><td>2024-07</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-07</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-07</td><td>Liberado</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2024-07</td><td>activo</td><td></td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-07</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-07</td><td>activo</td><td></td><td>NOA</td><td>20</td></tr><tr><td>Centros TX</td><td>2024-07</td><td>activo</td><td></td><td></td><td>3</td></tr><tr><td>Centros TX</td><td>2024-07</td><td>activo</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-07</td><td>activo</td><td></td><td>NEA</td><td>22</td></tr><tr><td>Centros TX</td><td>2024-07</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-07</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-07</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-07</td><td>activo</td><td>ROSARIO_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-06</td><td>Liberado</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-06</td><td>Liberado</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-06</td><td>Liberado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-06</td><td>activo</td><td></td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2024-06</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-06</td><td>activo</td><td></td><td></td><td>3</td></tr><tr><td>Centros TX</td><td>2024-06</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-06</td><td>activo</td><td></td><td>NOA</td><td>3</td></tr><tr><td>Centros TX</td><td>2024-06</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-05</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-05</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-05</td><td>Liberado</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-05</td><td>Liberado</td><td></td><td></td><td>4</td></tr><tr><td>Centros TX</td><td>2024-05</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-05</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-05</td><td>activo</td><td></td><td></td><td>2</td></tr><tr><td>Centros TX</td><td>2024-05</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2024-05</td><td>activo</td><td></td><td>NOA</td><td>14</td></tr><tr><td>Centros TX</td><td>2024-05</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-05</td><td>activo</td><td></td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-04</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-04</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-04</td><td>Liberado</td><td></td><td></td><td>3</td></tr><tr><td>Centros TX</td><td>2024-04</td><td>activo</td><td></td><td>NOA</td><td>4</td></tr><tr><td>Centros TX</td><td>2024-04</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-04</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-04</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2024-04</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-04</td><td>activo</td><td></td><td>NEA</td><td>8</td></tr><tr><td>Infraestructura</td><td>2024-03</td><td>Liberado</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-03</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-03</td><td>activo</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-03</td><td>activo</td><td>LA RIOJA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-03</td><td>activo</td><td></td><td>NEA</td><td>10</td></tr><tr><td>Centros TX</td><td>2024-03</td><td>activo</td><td></td><td>NOA</td><td>25</td></tr><tr><td>Centros TX</td><td>2024-03</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-03</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-02</td><td>Liberado</td><td>CATAMARCA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-02</td><td>activo</td><td></td><td></td><td>2</td></tr><tr><td>Centros TX</td><td>2024-02</td><td>activo</td><td>CATAMARCA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-02</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-02</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-01</td><td>Liberado</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-01</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2024-01</td><td>activo</td><td></td><td></td><td>3</td></tr><tr><td>Infraestructura</td><td>2023-12</td><td>Liberado</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-12</td><td>Liberado</td><td></td><td></td><td>2</td></tr><tr><td>Centros TX</td><td>2023-12</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-12</td><td>activo</td><td></td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-12</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-12</td><td>activo</td><td></td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-12</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-11</td><td>Liberado</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td></td><td>NEA</td><td>10</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td></td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td></td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td></td><td></td><td>13</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td></td><td>SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-11</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>5</td></tr><tr><td>Infraestructura</td><td>2023-10</td><td>Liberado</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td>CATAMARCA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td></td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td></td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td></td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td>JUNIN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td></td><td>NEA</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-10</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-09</td><td>Liberado</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-09</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-09</td><td>Liberado</td><td>JUNIN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-09</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-09</td><td>Liberado</td><td>CATAMARCA</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-09</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-09</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-09</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>CNEL SUAREZ</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td></td><td>NEA</td><td>21</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>NECOCHEA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td></td><td></td><td>6</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>5</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>SANTA ROSA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>5</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>9 DE JULIO</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td></td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>GRAL PICO</td><td>SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-09</td><td>activo</td><td>PEHUAJO</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-08</td><td>Liberado</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-08</td><td>Liberado</td><td></td><td></td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-08</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-08</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-08</td><td>Liberado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-08</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-08</td><td>Liberado</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-08</td><td>Liberado</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td></td><td>NEA</td><td>11</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td></td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td></td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>SANTA FE_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td></td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td></td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-08</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-07</td><td>Liberado</td><td>MENDOZA</td><td>CUYO</td><td>3</td></tr><tr><td>Infraestructura</td><td>2023-07</td><td>Liberado</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-07</td><td>Liberado</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-07</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-07</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>7</td></tr><tr><td>Infraestructura</td><td>2023-07</td><td>Liberado</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-07</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-07</td><td>Liberado</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td></td><td>NOA</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td></td><td></td><td>7</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>9 DE JULIO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>BONIFACIO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>SANTA ROSA</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td></td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td></td><td>NEA</td><td>17</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td></td><td>CUYO</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>FORMOSA_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-07</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-06</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-06</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-06</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Infraestructura</td><td>2023-06</td><td>Liberado</td><td></td><td></td><td>3</td></tr><tr><td>Infraestructura</td><td>2023-06</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-06</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>5</td></tr><tr><td>Centros TX</td><td>2023-06</td><td>activo</td><td></td><td>NEA</td><td>19</td></tr><tr><td>Centros TX</td><td>2023-06</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-06</td><td>activo</td><td>JUNIN DE LOS ANDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-06</td><td>activo</td><td></td><td></td><td>3</td></tr><tr><td>Centros TX</td><td>2023-06</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-06</td><td>activo</td><td></td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-06</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-06</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-05</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-05</td><td>Liberado</td><td>JUNIN DE LOS ANDES</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-05</td><td>Liberado</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-05</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-05</td><td>Liberado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>8</td></tr><tr><td>Infraestructura</td><td>2023-05</td><td>Liberado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td>SANTA CRUZ</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>5</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td></td><td></td><td>5</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td></td><td>NEA</td><td>6</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td>TANDIL</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-05</td><td>activo</td><td></td><td>NOA</td><td>8</td></tr><tr><td>Infraestructura</td><td>2023-04</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-04</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-04</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-04</td><td>Liberado</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2023-04</td><td>activo</td><td>TANDIL</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-04</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-04</td><td>activo</td><td></td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-04</td><td>activo</td><td></td><td></td><td>3</td></tr><tr><td>Centros TX</td><td>2023-04</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-04</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-04</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-04</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-04</td><td>activo</td><td>FORMOSA_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-04</td><td>activo</td><td></td><td>NEA</td><td>10</td></tr><tr><td>Infraestructura</td><td>2023-03</td><td>Liberado</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-03</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>MISIONES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td></td><td>NOA</td><td>16</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>NO DEFINIDO</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>NO DEFINIDO</td><td>NEA</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td></td><td>NEA</td><td>10</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>JUJUY</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-03</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-02</td><td>Liberado</td><td>BONIFACIO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-02</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-02</td><td>Liberado</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-02</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-02</td><td>Liberado</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-02</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-02</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Infraestructura</td><td>2023-02</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>MISIONES_ZONA 02</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>MISIONES_ZONA 01</td><td>NEA</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>PATAGONIA</td><td>SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>FORMOSA_ZONA 01</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>NO DEFINIDO</td><td>NEA</td><td>49</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>CORRIENTES_ZONA 01</td><td>NEA</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>RIO GALLEGOS</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>ANDINA</td><td>SUR</td><td>6</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>NOA_TUCUMAN</td><td>NOA</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td></td><td>NEA</td><td>60</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td></td><td>NOA</td><td>5</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>CATAMARCA</td><td>NOA</td><td>6</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>NO DEFINIDO</td><td>SUR</td><td>6</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td></td><td></td><td>2</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>10</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>MISIONES_ZONA 04</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>OLAVARRIA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>NO DEFINIDO</td><td>NOA</td><td>8</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>CENTRO</td><td>NEA</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>CORRIENTES_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>ESTE</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>MISIONES</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>CORRIENTES</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>SANTA CRUZ</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td></td><td>SUR</td><td>7</td></tr><tr><td>Centros TX</td><td>2023-02</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>11</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td></td><td>NEA</td><td>11</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td></td><td>SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td>NO DEFINIDO</td><td>NEA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td>SANTA CRUZ</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-01</td><td>Liberado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>JUJUY</td><td>NOA</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>6</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>NO DEFINIDO</td><td>NOA</td><td>13</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td></td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>10</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td></td><td>CUYO</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>NO DEFINIDO</td><td>NEA</td><td>9</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>NOA_SALTA</td><td>NOA</td><td>6</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>SALTA</td><td>NOA</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>ANASCO</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>CUYO_MDZ</td><td>CUYO</td><td>5</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td></td><td>NEA</td><td>22</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>10</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>6</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td></td><td>NOA</td><td>16</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>6</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>AMBA_OESTE</td><td>AMBA_OESTE</td><td>5</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>NOA_CORDOBA_SUR</td><td>NOA</td><td>19</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td></td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td></td><td>SUR</td><td>5</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>NO DEFINIDO</td><td>CUYO</td><td>5</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>9</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>LA RIOJA</td><td>NOA</td><td>4</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>6</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>NOA_CORDOBA_NORTE</td><td>NOA</td><td>11</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td></td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2023-01</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-12</td><td>Liberado</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-12</td><td>Liberado</td><td>NO DEFINIDO</td><td>NOA</td><td>4</td></tr><tr><td>Infraestructura</td><td>2022-12</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2022-12</td><td>Liberado</td><td>NO DEFINIDO</td><td>NEA</td><td>5</td></tr><tr><td>Infraestructura</td><td>2022-12</td><td>Liberado</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-12</td><td>Liberado</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-12</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2022-12</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-12</td><td>activo</td><td></td><td>NOA</td><td>4</td></tr><tr><td>Centros TX</td><td>2022-12</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-12</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-12</td><td>activo</td><td>AMBA_SUR</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-12</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-12</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-11</td><td>Liberado</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-11</td><td>Liberado</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-11</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-11</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-11</td><td>Liberado</td><td></td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-11</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-11</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-11</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-11</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-11</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-10</td><td>activo</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-10</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-10</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-09</td><td>Liberado</td><td>FORMOSA_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-09</td><td>Liberado</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-09</td><td>Liberado</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-09</td><td>Liberado</td><td></td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-09</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-09</td><td>activo</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-09</td><td>activo</td><td>CUYO</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-09</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-09</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-09</td><td>activo</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-08</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-08</td><td>Liberado</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-08</td><td>Liberado</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-08</td><td>Liberado</td><td>NO DEFINIDO</td><td>NOA</td><td>4</td></tr><tr><td>Infraestructura</td><td>2022-08</td><td>Liberado</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-08</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-08</td><td>Liberado</td><td></td><td>NOA</td><td>6</td></tr><tr><td>Centros TX</td><td>2022-08</td><td>activo</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-08</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2022-08</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-08</td><td>activo</td><td></td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2022-08</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2022-08</td><td>activo</td><td>CORRIENTES_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-08</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-07</td><td>Liberado</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-07</td><td>Liberado</td><td></td><td>NEA</td><td>5</td></tr><tr><td>Infraestructura</td><td>2022-07</td><td>Liberado</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-07</td><td>Liberado</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-07</td><td>Liberado</td><td>NO DEFINIDO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-07</td><td>Liberado</td><td>NO DEFINIDO</td><td>NEA</td><td>7</td></tr><tr><td>Infraestructura</td><td>2022-07</td><td>Liberado</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-07</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2022-07</td><td>activo</td><td>NO DEFINIDO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-07</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-07</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td>AMBA_OESTE</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td>USHUAIA - TIERRA DEL FUEGO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td>NO DEFINIDO</td><td>NOA</td><td>26</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td>NO DEFINIDO</td><td>NEA</td><td>30</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td></td><td>NEA</td><td>13</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td></td><td></td><td>4</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td></td><td>NOA</td><td>8</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td>NO DEFINIDO</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td></td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>Liberado</td><td>NO DEFINIDO</td><td>SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2022-06</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-06</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-06</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2022-06</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2022-06</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-06</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-06</td><td>activo</td><td>SANTA FE_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-06</td><td>activo</td><td></td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2022-06</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-05</td><td>Liberado</td><td>FORMOSA_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-05</td><td>Liberado</td><td>NO DEFINIDO</td><td>NEA</td><td>4</td></tr><tr><td>Infraestructura</td><td>2022-05</td><td>Liberado</td><td>SANTA ROSA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-05</td><td>Liberado</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-05</td><td>Liberado</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-05</td><td>Liberado</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-05</td><td>Liberado</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-05</td><td>Liberado</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-05</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-05</td><td>Liberado</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>5</td></tr><tr><td>Centros TX</td><td>2022-05</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2022-05</td><td>activo</td><td>CATAMARCA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-05</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-05</td><td>activo</td><td>NO DEFINIDO</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2022-05</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-05</td><td>activo</td><td>SALTA</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2022-05</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-05</td><td>activo</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-04</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-04</td><td>Liberado</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-04</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-04</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-04</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-04</td><td>activo</td><td>9 DE JULIO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-04</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-04</td><td>activo</td><td>ROSARIO_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-04</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-04</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-04</td><td>activo</td><td></td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2022-04</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2022-03</td><td>Liberado</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Infraestructura</td><td>2022-03</td><td>Liberado</td><td>TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-03</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-03</td><td>Liberado</td><td></td><td>SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2022-03</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-03</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2022-03</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-03</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-03</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-03</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-03</td><td>activo</td><td></td><td></td><td>3</td></tr><tr><td>Centros TX</td><td>2022-03</td><td>activo</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-02</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-02</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-02</td><td>Liberado</td><td></td><td>SUR</td><td>3</td></tr><tr><td>Infraestructura</td><td>2022-02</td><td>Liberado</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-02</td><td>Liberado</td><td></td><td>CUYO</td><td>3</td></tr><tr><td>Infraestructura</td><td>2022-02</td><td>Liberado</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-02</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-02</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-01</td><td>Liberado</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-01</td><td>Liberado</td><td></td><td>CUYO</td><td>8</td></tr><tr><td>Infraestructura</td><td>2022-01</td><td>Liberado</td><td></td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-01</td><td>Liberado</td><td>TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-01</td><td>Liberado</td><td>TRENQUE LAUQUEN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-01</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-01</td><td>Liberado</td><td></td><td>SUR</td><td>4</td></tr><tr><td>Infraestructura</td><td>2022-01</td><td>Liberado</td><td>TRELEW</td><td>SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2022-01</td><td>Liberado</td><td></td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2022-01</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-01</td><td>activo</td><td>JUJUY</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-01</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-01</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-01</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2022-01</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-12</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-12</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-12</td><td>Liberado</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-12</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-12</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2021-12</td><td>activo</td><td>LA RIOJA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-12</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-12</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-11</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-11</td><td>Liberado</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-11</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2021-11</td><td>activo</td><td>CATAMARCA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-11</td><td>activo</td><td></td><td>NEA</td><td>5</td></tr><tr><td>Centros TX</td><td>2021-11</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>3</td></tr><tr><td>Centros TX</td><td>2021-11</td><td>activo</td><td>PEHUAJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-11</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-11</td><td>activo</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-11</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-11</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-11</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-11</td><td>activo</td><td>9 DE JULIO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-11</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>3</td></tr><tr><td>Infraestructura</td><td>2021-10</td><td>Liberado</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-10</td><td>Liberado</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>6</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td></td><td>NEA</td><td>6</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>5</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>LA RIOJA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>7</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>SUR</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>3</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>NOA_CORDOBA_SUR</td><td>NOA</td><td>3</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>13</td></tr><tr><td>Centros TX</td><td>2021-10</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-09</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>7</td></tr><tr><td>Infraestructura</td><td>2021-09</td><td>Liberado</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>9 DE JULIO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>CUYO_MDZ</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-09</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-08</td><td>Liberado</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-08</td><td>Liberado</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-08</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Infraestructura</td><td>2021-08</td><td>Liberado</td><td>RIO GALLEGOS</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-08</td><td>Liberado</td><td>SANTA FE_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-08</td><td>Liberado</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-08</td><td>Liberado</td><td>NO DEFINIDO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-08</td><td>activo</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-08</td><td>activo</td><td>TRES ARROYOS</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-08</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-08</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-08</td><td>activo</td><td></td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2021-08</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-08</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-07</td><td>Liberado</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-07</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>8</td></tr><tr><td>Infraestructura</td><td>2021-07</td><td>Liberado</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-07</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-07</td><td>Liberado</td><td></td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-07</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-07</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>3</td></tr><tr><td>Centros TX</td><td>2021-07</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-07</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-06</td><td>Liberado</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-06</td><td>activo</td><td>AMBA_OESTE</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-06</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2021-06</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2021-06</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2021-06</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-06</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-06</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2021-05</td><td>Liberado</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-05</td><td>Liberado</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-05</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-05</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-05</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-05</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-04</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-04</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-03</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-03</td><td>Liberado</td><td>SANTA CRUZ</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-03</td><td>Liberado</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-03</td><td>Liberado</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-03</td><td>activo</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-03</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-03</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-03</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-02</td><td>Liberado</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-02</td><td>Liberado</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-02</td><td>Liberado</td><td>TRES ARROYOS</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-02</td><td>Liberado</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-02</td><td>Liberado</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-02</td><td>activo</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-02</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-01</td><td>Liberado</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-01</td><td>Liberado</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-01</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2021-01</td><td>activo</td><td>OLAVARRIA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-12</td><td>Liberado</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-12</td><td>Liberado</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Infraestructura</td><td>2020-12</td><td>Liberado</td><td>RIO GALLEGOS</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-12</td><td>Liberado</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-12</td><td>Liberado</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-12</td><td>Liberado</td><td></td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Infraestructura</td><td>2020-12</td><td>Liberado</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>ROSARIO_ZONA 05</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>JUNIN</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>NECOCHEA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>5</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>ROSARIO SUR</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>PEHUAJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>TRENQUE LAUQUEN</td><td>BUENOS AIRES</td><td>6</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>CNEL SUAREZ</td><td>SUR</td><td>6</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>9 DE JULIO</td><td>BUENOS AIRES</td><td>5</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>NO DEFINIDO</td><td>ATLANTICA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>7</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>ZAPALA</td><td>CUYO</td><td>17</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>19</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>TANDIL</td><td>BUENOS AIRES</td><td>9</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>RIO GALLEGOS</td><td>SUR</td><td>5</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>SANTA ROSA</td><td>SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>NO DEFINIDO</td><td>CUYO</td><td>5</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>7</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>ESQUEL</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>NO DEFINIDO</td><td>SUR</td><td>7</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>8</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>ENTRE RIOS_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>11</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>8</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>5</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>CHASCOMUS-DOLORES</td><td>BUENOS AIRES</td><td>7</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>CHACO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>FORMOSA_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>OLAVARRIA</td><td>BUENOS AIRES</td><td>7</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>TRES ARROYOS</td><td>SUR</td><td>6</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>6</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>4</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>13</td></tr><tr><td>Centros TX</td><td>2020-12</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-11</td><td>Liberado</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-11</td><td>Liberado</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-11</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-11</td><td>Liberado</td><td>9 DE JULIO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-11</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>JUNIN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>NECOCHEA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>SANTA ROSA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>NO DEFINIDO</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>ROSARIO NORTE</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>RIO GALLEGOS</td><td>SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2020-11</td><td>activo</td><td>ZAPALA</td><td>CUYO</td><td>2</td></tr><tr><td>Infraestructura</td><td>2020-10</td><td>Liberado</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-10</td><td>Liberado</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-10</td><td>Liberado</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>TANDIL</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td></td><td>ATLANTICA</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>CNEL SUAREZ</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>JUNIN</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-10</td><td>activo</td><td></td><td>SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2020-09</td><td>Liberado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-09</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-09</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-09</td><td>activo</td><td>SAN JUSTO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-09</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-09</td><td>activo</td><td>CORRIENTES</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-09</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-09</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-09</td><td>activo</td><td>JUJUY</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-08</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-08</td><td>Liberado</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-08</td><td>Liberado</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-08</td><td>Liberado</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-08</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-08</td><td>activo</td><td>CHACO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-08</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-08</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-08</td><td>activo</td><td>ESTE</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-08</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-08</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-08</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-08</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-08</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-07</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-07</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2020-07</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-07</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-07</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-07</td><td>activo</td><td>CNEL SUAREZ</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-06</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-06</td><td>activo</td><td>ENTRE RIOS</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-06</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-06</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-06</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2020-06</td><td>activo</td><td>LA CATOLICA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-05</td><td>Liberado</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-05</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-05</td><td>Liberado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-05</td><td>activo</td><td>NO DEFINIDO</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-05</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-05</td><td>activo</td><td>CHASCOMUS-DOLORES</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-05</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-05</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>17</td></tr><tr><td>Centros TX</td><td>2020-05</td><td>activo</td><td>LA RIOJA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-05</td><td>activo</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-05</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-05</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-05</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-05</td><td>activo</td><td>CUYO_MDZ</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-05</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-04</td><td>Liberado</td><td>TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-04</td><td>Liberado</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-04</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2020-04</td><td>Liberado</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-04</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-04</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-04</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-04</td><td>activo</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-04</td><td>activo</td><td>NO DEFINIDO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-04</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-04</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-04</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-04</td><td>activo</td><td></td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-04</td><td>activo</td><td>SAN JUSTO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-04</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>8</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>Liberado</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>Liberado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>CUYO_EXT</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>CORRIENTES</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>23</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-03</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-02</td><td>Liberado</td><td></td><td>AMBA_NORTE</td><td>6</td></tr><tr><td>Infraestructura</td><td>2020-02</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2020-02</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>4</td></tr><tr><td>Infraestructura</td><td>2020-02</td><td>Liberado</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-02</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-02</td><td>activo</td><td>RIO GALLEGOS</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-02</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-02</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2020-02</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>16</td></tr><tr><td>Centros TX</td><td>2020-02</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-02</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2020-02</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2020-02</td><td>activo</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-02</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-02</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-02</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>6</td></tr><tr><td>Infraestructura</td><td>2020-01</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-01</td><td>Liberado</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-01</td><td>Liberado</td><td>MENDOZA</td><td>CUYO</td><td>2</td></tr><tr><td>Infraestructura</td><td>2020-01</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-01</td><td>Liberado</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-01</td><td>Liberado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-01</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2020-01</td><td>Liberado</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-01</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-01</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-01</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-01</td><td>activo</td><td></td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-01</td><td>activo</td><td>ROSARIO NORTE</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-01</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2020-01</td><td>activo</td><td>MISIONES_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-01</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2020-01</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2020-01</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>7</td></tr><tr><td>Centros TX</td><td>2020-01</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>5</td></tr><tr><td>Centros TX</td><td>2020-01</td><td>activo</td><td>SAN JUSTO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-12</td><td>Liberado</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-12</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2019-12</td><td>Liberado</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-12</td><td>Liberado</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-12</td><td>Liberado</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-12</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-12</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-12</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>3</td></tr><tr><td>Centros TX</td><td>2019-12</td><td>activo</td><td>RIO GALLEGOS</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-12</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2019-12</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-12</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-12</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2019-11</td><td>Liberado</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-11</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-11</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>TANDIL</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>NOA_TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>CHACO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>SANTA FE_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>6</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>9</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>6</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>NO DEFINIDO</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>3</td></tr><tr><td>Centros TX</td><td>2019-11</td><td>activo</td><td>CHACO</td><td>NEA</td><td>3</td></tr><tr><td>Infraestructura</td><td>2019-10</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-10</td><td>Liberado</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-10</td><td>Liberado</td><td>MISIONES_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>MISIONES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>SAN JUSTO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>MISIONES_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>AMBA_OESTE</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>ROSARIO NORTE</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>SALTA</td><td>NOA</td><td>3</td></tr><tr><td>Centros TX</td><td>2019-10</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>11</td></tr><tr><td>Infraestructura</td><td>2019-09</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-09</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-09</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>4</td></tr><tr><td>Infraestructura</td><td>2019-09</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-09</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-09</td><td>Liberado</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-09</td><td>Liberado</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-09</td><td>Liberado</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>NO DEFINIDO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>ROSARIO_ZONA 05</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>4</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>8</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>ROSARIO_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-09</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-08</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-08</td><td>Liberado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-08</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-08</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-08</td><td>Liberado</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-08</td><td>Liberado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-08</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-08</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-08</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-08</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-08</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-08</td><td>activo</td><td>SAN JUSTO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-08</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Infraestructura</td><td>2019-07</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-07</td><td>Liberado</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-07</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-07</td><td>Liberado</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-07</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-07</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-07</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-07</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-07</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-07</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-07</td><td>activo</td><td>ROSARIO NORTE</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-07</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-07</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2019-07</td><td>activo</td><td>LA RIOJA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-07</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-07</td><td>activo</td><td>CHACO_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-06</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-06</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-06</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-06</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>3</td></tr><tr><td>Infraestructura</td><td>2019-06</td><td>Liberado</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2019-06</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-06</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>5</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-06</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>CHACO_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>RIO GALLEGOS</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>Liberado</td><td>JUNIN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>JUJUY</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>6</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>PEHUAJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>LA RIOJA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2019-05</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2019-04</td><td>Liberado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Infraestructura</td><td>2019-04</td><td>Liberado</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-04</td><td>Liberado</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>6</td></tr><tr><td>Infraestructura</td><td>2019-04</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-04</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-04</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-04</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-04</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-04</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-04</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-04</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-04</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-04</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-04</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-04</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>7</td></tr><tr><td>Centros TX</td><td>2019-04</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>5</td></tr><tr><td>Infraestructura</td><td>2019-03</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-03</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-03</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-03</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-03</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-03</td><td>Liberado</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-03</td><td>Liberado</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-03</td><td>Liberado</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-03</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>3</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>AMBA_OESTE</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>CHACO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>ENTRE RIOS_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>ROSARIO CENTRO</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-03</td><td>activo</td><td>CUYO_MDZ</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-02</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2019-02</td><td>Liberado</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-02</td><td>Liberado</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-02</td><td>Liberado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>6</td></tr><tr><td>Infraestructura</td><td>2019-02</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>5</td></tr><tr><td>Infraestructura</td><td>2019-02</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>5</td></tr><tr><td>Centros TX</td><td>2019-02</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-02</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-02</td><td>activo</td><td>SANTA FE_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-02</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-02</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2019-02</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-02</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>Liberado</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>Liberado</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>Liberado</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>Liberado</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>Liberado</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>Liberado</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-01</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-01</td><td>activo</td><td>NO DEFINIDO</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-01</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-01</td><td>activo</td><td>CENTRO</td><td>ATLANTICA</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-01</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-01</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-01</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-01</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>3</td></tr><tr><td>Centros TX</td><td>2019-01</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2019-01</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2019-01</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-12</td><td>Liberado</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-12</td><td>Liberado</td><td>RIO CUARTO</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-12</td><td>Liberado</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-12</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Infraestructura</td><td>2018-12</td><td>Liberado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-12</td><td>Liberado</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-12</td><td>Liberado</td><td>TANDIL</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-12</td><td>Liberado</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>NO DEFINIDO</td><td>ATLANTICA</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-12</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>Liberado</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>Liberado</td><td>ROSARIO_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>Liberado</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>Liberado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>Liberado</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>AMBA_OESTE</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>ROSARIO CENTRO</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>TRES ARROYOS</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>SAN JUSTO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>NO DEFINIDO</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>NO DEFINIDO</td><td>ATLANTICA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>ATLANTICA</td><td>ATLANTICA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>ROSARIO_ZONA 03</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>5</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>TANDIL</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-11</td><td>activo</td><td>CORRIENTES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-10</td><td>Liberado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-10</td><td>Liberado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-10</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-10</td><td>Liberado</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>8</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>5</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>AMBA_OESTE</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>NO DEFINIDO</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>ENTRE RIOS</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>7</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>6</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-10</td><td>activo</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-09</td><td>Liberado</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-09</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-09</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>ENTRE RIOS_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>5</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>5</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>8</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>12</td></tr><tr><td>Centros TX</td><td>2018-09</td><td>activo</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-08</td><td>Liberado</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-08</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-08</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-08</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Infraestructura</td><td>2018-08</td><td>Liberado</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-08</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-08</td><td>Liberado</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-08</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-08</td><td>Liberado</td><td>MENDOZA</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>CATAMARCA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>ROSARIO_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>5</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-08</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>ROSARIO_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-07</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-07</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-07</td><td>activo</td><td>CORRIENTES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-07</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-07</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-07</td><td>activo</td><td>CHACO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-07</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-07</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-07</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-07</td><td>activo</td><td>ROSARIO_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>ENTRE RIOS_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>BAHIA BLANCA</td><td>SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-06</td><td>activo</td><td>SAN FRANCISCO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-06</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-06</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-06</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-06</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-06</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-06</td><td>activo</td><td>OLAVARRIA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-06</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-06</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-06</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-06</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>5</td></tr><tr><td>Centros TX</td><td>2018-06</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>3</td></tr><tr><td>Infraestructura</td><td>2018-05</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-05</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-05</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-05</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-05</td><td>Liberado</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-05</td><td>activo</td><td>TANDIL</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-05</td><td>activo</td><td>USHUAIA - TIERRA DEL FUEGO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-05</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-05</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-05</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-05</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-05</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-05</td><td>activo</td><td>SAN FRANCISCO</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>MISIONES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td></td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>6</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>TANDIL</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>23</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>CORRIENTES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>USHUAIA - TIERRA DEL FUEGO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>SAN JUSTO</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>BONIFACIO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>ROSARIO_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-04</td><td>activo</td><td>SANTA FE_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-03</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-03</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-03</td><td>activo</td><td>RIO GALLEGOS</td><td>SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-03</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-03</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-03</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-03</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-03</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-03</td><td>activo</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-03</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-03</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-03</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-02</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-02</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-02</td><td>Liberado</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-02</td><td>Liberado</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-02</td><td>Liberado</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-02</td><td>Liberado</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-02</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-02</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-02</td><td>activo</td><td>USHUAIA - TIERRA DEL FUEGO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-02</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-02</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-02</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-02</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-02</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-02</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-02</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-02</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-01</td><td>Liberado</td><td>CHACO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-01</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-01</td><td>Liberado</td><td>BARILOCHE</td><td>CUYO</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-01</td><td>Liberado</td><td>RIO CUARTO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>PATAGONIA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>SANTA FE_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>ROSARIO CENTRO</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>ESQUEL</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>MISIONES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>9</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>NOA_CORDOBA_SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>CORRIENTES_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>GRAL ROCA</td><td>CUYO</td><td>4</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>5</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>LA CATOLICA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2018-01</td><td>activo</td><td>NO DEFINIDO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-12</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-12</td><td>Liberado</td><td>TANDIL</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-12</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-12</td><td>Liberado</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-12</td><td>Liberado</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Infraestructura</td><td>2017-12</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-12</td><td>Liberado</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-12</td><td>Liberado</td><td>BARRACAS</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>CHACO</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>ENTRE RIOS_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>VENADO TUERTO</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>5</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td></td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>ROSARIO_ZONA 02</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>8</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>NECOCHEA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>7</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>ROSARIO_ZONA 06</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>SANTA FE_ZONA 01</td><td>NEA</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>CATAMARCA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>ROSARIO_ZONA 03</td><td>NEA</td><td>5</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>CHASCOMUS-DOLORES</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>JUJUY</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>GUALEGUAYCHU</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-12</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>153</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>JUNIN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>7</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>5</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>7</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>7</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>CORRIENTES_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>LA CATOLICA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>ROSARIO_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>MISIONES_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>SAN JUSTO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>5</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>JUJUY</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-11</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>BAHIA BLANCA</td><td>SUR</td><td>103</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CORRIENTES_ZONA 01</td><td>NEA</td><td>29</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>333</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>49</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>ROSARIO_ZONA 06</td><td>NEA</td><td>4</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>PEHUAJO</td><td>BUENOS AIRES</td><td>16</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>155</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>MISIONES_ZONA 02</td><td>NEA</td><td>6</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>TRELEW</td><td>SUR</td><td>52</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CORRIENTES_ZONA 02</td><td>NEA</td><td>7</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td></td><td></td><td>10</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CHASCOMUS-DOLORES</td><td>BUENOS AIRES</td><td>22</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>TRES ARROYOS</td><td>SUR</td><td>19</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td></td><td>SUR</td><td>19</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>JUNIN</td><td>BUENOS AIRES</td><td>49</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>FORMOSA_ZONA 01</td><td>NEA</td><td>11</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>ENTRE RIOS_ZONA 01</td><td>NEA</td><td>12</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>36</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>TUCUMAN</td><td>NOA</td><td>108</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CHACO_ZONA 02</td><td>NEA</td><td>6</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td></td><td>BUENOS AIRES</td><td>23</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td></td><td>NOA</td><td>36</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td></td><td>NEA</td><td>32</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SAN JUSTO</td><td>AMBA_OESTE</td><td>11</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>ROSARIO_ZONA 02</td><td>NEA</td><td>53</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SANTA ROSA</td><td>SUR</td><td>53</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td></td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>57</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>228</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>MENDOZA</td><td>CUYO</td><td>192</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>MERLO</td><td>AMBA_OESTE</td><td>222</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SAN FRANCISCO</td><td>NOA</td><td>10</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td></td><td>CUYO</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>ROSARIO_ZONA 05</td><td>NEA</td><td>13</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CORRIENTES_ZONA 04</td><td>NEA</td><td>8</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>49</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>232</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>COMODORO</td><td>SUR</td><td>69</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CORDOBA SUR</td><td>NOA</td><td>119</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SAN JUAN</td><td>CUYO</td><td>116</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CATAMARCA</td><td>NOA</td><td>31</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>MISIONES_ZONA 04</td><td>NEA</td><td>4</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SALTA</td><td>NOA</td><td>73</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CNEL SUAREZ</td><td>SUR</td><td>22</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CHACO_ZONA 01</td><td>NEA</td><td>28</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>FLORES</td><td>AMBA_OESTE</td><td>249</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>123</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>VIEDMA</td><td>SUR</td><td>47</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>ROSARIO_ZONA 03</td><td>NEA</td><td>24</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>NO DEFINIDO</td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>39</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>NECOCHEA</td><td>BUENOS AIRES</td><td>21</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CUYO</td><td>AMBA_SUR</td><td>233</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>RIO CUARTO</td><td>NOA</td><td>28</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>ENTRE RIOS_ZONA 02</td><td>NEA</td><td>10</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>GRAL ROCA</td><td>CUYO</td><td>30</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>USHUAIA - TIERRA DEL FUEGO</td><td>SUR</td><td>25</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CORRIENTES_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>RIO GALLEGOS</td><td>SUR</td><td>42</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SAN RAFAEL</td><td>CUYO</td><td>53</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>TANDIL</td><td>BUENOS AIRES</td><td>27</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>NEUQUEN</td><td>CUYO</td><td>102</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>BARILOCHE</td><td>CUYO</td><td>72</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>192</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>RIO GRANDE - TIERRA DEL FUEGO</td><td>SUR</td><td>15</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>ENTRE RIOS_ZONA 03</td><td>NEA</td><td>16</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SANTA FE_ZONA 02</td><td>NEA</td><td>27</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>261</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>ESQUEL</td><td>SUR</td><td>33</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>CORDOBA NORTE</td><td>NOA</td><td>130</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>VILLA MERCEDES</td><td>CUYO</td><td>33</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>MISIONES_ZONA 01</td><td>NEA</td><td>30</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SANTA FE_ZONA 01</td><td>NEA</td><td>23</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SANTA FE_ZONA 04</td><td>NEA</td><td>8</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>JUJUY</td><td>NOA</td><td>40</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>OLAVARRIA</td><td>BUENOS AIRES</td><td>25</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>49</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>254</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>BARRACAS</td><td>AMBA_SUR</td><td>167</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>106</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>63</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>LA RIOJA</td><td>NOA</td><td>21</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>9 DE JULIO</td><td>BUENOS AIRES</td><td>19</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>GRAL PICO</td><td>SUR</td><td>32</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SAN LUIS</td><td>CUYO</td><td>53</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>ZAPALA</td><td>CUYO</td><td>27</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>TRENQUE LAUQUEN</td><td>BUENOS AIRES</td><td>21</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>FORMOSA_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>JUNIN DE LOS ANDES</td><td>CUYO</td><td>3</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>Liberado</td><td>SANTA FE_ZONA 05</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>5</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>CORRIENTES</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>LA RIOJA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>JUNIN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>27</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>SANTA FE_ZONA 01</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>OLAVARRIA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>ENTRE RIOS</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>TRENQUE LAUQUEN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-10</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>8</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>CHASCOMUS-DOLORES</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>JUNIN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>NECOCHEA</td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>USHUAIA - TIERRA DEL FUEGO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>TANDIL</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>25</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>9 DE JULIO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>RIO GRANDE - TIERRA DEL FUEGO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>6</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>52</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>PEHUAJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>SANTA ROSA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-09</td><td>activo</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>SAN FRANCISCO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>SALTA</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>7</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>5</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>5</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>12</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>CORRIENTES_ZONA 01</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-08</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>5</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>TRES ARROYOS</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>6</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>18</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>8</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>NOA_CORDOBA_NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>NO DEFINIDO</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>11</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>22</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>ROSARIO_ZONA 02</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>CNEL SUAREZ</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-07</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>9</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>NO DEFINIDO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>SANTA FE_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>CHASCOMUS-DOLORES</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>JUNIN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>NOA_CORDOBA_SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>LA CATOLICA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-06</td><td>activo</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>MISIONES_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>GRAL PICO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>NOA_CORDOBA_SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-05</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>CUYO_EXT</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-04</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>JUJUY</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>GRAL PICO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-03</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>4</td></tr><tr><td>Centros TX</td><td>2017-02</td><td>activo</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-02</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-02</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-02</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-02</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-02</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-02</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2017-02</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-02</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-02</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>CHACO_ZONA 01</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>9</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>14</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>CORRIENTES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>5</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2017-01</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>OLAVARRIA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-12</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>4</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>NOA_TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>5</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>6</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>RIO GALLEGOS</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>SALTA</td><td>NOA</td><td>3</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>USHUAIA - TIERRA DEL FUEGO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>CORRIENTES</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>8</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>LA RIOJA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>11</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>AMBA_OESTE</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>6</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>SANTA FE_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-11</td><td>activo</td><td>JUNIN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-10</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-10</td><td>activo</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-10</td><td>activo</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-10</td><td>activo</td><td>RIO GRANDE - TIERRA DEL FUEGO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-10</td><td>activo</td><td>SANTA ROSA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-10</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-10</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-10</td><td>activo</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-10</td><td>activo</td><td>JUJUY</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-10</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-10</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-09</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-09</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-09</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-09</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-09</td><td>activo</td><td>ROSARIO_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-09</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-09</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>ROSARIO_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>24</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>CHACO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>TRENQUE LAUQUEN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>LA RIOJA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>JUNIN</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>NO DEFINIDO</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>SANTA FE_ZONA 01</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>TANDIL</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-08</td><td>activo</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>USHUAIA - TIERRA DEL FUEGO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>RIO GRANDE - TIERRA DEL FUEGO</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>SANTA FE_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-07</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>JUJUY</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>CORRIENTES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>SALTA</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>CATAMARCA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>CHACO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>ENTRE RIOS_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>RIO GRANDE - TIERRA DEL FUEGO</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>RIO GALLEGOS</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>MISIONES</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-06</td><td>activo</td><td>OLAVARRIA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>ROSARIO_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>SANTA ROSA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-05</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>PATAGONIA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>9</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>8</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>CORRIENTES</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>ENTRE RIOS_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>JUJUY</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td></td><td></td><td>2</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>RIO GALLEGOS</td><td>SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-04</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>MISIONES_ZONA 01</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>SANTA ROSA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>TANDIL</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-03</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>PATAGONIA</td><td>SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-02</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>5</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>MISIONES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>6</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>CORRIENTES</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td></td><td></td><td>1</td></tr><tr><td>Centros TX</td><td>2016-01</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ANASCO</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>USHUAIA - TIERRA DEL FUEGO</td><td>SUR</td><td>28</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td></td><td></td><td>33</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>9 DE JULIO</td><td>BUENOS AIRES</td><td>31</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CHACO_ZONA 02</td><td>NEA</td><td>7</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>MISIONES_ZONA 02</td><td>NEA</td><td>9</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ENTRE RIOS_ZONA 01</td><td>NEA</td><td>18</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CORRIENTES</td><td>NEA</td><td>15</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SAN RAFAEL</td><td>CUYO</td><td>58</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ROSARIO_ZONA 03</td><td>NEA</td><td>25</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>BARRACAS</td><td>AMBA_SUR</td><td>184</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CUYO_EXT</td><td>CUYO</td><td>9</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>NECOCHEA</td><td>BUENOS AIRES</td><td>19</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>PASO DE LOS LIBRES</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>BARILOCHE</td><td>CUYO</td><td>64</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CORRIENTES_ZONA 03</td><td>NEA</td><td>3</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>MISIONES_ZONA 03</td><td>NEA</td><td>5</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ROSARIO SUR</td><td>NEA</td><td>13</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SANTA FE_ZONA 05</td><td>NEA</td><td>12</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SALTA</td><td>NOA</td><td>100</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>JUNIN</td><td>BUENOS AIRES</td><td>54</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SANTA FE_ZONA 03</td><td>NEA</td><td>54</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ROSARIO CENTRO</td><td>NEA</td><td>40</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>231</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>GRAL ROCA</td><td>CUYO</td><td>31</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ENTRE RIOS_ZONA 02</td><td>NEA</td><td>18</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>JUNIN DE LOS ANDES</td><td>CUYO</td><td>4</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>MISIONES</td><td>NEA</td><td>9</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CORRIENTES_ZONA 01</td><td>NEA</td><td>33</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>COMODORO</td><td>SUR</td><td>78</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>PEHUAJO</td><td>BUENOS AIRES</td><td>20</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ROSARIO_ZONA 01</td><td>NEA</td><td>49</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CUYO_MDZ</td><td>CUYO</td><td>2</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ESQUEL</td><td>SUR</td><td>33</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>PATAGONIA</td><td>SUR</td><td>25</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>RIO CUARTO</td><td>NOA</td><td>46</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SANTA FE_ZONA 01</td><td>NEA</td><td>23</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SAN JUSTO</td><td>AMBA_OESTE</td><td>8</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>TANDIL</td><td>BUENOS AIRES</td><td>24</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>GRAL PICO</td><td>SUR</td><td>41</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>GLEW</td><td>ATLANTICA</td><td>1</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>NOA_CORDOBA_SUR</td><td>NOA</td><td>7</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>221</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>NO DEFINIDO</td><td>CUYO</td><td>10</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>RIO GALLEGOS</td><td>SUR</td><td>53</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>136</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>FORMOSA_ZONA 02</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ENTRE RIOS</td><td>NEA</td><td>14</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ROSARIO NORTE</td><td>NEA</td><td>35</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SANTA ROSA</td><td>SUR</td><td>62</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SANTA FE_ZONA 02</td><td>NEA</td><td>32</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>TRENQUE LAUQUEN</td><td>BUENOS AIRES</td><td>22</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>OLAVARRIA</td><td>BUENOS AIRES</td><td>35</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ANDINA</td><td>SUR</td><td>7</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CORRIENTES_ZONA 04</td><td>NEA</td><td>10</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>NO DEFINIDO</td><td>NEA</td><td>7</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>TRES ARROYOS</td><td>SUR</td><td>26</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>FORMOSA</td><td>NEA</td><td>4</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>60</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>TUCUMAN</td><td>NOA</td><td>126</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>MENDOZA</td><td>CUYO</td><td>253</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ROSARIO_ZONA 06</td><td>NEA</td><td>11</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SAN FRANCISCO</td><td>NOA</td><td>18</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CHACO</td><td>NEA</td><td>12</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CHASCOMUS-DOLORES</td><td>BUENOS AIRES</td><td>32</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>MISIONES_ZONA 04</td><td>NEA</td><td>13</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>GUALEGUAYCHU</td><td>NEA</td><td>2</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ROSARIO_ZONA 02</td><td>NEA</td><td>48</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>NO DEFINIDO</td><td>ATLANTICA</td><td>7</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>48</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>VIEDMA</td><td>SUR</td><td>45</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>BAHIA BLANCA</td><td>SUR</td><td>107</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>363</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SAN JUAN</td><td>CUYO</td><td>129</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CATAMARCA</td><td>NOA</td><td>50</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CNEL SUAREZ</td><td>SUR</td><td>30</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>54</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ZAPALA</td><td>CUYO</td><td>38</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CORDOBA SUR</td><td>NOA</td><td>121</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>MERLO</td><td>AMBA_OESTE</td><td>215</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CUYO</td><td>AMBA_SUR</td><td>435</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>42</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>202</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>27</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>LUJAN</td><td>AMBA_NORTE</td><td>194</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ROSARIO_ZONA 05</td><td>NEA</td><td>20</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CENTRO</td><td>NEA</td><td>6</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SANTA FE_ZONA 04</td><td>NEA</td><td>11</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>FORMOSA_ZONA 01</td><td>NEA</td><td>16</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>NO DEFINIDO</td><td>SUR</td><td>26</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>291</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CORDOBA NORTE</td><td>NOA</td><td>187</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ENTRE RIOS_ZONA 03</td><td>NEA</td><td>19</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>NOA_CORDOBA_NORTE</td><td>NOA</td><td>4</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>326</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>VILLA MERCEDES</td><td>CUYO</td><td>36</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CORRIENTES_ZONA 02</td><td>NEA</td><td>9</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>TRELEW</td><td>SUR</td><td>66</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>AMBA_OESTE</td><td>AMBA_OESTE</td><td>24</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>MISIONES_ZONA 01</td><td>NEA</td><td>32</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td></td><td>NEA</td><td>59</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>686</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>NEUQUEN</td><td>CUYO</td><td>109</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>SAN LUIS</td><td>CUYO</td><td>62</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>JUJUY</td><td>NOA</td><td>47</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td></td><td>SUR</td><td>10</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>329</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>GLEW-CANUELAS</td><td>BUENOS AIRES</td><td>58</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>FLORES</td><td>AMBA_OESTE</td><td>261</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>RIO GRANDE - TIERRA DEL FUEGO</td><td>SUR</td><td>13</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>CHACO_ZONA 01</td><td>NEA</td><td>34</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>LA RIOJA</td><td>NOA</td><td>33</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td></td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>ESTE</td><td>NEA</td><td>1</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td></td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td></td><td>NOA</td><td>1</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>22</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>LA PLATA</td><td>ATLANTICA</td><td>1</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>AMBA_OESTE</td><td>MERLO</td><td>2</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>LA CATOLICA</td><td>AMBA_SUR</td><td>14</td></tr><tr><td>Centros TX</td><td>2015-12</td><td>activo</td><td>AMBA SUR NAVIAX</td><td>AMBA_SUR</td><td>1</td></tr></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [
        [
         "Centros TX",
         "2025-05",
         "activo",
         "",
         "",
         5
        ],
        [
         "Centros TX",
         "2025-05",
         "activo",
         "",
         "NEA",
         8
        ],
        [
         "Centros TX",
         "2025-05",
         "activo",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2025-05",
         "activo",
         "",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2025-05",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2025-05",
         "activo",
         "",
         "NOA",
         10
        ],
        [
         "Centros TX",
         "2025-05",
         "activo",
         "",
         "SUR",
         5
        ],
        [
         "Centros TX",
         "2025-05",
         "activo",
         "CUYO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2025-05",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2025-04",
         "Liberado",
         "",
         "",
         4
        ],
        [
         "Centros TX",
         "2025-04",
         "activo",
         "",
         "NOA",
         6
        ],
        [
         "Centros TX",
         "2025-04",
         "activo",
         "PEHUAJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2025-04",
         "activo",
         "",
         "",
         5
        ],
        [
         "Centros TX",
         "2025-04",
         "activo",
         "MISIONES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2025-04",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2025-04",
         "activo",
         "",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2025-04",
         "activo",
         "LA RIOJA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2025-04",
         "activo",
         "",
         "NEA",
         7
        ],
        [
         "Centros TX",
         "2025-04",
         "activo",
         "",
         "BUENOS AIRES",
         18
        ],
        [
         "Centros TX",
         "2025-04",
         "activo",
         "",
         "SUR",
         12
        ],
        [
         "Centros TX",
         "2025-04",
         "activo",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2025-03",
         "activo",
         "",
         "",
         2
        ],
        [
         "Centros TX",
         "2025-03",
         "activo",
         "",
         "NEA",
         22
        ],
        [
         "Centros TX",
         "2025-03",
         "activo",
         "",
         "NOA",
         9
        ],
        [
         "Centros TX",
         "2025-03",
         "activo",
         "",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2025-03",
         "activo",
         "",
         "BUENOS AIRES",
         4
        ],
        [
         "Infraestructura",
         "2025-02",
         "Liberado",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2025-02",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2025-02",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2025-02",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2025-01",
         "Liberado",
         "",
         "",
         5
        ],
        [
         "Infraestructura",
         "2025-01",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2025-01",
         "activo",
         "",
         "",
         2
        ],
        [
         "Centros TX",
         "2025-01",
         "activo",
         "",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2025-01",
         "activo",
         "",
         "NEA",
         10
        ],
        [
         "Centros TX",
         "2025-01",
         "activo",
         "",
         "NOA",
         8
        ],
        [
         "Centros TX",
         "2025-01",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2025-01",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2025-01",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2024-12",
         "Liberado",
         "",
         "",
         6
        ],
        [
         "Centros TX",
         "2024-12",
         "activo",
         "",
         "NEA",
         7
        ],
        [
         "Centros TX",
         "2024-12",
         "activo",
         "",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2024-12",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2024-12",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2024-12",
         "activo",
         "ROSARIO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2024-11",
         "Liberado",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2024-11",
         "Liberado",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2024-11",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2024-11",
         "Liberado",
         "",
         "",
         6
        ],
        [
         "Centros TX",
         "2024-11",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2024-11",
         "activo",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2024-11",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2024-10",
         "Liberado",
         "",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2024-10",
         "Liberado",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2024-10",
         "activo",
         "",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2024-10",
         "activo",
         "",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2024-10",
         "activo",
         "",
         "NEA",
         13
        ],
        [
         "Centros TX",
         "2024-10",
         "activo",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2024-09",
         "Liberado",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2024-09",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2024-09",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2024-09",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2024-09",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2024-09",
         "activo",
         "",
         "",
         2
        ],
        [
         "Centros TX",
         "2024-09",
         "activo",
         "",
         "NOA",
         10
        ],
        [
         "Centros TX",
         "2024-09",
         "activo",
         "",
         "NEA",
         20
        ],
        [
         "Centros TX",
         "2024-09",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2024-08",
         "Liberado",
         "",
         "",
         2
        ],
        [
         "Centros TX",
         "2024-08",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2024-08",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2024-08",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2024-08",
         "activo",
         "CATAMARCA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2024-08",
         "activo",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2024-08",
         "activo",
         "",
         "NOA",
         10
        ],
        [
         "Centros TX",
         "2024-08",
         "activo",
         "",
         "NEA",
         10
        ],
        [
         "Infraestructura",
         "2024-07",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2024-07",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2024-07",
         "Liberado",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2024-07",
         "activo",
         "",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2024-07",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2024-07",
         "activo",
         "",
         "NOA",
         20
        ],
        [
         "Centros TX",
         "2024-07",
         "activo",
         "",
         "",
         3
        ],
        [
         "Centros TX",
         "2024-07",
         "activo",
         "",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2024-07",
         "activo",
         "",
         "NEA",
         22
        ],
        [
         "Centros TX",
         "2024-07",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2024-07",
         "activo",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2024-07",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2024-07",
         "activo",
         "ROSARIO_ZONA 02",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2024-06",
         "Liberado",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2024-06",
         "Liberado",
         "",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2024-06",
         "Liberado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2024-06",
         "activo",
         "",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2024-06",
         "activo",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2024-06",
         "activo",
         "",
         "",
         3
        ],
        [
         "Centros TX",
         "2024-06",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2024-06",
         "activo",
         "",
         "NOA",
         3
        ],
        [
         "Centros TX",
         "2024-06",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2024-05",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2024-05",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2024-05",
         "Liberado",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2024-05",
         "Liberado",
         "",
         "",
         4
        ],
        [
         "Centros TX",
         "2024-05",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2024-05",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2024-05",
         "activo",
         "",
         "",
         2
        ],
        [
         "Centros TX",
         "2024-05",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2024-05",
         "activo",
         "",
         "NOA",
         14
        ],
        [
         "Centros TX",
         "2024-05",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2024-05",
         "activo",
         "",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2024-04",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2024-04",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2024-04",
         "Liberado",
         "",
         "",
         3
        ],
        [
         "Centros TX",
         "2024-04",
         "activo",
         "",
         "NOA",
         4
        ],
        [
         "Centros TX",
         "2024-04",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2024-04",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2024-04",
         "activo",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2024-04",
         "activo",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2024-04",
         "activo",
         "",
         "NEA",
         8
        ],
        [
         "Infraestructura",
         "2024-03",
         "Liberado",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2024-03",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2024-03",
         "activo",
         "",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2024-03",
         "activo",
         "LA RIOJA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2024-03",
         "activo",
         "",
         "NEA",
         10
        ],
        [
         "Centros TX",
         "2024-03",
         "activo",
         "",
         "NOA",
         25
        ],
        [
         "Centros TX",
         "2024-03",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2024-03",
         "activo",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2024-02",
         "Liberado",
         "CATAMARCA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2024-02",
         "activo",
         "",
         "",
         2
        ],
        [
         "Centros TX",
         "2024-02",
         "activo",
         "CATAMARCA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2024-02",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2024-02",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2024-01",
         "Liberado",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2024-01",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2024-01",
         "activo",
         "",
         "",
         3
        ],
        [
         "Infraestructura",
         "2023-12",
         "Liberado",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2023-12",
         "Liberado",
         "",
         "",
         2
        ],
        [
         "Centros TX",
         "2023-12",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2023-12",
         "activo",
         "",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-12",
         "activo",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-12",
         "activo",
         "",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2023-12",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2023-11",
         "Liberado",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "",
         "NEA",
         10
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "MENDOZA",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "",
         "",
         13
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "",
         "SUR",
         4
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2023-11",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         5
        ],
        [
         "Infraestructura",
         "2023-10",
         "Liberado",
         "",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "CATAMARCA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "JUNIN",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "",
         "NEA",
         4
        ],
        [
         "Centros TX",
         "2023-10",
         "activo",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2023-09",
         "Liberado",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2023-09",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2023-09",
         "Liberado",
         "JUNIN",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2023-09",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2023-09",
         "Liberado",
         "CATAMARCA",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2023-09",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2023-09",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2023-09",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "CNEL SUAREZ",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "SANTA FE_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "",
         "NEA",
         21
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "NEUQUEN",
         "CUYO",
         3
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "NECOCHEA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "PERGAMINO",
         "BUENOS AIRES",
         3
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "",
         "",
         6
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         5
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "SANTA ROSA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         5
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "9 DE JULIO",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "GRAL PICO",
         "SUR",
         3
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "RIO CUARTO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-09",
         "activo",
         "PEHUAJO",
         "BUENOS AIRES",
         2
        ],
        [
         "Infraestructura",
         "2023-08",
         "Liberado",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2023-08",
         "Liberado",
         "",
         "",
         2
        ],
        [
         "Infraestructura",
         "2023-08",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2023-08",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Infraestructura",
         "2023-08",
         "Liberado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2023-08",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2023-08",
         "Liberado",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2023-08",
         "Liberado",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "",
         "NEA",
         11
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "SANTA FE_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "CUYO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2023-08",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2023-07",
         "Liberado",
         "MENDOZA",
         "CUYO",
         3
        ],
        [
         "Infraestructura",
         "2023-07",
         "Liberado",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2023-07",
         "Liberado",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2023-07",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2023-07",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         7
        ],
        [
         "Infraestructura",
         "2023-07",
         "Liberado",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2023-07",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2023-07",
         "Liberado",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "",
         "NOA",
         4
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "",
         "",
         7
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "9 DE JULIO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "BONIFACIO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "SANTA ROSA",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "",
         "NEA",
         17
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "",
         "CUYO",
         4
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "FORMOSA_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2023-07",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2023-06",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2023-06",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2023-06",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         3
        ],
        [
         "Infraestructura",
         "2023-06",
         "Liberado",
         "",
         "",
         3
        ],
        [
         "Infraestructura",
         "2023-06",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2023-06",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         5
        ],
        [
         "Centros TX",
         "2023-06",
         "activo",
         "",
         "NEA",
         19
        ],
        [
         "Centros TX",
         "2023-06",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2023-06",
         "activo",
         "JUNIN DE LOS ANDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-06",
         "activo",
         "",
         "",
         3
        ],
        [
         "Centros TX",
         "2023-06",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-06",
         "activo",
         "",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-06",
         "activo",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-06",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2023-05",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2023-05",
         "Liberado",
         "JUNIN DE LOS ANDES",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2023-05",
         "Liberado",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2023-05",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2023-05",
         "Liberado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         8
        ],
        [
         "Infraestructura",
         "2023-05",
         "Liberado",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "SANTA CRUZ",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         5
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "",
         "",
         5
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "",
         "NEA",
         6
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "TANDIL",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2023-05",
         "activo",
         "",
         "NOA",
         8
        ],
        [
         "Infraestructura",
         "2023-04",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2023-04",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2023-04",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2023-04",
         "Liberado",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2023-04",
         "activo",
         "TANDIL",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-04",
         "activo",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-04",
         "activo",
         "",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-04",
         "activo",
         "",
         "",
         3
        ],
        [
         "Centros TX",
         "2023-04",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         4
        ],
        [
         "Centros TX",
         "2023-04",
         "activo",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-04",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2023-04",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-04",
         "activo",
         "FORMOSA_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2023-04",
         "activo",
         "",
         "NEA",
         10
        ],
        [
         "Infraestructura",
         "2023-03",
         "Liberado",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2023-03",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "MISIONES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "",
         "NOA",
         16
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "NO DEFINIDO",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "NO DEFINIDA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "NO DEFINIDO",
         "NEA",
         3
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "",
         "NEA",
         10
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "TUCUMAN",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "JUJUY",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-03",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2023-02",
         "Liberado",
         "BONIFACIO",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2023-02",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2023-02",
         "Liberado",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Infraestructura",
         "2023-02",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2023-02",
         "Liberado",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2023-02",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2023-02",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         3
        ],
        [
         "Infraestructura",
         "2023-02",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "MISIONES_ZONA 02",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "MISIONES_ZONA 01",
         "NEA",
         3
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "ROSARIO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "PATAGONIA",
         "SUR",
         3
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "COMODORO",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "FORMOSA_ZONA 01",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "NO DEFINIDO",
         "NEA",
         49
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "CORRIENTES_ZONA 01",
         "NEA",
         4
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "RIO GALLEGOS",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "ANDINA",
         "SUR",
         6
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "NOA_TUCUMAN",
         "NOA",
         4
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "",
         "NEA",
         60
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "AMBA_SUR",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "",
         "NOA",
         5
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "CATAMARCA",
         "NOA",
         6
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "NO DEFINIDO",
         "SUR",
         6
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "SANTA FE_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "",
         "",
         2
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "TUCUMAN",
         "NOA",
         10
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "MISIONES_ZONA 04",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "OLAVARRIA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "NO DEFINIDO",
         "NOA",
         8
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "CENTRO",
         "NEA",
         4
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "CORRIENTES_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "ESTE",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "MISIONES",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "CORRIENTES",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "SANTA CRUZ",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "",
         "SUR",
         7
        ],
        [
         "Centros TX",
         "2023-02",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         11
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "",
         "NEA",
         11
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "",
         "SUR",
         2
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "NO DEFINIDO",
         "NEA",
         2
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "SANTA CRUZ",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2023-01",
         "Liberado",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "JUJUY",
         "NOA",
         3
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "CORDOBA SUR",
         "NOA",
         6
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "SAN JUAN",
         "CUYO",
         3
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "NO DEFINIDO",
         "NOA",
         13
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "NO DEFINIDA",
         "BUENOS AIRES",
         3
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "AMBA_SUR",
         "AMBA_SUR",
         10
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "",
         "CUYO",
         3
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "NO DEFINIDO",
         "NEA",
         9
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "NOA_SALTA",
         "NOA",
         6
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "SALTA",
         "NOA",
         4
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "ANASCO",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "CUYO_MDZ",
         "CUYO",
         5
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "",
         "NEA",
         22
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "RIO CUARTO",
         "NOA",
         4
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "AMBA_NORTE",
         "AMBA_NORTE",
         10
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         6
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "",
         "NOA",
         16
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "MENDOZA",
         "CUYO",
         6
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "AMBA_OESTE",
         "AMBA_OESTE",
         5
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "NOA_CORDOBA_SUR",
         "NOA",
         19
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "",
         "SUR",
         5
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         4
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "NO DEFINIDO",
         "CUYO",
         5
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         9
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "LA RIOJA",
         "NOA",
         4
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         6
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "NOA_CORDOBA_NORTE",
         "NOA",
         11
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2023-01",
         "activo",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2022-12",
         "Liberado",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2022-12",
         "Liberado",
         "NO DEFINIDO",
         "NOA",
         4
        ],
        [
         "Infraestructura",
         "2022-12",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2022-12",
         "Liberado",
         "NO DEFINIDO",
         "NEA",
         5
        ],
        [
         "Infraestructura",
         "2022-12",
         "Liberado",
         "",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-12",
         "Liberado",
         "",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2022-12",
         "activo",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2022-12",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2022-12",
         "activo",
         "",
         "NOA",
         4
        ],
        [
         "Centros TX",
         "2022-12",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2022-12",
         "activo",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2022-12",
         "activo",
         "AMBA_SUR",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-12",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2022-12",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2022-11",
         "Liberado",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2022-11",
         "Liberado",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2022-11",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2022-11",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-11",
         "Liberado",
         "",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-11",
         "activo",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-11",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2022-11",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-11",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2022-11",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2022-10",
         "activo",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-10",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-10",
         "activo",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2022-09",
         "Liberado",
         "FORMOSA_ZONA 01",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2022-09",
         "Liberado",
         "",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-09",
         "Liberado",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2022-09",
         "Liberado",
         "",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2022-09",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2022-09",
         "activo",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2022-09",
         "activo",
         "CUYO",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-09",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2022-09",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2022-09",
         "activo",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2022-08",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2022-08",
         "Liberado",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2022-08",
         "Liberado",
         "",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2022-08",
         "Liberado",
         "NO DEFINIDO",
         "NOA",
         4
        ],
        [
         "Infraestructura",
         "2022-08",
         "Liberado",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2022-08",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-08",
         "Liberado",
         "",
         "NOA",
         6
        ],
        [
         "Centros TX",
         "2022-08",
         "activo",
         "AMBA_SUR",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2022-08",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         3
        ],
        [
         "Centros TX",
         "2022-08",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2022-08",
         "activo",
         "",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2022-08",
         "activo",
         "SAN JUAN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2022-08",
         "activo",
         "CORRIENTES_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2022-08",
         "activo",
         "TUCUMAN",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2022-07",
         "Liberado",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2022-07",
         "Liberado",
         "",
         "NEA",
         5
        ],
        [
         "Infraestructura",
         "2022-07",
         "Liberado",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2022-07",
         "Liberado",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2022-07",
         "Liberado",
         "NO DEFINIDO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-07",
         "Liberado",
         "NO DEFINIDO",
         "NEA",
         7
        ],
        [
         "Infraestructura",
         "2022-07",
         "Liberado",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2022-07",
         "activo",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2022-07",
         "activo",
         "NO DEFINIDO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2022-07",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2022-07",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "AMBA_OESTE",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "USHUAIA - TIERRA DEL FUEGO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "NO DEFINIDO",
         "NOA",
         26
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "NO DEFINIDO",
         "NEA",
         30
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "",
         "NEA",
         13
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "",
         "",
         4
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "",
         "NOA",
         8
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "NO DEFINIDA",
         "BUENOS AIRES",
         3
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "NO DEFINIDO",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "",
         "BUENOS AIRES",
         2
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-06",
         "Liberado",
         "NO DEFINIDO",
         "SUR",
         3
        ],
        [
         "Centros TX",
         "2022-06",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-06",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2022-06",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2022-06",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2022-06",
         "activo",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2022-06",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2022-06",
         "activo",
         "SANTA FE_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2022-06",
         "activo",
         "",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2022-06",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2022-05",
         "Liberado",
         "FORMOSA_ZONA 02",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2022-05",
         "Liberado",
         "NO DEFINIDO",
         "NEA",
         4
        ],
        [
         "Infraestructura",
         "2022-05",
         "Liberado",
         "SANTA ROSA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-05",
         "Liberado",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-05",
         "Liberado",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2022-05",
         "Liberado",
         "",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2022-05",
         "Liberado",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2022-05",
         "Liberado",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2022-05",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2022-05",
         "Liberado",
         "NO DEFINIDA",
         "BUENOS AIRES",
         5
        ],
        [
         "Centros TX",
         "2022-05",
         "activo",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2022-05",
         "activo",
         "CATAMARCA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2022-05",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2022-05",
         "activo",
         "NO DEFINIDO",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2022-05",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2022-05",
         "activo",
         "SALTA",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2022-05",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2022-05",
         "activo",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2022-04",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2022-04",
         "Liberado",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-04",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2022-04",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-04",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-04",
         "activo",
         "9 DE JULIO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2022-04",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2022-04",
         "activo",
         "ROSARIO_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2022-04",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2022-04",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-04",
         "activo",
         "",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2022-04",
         "activo",
         "RIO CUARTO",
         "NOA",
         2
        ],
        [
         "Infraestructura",
         "2022-03",
         "Liberado",
         "CHIVILCOY",
         "BUENOS AIRES",
         2
        ],
        [
         "Infraestructura",
         "2022-03",
         "Liberado",
         "TUCUMAN",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2022-03",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2022-03",
         "Liberado",
         "",
         "SUR",
         4
        ],
        [
         "Centros TX",
         "2022-03",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2022-03",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2022-03",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2022-03",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-03",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-03",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2022-03",
         "activo",
         "",
         "",
         3
        ],
        [
         "Centros TX",
         "2022-03",
         "activo",
         "SANTA FE_ZONA 03",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2022-02",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2022-02",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2022-02",
         "Liberado",
         "",
         "SUR",
         3
        ],
        [
         "Infraestructura",
         "2022-02",
         "Liberado",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2022-02",
         "Liberado",
         "",
         "CUYO",
         3
        ],
        [
         "Infraestructura",
         "2022-02",
         "Liberado",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2022-02",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2022-02",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2022-01",
         "Liberado",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2022-01",
         "Liberado",
         "",
         "CUYO",
         8
        ],
        [
         "Infraestructura",
         "2022-01",
         "Liberado",
         "",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2022-01",
         "Liberado",
         "TUCUMAN",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2022-01",
         "Liberado",
         "TRENQUE LAUQUEN",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2022-01",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2022-01",
         "Liberado",
         "",
         "SUR",
         4
        ],
        [
         "Infraestructura",
         "2022-01",
         "Liberado",
         "TRELEW",
         "SUR",
         2
        ],
        [
         "Infraestructura",
         "2022-01",
         "Liberado",
         "",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2022-01",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2022-01",
         "activo",
         "JUJUY",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2022-01",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2022-01",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2022-01",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2022-01",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2021-12",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2021-12",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2021-12",
         "Liberado",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2021-12",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2021-12",
         "activo",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2021-12",
         "activo",
         "LA RIOJA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2021-12",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2021-12",
         "activo",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2021-11",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2021-11",
         "Liberado",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2021-11",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2021-11",
         "activo",
         "CATAMARCA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2021-11",
         "activo",
         "",
         "NEA",
         5
        ],
        [
         "Centros TX",
         "2021-11",
         "activo",
         "RIO CUARTO",
         "NOA",
         3
        ],
        [
         "Centros TX",
         "2021-11",
         "activo",
         "PEHUAJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2021-11",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2021-11",
         "activo",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2021-11",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2021-11",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2021-11",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2021-11",
         "activo",
         "9 DE JULIO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2021-11",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         3
        ],
        [
         "Infraestructura",
         "2021-10",
         "Liberado",
         "",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2021-10",
         "Liberado",
         "",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "CUYO",
         "AMBA_SUR",
         6
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "PERGAMINO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "",
         "NEA",
         6
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         5
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "LA RIOJA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         7
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "SUR",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "CORDOBA SUR",
         "NOA",
         3
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "NO DEFINIDA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "NOA_CORDOBA_SUR",
         "NOA",
         3
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "RIO CUARTO",
         "NOA",
         13
        ],
        [
         "Centros TX",
         "2021-10",
         "activo",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2021-09",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         7
        ],
        [
         "Infraestructura",
         "2021-09",
         "Liberado",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "9 DE JULIO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "CUYO_MDZ",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "AMBA_SUR",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2021-09",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2021-08",
         "Liberado",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2021-08",
         "Liberado",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2021-08",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         3
        ],
        [
         "Infraestructura",
         "2021-08",
         "Liberado",
         "RIO GALLEGOS",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2021-08",
         "Liberado",
         "SANTA FE_ZONA 02",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2021-08",
         "Liberado",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2021-08",
         "Liberado",
         "NO DEFINIDO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2021-08",
         "activo",
         "SANTA FE_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2021-08",
         "activo",
         "TRES ARROYOS",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2021-08",
         "activo",
         "TUCUMAN",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2021-08",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2021-08",
         "activo",
         "",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2021-08",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2021-08",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2021-07",
         "Liberado",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2021-07",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         8
        ],
        [
         "Infraestructura",
         "2021-07",
         "Liberado",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2021-07",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2021-07",
         "Liberado",
         "",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2021-07",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2021-07",
         "activo",
         "MENDOZA",
         "CUYO",
         3
        ],
        [
         "Centros TX",
         "2021-07",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2021-07",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2021-06",
         "Liberado",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2021-06",
         "activo",
         "AMBA_OESTE",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2021-06",
         "activo",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2021-06",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2021-06",
         "activo",
         "CUYO",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2021-06",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2021-06",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2021-06",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2021-05",
         "Liberado",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2021-05",
         "Liberado",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2021-05",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2021-05",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2021-05",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2021-05",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2021-04",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2021-04",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2021-03",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2021-03",
         "Liberado",
         "SANTA CRUZ",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2021-03",
         "Liberado",
         "NO DEFINIDA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2021-03",
         "Liberado",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2021-03",
         "activo",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2021-03",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2021-03",
         "activo",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2021-03",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2021-02",
         "Liberado",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2021-02",
         "Liberado",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2021-02",
         "Liberado",
         "TRES ARROYOS",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2021-02",
         "Liberado",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2021-02",
         "Liberado",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2021-02",
         "activo",
         "AMBA_NORTE",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2021-02",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2021-01",
         "Liberado",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2021-01",
         "Liberado",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2021-01",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2021-01",
         "activo",
         "OLAVARRIA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2020-12",
         "Liberado",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2020-12",
         "Liberado",
         "CHIVILCOY",
         "BUENOS AIRES",
         2
        ],
        [
         "Infraestructura",
         "2020-12",
         "Liberado",
         "RIO GALLEGOS",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-12",
         "Liberado",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-12",
         "Liberado",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2020-12",
         "Liberado",
         "",
         "BUENOS AIRES",
         2
        ],
        [
         "Infraestructura",
         "2020-12",
         "Liberado",
         "AMBA_NORTE",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "ROSARIO_ZONA 05",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "JUNIN",
         "BUENOS AIRES",
         3
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "NECOCHEA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "PERGAMINO",
         "BUENOS AIRES",
         5
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "ROSARIO SUR",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "PEHUAJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "TRENQUE LAUQUEN",
         "BUENOS AIRES",
         6
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "CNEL SUAREZ",
         "SUR",
         6
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "9 DE JULIO",
         "BUENOS AIRES",
         5
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "AMBA_NORTE",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "NO DEFINIDO",
         "ATLANTICA",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         7
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "ZAPALA",
         "CUYO",
         17
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         19
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "TANDIL",
         "BUENOS AIRES",
         9
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "RIO GALLEGOS",
         "SUR",
         5
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "SANTA ROSA",
         "SUR",
         4
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "NO DEFINIDO",
         "CUYO",
         5
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "VIEDMA",
         "SUR",
         7
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "ESQUEL",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "NO DEFINIDO",
         "SUR",
         7
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "NEUQUEN",
         "CUYO",
         8
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "ENTRE RIOS_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "BARILOCHE",
         "CUYO",
         11
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         8
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "SAN JUAN",
         "CUYO",
         5
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "CHASCOMUS-DOLORES",
         "BUENOS AIRES",
         7
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "CHACO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "FORMOSA_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "OLAVARRIA",
         "BUENOS AIRES",
         7
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "TRES ARROYOS",
         "SUR",
         6
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "COMODORO",
         "SUR",
         6
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "MENDOZA",
         "CUYO",
         4
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         13
        ],
        [
         "Centros TX",
         "2020-12",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-11",
         "Liberado",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2020-11",
         "Liberado",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2020-11",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2020-11",
         "Liberado",
         "9 DE JULIO",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2020-11",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "PERGAMINO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "JUNIN",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "NECOCHEA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "SANTA ROSA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "NO DEFINIDO",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "ROSARIO NORTE",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "COMODORO",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "RIO GALLEGOS",
         "SUR",
         3
        ],
        [
         "Centros TX",
         "2020-11",
         "activo",
         "ZAPALA",
         "CUYO",
         2
        ],
        [
         "Infraestructura",
         "2020-10",
         "Liberado",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-10",
         "Liberado",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2020-10",
         "Liberado",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "TANDIL",
         "BUENOS AIRES",
         3
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "",
         "ATLANTICA",
         2
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "CNEL SUAREZ",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "JUNIN",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "BARILOCHE",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "MENDOZA",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "SAN JUAN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2020-10",
         "activo",
         "",
         "SUR",
         2
        ],
        [
         "Infraestructura",
         "2020-09",
         "Liberado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2020-09",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-09",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-09",
         "activo",
         "SAN JUSTO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2020-09",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2020-09",
         "activo",
         "CORRIENTES",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2020-09",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2020-09",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2020-09",
         "activo",
         "JUJUY",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2020-08",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2020-08",
         "Liberado",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-08",
         "Liberado",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-08",
         "Liberado",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2020-08",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2020-08",
         "activo",
         "CHACO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-08",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2020-08",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-08",
         "activo",
         "ESTE",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-08",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2020-08",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-08",
         "activo",
         "MENDOZA",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2020-08",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-08",
         "activo",
         "TUCUMAN",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2020-07",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2020-07",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         3
        ],
        [
         "Centros TX",
         "2020-07",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-07",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-07",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-07",
         "activo",
         "CNEL SUAREZ",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2020-06",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2020-06",
         "activo",
         "ENTRE RIOS",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-06",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-06",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-06",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2020-06",
         "activo",
         "LA CATOLICA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-05",
         "Liberado",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2020-05",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2020-05",
         "Liberado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2020-05",
         "activo",
         "NO DEFINIDO",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-05",
         "activo",
         "BARILOCHE",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2020-05",
         "activo",
         "CHASCOMUS-DOLORES",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-05",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-05",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         17
        ],
        [
         "Centros TX",
         "2020-05",
         "activo",
         "LA RIOJA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-05",
         "activo",
         "AMBA_SUR",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-05",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-05",
         "activo",
         "RIO CUARTO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-05",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-05",
         "activo",
         "CUYO_MDZ",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-05",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2020-04",
         "Liberado",
         "TUCUMAN",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2020-04",
         "Liberado",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2020-04",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2020-04",
         "Liberado",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2020-04",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-04",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-04",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-04",
         "activo",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-04",
         "activo",
         "NO DEFINIDO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2020-04",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-04",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2020-04",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2020-04",
         "activo",
         "",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2020-04",
         "activo",
         "SAN JUSTO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2020-04",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         8
        ],
        [
         "Infraestructura",
         "2020-03",
         "Liberado",
         "",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2020-03",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-03",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-03",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-03",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2020-03",
         "Liberado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2020-03",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "CUYO_EXT",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "CORRIENTES",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         23
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-03",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2020-02",
         "Liberado",
         "",
         "AMBA_NORTE",
         6
        ],
        [
         "Infraestructura",
         "2020-02",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2020-02",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         4
        ],
        [
         "Infraestructura",
         "2020-02",
         "Liberado",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-02",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2020-02",
         "activo",
         "RIO GALLEGOS",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2020-02",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-02",
         "activo",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2020-02",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         16
        ],
        [
         "Centros TX",
         "2020-02",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2020-02",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2020-02",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         4
        ],
        [
         "Centros TX",
         "2020-02",
         "activo",
         "ROSARIO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-02",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2020-02",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2020-02",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         6
        ],
        [
         "Infraestructura",
         "2020-01",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-01",
         "Liberado",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2020-01",
         "Liberado",
         "MENDOZA",
         "CUYO",
         2
        ],
        [
         "Infraestructura",
         "2020-01",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2020-01",
         "Liberado",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2020-01",
         "Liberado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2020-01",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2020-01",
         "Liberado",
         "PERGAMINO",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2020-01",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2020-01",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2020-01",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-01",
         "activo",
         "",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2020-01",
         "activo",
         "ROSARIO NORTE",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-01",
         "activo",
         "MENDOZA",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2020-01",
         "activo",
         "MISIONES_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2020-01",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2020-01",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         4
        ],
        [
         "Centros TX",
         "2020-01",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         7
        ],
        [
         "Centros TX",
         "2020-01",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         5
        ],
        [
         "Centros TX",
         "2020-01",
         "activo",
         "SAN JUSTO",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-12",
         "Liberado",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-12",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         2
        ],
        [
         "Infraestructura",
         "2019-12",
         "Liberado",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2019-12",
         "Liberado",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-12",
         "Liberado",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2019-12",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2019-12",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2019-12",
         "activo",
         "NEUQUEN",
         "CUYO",
         3
        ],
        [
         "Centros TX",
         "2019-12",
         "activo",
         "RIO GALLEGOS",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2019-12",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         4
        ],
        [
         "Centros TX",
         "2019-12",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-12",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-12",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2019-11",
         "Liberado",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-11",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-11",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "TANDIL",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "NOA_TUCUMAN",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "NO DEFINIDA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "ROSARIO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "CHACO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "SANTA FE_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "SANTA FE_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         6
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "AMBA_SUR",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         9
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         6
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "NO DEFINIDO",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "BARILOCHE",
         "CUYO",
         3
        ],
        [
         "Centros TX",
         "2019-11",
         "activo",
         "CHACO",
         "NEA",
         3
        ],
        [
         "Infraestructura",
         "2019-10",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-10",
         "Liberado",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2019-10",
         "Liberado",
         "MISIONES_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "MISIONES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "SAN JUSTO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "MISIONES_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "AMBA_OESTE",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "ROSARIO NORTE",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "SALTA",
         "NOA",
         3
        ],
        [
         "Centros TX",
         "2019-10",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         11
        ],
        [
         "Infraestructura",
         "2019-09",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-09",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-09",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         4
        ],
        [
         "Infraestructura",
         "2019-09",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2019-09",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2019-09",
         "Liberado",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-09",
         "Liberado",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2019-09",
         "Liberado",
         "ROSARIO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "NO DEFINIDO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "ROSARIO_ZONA 05",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "CORDOBA SUR",
         "NOA",
         4
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         8
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "ROSARIO_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2019-09",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-08",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-08",
         "Liberado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2019-08",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-08",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2019-08",
         "Liberado",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2019-08",
         "Liberado",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-08",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-08",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2019-08",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2019-08",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2019-08",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-08",
         "activo",
         "SAN JUSTO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2019-08",
         "activo",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Infraestructura",
         "2019-07",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-07",
         "Liberado",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2019-07",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-07",
         "Liberado",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2019-07",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2019-07",
         "activo",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2019-07",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2019-07",
         "activo",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-07",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-07",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2019-07",
         "activo",
         "ROSARIO NORTE",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-07",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-07",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2019-07",
         "activo",
         "LA RIOJA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2019-07",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-07",
         "activo",
         "CHACO_ZONA 02",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2019-06",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2019-06",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-06",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-06",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         3
        ],
        [
         "Infraestructura",
         "2019-06",
         "Liberado",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Infraestructura",
         "2019-06",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-06",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "RIO CUARTO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         3
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         5
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "AMBA_NORTE",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2019-06",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "CHACO_ZONA 02",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "RIO GALLEGOS",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "LA PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "Liberado",
         "JUNIN",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "JUJUY",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         6
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "PERGAMINO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "PEHUAJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "LA RIOJA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         3
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2019-05",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         2
        ],
        [
         "Infraestructura",
         "2019-04",
         "Liberado",
         "LA PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Infraestructura",
         "2019-04",
         "Liberado",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2019-04",
         "Liberado",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         6
        ],
        [
         "Infraestructura",
         "2019-04",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-04",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-04",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-04",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-04",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2019-04",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2019-04",
         "activo",
         "RIO CUARTO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2019-04",
         "activo",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2019-04",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-04",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-04",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-04",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         7
        ],
        [
         "Centros TX",
         "2019-04",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         5
        ],
        [
         "Infraestructura",
         "2019-03",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-03",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2019-03",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-03",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-03",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-03",
         "Liberado",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2019-03",
         "Liberado",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-03",
         "Liberado",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2019-03",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         3
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         3
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "AMBA_OESTE",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "CHACO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "ENTRE RIOS_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "ROSARIO CENTRO",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "AMBA_SUR",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2019-03",
         "activo",
         "CUYO_MDZ",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2019-02",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2019-02",
         "Liberado",
         "SANTA FE_ZONA 03",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2019-02",
         "Liberado",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-02",
         "Liberado",
         "LA PLATA",
         "BUENOS AIRES",
         6
        ],
        [
         "Infraestructura",
         "2019-02",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         5
        ],
        [
         "Infraestructura",
         "2019-02",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         5
        ],
        [
         "Centros TX",
         "2019-02",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2019-02",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-02",
         "activo",
         "SANTA FE_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2019-02",
         "activo",
         "SAN JUAN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2019-02",
         "activo",
         "CUYO",
         "AMBA_SUR",
         3
        ],
        [
         "Centros TX",
         "2019-02",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-02",
         "activo",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-01",
         "Liberado",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2019-01",
         "Liberado",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2019-01",
         "Liberado",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2019-01",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         3
        ],
        [
         "Infraestructura",
         "2019-01",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-01",
         "Liberado",
         "ROSARIO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2019-01",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-01",
         "Liberado",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2019-01",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2019-01",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         2
        ],
        [
         "Infraestructura",
         "2019-01",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         4
        ],
        [
         "Infraestructura",
         "2019-01",
         "Liberado",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-01",
         "activo",
         "CUYO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2019-01",
         "activo",
         "NO DEFINIDO",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-01",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2019-01",
         "activo",
         "CENTRO",
         "ATLANTICA",
         1
        ],
        [
         "Centros TX",
         "2019-01",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2019-01",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2019-01",
         "activo",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2019-01",
         "activo",
         "MENDOZA",
         "CUYO",
         3
        ],
        [
         "Centros TX",
         "2019-01",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2019-01",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2019-01",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-12",
         "Liberado",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-12",
         "Liberado",
         "RIO CUARTO",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2018-12",
         "Liberado",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2018-12",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         4
        ],
        [
         "Infraestructura",
         "2018-12",
         "Liberado",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-12",
         "Liberado",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-12",
         "Liberado",
         "TANDIL",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-12",
         "Liberado",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "NO DEFINIDO",
         "ATLANTICA",
         2
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-12",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2018-11",
         "Liberado",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2018-11",
         "Liberado",
         "ROSARIO_ZONA 03",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2018-11",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-11",
         "Liberado",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2018-11",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         4
        ],
        [
         "Infraestructura",
         "2018-11",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-11",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-11",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2018-11",
         "Liberado",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-11",
         "Liberado",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-11",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "MENDOZA",
         "CUYO",
         4
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "AMBA_OESTE",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "ROSARIO CENTRO",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "AMBA_NORTE",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "TRES ARROYOS",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "SAN JUSTO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "NO DEFINIDO",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "AMBA_SUR",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         4
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "BARILOCHE",
         "CUYO",
         3
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "NO DEFINIDO",
         "ATLANTICA",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "ATLANTICA",
         "ATLANTICA",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "CUYO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "ROSARIO_ZONA 03",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         5
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "TANDIL",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-11",
         "activo",
         "CORRIENTES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2018-10",
         "Liberado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-10",
         "Liberado",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-10",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2018-10",
         "Liberado",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "SAN JUAN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "AMBA_SUR",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         4
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         3
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "CUYO",
         "AMBA_SUR",
         8
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         5
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "AMBA_NORTE",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "AMBA_OESTE",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         3
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "NO DEFINIDO",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "ENTRE RIOS",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         7
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "MENDOZA",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         6
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         4
        ],
        [
         "Centros TX",
         "2018-10",
         "activo",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2018-09",
         "Liberado",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2018-09",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-09",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "PERGAMINO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         4
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "ENTRE RIOS_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "MENDOZA",
         "CUYO",
         3
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         5
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         5
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         8
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "AMBA_NORTE",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         3
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "CUYO",
         "AMBA_SUR",
         12
        ],
        [
         "Centros TX",
         "2018-09",
         "activo",
         "AMBA_SUR",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-08",
         "Liberado",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2018-08",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-08",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2018-08",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         3
        ],
        [
         "Infraestructura",
         "2018-08",
         "Liberado",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-08",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2018-08",
         "Liberado",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-08",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2018-08",
         "Liberado",
         "MENDOZA",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "CATAMARCA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         3
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "ROSARIO_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         5
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-08",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "ROSARIO_ZONA 03",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         3
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "LA PLATA",
         "BUENOS AIRES",
         3
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2018-07",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2018-07",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2018-07",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2018-07",
         "activo",
         "CORRIENTES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-07",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-07",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-07",
         "activo",
         "CHACO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-07",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-07",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2018-07",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-07",
         "activo",
         "ROSARIO_ZONA 02",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         3
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "ENTRE RIOS_ZONA 03",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "BAHIA BLANCA",
         "SUR",
         2
        ],
        [
         "Infraestructura",
         "2018-06",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2018-06",
         "activo",
         "SAN FRANCISCO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-06",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-06",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         3
        ],
        [
         "Centros TX",
         "2018-06",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-06",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-06",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-06",
         "activo",
         "OLAVARRIA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-06",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-06",
         "activo",
         "RIO CUARTO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-06",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-06",
         "activo",
         "BARILOCHE",
         "CUYO",
         5
        ],
        [
         "Centros TX",
         "2018-06",
         "activo",
         "COMODORO",
         "SUR",
         3
        ],
        [
         "Infraestructura",
         "2018-05",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-05",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-05",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2018-05",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-05",
         "Liberado",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-05",
         "activo",
         "TANDIL",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-05",
         "activo",
         "USHUAIA - TIERRA DEL FUEGO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2018-05",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-05",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-05",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2018-05",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-05",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-05",
         "activo",
         "SAN FRANCISCO",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         3
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "MISIONES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "AMBA_SUR",
         "AMBA_SUR",
         6
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "TANDIL",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         2
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "AMBA_NORTE",
         "AMBA_NORTE",
         23
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "CORRIENTES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "USHUAIA - TIERRA DEL FUEGO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "SAN JUSTO",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         4
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "BONIFACIO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "CUYO",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "ROSARIO_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2018-04",
         "activo",
         "SANTA FE_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-03",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-03",
         "activo",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2018-03",
         "activo",
         "RIO GALLEGOS",
         "SUR",
         4
        ],
        [
         "Centros TX",
         "2018-03",
         "activo",
         "NEUQUEN",
         "CUYO",
         3
        ],
        [
         "Centros TX",
         "2018-03",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-03",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2018-03",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2018-03",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-03",
         "activo",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-03",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-03",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2018-03",
         "activo",
         "VIEDMA",
         "SUR",
         2
        ],
        [
         "Infraestructura",
         "2018-02",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-02",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2018-02",
         "Liberado",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2018-02",
         "Liberado",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-02",
         "Liberado",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-02",
         "Liberado",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-02",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2018-02",
         "activo",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2018-02",
         "activo",
         "USHUAIA - TIERRA DEL FUEGO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2018-02",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-02",
         "activo",
         "MENDOZA",
         "CUYO",
         4
        ],
        [
         "Centros TX",
         "2018-02",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2018-02",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-02",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2018-02",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-02",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         3
        ],
        [
         "Centros TX",
         "2018-02",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-01",
         "Liberado",
         "CHACO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2018-01",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-01",
         "Liberado",
         "BARILOCHE",
         "CUYO",
         2
        ],
        [
         "Infraestructura",
         "2018-01",
         "Liberado",
         "RIO CUARTO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "PATAGONIA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "TUCUMAN",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "SANTA FE_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "NEUQUEN",
         "CUYO",
         4
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "CUYO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "ROSARIO CENTRO",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "ESQUEL",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "MISIONES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "MENDOZA",
         "CUYO",
         9
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "NOA_CORDOBA_SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "VIEDMA",
         "SUR",
         4
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         3
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "CORRIENTES_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "COMODORO",
         "SUR",
         3
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "GRAL ROCA",
         "CUYO",
         4
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "BARILOCHE",
         "CUYO",
         5
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "LA CATOLICA",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2018-01",
         "activo",
         "NO DEFINIDO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-12",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-12",
         "Liberado",
         "TANDIL",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2017-12",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-12",
         "Liberado",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-12",
         "Liberado",
         "MAR DE AJO",
         "BUENOS AIRES",
         3
        ],
        [
         "Infraestructura",
         "2017-12",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         2
        ],
        [
         "Infraestructura",
         "2017-12",
         "Liberado",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2017-12",
         "Liberado",
         "BARRACAS",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "CHACO",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "ENTRE RIOS_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "VENADO TUERTO",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         5
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "ROSARIO_ZONA 02",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         8
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "NECOCHEA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "MENDOZA",
         "CUYO",
         7
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "TUCUMAN",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "ROSARIO_ZONA 06",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         4
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "SANTA FE_ZONA 01",
         "NEA",
         4
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "CATAMARCA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "ROSARIO_ZONA 03",
         "NEA",
         5
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "ROSARIO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "CHASCOMUS-DOLORES",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "CUYO",
         "AMBA_SUR",
         3
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "JUJUY",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "GUALEGUAYCHU",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-12",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         153
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "JUNIN",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         7
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         5
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         3
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         7
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         7
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         4
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         4
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "CORRIENTES_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "SANTA FE_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "LA CATOLICA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "ROSARIO_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "MISIONES_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "SAN JUSTO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "CUYO",
         "AMBA_SUR",
         5
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "JUJUY",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2017-11",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "BAHIA BLANCA",
         "SUR",
         103
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CORRIENTES_ZONA 01",
         "NEA",
         29
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         333
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "ROSARIO_ZONA 01",
         "NEA",
         49
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "AMBA_NORTE",
         "AMBA_NORTE",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "ROSARIO_ZONA 06",
         "NEA",
         4
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "PEHUAJO",
         "BUENOS AIRES",
         16
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "LA PLATA",
         "BUENOS AIRES",
         155
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "MISIONES_ZONA 02",
         "NEA",
         6
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "TRELEW",
         "SUR",
         52
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CORRIENTES_ZONA 02",
         "NEA",
         7
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "",
         "",
         10
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CHASCOMUS-DOLORES",
         "BUENOS AIRES",
         22
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "TRES ARROYOS",
         "SUR",
         19
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "",
         "SUR",
         19
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "JUNIN",
         "BUENOS AIRES",
         49
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "FORMOSA_ZONA 01",
         "NEA",
         11
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "ENTRE RIOS_ZONA 01",
         "NEA",
         12
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "PERGAMINO",
         "BUENOS AIRES",
         36
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "TUCUMAN",
         "NOA",
         108
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CHACO_ZONA 02",
         "NEA",
         6
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "",
         "BUENOS AIRES",
         23
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "",
         "NOA",
         36
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "",
         "NEA",
         32
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SAN JUSTO",
         "AMBA_OESTE",
         11
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "ROSARIO_ZONA 02",
         "NEA",
         53
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SANTA ROSA",
         "SUR",
         53
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "",
         "AMBA_NORTE",
         3
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "ROSARIO_ZONA 04",
         "NEA",
         57
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         228
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "MENDOZA",
         "CUYO",
         192
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "MERLO",
         "AMBA_OESTE",
         222
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SAN FRANCISCO",
         "NOA",
         10
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "",
         "CUYO",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "ROSARIO_ZONA 05",
         "NEA",
         13
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CORRIENTES_ZONA 04",
         "NEA",
         8
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CHIVILCOY",
         "BUENOS AIRES",
         49
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "LUJAN",
         "AMBA_NORTE",
         232
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "COMODORO",
         "SUR",
         69
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CORDOBA SUR",
         "NOA",
         119
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SAN JUAN",
         "CUYO",
         116
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CATAMARCA",
         "NOA",
         31
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "MISIONES_ZONA 04",
         "NEA",
         4
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SALTA",
         "NOA",
         73
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CNEL SUAREZ",
         "SUR",
         22
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CHACO_ZONA 01",
         "NEA",
         28
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "FLORES",
         "AMBA_OESTE",
         249
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         123
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "VIEDMA",
         "SUR",
         47
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "ROSARIO_ZONA 03",
         "NEA",
         24
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "NO DEFINIDO",
         "",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SANTIAGO DEL ESTERO",
         "NOA",
         39
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "NECOCHEA",
         "BUENOS AIRES",
         21
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CUYO",
         "AMBA_SUR",
         233
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "RIO CUARTO",
         "NOA",
         28
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "ENTRE RIOS_ZONA 02",
         "NEA",
         10
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "GRAL ROCA",
         "CUYO",
         30
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "USHUAIA - TIERRA DEL FUEGO",
         "SUR",
         25
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CORRIENTES_ZONA 03",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "RIO GALLEGOS",
         "SUR",
         42
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SAN RAFAEL",
         "CUYO",
         53
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "TANDIL",
         "BUENOS AIRES",
         27
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "NEUQUEN",
         "CUYO",
         102
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "BARILOCHE",
         "CUYO",
         72
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "MAR DE AJO",
         "BUENOS AIRES",
         192
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "RIO GRANDE - TIERRA DEL FUEGO",
         "SUR",
         15
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "ENTRE RIOS_ZONA 03",
         "NEA",
         16
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SANTA FE_ZONA 02",
         "NEA",
         27
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         261
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "ESQUEL",
         "SUR",
         33
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "CORDOBA NORTE",
         "NOA",
         130
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "VILLA MERCEDES",
         "CUYO",
         33
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "MISIONES_ZONA 01",
         "NEA",
         30
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SANTA FE_ZONA 01",
         "NEA",
         23
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SANTA FE_ZONA 04",
         "NEA",
         8
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "JUJUY",
         "NOA",
         40
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "OLAVARRIA",
         "BUENOS AIRES",
         25
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SANTA FE_ZONA 03",
         "NEA",
         49
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "JUNCAL",
         "AMBA_NORTE",
         254
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "BARRACAS",
         "AMBA_SUR",
         167
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "FCO SOLANO",
         "AMBA_SUR",
         106
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         63
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "LA RIOJA",
         "NOA",
         21
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "9 DE JULIO",
         "BUENOS AIRES",
         19
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "GRAL PICO",
         "SUR",
         32
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SAN LUIS",
         "CUYO",
         53
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "ZAPALA",
         "CUYO",
         27
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "TRENQUE LAUQUEN",
         "BUENOS AIRES",
         21
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "FORMOSA_ZONA 02",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "JUNIN DE LOS ANDES",
         "CUYO",
         3
        ],
        [
         "Infraestructura",
         "2017-11",
         "Liberado",
         "SANTA FE_ZONA 05",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "VIEDMA",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         5
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "CORRIENTES",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "LA RIOJA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "JUNIN",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "MENDOZA",
         "CUYO",
         27
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "SANTA FE_ZONA 01",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "OLAVARRIA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         4
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "ENTRE RIOS",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "TRENQUE LAUQUEN",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-10",
         "activo",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         8
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "VIEDMA",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "CHASCOMUS-DOLORES",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "JUNIN",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "CORDOBA SUR",
         "NOA",
         3
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "NECOCHEA",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "USHUAIA - TIERRA DEL FUEGO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "TANDIL",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         3
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         25
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "9 DE JULIO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "RIO GRANDE - TIERRA DEL FUEGO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "SAN JUAN",
         "CUYO",
         4
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         6
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         52
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "PEHUAJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "SANTA ROSA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-09",
         "activo",
         "PERGAMINO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         4
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "SAN FRANCISCO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "SALTA",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "SAN JUAN",
         "CUYO",
         7
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         3
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "MENDOZA",
         "CUYO",
         3
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "SANTA FE_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "AMBA_SUR",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "BARILOCHE",
         "CUYO",
         5
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "TRELEW",
         "SUR",
         5
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "VIEDMA",
         "SUR",
         3
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         4
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         12
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "CORRIENTES_ZONA 01",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2017-08",
         "activo",
         "NEUQUEN",
         "CUYO",
         5
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "TRES ARROYOS",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         6
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "MENDOZA",
         "CUYO",
         18
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         4
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         8
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "AMBA_NORTE",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "PERGAMINO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "NOA_CORDOBA_NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "NO DEFINIDO",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         11
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "CUYO",
         "AMBA_SUR",
         22
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "ROSARIO_ZONA 02",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "ROSARIO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "CNEL SUAREZ",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-07",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         9
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "TUCUMAN",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         4
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "NO DEFINIDO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         3
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "SANTA FE_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "CUYO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "CHASCOMUS-DOLORES",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "JUNIN",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "NOA_CORDOBA_SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "MENDOZA",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "LA CATOLICA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-06",
         "activo",
         "PERGAMINO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "MISIONES_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "GRAL PICO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "BARILOCHE",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "RIO CUARTO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "NOA_CORDOBA_SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2017-05",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "SAN LUIS",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "CUYO_EXT",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-04",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         4
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         4
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "JUJUY",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "GRAL PICO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "MENDOZA",
         "CUYO",
         3
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "SAN LUIS",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2017-03",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         4
        ],
        [
         "Centros TX",
         "2017-02",
         "activo",
         "ROSARIO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-02",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2017-02",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2017-02",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2017-02",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-02",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-02",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2017-02",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-02",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2017-02",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "CHACO_ZONA 01",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         9
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         14
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "CORRIENTES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         5
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2017-01",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         2
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "OLAVARRIA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-12",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         3
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "RIO CUARTO",
         "NOA",
         4
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "NOA_TUCUMAN",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         5
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         6
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "RIO GALLEGOS",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "SAN JUAN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "ROSARIO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "SALTA",
         "NOA",
         3
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "USHUAIA - TIERRA DEL FUEGO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "CORRIENTES",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "SANTA FE_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         8
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "LA RIOJA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         11
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "AMBA_OESTE",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         6
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "TUCUMAN",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "SANTA FE_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-11",
         "activo",
         "JUNIN",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-10",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-10",
         "activo",
         "PERGAMINO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-10",
         "activo",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-10",
         "activo",
         "RIO GRANDE - TIERRA DEL FUEGO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-10",
         "activo",
         "SANTA ROSA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-10",
         "activo",
         "RIO CUARTO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-10",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-10",
         "activo",
         "SANTA FE_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-10",
         "activo",
         "JUJUY",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-10",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-10",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-09",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-09",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-09",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-09",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-09",
         "activo",
         "ROSARIO_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-09",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-09",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "ROSARIO_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "MENDOZA",
         "CUYO",
         24
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "CHACO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "TRENQUE LAUQUEN",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "LA RIOJA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "JUNIN",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "NO DEFINIDO",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "SANTA FE_ZONA 01",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "TANDIL",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-08",
         "activo",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "USHUAIA - TIERRA DEL FUEGO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "RIO GRANDE - TIERRA DEL FUEGO",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "SANTA FE_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-07",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         3
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "JUJUY",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "TUCUMAN",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "CORRIENTES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "SALTA",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "CATAMARCA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "COMODORO",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "CHACO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "ENTRE RIOS_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "RIO GRANDE - TIERRA DEL FUEGO",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "RIO GALLEGOS",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "SANTA FE_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "SAN JUAN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "MISIONES",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-06",
         "activo",
         "OLAVARRIA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "ROSARIO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "ROSARIO_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "TUCUMAN",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "SANTA ROSA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2016-05",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "PATAGONIA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         9
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         8
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "CORRIENTES",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "ENTRE RIOS_ZONA 02",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "JUJUY",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "",
         "",
         2
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "RIO GALLEGOS",
         "SUR",
         2
        ],
        [
         "Centros TX",
         "2016-04",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "MISIONES_ZONA 01",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "SANTA ROSA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "TUCUMAN",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "TANDIL",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-03",
         "activo",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "CORDOBA SUR",
         "NOA",
         2
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "ROSARIO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "SANTA FE_ZONA 03",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "CUYO",
         "AMBA_SUR",
         2
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "PATAGONIA",
         "SUR",
         1
        ],
        [
         "Centros TX",
         "2016-02",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         3
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         2
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         5
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "MISIONES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         6
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "CORRIENTES",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "",
         "",
         1
        ],
        [
         "Centros TX",
         "2016-01",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ANASCO",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "USHUAIA - TIERRA DEL FUEGO",
         "SUR",
         28
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "",
         "",
         33
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "9 DE JULIO",
         "BUENOS AIRES",
         31
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CHACO_ZONA 02",
         "NEA",
         7
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "MISIONES_ZONA 02",
         "NEA",
         9
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ENTRE RIOS_ZONA 01",
         "NEA",
         18
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CORRIENTES",
         "NEA",
         15
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SAN RAFAEL",
         "CUYO",
         58
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ROSARIO_ZONA 03",
         "NEA",
         25
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "BARRACAS",
         "AMBA_SUR",
         184
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CUYO_EXT",
         "CUYO",
         9
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "NECOCHEA",
         "BUENOS AIRES",
         19
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "PASO DE LOS LIBRES",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "BARILOCHE",
         "CUYO",
         64
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CORRIENTES_ZONA 03",
         "NEA",
         3
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "MISIONES_ZONA 03",
         "NEA",
         5
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ROSARIO SUR",
         "NEA",
         13
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SANTA FE_ZONA 05",
         "NEA",
         12
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SALTA",
         "NOA",
         100
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "JUNIN",
         "BUENOS AIRES",
         54
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SANTA FE_ZONA 03",
         "NEA",
         54
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ROSARIO CENTRO",
         "NEA",
         40
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "LOS POLVORINES",
         "AMBA_NORTE",
         231
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "GRAL ROCA",
         "CUYO",
         31
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ENTRE RIOS_ZONA 02",
         "NEA",
         18
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "JUNIN DE LOS ANDES",
         "CUYO",
         4
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "MISIONES",
         "NEA",
         9
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CORRIENTES_ZONA 01",
         "NEA",
         33
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "COMODORO",
         "SUR",
         78
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "PEHUAJO",
         "BUENOS AIRES",
         20
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ROSARIO_ZONA 01",
         "NEA",
         49
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CUYO_MDZ",
         "CUYO",
         2
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ESQUEL",
         "SUR",
         33
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "PATAGONIA",
         "SUR",
         25
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "RIO CUARTO",
         "NOA",
         46
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SANTA FE_ZONA 01",
         "NEA",
         23
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SAN JUSTO",
         "AMBA_OESTE",
         8
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "TANDIL",
         "BUENOS AIRES",
         24
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "GRAL PICO",
         "SUR",
         41
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "GLEW",
         "ATLANTICA",
         1
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "NOA_CORDOBA_SUR",
         "NOA",
         7
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         221
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "NO DEFINIDO",
         "CUYO",
         10
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "RIO GALLEGOS",
         "SUR",
         53
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "MAR DE AJO",
         "BUENOS AIRES",
         136
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "FORMOSA_ZONA 02",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ENTRE RIOS",
         "NEA",
         14
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ROSARIO NORTE",
         "NEA",
         35
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SANTA ROSA",
         "SUR",
         62
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SANTA FE_ZONA 02",
         "NEA",
         32
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "TRENQUE LAUQUEN",
         "BUENOS AIRES",
         22
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "OLAVARRIA",
         "BUENOS AIRES",
         35
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ANDINA",
         "SUR",
         7
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CORRIENTES_ZONA 04",
         "NEA",
         10
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "NO DEFINIDO",
         "NEA",
         7
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "TRES ARROYOS",
         "SUR",
         26
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "FORMOSA",
         "NEA",
         4
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SANTIAGO DEL ESTERO",
         "NOA",
         60
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "TUCUMAN",
         "NOA",
         126
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "MENDOZA",
         "CUYO",
         253
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ROSARIO_ZONA 06",
         "NEA",
         11
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SAN FRANCISCO",
         "NOA",
         18
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CHACO",
         "NEA",
         12
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CHASCOMUS-DOLORES",
         "BUENOS AIRES",
         32
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "MISIONES_ZONA 04",
         "NEA",
         13
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "GUALEGUAYCHU",
         "NEA",
         2
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ROSARIO_ZONA 02",
         "NEA",
         48
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "NO DEFINIDO",
         "ATLANTICA",
         7
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ROSARIO_ZONA 04",
         "NEA",
         48
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "VIEDMA",
         "SUR",
         45
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "BAHIA BLANCA",
         "SUR",
         107
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "JUNCAL",
         "AMBA_NORTE",
         363
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SAN JUAN",
         "CUYO",
         129
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CATAMARCA",
         "NOA",
         50
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CNEL SUAREZ",
         "SUR",
         30
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CHIVILCOY",
         "BUENOS AIRES",
         54
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ZAPALA",
         "CUYO",
         38
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CORDOBA SUR",
         "NOA",
         121
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "MERLO",
         "AMBA_OESTE",
         215
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CUYO",
         "AMBA_SUR",
         435
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "PERGAMINO",
         "BUENOS AIRES",
         42
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "LA PLATA",
         "BUENOS AIRES",
         202
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "AMBA_NORTE",
         "AMBA_NORTE",
         27
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "LUJAN",
         "AMBA_NORTE",
         194
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ROSARIO_ZONA 05",
         "NEA",
         20
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CENTRO",
         "NEA",
         6
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SANTA FE_ZONA 04",
         "NEA",
         11
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "FORMOSA_ZONA 01",
         "NEA",
         16
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "NO DEFINIDO",
         "SUR",
         26
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         291
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CORDOBA NORTE",
         "NOA",
         187
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ENTRE RIOS_ZONA 03",
         "NEA",
         19
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "NOA_CORDOBA_NORTE",
         "NOA",
         4
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         326
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "VILLA MERCEDES",
         "CUYO",
         36
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CORRIENTES_ZONA 02",
         "NEA",
         9
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "TRELEW",
         "SUR",
         66
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "NO DEFINIDA",
         "BUENOS AIRES",
         4
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "AMBA_OESTE",
         "AMBA_OESTE",
         24
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "MISIONES_ZONA 01",
         "NEA",
         32
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "",
         "NEA",
         59
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "FCO SOLANO",
         "AMBA_SUR",
         686
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "NEUQUEN",
         "CUYO",
         109
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "SAN LUIS",
         "CUYO",
         62
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "JUJUY",
         "NOA",
         47
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "",
         "SUR",
         10
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         329
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "GLEW-CANUELAS",
         "BUENOS AIRES",
         58
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "FLORES",
         "AMBA_OESTE",
         261
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "RIO GRANDE - TIERRA DEL FUEGO",
         "SUR",
         13
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "CHACO_ZONA 01",
         "NEA",
         34
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "LA RIOJA",
         "NOA",
         33
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "",
         "AMBA_OESTE",
         1
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "ESTE",
         "NEA",
         1
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "",
         "AMBA_NORTE",
         2
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "",
         "NOA",
         1
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "AMBA_SUR",
         "AMBA_SUR",
         22
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "LA PLATA",
         "ATLANTICA",
         1
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "AMBA_OESTE",
         "MERLO",
         2
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "LA CATOLICA",
         "AMBA_SUR",
         14
        ],
        [
         "Centros TX",
         "2015-12",
         "activo",
         "AMBA SUR NAVIAX",
         "AMBA_SUR",
         1
        ]
       ],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "modulo",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "yearMonth",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "estado",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "areaOYM",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "cabeceraOYM",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "num_emplazamientos",
            "nullable": false,
            "type": "long"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 19
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "modulo",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "yearMonth",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "estado",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "areaOYM",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "cabeceraOYM",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "num_emplazamientos",
         "type": "\"long\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "SELECT\n",
    "  modulo,\n",
    "  date_format(fecha_modificacion, 'yyyy-MM') AS yearMonth,\n",
    "  estado,\n",
    "  areaOYM,\n",
    "  cabeceraOYM,\n",
    "  COUNT(*) AS num_emplazamientos\n",
    "FROM silver_emplazamientos_modulo\n",
    "WHERE estado in ('Liberado','activo')\n",
    "GROUP BY modulo, yearMonth, estado, areaOYM, cabeceraOYM\n",
    "ORDER BY yearMonth DESC"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "45c534d9-b018-4ee8-9083-28a4d04214b1",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "7. Evolución Mensual de Emplazamientos No Productivos por Módulo"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "35cc60b7-8e68-462e-8451-c6c5b784599c",
     "showTitle": false,
     "tableResultSettingsMap": {
      "0": {
       "dataGridStateBlob": "{\"version\":1,\"tableState\":{\"columnPinning\":{\"left\":[\"#row_number#\"],\"right\":[]},\"columnSizing\":{},\"columnVisibility\":{}},\"settings\":{\"columns\":{}},\"syncTimestamp\":1768156484829}",
       "filterBlob": null,
       "queryPlanFiltersBlob": null,
       "tableResultIndex": 0
      }
     },
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>modulo</th><th>yearMonth</th><th>estado</th><th>areaOYM</th><th>cabeceraOYM</th><th>emplazamientos</th></tr></thead><tbody><tr><td>Sitios RAN</td><td>2025-05</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2025-03</td><td>No Utilizado</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2025-03</td><td>No Utilizado</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2024-11</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2024-11</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-10</td><td>No Utilizado</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2024-10</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2024-10</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-09</td><td>No Utilizado</td><td></td><td></td><td>1</td></tr><tr><td>Sitios RAN</td><td>2024-08</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2024-07</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-06</td><td>No Utilizado</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-06</td><td>No Utilizado</td><td></td><td></td><td>8</td></tr><tr><td>Sitios RAN</td><td>2024-06</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2024-05</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2024-04</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2024-02</td><td>No Utilizado</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-11</td><td>No Utilizado</td><td></td><td></td><td>1</td></tr><tr><td>Sitios RAN</td><td>2023-11</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-10</td><td>No Utilizado</td><td></td><td></td><td>2</td></tr><tr><td>Sitios RAN</td><td>2023-10</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>8</td></tr><tr><td>Infraestructura</td><td>2023-09</td><td>No Utilizado</td><td></td><td></td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-08</td><td>No Utilizado</td><td></td><td></td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-08</td><td>No Utilizado</td><td></td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2023-08</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2023-08</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2023-08</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-07</td><td>No Utilizado</td><td></td><td></td><td>1</td></tr><tr><td>Sitios RAN</td><td>2023-07</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-06</td><td>No Utilizado</td><td></td><td></td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-06</td><td>No Utilizado</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-06</td><td>No Utilizado</td><td></td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2023-05</td><td>A Restituir</td><td></td><td></td><td>2</td></tr><tr><td>Infraestructura</td><td>2023-05</td><td>No Utilizado</td><td></td><td></td><td>1</td></tr><tr><td>Sitios RAN</td><td>2023-04</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2023-01</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-12</td><td>No Utilizado</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-12</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-10</td><td>No Utilizado</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-09</td><td>No Utilizado</td><td></td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-08</td><td>No Utilizado</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2022-08</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>10</td></tr><tr><td>Sitios RAN</td><td>2022-08</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>4</td></tr><tr><td>Infraestructura</td><td>2022-07</td><td>No Utilizado</td><td></td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-07</td><td>No Utilizado</td><td></td><td>NEA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2022-07</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>17</td></tr><tr><td>Sitios RAN</td><td>2022-07</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>24</td></tr><tr><td>Sitios RAN</td><td>2022-07</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>20</td></tr><tr><td>Sitios RAN</td><td>2022-07</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>14</td></tr><tr><td>Sitios RAN</td><td>2022-07</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>7</td></tr><tr><td>Sitios RAN</td><td>2022-07</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>4</td></tr><tr><td>Sitios RAN</td><td>2022-07</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>7</td></tr><tr><td>Sitios RAN</td><td>2022-07</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>8</td></tr><tr><td>Sitios RAN</td><td>2022-07</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>6</td></tr><tr><td>Sitios RAN</td><td>2022-07</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>13</td></tr><tr><td>Sitios RAN</td><td>2022-07</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>12</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>No Utilizado</td><td></td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>No Utilizado</td><td></td><td>NOA</td><td>5</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>No Utilizado</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>No Utilizado</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>No Utilizado</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>No Utilizado</td><td></td><td>NEA</td><td>6</td></tr><tr><td>Infraestructura</td><td>2022-06</td><td>No Utilizado</td><td></td><td>BUENOS AIRES</td><td>4</td></tr><tr><td>Infraestructura</td><td>2022-05</td><td>No Utilizado</td><td></td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-01</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2022-01</td><td>A Restituir</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2022-01</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2022-01</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2021-10</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2021-09</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2021-09</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>4</td></tr><tr><td>Sitios RAN</td><td>2021-09</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2021-09</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2021-08</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2021-07</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>6</td></tr><tr><td>Infraestructura</td><td>2021-03</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-02</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2021-02</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2021-02</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2021-02</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2021-01</td><td>No Utilizado</td><td></td><td>CUYO</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2021-01</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2021-01</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>8</td></tr><tr><td>Sitios RAN</td><td>2021-01</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>4</td></tr><tr><td>Sitios RAN</td><td>2021-01</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2021-01</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2021-01</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2021-01</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2020-12</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>4</td></tr><tr><td>Sitios RAN</td><td>2020-12</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2020-12</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2020-12</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2020-12</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2020-11</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2020-11</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2020-11</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2020-11</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2020-11</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-10</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-09</td><td>No Utilizado</td><td></td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-09</td><td>No Utilizado</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-08</td><td>No Utilizado</td><td></td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-07</td><td>No Utilizado</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-07</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-07</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2020-06</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>4</td></tr><tr><td>Infraestructura</td><td>2020-05</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2020-05</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-04</td><td>No Utilizado</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Infraestructura</td><td>2020-04</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>CUYO</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2020-04</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2020-04</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2020-04</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2020-04</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>A Restituir</td><td>LA CATOLICA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-03</td><td>No Utilizado</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-02</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-02</td><td>No Utilizado</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-01</td><td>A Restituir</td><td>FLORES</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2020-01</td><td>A Restituir</td><td>CUYO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-12</td><td>No Utilizado</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-12</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NOA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2019-12</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2019-12</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-11</td><td>No Utilizado</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-10</td><td>No Utilizado</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2019-10</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2019-10</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-09</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2019-09</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2019-08</td><td>A Restituir</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2019-08</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2019-07</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2019-06</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-05</td><td>No Utilizado</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2019-05</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-04</td><td>No Utilizado</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-04</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-04</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2019-04</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2019-04</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-03</td><td>No Utilizado</td><td>CORDOBA NORTE</td><td>NOA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2019-03</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>26</td></tr><tr><td>Sitios RAN</td><td>2019-03</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>6</td></tr><tr><td>Sitios RAN</td><td>2019-03</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>20</td></tr><tr><td>Sitios RAN</td><td>2019-03</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>5</td></tr><tr><td>Sitios RAN</td><td>2019-03</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>6</td></tr><tr><td>Sitios RAN</td><td>2019-03</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>12</td></tr><tr><td>Sitios RAN</td><td>2019-03</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>5</td></tr><tr><td>Sitios RAN</td><td>2019-03</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>9</td></tr><tr><td>Sitios RAN</td><td>2019-03</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>12</td></tr><tr><td>Sitios RAN</td><td>2019-03</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>14</td></tr><tr><td>Infraestructura</td><td>2019-02</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NEA</td><td>3</td></tr><tr><td>Infraestructura</td><td>2019-02</td><td>A Restituir</td><td>SAN JUAN</td><td>CUYO</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2019-02</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>No Utilizado</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2019-01</td><td>A Restituir</td><td>LA CATOLICA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-12</td><td>No Utilizado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>A Restituir</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>A Restituir</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>No Utilizado</td><td>MENDOZA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-11</td><td>No Utilizado</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2018-11</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-10</td><td>No Utilizado</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-10</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2018-10</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-09</td><td>A Restituir</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-08</td><td>No Utilizado</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-08</td><td>No Utilizado</td><td>LOMAS DE ZAMORA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-08</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2018-08</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>No Utilizado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>No Utilizado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NOA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>No Utilizado</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>No Utilizado</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-07</td><td>No Utilizado</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2018-07</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2018-07</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>No Utilizado</td><td>AMBA_OESTE</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>A Restituir</td><td>ROSARIO_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>No Utilizado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>No Utilizado</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2018-06</td><td>No Utilizado</td><td>NEUQUEN</td><td>CUYO</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2018-06</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2018-06</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2018-06</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2018-06</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2018-06</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2018-05</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2018-05</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-04</td><td>No Utilizado</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2018-04</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2018-03</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2018-03</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-02</td><td>No Utilizado</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-02</td><td>A Restituir</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-02</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-02</td><td>No Utilizado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2018-01</td><td>No Utilizado</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2018-01</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-12</td><td>No Utilizado</td><td>uruguay</td><td></td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-12</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-12</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NOA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2017-12</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>9 DE JULIO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>BARRACAS</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>CORRIENTES</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>ROSARIO SUR</td><td>NEA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>SANTA FE_ZONA 05</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>RAMOS MEJIA</td><td>AMBA_OESTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>SANTA ROSA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>BUENOS AIRES</td><td>290</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>NOA_CORDOBA_NORTE</td><td>NOA</td><td>61</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>9</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>ROSARIO CENTRO</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td></td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NOA</td><td>160</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>NOA_SALTA</td><td>NOA</td><td>14</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>MAR DEL PLATA</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>BAHIA BLANCA</td><td>SUR</td><td>3</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>CUYO_EXT</td><td>CUYO</td><td>24</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>VIEDMA</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>GRAL ROCA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>LA CATOLICA</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>ENTRE RIOS_ZONA 03</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>RIO GALLEGOS</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>PATAGONIA</td><td>SUR</td><td>93</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>TUCUMAN</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>ATLANTICA</td><td>BUENOS AIRES</td><td>70</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>MENDOZA</td><td>CUYO</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>MISIONES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>CATAMARCA</td><td>NOA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>SAN JUAN</td><td>CUYO</td><td>3</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>GRAL PICO</td><td>SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>CORRIENTES_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td><td>593</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>4</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>CUYO</td><td>AMBA_SUR</td><td>12</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>SUR</td><td>117</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>JUJUY</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>CHASCOMUS-DOLORES</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td></td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>CORDOBA NORTE</td><td>NOA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>CNEL SUAREZ</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>FLORES</td><td>AMBA_OESTE</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>MENDOZA</td><td>CUYO</td><td>6</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>NEUQUEN</td><td>CUYO</td><td>3</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>LUJAN</td><td>AMBA_NORTE</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>VILLA MERCEDES</td><td>CUYO</td><td>5</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>MISIONES_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>JUNIN</td><td>BUENOS AIRES</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>FLORES</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>LA RIOJA</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>FCO SOLANO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>PEHUAJO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>uruguay</td><td></td><td>6</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>NEA</td><td>NEA</td><td>79</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>LA PLATA</td><td>BUENOS AIRES</td><td>3</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>MAR DE AJO</td><td>BUENOS AIRES</td><td>6</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>MISIONES_ZONA 04</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>CORDOBA NORTE</td><td>NOA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>SANTA FE_ZONA 05</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>LOS POLVORINES</td><td>AMBA_NORTE</td><td>3</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>NEUQUEN</td><td>CUYO</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>SALTA</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>NO DEFINIDA</td><td>BUENOS AIRES</td><td>39</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>SALTA</td><td>NOA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>CUYO_MDZ</td><td>CUYO</td><td>23</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>ENTRE RIOS_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>BARRACAS</td><td>AMBA_SUR</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>AMBA_OESTE</td><td>AMBA_OESTE</td><td>303</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>MONTE CHINGOLO</td><td>AMBA_SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>JUNCAL</td><td>AMBA_NORTE</td><td>7</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>ANDINA</td><td>SUR</td><td>38</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td></td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>LITORAL</td><td>NEA</td><td>236</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>NOA_CORDOBA_SUR</td><td>NOA</td><td>61</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>FORMOSA_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>AMBA_SUR</td><td>AMBA_SUR</td><td>290</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>CORRIENTES_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>CUYO</td><td>44</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>MERLO</td><td>AMBA_OESTE</td><td>3</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>ENTRE RIOS_ZONA 02</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>COMODORO</td><td>SUR</td><td>6</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td></td><td></td><td>749</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>PERGAMINO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>A Restituir</td><td>CUYO</td><td>AMBA_SUR</td><td>10</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>NOA_TUCUMAN</td><td>NOA</td><td>17</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>NO DEFINIDO</td><td>NEA</td><td>266</td></tr><tr><td>Sitios RAN</td><td>2017-11</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td></td><td>AMBA_SUR</td><td>19</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td></td><td>AMBA_NORTE</td><td>32</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>ZAPALA</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>CORDOBA SUR</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>CHACO_ZONA 01</td><td>NEA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>CENTRO</td><td>NEA</td><td>2</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td></td><td>CUYO</td><td>11</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>BARILOCHE</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td></td><td>AMBA_OESTE</td><td>15</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td></td><td>SUR</td><td>25</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>TRELEW</td><td>SUR</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>SANTIAGO DEL ESTERO</td><td>NOA</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>CENTRO</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>CHIVILCOY</td><td>BUENOS AIRES</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td></td><td>NOA</td><td>13</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td></td><td>NEA</td><td>28</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>SAN RAFAEL</td><td>CUYO</td><td>1</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td></td><td>BUENOS AIRES</td><td>16</td></tr><tr><td>Infraestructura</td><td>2017-11</td><td>No Utilizado</td><td>ESQUEL</td><td>SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-09</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2017-09</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-09</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-09</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-09</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-08</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2017-08</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-06</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-06</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2017-06</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-05</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-05</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-04</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-03</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-03</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>5</td></tr><tr><td>Sitios RAN</td><td>2017-03</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-03</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>4</td></tr><tr><td>Sitios RAN</td><td>2017-03</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2017-03</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2017-03</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-03</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-02</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-02</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-01</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2017-01</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-09</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2016-09</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2016-09</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2016-09</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-09</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2016-09</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-09</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-08</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-08</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>8</td></tr><tr><td>Sitios RAN</td><td>2016-08</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-08</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-08</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2016-07</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-07</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-07</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2016-07</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-06</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>6</td></tr><tr><td>Sitios RAN</td><td>2016-06</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>6</td></tr><tr><td>Sitios RAN</td><td>2016-06</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>6</td></tr><tr><td>Sitios RAN</td><td>2016-06</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>14</td></tr><tr><td>Sitios RAN</td><td>2016-06</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2016-06</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>5</td></tr><tr><td>Sitios RAN</td><td>2016-06</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>6</td></tr><tr><td>Sitios RAN</td><td>2016-06</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>5</td></tr><tr><td>Sitios RAN</td><td>2016-06</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2016-06</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>8</td></tr><tr><td>Sitios RAN</td><td>2016-06</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>6</td></tr><tr><td>Sitios RAN</td><td>2016-05</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-05</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-04</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>7</td></tr><tr><td>Sitios RAN</td><td>2016-04</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2016-04</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>4</td></tr><tr><td>Sitios RAN</td><td>2016-04</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-04</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-03</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-02</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2016-02</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-02</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2016-02</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-02</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2016-02</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2016-02</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2016-01</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2016-01</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2015-12</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-12</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-12</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-12</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-12</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-11</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-11</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-11</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-11</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-10</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2015-10</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-09</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>4</td></tr><tr><td>Sitios RAN</td><td>2015-09</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2015-09</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-08</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-08</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2015-08</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2015-08</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-08</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-08</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>11</td></tr><tr><td>Sitios RAN</td><td>2015-08</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-08</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-07</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>78</td></tr><tr><td>Sitios RAN</td><td>2015-07</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>41</td></tr><tr><td>Sitios RAN</td><td>2015-07</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>28</td></tr><tr><td>Sitios RAN</td><td>2015-07</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>44</td></tr><tr><td>Sitios RAN</td><td>2015-07</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>40</td></tr><tr><td>Sitios RAN</td><td>2015-07</td><td>desdefinido</td><td>AMBA</td><td>SUBTE</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2015-07</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>32</td></tr><tr><td>Sitios RAN</td><td>2015-07</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>27</td></tr><tr><td>Sitios RAN</td><td>2015-07</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>41</td></tr><tr><td>Sitios RAN</td><td>2015-07</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>37</td></tr><tr><td>Sitios RAN</td><td>2015-07</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>38</td></tr><tr><td>Sitios RAN</td><td>2015-07</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>21</td></tr><tr><td>Sitios RAN</td><td>2015-06</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2015-06</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-06</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-05</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-04</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2015-03</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2015-01</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2014-11</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2014-10</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2014-09</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>17</td></tr><tr><td>Sitios RAN</td><td>2014-09</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2014-08</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>28</td></tr><tr><td>Sitios RAN</td><td>2014-08</td><td>desdefinido</td><td>INT</td><td>LITORAL</td><td>4</td></tr><tr><td>Sitios RAN</td><td>2014-08</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>14</td></tr><tr><td>Sitios RAN</td><td>2014-08</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2014-07</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL MACROMICROCENTRO/LA PLATA</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2014-07</td><td>desdefinido</td><td>AMBA</td><td>AMBA NORTE</td><td>2</td></tr><tr><td>Sitios RAN</td><td>2014-07</td><td>desdefinido</td><td>AMBA</td><td>CAPITAL FEDERAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2014-07</td><td>desdefinido</td><td>AMBA</td><td>AMBA SUR</td><td>3</td></tr><tr><td>Sitios RAN</td><td>2014-07</td><td>desdefinido</td><td>INT</td><td>CENTRAL</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2014-07</td><td>desdefinido</td><td>AMBA</td><td>AMBA OESTE</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2014-06</td><td>desdefinido</td><td>INT</td><td>NOA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2014-06</td><td>desdefinido</td><td>INT</td><td>ATLÁNTICA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2014-05</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2014-05</td><td>desdefinido</td><td>INT</td><td>PATAGONIA</td><td>1</td></tr><tr><td>Sitios RAN</td><td>2014-04</td><td>desdefinido</td><td>INT</td><td>CUYO</td><td>4</td></tr></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [
        [
         "Sitios RAN",
         "2025-05",
         "desdefinido",
         "INT",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2025-03",
         "No Utilizado",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2025-03",
         "No Utilizado",
         "",
         "NEA",
         1
        ],
        [
         "Sitios RAN",
         "2024-11",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2024-11",
         "desdefinido",
         "INT",
         "LITORAL",
         1
        ],
        [
         "Infraestructura",
         "2024-10",
         "No Utilizado",
         "",
         "SUR",
         1
        ],
        [
         "Sitios RAN",
         "2024-10",
         "desdefinido",
         "INT",
         "CUYO",
         2
        ],
        [
         "Sitios RAN",
         "2024-10",
         "desdefinido",
         "INT",
         "PATAGONIA",
         1
        ],
        [
         "Infraestructura",
         "2024-09",
         "No Utilizado",
         "",
         "",
         1
        ],
        [
         "Sitios RAN",
         "2024-08",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         1
        ],
        [
         "Sitios RAN",
         "2024-07",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Infraestructura",
         "2024-06",
         "No Utilizado",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2024-06",
         "No Utilizado",
         "",
         "",
         8
        ],
        [
         "Sitios RAN",
         "2024-06",
         "desdefinido",
         "INT",
         "LITORAL",
         1
        ],
        [
         "Sitios RAN",
         "2024-05",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2024-04",
         "desdefinido",
         "INT",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2024-02",
         "No Utilizado",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2023-11",
         "No Utilizado",
         "",
         "",
         1
        ],
        [
         "Sitios RAN",
         "2023-11",
         "desdefinido",
         "INT",
         "CUYO",
         2
        ],
        [
         "Infraestructura",
         "2023-10",
         "No Utilizado",
         "",
         "",
         2
        ],
        [
         "Sitios RAN",
         "2023-10",
         "desdefinido",
         "INT",
         "NOA",
         8
        ],
        [
         "Infraestructura",
         "2023-09",
         "No Utilizado",
         "",
         "",
         2
        ],
        [
         "Infraestructura",
         "2023-08",
         "No Utilizado",
         "",
         "",
         2
        ],
        [
         "Infraestructura",
         "2023-08",
         "No Utilizado",
         "",
         "BUENOS AIRES",
         2
        ],
        [
         "Sitios RAN",
         "2023-08",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         1
        ],
        [
         "Sitios RAN",
         "2023-08",
         "desdefinido",
         "INT",
         "PATAGONIA",
         2
        ],
        [
         "Sitios RAN",
         "2023-08",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         2
        ],
        [
         "Infraestructura",
         "2023-07",
         "No Utilizado",
         "",
         "",
         1
        ],
        [
         "Sitios RAN",
         "2023-07",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Infraestructura",
         "2023-06",
         "No Utilizado",
         "",
         "",
         2
        ],
        [
         "Infraestructura",
         "2023-06",
         "No Utilizado",
         "",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2023-06",
         "No Utilizado",
         "",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2023-05",
         "A Restituir",
         "",
         "",
         2
        ],
        [
         "Infraestructura",
         "2023-05",
         "No Utilizado",
         "",
         "",
         1
        ],
        [
         "Sitios RAN",
         "2023-04",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         1
        ],
        [
         "Sitios RAN",
         "2023-01",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-12",
         "No Utilizado",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2022-12",
         "No Utilizado",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2022-10",
         "No Utilizado",
         "",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2022-09",
         "No Utilizado",
         "",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2022-08",
         "No Utilizado",
         "",
         "SUR",
         1
        ],
        [
         "Sitios RAN",
         "2022-08",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         10
        ],
        [
         "Sitios RAN",
         "2022-08",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         4
        ],
        [
         "Infraestructura",
         "2022-07",
         "No Utilizado",
         "",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2022-07",
         "No Utilizado",
         "",
         "NEA",
         2
        ],
        [
         "Sitios RAN",
         "2022-07",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         17
        ],
        [
         "Sitios RAN",
         "2022-07",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         24
        ],
        [
         "Sitios RAN",
         "2022-07",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         20
        ],
        [
         "Sitios RAN",
         "2022-07",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         14
        ],
        [
         "Sitios RAN",
         "2022-07",
         "desdefinido",
         "INT",
         "LITORAL",
         7
        ],
        [
         "Sitios RAN",
         "2022-07",
         "desdefinido",
         "INT",
         "CENTRAL",
         4
        ],
        [
         "Sitios RAN",
         "2022-07",
         "desdefinido",
         "INT",
         "CUYO",
         7
        ],
        [
         "Sitios RAN",
         "2022-07",
         "desdefinido",
         "INT",
         "PATAGONIA",
         8
        ],
        [
         "Sitios RAN",
         "2022-07",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         6
        ],
        [
         "Sitios RAN",
         "2022-07",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         13
        ],
        [
         "Sitios RAN",
         "2022-07",
         "desdefinido",
         "INT",
         "NOA",
         12
        ],
        [
         "Infraestructura",
         "2022-06",
         "No Utilizado",
         "",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-06",
         "No Utilizado",
         "",
         "NOA",
         5
        ],
        [
         "Infraestructura",
         "2022-06",
         "No Utilizado",
         "",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-06",
         "No Utilizado",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2022-06",
         "No Utilizado",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2022-06",
         "No Utilizado",
         "",
         "NEA",
         6
        ],
        [
         "Infraestructura",
         "2022-06",
         "No Utilizado",
         "",
         "BUENOS AIRES",
         4
        ],
        [
         "Infraestructura",
         "2022-05",
         "No Utilizado",
         "",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2022-01",
         "No Utilizado",
         "NO DEFINIDO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2022-01",
         "A Restituir",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Sitios RAN",
         "2022-01",
         "desdefinido",
         "INT",
         "CENTRAL",
         2
        ],
        [
         "Sitios RAN",
         "2022-01",
         "desdefinido",
         "INT",
         "LITORAL",
         1
        ],
        [
         "Sitios RAN",
         "2021-10",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2021-09",
         "desdefinido",
         "INT",
         "LITORAL",
         2
        ],
        [
         "Sitios RAN",
         "2021-09",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         4
        ],
        [
         "Sitios RAN",
         "2021-09",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2021-09",
         "desdefinido",
         "INT",
         "NOA",
         1
        ],
        [
         "Sitios RAN",
         "2021-08",
         "desdefinido",
         "INT",
         "PATAGONIA",
         1
        ],
        [
         "Sitios RAN",
         "2021-07",
         "desdefinido",
         "INT",
         "PATAGONIA",
         6
        ],
        [
         "Infraestructura",
         "2021-03",
         "No Utilizado",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2021-02",
         "No Utilizado",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Sitios RAN",
         "2021-02",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         3
        ],
        [
         "Sitios RAN",
         "2021-02",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         1
        ],
        [
         "Sitios RAN",
         "2021-02",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Infraestructura",
         "2021-01",
         "No Utilizado",
         "",
         "CUYO",
         2
        ],
        [
         "Sitios RAN",
         "2021-01",
         "desdefinido",
         "INT",
         "PATAGONIA",
         1
        ],
        [
         "Sitios RAN",
         "2021-01",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         8
        ],
        [
         "Sitios RAN",
         "2021-01",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         4
        ],
        [
         "Sitios RAN",
         "2021-01",
         "desdefinido",
         "INT",
         "CENTRAL",
         3
        ],
        [
         "Sitios RAN",
         "2021-01",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2021-01",
         "desdefinido",
         "INT",
         "CUYO",
         2
        ],
        [
         "Sitios RAN",
         "2021-01",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         2
        ],
        [
         "Sitios RAN",
         "2020-12",
         "desdefinido",
         "INT",
         "LITORAL",
         4
        ],
        [
         "Sitios RAN",
         "2020-12",
         "desdefinido",
         "INT",
         "NOA",
         3
        ],
        [
         "Sitios RAN",
         "2020-12",
         "desdefinido",
         "INT",
         "CENTRAL",
         2
        ],
        [
         "Sitios RAN",
         "2020-12",
         "desdefinido",
         "INT",
         "PATAGONIA",
         1
        ],
        [
         "Sitios RAN",
         "2020-12",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         1
        ],
        [
         "Sitios RAN",
         "2020-11",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         1
        ],
        [
         "Sitios RAN",
         "2020-11",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         2
        ],
        [
         "Sitios RAN",
         "2020-11",
         "desdefinido",
         "INT",
         "CUYO",
         1
        ],
        [
         "Sitios RAN",
         "2020-11",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         1
        ],
        [
         "Sitios RAN",
         "2020-11",
         "desdefinido",
         "INT",
         "PATAGONIA",
         1
        ],
        [
         "Infraestructura",
         "2020-10",
         "No Utilizado",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2020-09",
         "No Utilizado",
         "",
         "",
         1
        ],
        [
         "Infraestructura",
         "2020-09",
         "No Utilizado",
         "",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-08",
         "No Utilizado",
         "",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2020-07",
         "No Utilizado",
         "NO DEFINIDA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2020-07",
         "No Utilizado",
         "NO DEFINIDO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-07",
         "No Utilizado",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Sitios RAN",
         "2020-06",
         "desdefinido",
         "INT",
         "PATAGONIA",
         4
        ],
        [
         "Infraestructura",
         "2020-05",
         "No Utilizado",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Sitios RAN",
         "2020-05",
         "desdefinido",
         "INT",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2020-04",
         "No Utilizado",
         "NO DEFINIDA",
         "BUENOS AIRES",
         2
        ],
        [
         "Infraestructura",
         "2020-04",
         "No Utilizado",
         "NO DEFINIDO",
         "CUYO",
         1
        ],
        [
         "Sitios RAN",
         "2020-04",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2020-04",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2020-04",
         "desdefinido",
         "INT",
         "CUYO",
         2
        ],
        [
         "Sitios RAN",
         "2020-04",
         "desdefinido",
         "INT",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2020-03",
         "No Utilizado",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2020-03",
         "No Utilizado",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2020-03",
         "No Utilizado",
         "NO DEFINIDO",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2020-03",
         "No Utilizado",
         "NO DEFINIDO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-03",
         "A Restituir",
         "LA CATOLICA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2020-03",
         "No Utilizado",
         "NO DEFINIDA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2020-02",
         "No Utilizado",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2020-02",
         "No Utilizado",
         "NO DEFINIDA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2020-01",
         "A Restituir",
         "FLORES",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2020-01",
         "A Restituir",
         "CUYO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-12",
         "No Utilizado",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-12",
         "No Utilizado",
         "NO DEFINIDO",
         "NOA",
         2
        ],
        [
         "Sitios RAN",
         "2019-12",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2019-12",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-11",
         "No Utilizado",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-10",
         "No Utilizado",
         "AMBA_SUR",
         "AMBA_SUR",
         1
        ],
        [
         "Sitios RAN",
         "2019-10",
         "desdefinido",
         "INT",
         "NOA",
         1
        ],
        [
         "Sitios RAN",
         "2019-10",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Infraestructura",
         "2019-09",
         "No Utilizado",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Sitios RAN",
         "2019-09",
         "desdefinido",
         "INT",
         "NOA",
         2
        ],
        [
         "Infraestructura",
         "2019-08",
         "A Restituir",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Sitios RAN",
         "2019-08",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2019-07",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         1
        ],
        [
         "Sitios RAN",
         "2019-06",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-05",
         "No Utilizado",
         "AMBA_SUR",
         "AMBA_SUR",
         1
        ],
        [
         "Sitios RAN",
         "2019-05",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-04",
         "No Utilizado",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2019-04",
         "No Utilizado",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2019-04",
         "No Utilizado",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Sitios RAN",
         "2019-04",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2019-04",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         1
        ],
        [
         "Infraestructura",
         "2019-03",
         "No Utilizado",
         "CORDOBA NORTE",
         "NOA",
         1
        ],
        [
         "Sitios RAN",
         "2019-03",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         26
        ],
        [
         "Sitios RAN",
         "2019-03",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         6
        ],
        [
         "Sitios RAN",
         "2019-03",
         "desdefinido",
         "INT",
         "LITORAL",
         20
        ],
        [
         "Sitios RAN",
         "2019-03",
         "desdefinido",
         "INT",
         "PATAGONIA",
         5
        ],
        [
         "Sitios RAN",
         "2019-03",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         6
        ],
        [
         "Sitios RAN",
         "2019-03",
         "desdefinido",
         "INT",
         "CENTRAL",
         12
        ],
        [
         "Sitios RAN",
         "2019-03",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         5
        ],
        [
         "Sitios RAN",
         "2019-03",
         "desdefinido",
         "INT",
         "CUYO",
         9
        ],
        [
         "Sitios RAN",
         "2019-03",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         12
        ],
        [
         "Sitios RAN",
         "2019-03",
         "desdefinido",
         "INT",
         "NOA",
         14
        ],
        [
         "Infraestructura",
         "2019-02",
         "No Utilizado",
         "NO DEFINIDO",
         "NEA",
         3
        ],
        [
         "Infraestructura",
         "2019-02",
         "A Restituir",
         "SAN JUAN",
         "CUYO",
         1
        ],
        [
         "Sitios RAN",
         "2019-02",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Infraestructura",
         "2019-01",
         "No Utilizado",
         "AMBA_SUR",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2019-01",
         "A Restituir",
         "LA CATOLICA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-12",
         "No Utilizado",
         "FCO SOLANO",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2018-11",
         "A Restituir",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-11",
         "A Restituir",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-11",
         "No Utilizado",
         "MENDOZA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-11",
         "No Utilizado",
         "NO DEFINIDA",
         "BUENOS AIRES",
         1
        ],
        [
         "Sitios RAN",
         "2018-11",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-10",
         "No Utilizado",
         "AMBA_NORTE",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-10",
         "No Utilizado",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Sitios RAN",
         "2018-10",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-09",
         "A Restituir",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-08",
         "No Utilizado",
         "AMBA_NORTE",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-08",
         "No Utilizado",
         "LOMAS DE ZAMORA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-08",
         "No Utilizado",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Sitios RAN",
         "2018-08",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         2
        ],
        [
         "Infraestructura",
         "2018-07",
         "No Utilizado",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "No Utilizado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "No Utilizado",
         "NO DEFINIDO",
         "NOA",
         2
        ],
        [
         "Infraestructura",
         "2018-07",
         "No Utilizado",
         "NO DEFINIDA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "No Utilizado",
         "NO DEFINIDO",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "No Utilizado",
         "AMBA_SUR",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2018-07",
         "No Utilizado",
         "AMBA_NORTE",
         "AMBA_NORTE",
         2
        ],
        [
         "Sitios RAN",
         "2018-07",
         "desdefinido",
         "INT",
         "LITORAL",
         2
        ],
        [
         "Sitios RAN",
         "2018-07",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         2
        ],
        [
         "Infraestructura",
         "2018-06",
         "No Utilizado",
         "AMBA_OESTE",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2018-06",
         "A Restituir",
         "ROSARIO_ZONA 04",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "No Utilizado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-06",
         "No Utilizado",
         "AMBA_SUR",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2018-06",
         "No Utilizado",
         "NEUQUEN",
         "CUYO",
         1
        ],
        [
         "Sitios RAN",
         "2018-06",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         2
        ],
        [
         "Sitios RAN",
         "2018-06",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2018-06",
         "desdefinido",
         "INT",
         "NOA",
         1
        ],
        [
         "Sitios RAN",
         "2018-06",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2018-06",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         2
        ],
        [
         "Sitios RAN",
         "2018-05",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         1
        ],
        [
         "Sitios RAN",
         "2018-05",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "No Utilizado",
         "NO DEFINIDO",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2018-04",
         "No Utilizado",
         "",
         "SUR",
         1
        ],
        [
         "Sitios RAN",
         "2018-04",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2018-03",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2018-03",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-02",
         "No Utilizado",
         "AMBA_NORTE",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-02",
         "A Restituir",
         "JUNCAL",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2018-02",
         "No Utilizado",
         "NO DEFINIDO",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2018-02",
         "No Utilizado",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2018-01",
         "No Utilizado",
         "NO DEFINIDA",
         "BUENOS AIRES",
         1
        ],
        [
         "Sitios RAN",
         "2018-01",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-12",
         "No Utilizado",
         "uruguay",
         "",
         1
        ],
        [
         "Infraestructura",
         "2017-12",
         "No Utilizado",
         "NO DEFINIDO",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-12",
         "No Utilizado",
         "NO DEFINIDO",
         "NOA",
         2
        ],
        [
         "Sitios RAN",
         "2017-12",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "9 DE JULIO",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "BARRACAS",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "CORRIENTES",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "ROSARIO SUR",
         "NEA",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "SANTA FE_ZONA 05",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "TUCUMAN",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "RAMOS MEJIA",
         "AMBA_OESTE",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "SANTA ROSA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "NO DEFINIDO",
         "BUENOS AIRES",
         290
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "NOA_CORDOBA_NORTE",
         "NOA",
         61
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         3
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "JUNCAL",
         "AMBA_NORTE",
         9
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "ROSARIO CENTRO",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "NO DEFINIDO",
         "NOA",
         160
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "NOA_SALTA",
         "NOA",
         14
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "MAR DEL PLATA",
         "BUENOS AIRES",
         3
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "BAHIA BLANCA",
         "SUR",
         3
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "CUYO_EXT",
         "CUYO",
         24
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "VIEDMA",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "GRAL ROCA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "LA CATOLICA",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "ENTRE RIOS_ZONA 03",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "RIO GALLEGOS",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "PATAGONIA",
         "SUR",
         93
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "TUCUMAN",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "ATLANTICA",
         "BUENOS AIRES",
         70
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "MENDOZA",
         "CUYO",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "MISIONES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "CATAMARCA",
         "NOA",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "SAN JUAN",
         "CUYO",
         3
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "GRAL PICO",
         "SUR",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "CORRIENTES_ZONA 01",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "AMBA_NORTE",
         "AMBA_NORTE",
         593
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "FCO SOLANO",
         "AMBA_SUR",
         4
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "CUYO",
         "AMBA_SUR",
         12
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "LOS POLVORINES",
         "AMBA_NORTE",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "NO DEFINIDO",
         "SUR",
         117
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "JUJUY",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "CHASCOMUS-DOLORES",
         "BUENOS AIRES",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "CORDOBA NORTE",
         "NOA",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "CNEL SUAREZ",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "FLORES",
         "AMBA_OESTE",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "MENDOZA",
         "CUYO",
         6
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "CHIVILCOY",
         "BUENOS AIRES",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "NEUQUEN",
         "CUYO",
         3
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "LUJAN",
         "AMBA_NORTE",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "VILLA MERCEDES",
         "CUYO",
         5
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "MISIONES_ZONA 02",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "JUNIN",
         "BUENOS AIRES",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "FLORES",
         "AMBA_OESTE",
         3
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "LA RIOJA",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "MAR DE AJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "FCO SOLANO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "PEHUAJO",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "LA PLATA",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "uruguay",
         "",
         6
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "NEA",
         "NEA",
         79
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "LA PLATA",
         "BUENOS AIRES",
         3
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "MAR DE AJO",
         "BUENOS AIRES",
         6
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "MISIONES_ZONA 04",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "CORDOBA NORTE",
         "NOA",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "SANTA FE_ZONA 05",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "LOS POLVORINES",
         "AMBA_NORTE",
         3
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "NEUQUEN",
         "CUYO",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "SALTA",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "NO DEFINIDA",
         "BUENOS AIRES",
         39
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "SALTA",
         "NOA",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "CUYO_MDZ",
         "CUYO",
         23
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "ENTRE RIOS_ZONA 01",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "BARRACAS",
         "AMBA_SUR",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "AMBA_OESTE",
         "AMBA_OESTE",
         303
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "MONTE CHINGOLO",
         "AMBA_SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "JUNCAL",
         "AMBA_NORTE",
         7
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "ANDINA",
         "SUR",
         38
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "LITORAL",
         "NEA",
         236
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "NOA_CORDOBA_SUR",
         "NOA",
         61
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "FORMOSA_ZONA 01",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "AMBA_SUR",
         "AMBA_SUR",
         290
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "CORRIENTES_ZONA 02",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "NO DEFINIDO",
         "CUYO",
         44
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "MERLO",
         "AMBA_OESTE",
         3
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "ENTRE RIOS_ZONA 02",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "COMODORO",
         "SUR",
         6
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "",
         "",
         749
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "PERGAMINO",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "A Restituir",
         "CUYO",
         "AMBA_SUR",
         10
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "NOA_TUCUMAN",
         "NOA",
         17
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "NO DEFINIDO",
         "NEA",
         266
        ],
        [
         "Sitios RAN",
         "2017-11",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "",
         "AMBA_SUR",
         19
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "",
         "AMBA_NORTE",
         32
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "ZAPALA",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "CORDOBA SUR",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "CHACO_ZONA 01",
         "NEA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "CENTRO",
         "NEA",
         2
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "",
         "CUYO",
         11
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "BARILOCHE",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "",
         "AMBA_OESTE",
         15
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "",
         "SUR",
         25
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "TRELEW",
         "SUR",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "SANTIAGO DEL ESTERO",
         "NOA",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "CENTRO",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "CHIVILCOY",
         "BUENOS AIRES",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "",
         "NOA",
         13
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "",
         "NEA",
         28
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "SAN RAFAEL",
         "CUYO",
         1
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "",
         "BUENOS AIRES",
         16
        ],
        [
         "Infraestructura",
         "2017-11",
         "No Utilizado",
         "ESQUEL",
         "SUR",
         1
        ],
        [
         "Sitios RAN",
         "2017-09",
         "desdefinido",
         "INT",
         "NOA",
         2
        ],
        [
         "Sitios RAN",
         "2017-09",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         1
        ],
        [
         "Sitios RAN",
         "2017-09",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         1
        ],
        [
         "Sitios RAN",
         "2017-09",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         1
        ],
        [
         "Sitios RAN",
         "2017-09",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2017-08",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         3
        ],
        [
         "Sitios RAN",
         "2017-08",
         "desdefinido",
         "INT",
         "CUYO",
         1
        ],
        [
         "Sitios RAN",
         "2017-06",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2017-06",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         2
        ],
        [
         "Sitios RAN",
         "2017-06",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2017-05",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2017-05",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         1
        ],
        [
         "Sitios RAN",
         "2017-04",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2017-03",
         "desdefinido",
         "INT",
         "NOA",
         1
        ],
        [
         "Sitios RAN",
         "2017-03",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         5
        ],
        [
         "Sitios RAN",
         "2017-03",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2017-03",
         "desdefinido",
         "INT",
         "CUYO",
         4
        ],
        [
         "Sitios RAN",
         "2017-03",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         2
        ],
        [
         "Sitios RAN",
         "2017-03",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         2
        ],
        [
         "Sitios RAN",
         "2017-03",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         1
        ],
        [
         "Sitios RAN",
         "2017-03",
         "desdefinido",
         "INT",
         "PATAGONIA",
         1
        ],
        [
         "Sitios RAN",
         "2017-02",
         "desdefinido",
         "INT",
         "NOA",
         1
        ],
        [
         "Sitios RAN",
         "2017-02",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2017-01",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         1
        ],
        [
         "Sitios RAN",
         "2017-01",
         "desdefinido",
         "INT",
         "LITORAL",
         1
        ],
        [
         "Sitios RAN",
         "2016-09",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         2
        ],
        [
         "Sitios RAN",
         "2016-09",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         3
        ],
        [
         "Sitios RAN",
         "2016-09",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         2
        ],
        [
         "Sitios RAN",
         "2016-09",
         "desdefinido",
         "INT",
         "LITORAL",
         1
        ],
        [
         "Sitios RAN",
         "2016-09",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         2
        ],
        [
         "Sitios RAN",
         "2016-09",
         "desdefinido",
         "INT",
         "PATAGONIA",
         1
        ],
        [
         "Sitios RAN",
         "2016-09",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2016-08",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2016-08",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         8
        ],
        [
         "Sitios RAN",
         "2016-08",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         1
        ],
        [
         "Sitios RAN",
         "2016-08",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2016-08",
         "desdefinido",
         "INT",
         "CUYO",
         2
        ],
        [
         "Sitios RAN",
         "2016-07",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         1
        ],
        [
         "Sitios RAN",
         "2016-07",
         "desdefinido",
         "INT",
         "PATAGONIA",
         1
        ],
        [
         "Sitios RAN",
         "2016-07",
         "desdefinido",
         "INT",
         "LITORAL",
         2
        ],
        [
         "Sitios RAN",
         "2016-07",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2016-06",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         6
        ],
        [
         "Sitios RAN",
         "2016-06",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         6
        ],
        [
         "Sitios RAN",
         "2016-06",
         "desdefinido",
         "INT",
         "PATAGONIA",
         6
        ],
        [
         "Sitios RAN",
         "2016-06",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         14
        ],
        [
         "Sitios RAN",
         "2016-06",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         3
        ],
        [
         "Sitios RAN",
         "2016-06",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         5
        ],
        [
         "Sitios RAN",
         "2016-06",
         "desdefinido",
         "INT",
         "NOA",
         6
        ],
        [
         "Sitios RAN",
         "2016-06",
         "desdefinido",
         "INT",
         "CUYO",
         5
        ],
        [
         "Sitios RAN",
         "2016-06",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         3
        ],
        [
         "Sitios RAN",
         "2016-06",
         "desdefinido",
         "INT",
         "LITORAL",
         8
        ],
        [
         "Sitios RAN",
         "2016-06",
         "desdefinido",
         "INT",
         "CENTRAL",
         6
        ],
        [
         "Sitios RAN",
         "2016-05",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         1
        ],
        [
         "Sitios RAN",
         "2016-05",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2016-04",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         7
        ],
        [
         "Sitios RAN",
         "2016-04",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         3
        ],
        [
         "Sitios RAN",
         "2016-04",
         "desdefinido",
         "INT",
         "LITORAL",
         4
        ],
        [
         "Sitios RAN",
         "2016-04",
         "desdefinido",
         "INT",
         "PATAGONIA",
         1
        ],
        [
         "Sitios RAN",
         "2016-04",
         "desdefinido",
         "INT",
         "CUYO",
         1
        ],
        [
         "Sitios RAN",
         "2016-03",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         1
        ],
        [
         "Sitios RAN",
         "2016-02",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         3
        ],
        [
         "Sitios RAN",
         "2016-02",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2016-02",
         "desdefinido",
         "INT",
         "NOA",
         2
        ],
        [
         "Sitios RAN",
         "2016-02",
         "desdefinido",
         "INT",
         "LITORAL",
         1
        ],
        [
         "Sitios RAN",
         "2016-02",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         3
        ],
        [
         "Sitios RAN",
         "2016-02",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         2
        ],
        [
         "Sitios RAN",
         "2016-02",
         "desdefinido",
         "INT",
         "CUYO",
         3
        ],
        [
         "Sitios RAN",
         "2016-01",
         "desdefinido",
         "INT",
         "CUYO",
         1
        ],
        [
         "Sitios RAN",
         "2016-01",
         "desdefinido",
         "INT",
         "NOA",
         2
        ],
        [
         "Sitios RAN",
         "2015-12",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2015-12",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2015-12",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         1
        ],
        [
         "Sitios RAN",
         "2015-12",
         "desdefinido",
         "INT",
         "CUYO",
         1
        ],
        [
         "Sitios RAN",
         "2015-12",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         1
        ],
        [
         "Sitios RAN",
         "2015-11",
         "desdefinido",
         "INT",
         "CUYO",
         1
        ],
        [
         "Sitios RAN",
         "2015-11",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2015-11",
         "desdefinido",
         "INT",
         "LITORAL",
         1
        ],
        [
         "Sitios RAN",
         "2015-11",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         1
        ],
        [
         "Sitios RAN",
         "2015-10",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         2
        ],
        [
         "Sitios RAN",
         "2015-10",
         "desdefinido",
         "INT",
         "PATAGONIA",
         1
        ],
        [
         "Sitios RAN",
         "2015-09",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         4
        ],
        [
         "Sitios RAN",
         "2015-09",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         2
        ],
        [
         "Sitios RAN",
         "2015-09",
         "desdefinido",
         "INT",
         "CUYO",
         1
        ],
        [
         "Sitios RAN",
         "2015-08",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         1
        ],
        [
         "Sitios RAN",
         "2015-08",
         "desdefinido",
         "INT",
         "NOA",
         2
        ],
        [
         "Sitios RAN",
         "2015-08",
         "desdefinido",
         "INT",
         "PATAGONIA",
         2
        ],
        [
         "Sitios RAN",
         "2015-08",
         "desdefinido",
         "INT",
         "CUYO",
         1
        ],
        [
         "Sitios RAN",
         "2015-08",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         1
        ],
        [
         "Sitios RAN",
         "2015-08",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         11
        ],
        [
         "Sitios RAN",
         "2015-08",
         "desdefinido",
         "INT",
         "LITORAL",
         1
        ],
        [
         "Sitios RAN",
         "2015-08",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2015-07",
         "desdefinido",
         "INT",
         "LITORAL",
         78
        ],
        [
         "Sitios RAN",
         "2015-07",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         41
        ],
        [
         "Sitios RAN",
         "2015-07",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         28
        ],
        [
         "Sitios RAN",
         "2015-07",
         "desdefinido",
         "INT",
         "PATAGONIA",
         44
        ],
        [
         "Sitios RAN",
         "2015-07",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         40
        ],
        [
         "Sitios RAN",
         "2015-07",
         "desdefinido",
         "AMBA",
         "SUBTE",
         3
        ],
        [
         "Sitios RAN",
         "2015-07",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         32
        ],
        [
         "Sitios RAN",
         "2015-07",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         27
        ],
        [
         "Sitios RAN",
         "2015-07",
         "desdefinido",
         "INT",
         "NOA",
         41
        ],
        [
         "Sitios RAN",
         "2015-07",
         "desdefinido",
         "INT",
         "CUYO",
         37
        ],
        [
         "Sitios RAN",
         "2015-07",
         "desdefinido",
         "INT",
         "CENTRAL",
         38
        ],
        [
         "Sitios RAN",
         "2015-07",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         21
        ],
        [
         "Sitios RAN",
         "2015-06",
         "desdefinido",
         "INT",
         "PATAGONIA",
         2
        ],
        [
         "Sitios RAN",
         "2015-06",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2015-06",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2015-05",
         "desdefinido",
         "INT",
         "NOA",
         1
        ],
        [
         "Sitios RAN",
         "2015-04",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         1
        ],
        [
         "Sitios RAN",
         "2015-03",
         "desdefinido",
         "INT",
         "CUYO",
         3
        ],
        [
         "Sitios RAN",
         "2015-01",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2014-11",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         1
        ],
        [
         "Sitios RAN",
         "2014-10",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2014-09",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         17
        ],
        [
         "Sitios RAN",
         "2014-09",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         1
        ],
        [
         "Sitios RAN",
         "2014-08",
         "desdefinido",
         "INT",
         "PATAGONIA",
         28
        ],
        [
         "Sitios RAN",
         "2014-08",
         "desdefinido",
         "INT",
         "LITORAL",
         4
        ],
        [
         "Sitios RAN",
         "2014-08",
         "desdefinido",
         "INT",
         "NOA",
         14
        ],
        [
         "Sitios RAN",
         "2014-08",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2014-07",
         "desdefinido",
         "AMBA",
         "CAPITAL MACROMICROCENTRO/LA PLATA",
         2
        ],
        [
         "Sitios RAN",
         "2014-07",
         "desdefinido",
         "AMBA",
         "AMBA NORTE",
         2
        ],
        [
         "Sitios RAN",
         "2014-07",
         "desdefinido",
         "AMBA",
         "CAPITAL FEDERAL",
         1
        ],
        [
         "Sitios RAN",
         "2014-07",
         "desdefinido",
         "AMBA",
         "AMBA SUR",
         3
        ],
        [
         "Sitios RAN",
         "2014-07",
         "desdefinido",
         "INT",
         "CENTRAL",
         1
        ],
        [
         "Sitios RAN",
         "2014-07",
         "desdefinido",
         "AMBA",
         "AMBA OESTE",
         1
        ],
        [
         "Sitios RAN",
         "2014-06",
         "desdefinido",
         "INT",
         "NOA",
         1
        ],
        [
         "Sitios RAN",
         "2014-06",
         "desdefinido",
         "INT",
         "ATLÁNTICA",
         1
        ],
        [
         "Sitios RAN",
         "2014-05",
         "desdefinido",
         "INT",
         "CUYO",
         1
        ],
        [
         "Sitios RAN",
         "2014-05",
         "desdefinido",
         "INT",
         "PATAGONIA",
         1
        ],
        [
         "Sitios RAN",
         "2014-04",
         "desdefinido",
         "INT",
         "CUYO",
         4
        ]
       ],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "modulo",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "yearMonth",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "estado",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "areaOYM",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "cabeceraOYM",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "emplazamientos",
            "nullable": false,
            "type": "long"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 20
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "modulo",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "yearMonth",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "estado",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "areaOYM",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "cabeceraOYM",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "emplazamientos",
         "type": "\"long\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "SELECT\n",
    "  modulo,\n",
    "  date_format(fecha_modificacion, 'yyyy-MM') AS yearMonth,\n",
    "  estado,\n",
    "  areaOYM,\n",
    "  cabeceraOYM,\n",
    "  COUNT(*) AS emplazamientos\n",
    "FROM silver_emplazamientos_modulo\n",
    "WHERE estado in ('desdefinido','No Utilizado','A Restituir')\n",
    "GROUP BY modulo, yearMonth, estado, areaOYM, cabeceraOYM\n",
    "ORDER BY yearMonth DESC\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "302b19b2-42a5-4329-80a9-5ff8823d89b5",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "8. Disponibilidad y Estado de la Red Operativo (Por Area OyM)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "1371d724-448a-4acf-94eb-74bdc86ce9ac",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>areaOYM</th><th>emplazamientos_totales</th><th>emplaz_disponibles</th><th>emplaz_disp_p</th></tr></thead><tbody><tr><td>ROSARIO_ZONA 06</td><td>16</td><td>16</td><td>100.00</td></tr><tr><td>PASO DE LOS LIBRES</td><td>2</td><td>2</td><td>100.00</td></tr><tr><td>SANTA FE_ZONA 04</td><td>20</td><td>20</td><td>100.00</td></tr><tr><td>CORRIENTES_ZONA 03</td><td>6</td><td>6</td><td>100.00</td></tr><tr><td>CHACO_ZONA 02</td><td>15</td><td>15</td><td>100.00</td></tr><tr><td>FORMOSA_ZONA 02</td><td>5</td><td>5</td><td>100.00</td></tr><tr><td>NECOCHEA</td><td>50</td><td>50</td><td>100.00</td></tr><tr><td>SAN FRANCISCO</td><td>31</td><td>31</td><td>100.00</td></tr><tr><td>MISIONES_ZONA 03</td><td>5</td><td>5</td><td>100.00</td></tr><tr><td>GLEW</td><td>1</td><td>1</td><td>100.00</td></tr><tr><td>GUALEGUAYCHU</td><td>3</td><td>3</td><td>100.00</td></tr><tr><td>VENADO TUERTO</td><td>1</td><td>1</td><td>100.00</td></tr><tr><td>SUR</td><td>1</td><td>1</td><td>100.00</td></tr><tr><td>AMBA SUR NAVIAX</td><td>1</td><td>1</td><td>100.00</td></tr><tr><td>SANTA ROSA</td><td>129</td><td>128</td><td>99.22</td></tr><tr><td>SANTA FE_ZONA 03</td><td>116</td><td>115</td><td>99.14</td></tr><tr><td>ROSARIO_ZONA 01</td><td>111</td><td>110</td><td>99.10</td></tr><tr><td>GRAL ROCA</td><td>80</td><td>79</td><td>98.75</td></tr><tr><td>OLAVARRIA</td><td>74</td><td>73</td><td>98.65</td></tr><tr><td>SANTA FE_ZONA 02</td><td>66</td><td>65</td><td>98.48</td></tr><tr><td>ROSARIO_ZONA 03</td><td>62</td><td>61</td><td>98.39</td></tr><tr><td>JUNIN</td><td>120</td><td>118</td><td>98.33</td></tr><tr><td>SANTA FE_ZONA 01</td><td>58</td><td>57</td><td>98.28</td></tr><tr><td>ZAPALA</td><td>98</td><td>96</td><td>97.96</td></tr><tr><td>LUJAN</td><td>569</td><td>555</td><td>97.54</td></tr><tr><td>GRAL PICO</td><td>80</td><td>78</td><td>97.50</td></tr><tr><td>ENTRE RIOS_ZONA 03</td><td>40</td><td>39</td><td>97.50</td></tr><tr><td>TUCUMAN</td><td>269</td><td>262</td><td>97.40</td></tr><tr><td>LOS POLVORINES</td><td>786</td><td>764</td><td>97.20</td></tr><tr><td>RIO GRANDE - TIERRA DEL FUEGO</td><td>35</td><td>34</td><td>97.14</td></tr><tr><td>CORDOBA SUR</td><td>311</td><td>302</td><td>97.11</td></tr><tr><td>JUJUY</td><td>103</td><td>100</td><td>97.09</td></tr><tr><td>BARILOCHE</td><td>199</td><td>193</td><td>96.98</td></tr><tr><td>VIEDMA</td><td>131</td><td>127</td><td>96.95</td></tr><tr><td>RAMOS MEJIA</td><td>753</td><td>728</td><td>96.68</td></tr><tr><td>MONTE CHINGOLO</td><td>525</td><td>507</td><td>96.57</td></tr><tr><td>ROSARIO_ZONA 02</td><td>116</td><td>112</td><td>96.55</td></tr><tr><td>TRES ARROYOS</td><td>57</td><td>55</td><td>96.49</td></tr><tr><td>CHIVILCOY</td><td>138</td><td>133</td><td>96.38</td></tr><tr><td>ROSARIO_ZONA 04</td><td>133</td><td>128</td><td>96.24</td></tr><tr><td>TANDIL</td><td>77</td><td>74</td><td>96.10</td></tr><tr><td>CHACO_ZONA 01</td><td>75</td><td>72</td><td>96.00</td></tr><tr><td>MERLO</td><td>553</td><td>530</td><td>95.84</td></tr><tr><td>TRELEW</td><td>144</td><td>138</td><td>95.83</td></tr><tr><td>RIO CUARTO</td><td>114</td><td>109</td><td>95.61</td></tr><tr><td>BARRACAS</td><td>451</td><td>431</td><td>95.57</td></tr><tr><td>PEHUAJO</td><td>45</td><td>43</td><td>95.56</td></tr><tr><td>BAHIA BLANCA</td><td>268</td><td>256</td><td>95.52</td></tr><tr><td>9 DE JULIO</td><td>66</td><td>63</td><td>95.45</td></tr><tr><td>FLORES</td><td>659</td><td>628</td><td>95.30</td></tr><tr><td>USHUAIA - TIERRA DEL FUEGO</td><td>63</td><td>60</td><td>95.24</td></tr><tr><td>COMODORO</td><td>189</td><td>180</td><td>95.24</td></tr><tr><td>LOMAS DE ZAMORA</td><td>581</td><td>552</td><td>95.01</td></tr><tr><td>CORRIENTES_ZONA 04</td><td>20</td><td>19</td><td>95.00</td></tr><tr><td>SAN LUIS</td><td>140</td><td>133</td><td>95.00</td></tr><tr><td>PERGAMINO</td><td>100</td><td>95</td><td>95.00</td></tr><tr><td>SANTIAGO DEL ESTERO</td><td>139</td><td>132</td><td>94.96</td></tr><tr><td>CORRIENTES_ZONA 01</td><td>77</td><td>73</td><td>94.81</td></tr><tr><td>ROSARIO_ZONA 05</td><td>37</td><td>35</td><td>94.59</td></tr><tr><td>TRENQUE LAUQUEN</td><td>55</td><td>52</td><td>94.55</td></tr><tr><td>LA RIOJA</td><td>72</td><td>68</td><td>94.44</td></tr><tr><td>RIO GALLEGOS</td><td>125</td><td>118</td><td>94.40</td></tr><tr><td>FORMOSA_ZONA 01</td><td>34</td><td>32</td><td>94.12</td></tr><tr><td>MAR DEL PLATA</td><td>576</td><td>542</td><td>94.10</td></tr><tr><td>ESQUEL</td><td>84</td><td>79</td><td>94.05</td></tr><tr><td>CNEL SUAREZ</td><td>66</td><td>62</td><td>93.94</td></tr><tr><td>FCO SOLANO</td><td>1004</td><td>938</td><td>93.43</td></tr><tr><td>SALTA</td><td>224</td><td>209</td><td>93.30</td></tr><tr><td>CATAMARCA</td><td>104</td><td>97</td><td>93.27</td></tr><tr><td>CHASCOMUS-DOLORES</td><td>70</td><td>65</td><td>92.86</td></tr><tr><td>CORDOBA NORTE</td><td>450</td><td>417</td><td>92.67</td></tr><tr><td>JUNCAL</td><td>774</td><td>717</td><td>92.64</td></tr><tr><td>MAR DE AJO</td><td>480</td><td>444</td><td>92.50</td></tr><tr><td>SAN JUAN</td><td>329</td><td>304</td><td>92.40</td></tr><tr><td>SAN RAFAEL</td><td>144</td><td>133</td><td>92.36</td></tr><tr><td>MISIONES</td><td>13</td><td>12</td><td>92.31</td></tr><tr><td>GLEW-CANUELAS</td><td>169</td><td>155</td><td>91.72</td></tr><tr><td>NEUQUEN</td><td>318</td><td>291</td><td>91.51</td></tr><tr><td>MISIONES_ZONA 01</td><td>80</td><td>73</td><td>91.25</td></tr><tr><td>MENDOZA</td><td>671</td><td>612</td><td>91.21</td></tr><tr><td>ENTRE RIOS_ZONA 01</td><td>34</td><td>31</td><td>91.18</td></tr><tr><td>ENTRE RIOS_ZONA 02</td><td>33</td><td>30</td><td>90.91</td></tr><tr><td>MISIONES_ZONA 02</td><td>22</td><td>20</td><td>90.91</td></tr><tr><td>VILLA MERCEDES</td><td>91</td><td>82</td><td>90.11</td></tr><tr><td>JUNIN DE LOS ANDES</td><td>10</td><td>9</td><td>90.00</td></tr><tr><td>LA PLATA</td><td>530</td><td>476</td><td>89.81</td></tr><tr><td>CUYO</td><td>913</td><td>813</td><td>89.05</td></tr><tr><td>SAN JUSTO</td><td>32</td><td>28</td><td>87.50</td></tr><tr><td>CORRIENTES_ZONA 02</td><td>20</td><td>17</td><td>85.00</td></tr><tr><td>MISIONES_ZONA 04</td><td>25</td><td>21</td><td>84.00</td></tr><tr><td>FORMOSA</td><td>5</td><td>4</td><td>80.00</td></tr><tr><td>CORRIENTES</td><td>30</td><td>24</td><td>80.00</td></tr><tr><td>CHACO</td><td>20</td><td>16</td><td>80.00</td></tr><tr><td>SANTA CRUZ</td><td>5</td><td>4</td><td>80.00</td></tr><tr><td>ROSARIO NORTE</td><td>50</td><td>39</td><td>78.00</td></tr><tr><td>SANTA FE_ZONA 05</td><td>17</td><td>13</td><td>76.47</td></tr><tr><td>BONIFACIO</td><td>4</td><td>3</td><td>75.00</td></tr><tr><td>INT</td><td>6896</td><td>5092</td><td>73.84</td></tr><tr><td>ROSARIO SUR</td><td>19</td><td>14</td><td>73.68</td></tr><tr><td>LA CATOLICA</td><td>26</td><td>19</td><td>73.08</td></tr><tr><td>ROSARIO CENTRO</td><td>59</td><td>43</td><td>72.88</td></tr><tr><td>AMBA</td><td>5164</td><td>3590</td><td>69.52</td></tr><tr><td>ENTRE RIOS</td><td>25</td><td>17</td><td>68.00</td></tr><tr><td>ANASCO</td><td>5</td><td>3</td><td>60.00</td></tr><tr><td>CENTRO</td><td>19</td><td>11</td><td>57.89</td></tr><tr><td>ESTE</td><td>7</td><td>4</td><td>57.14</td></tr><tr><td></td><td>2218</td><td>1154</td><td>52.03</td></tr><tr><td>NOA_CORDOBA_SUR</td><td>113</td><td>32</td><td>28.32</td></tr><tr><td>CUYO_EXT</td><td>48</td><td>11</td><td>22.92</td></tr><tr><td>NOA_SALTA</td><td>28</td><td>6</td><td>21.43</td></tr><tr><td>NO DEFINIDO</td><td>1406</td><td>279</td><td>19.84</td></tr><tr><td>ANDINA</td><td>66</td><td>13</td><td>19.70</td></tr><tr><td>PATAGONIA</td><td>160</td><td>31</td><td>19.38</td></tr><tr><td>CUYO_MDZ</td><td>53</td><td>10</td><td>18.87</td></tr><tr><td>NOA_TUCUMAN</td><td>38</td><td>6</td><td>15.79</td></tr><tr><td>NOA_CORDOBA_NORTE</td><td>104</td><td>16</td><td>15.38</td></tr><tr><td>AMBA_SUR</td><td>457</td><td>52</td><td>11.38</td></tr><tr><td>NO DEFINIDA</td><td>177</td><td>19</td><td>10.73</td></tr><tr><td>AMBA_NORTE</td><td>777</td><td>73</td><td>9.40</td></tr><tr><td>AMBA_OESTE</td><td>425</td><td>39</td><td>9.18</td></tr><tr><td>ATLANTICA</td><td>71</td><td>1</td><td>1.41</td></tr><tr><td>NEA</td><td>90</td><td>0</td><td>0.00</td></tr><tr><td>uruguay</td><td>8</td><td>0</td><td>0.00</td></tr><tr><td>LITORAL</td><td>249</td><td>0</td><td>0.00</td></tr></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [
        [
         "ROSARIO_ZONA 06",
         16,
         16,
         "100.00"
        ],
        [
         "PASO DE LOS LIBRES",
         2,
         2,
         "100.00"
        ],
        [
         "SANTA FE_ZONA 04",
         20,
         20,
         "100.00"
        ],
        [
         "CORRIENTES_ZONA 03",
         6,
         6,
         "100.00"
        ],
        [
         "CHACO_ZONA 02",
         15,
         15,
         "100.00"
        ],
        [
         "FORMOSA_ZONA 02",
         5,
         5,
         "100.00"
        ],
        [
         "NECOCHEA",
         50,
         50,
         "100.00"
        ],
        [
         "SAN FRANCISCO",
         31,
         31,
         "100.00"
        ],
        [
         "MISIONES_ZONA 03",
         5,
         5,
         "100.00"
        ],
        [
         "GLEW",
         1,
         1,
         "100.00"
        ],
        [
         "GUALEGUAYCHU",
         3,
         3,
         "100.00"
        ],
        [
         "VENADO TUERTO",
         1,
         1,
         "100.00"
        ],
        [
         "SUR",
         1,
         1,
         "100.00"
        ],
        [
         "AMBA SUR NAVIAX",
         1,
         1,
         "100.00"
        ],
        [
         "SANTA ROSA",
         129,
         128,
         "99.22"
        ],
        [
         "SANTA FE_ZONA 03",
         116,
         115,
         "99.14"
        ],
        [
         "ROSARIO_ZONA 01",
         111,
         110,
         "99.10"
        ],
        [
         "GRAL ROCA",
         80,
         79,
         "98.75"
        ],
        [
         "OLAVARRIA",
         74,
         73,
         "98.65"
        ],
        [
         "SANTA FE_ZONA 02",
         66,
         65,
         "98.48"
        ],
        [
         "ROSARIO_ZONA 03",
         62,
         61,
         "98.39"
        ],
        [
         "JUNIN",
         120,
         118,
         "98.33"
        ],
        [
         "SANTA FE_ZONA 01",
         58,
         57,
         "98.28"
        ],
        [
         "ZAPALA",
         98,
         96,
         "97.96"
        ],
        [
         "LUJAN",
         569,
         555,
         "97.54"
        ],
        [
         "GRAL PICO",
         80,
         78,
         "97.50"
        ],
        [
         "ENTRE RIOS_ZONA 03",
         40,
         39,
         "97.50"
        ],
        [
         "TUCUMAN",
         269,
         262,
         "97.40"
        ],
        [
         "LOS POLVORINES",
         786,
         764,
         "97.20"
        ],
        [
         "RIO GRANDE - TIERRA DEL FUEGO",
         35,
         34,
         "97.14"
        ],
        [
         "CORDOBA SUR",
         311,
         302,
         "97.11"
        ],
        [
         "JUJUY",
         103,
         100,
         "97.09"
        ],
        [
         "BARILOCHE",
         199,
         193,
         "96.98"
        ],
        [
         "VIEDMA",
         131,
         127,
         "96.95"
        ],
        [
         "RAMOS MEJIA",
         753,
         728,
         "96.68"
        ],
        [
         "MONTE CHINGOLO",
         525,
         507,
         "96.57"
        ],
        [
         "ROSARIO_ZONA 02",
         116,
         112,
         "96.55"
        ],
        [
         "TRES ARROYOS",
         57,
         55,
         "96.49"
        ],
        [
         "CHIVILCOY",
         138,
         133,
         "96.38"
        ],
        [
         "ROSARIO_ZONA 04",
         133,
         128,
         "96.24"
        ],
        [
         "TANDIL",
         77,
         74,
         "96.10"
        ],
        [
         "CHACO_ZONA 01",
         75,
         72,
         "96.00"
        ],
        [
         "MERLO",
         553,
         530,
         "95.84"
        ],
        [
         "TRELEW",
         144,
         138,
         "95.83"
        ],
        [
         "RIO CUARTO",
         114,
         109,
         "95.61"
        ],
        [
         "BARRACAS",
         451,
         431,
         "95.57"
        ],
        [
         "PEHUAJO",
         45,
         43,
         "95.56"
        ],
        [
         "BAHIA BLANCA",
         268,
         256,
         "95.52"
        ],
        [
         "9 DE JULIO",
         66,
         63,
         "95.45"
        ],
        [
         "FLORES",
         659,
         628,
         "95.30"
        ],
        [
         "USHUAIA - TIERRA DEL FUEGO",
         63,
         60,
         "95.24"
        ],
        [
         "COMODORO",
         189,
         180,
         "95.24"
        ],
        [
         "LOMAS DE ZAMORA",
         581,
         552,
         "95.01"
        ],
        [
         "CORRIENTES_ZONA 04",
         20,
         19,
         "95.00"
        ],
        [
         "SAN LUIS",
         140,
         133,
         "95.00"
        ],
        [
         "PERGAMINO",
         100,
         95,
         "95.00"
        ],
        [
         "SANTIAGO DEL ESTERO",
         139,
         132,
         "94.96"
        ],
        [
         "CORRIENTES_ZONA 01",
         77,
         73,
         "94.81"
        ],
        [
         "ROSARIO_ZONA 05",
         37,
         35,
         "94.59"
        ],
        [
         "TRENQUE LAUQUEN",
         55,
         52,
         "94.55"
        ],
        [
         "LA RIOJA",
         72,
         68,
         "94.44"
        ],
        [
         "RIO GALLEGOS",
         125,
         118,
         "94.40"
        ],
        [
         "FORMOSA_ZONA 01",
         34,
         32,
         "94.12"
        ],
        [
         "MAR DEL PLATA",
         576,
         542,
         "94.10"
        ],
        [
         "ESQUEL",
         84,
         79,
         "94.05"
        ],
        [
         "CNEL SUAREZ",
         66,
         62,
         "93.94"
        ],
        [
         "FCO SOLANO",
         1004,
         938,
         "93.43"
        ],
        [
         "SALTA",
         224,
         209,
         "93.30"
        ],
        [
         "CATAMARCA",
         104,
         97,
         "93.27"
        ],
        [
         "CHASCOMUS-DOLORES",
         70,
         65,
         "92.86"
        ],
        [
         "CORDOBA NORTE",
         450,
         417,
         "92.67"
        ],
        [
         "JUNCAL",
         774,
         717,
         "92.64"
        ],
        [
         "MAR DE AJO",
         480,
         444,
         "92.50"
        ],
        [
         "SAN JUAN",
         329,
         304,
         "92.40"
        ],
        [
         "SAN RAFAEL",
         144,
         133,
         "92.36"
        ],
        [
         "MISIONES",
         13,
         12,
         "92.31"
        ],
        [
         "GLEW-CANUELAS",
         169,
         155,
         "91.72"
        ],
        [
         "NEUQUEN",
         318,
         291,
         "91.51"
        ],
        [
         "MISIONES_ZONA 01",
         80,
         73,
         "91.25"
        ],
        [
         "MENDOZA",
         671,
         612,
         "91.21"
        ],
        [
         "ENTRE RIOS_ZONA 01",
         34,
         31,
         "91.18"
        ],
        [
         "ENTRE RIOS_ZONA 02",
         33,
         30,
         "90.91"
        ],
        [
         "MISIONES_ZONA 02",
         22,
         20,
         "90.91"
        ],
        [
         "VILLA MERCEDES",
         91,
         82,
         "90.11"
        ],
        [
         "JUNIN DE LOS ANDES",
         10,
         9,
         "90.00"
        ],
        [
         "LA PLATA",
         530,
         476,
         "89.81"
        ],
        [
         "CUYO",
         913,
         813,
         "89.05"
        ],
        [
         "SAN JUSTO",
         32,
         28,
         "87.50"
        ],
        [
         "CORRIENTES_ZONA 02",
         20,
         17,
         "85.00"
        ],
        [
         "MISIONES_ZONA 04",
         25,
         21,
         "84.00"
        ],
        [
         "FORMOSA",
         5,
         4,
         "80.00"
        ],
        [
         "CORRIENTES",
         30,
         24,
         "80.00"
        ],
        [
         "CHACO",
         20,
         16,
         "80.00"
        ],
        [
         "SANTA CRUZ",
         5,
         4,
         "80.00"
        ],
        [
         "ROSARIO NORTE",
         50,
         39,
         "78.00"
        ],
        [
         "SANTA FE_ZONA 05",
         17,
         13,
         "76.47"
        ],
        [
         "BONIFACIO",
         4,
         3,
         "75.00"
        ],
        [
         "INT",
         6896,
         5092,
         "73.84"
        ],
        [
         "ROSARIO SUR",
         19,
         14,
         "73.68"
        ],
        [
         "LA CATOLICA",
         26,
         19,
         "73.08"
        ],
        [
         "ROSARIO CENTRO",
         59,
         43,
         "72.88"
        ],
        [
         "AMBA",
         5164,
         3590,
         "69.52"
        ],
        [
         "ENTRE RIOS",
         25,
         17,
         "68.00"
        ],
        [
         "ANASCO",
         5,
         3,
         "60.00"
        ],
        [
         "CENTRO",
         19,
         11,
         "57.89"
        ],
        [
         "ESTE",
         7,
         4,
         "57.14"
        ],
        [
         "",
         2218,
         1154,
         "52.03"
        ],
        [
         "NOA_CORDOBA_SUR",
         113,
         32,
         "28.32"
        ],
        [
         "CUYO_EXT",
         48,
         11,
         "22.92"
        ],
        [
         "NOA_SALTA",
         28,
         6,
         "21.43"
        ],
        [
         "NO DEFINIDO",
         1406,
         279,
         "19.84"
        ],
        [
         "ANDINA",
         66,
         13,
         "19.70"
        ],
        [
         "PATAGONIA",
         160,
         31,
         "19.38"
        ],
        [
         "CUYO_MDZ",
         53,
         10,
         "18.87"
        ],
        [
         "NOA_TUCUMAN",
         38,
         6,
         "15.79"
        ],
        [
         "NOA_CORDOBA_NORTE",
         104,
         16,
         "15.38"
        ],
        [
         "AMBA_SUR",
         457,
         52,
         "11.38"
        ],
        [
         "NO DEFINIDA",
         177,
         19,
         "10.73"
        ],
        [
         "AMBA_NORTE",
         777,
         73,
         "9.40"
        ],
        [
         "AMBA_OESTE",
         425,
         39,
         "9.18"
        ],
        [
         "ATLANTICA",
         71,
         1,
         "1.41"
        ],
        [
         "NEA",
         90,
         0,
         "0.00"
        ],
        [
         "uruguay",
         8,
         0,
         "0.00"
        ],
        [
         "LITORAL",
         249,
         0,
         "0.00"
        ]
       ],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "areaOYM",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "emplazamientos_totales",
            "nullable": false,
            "type": "long"
           },
           {
            "metadata": {},
            "name": "emplaz_disponibles",
            "nullable": true,
            "type": "long"
           },
           {
            "metadata": {},
            "name": "emplaz_disp_p",
            "nullable": true,
            "type": "decimal(27,2)"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 21
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "areaOYM",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "emplazamientos_totales",
         "type": "\"long\""
        },
        {
         "metadata": "{}",
         "name": "emplaz_disponibles",
         "type": "\"long\""
        },
        {
         "metadata": "{}",
         "name": "emplaz_disp_p",
         "type": "\"decimal(27,2)\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "SELECT \n",
    "    areaOYM,\n",
    "    COUNT(*) as emplazamientos_totales,\n",
    "    --SUM(CASE WHEN estado in ('activo','Liberado') THEN 1 ELSE 0 END) as activos,\n",
    "    ROUND(SUM(CASE WHEN estado in ('activo','Liberado','liberado') THEN 1 ELSE 0 END), 0) as emplaz_disponibles,\n",
    "    ROUND(\n",
    "        SUM(CASE WHEN estado in ('activo','Liberado','liberado') THEN 1 ELSE 0 END) * 100.0 / COUNT(*), \n",
    "        2\n",
    "    ) as emplaz_disp_p\n",
    "FROM silver_emplazamientos_modulo\n",
    "GROUP BY areaOYM\n",
    "ORDER BY emplaz_disp_p DESC;"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "cbaea11c-fe3a-44d6-9fa4-e470ca575cd5",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "9. Disponibilidad y Estado de la Red Operativo (Por Modulo)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "efe716a4-b117-4922-931a-2e1f2a777a46",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>modulo</th><th>emplazamientos_totales</th><th>emplaz_disponibles</th><th>emplaz_disp_p</th></tr></thead><tbody><tr><td>Centros TX</td><td>11264</td><td>11234</td><td>99.73</td></tr><tr><td>Sitios RAN</td><td>12060</td><td>8682</td><td>71.99</td></tr><tr><td>Infraestructura</td><td>12181</td><td>6468</td><td>53.10</td></tr></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [
        [
         "Centros TX",
         11264,
         11234,
         "99.73"
        ],
        [
         "Sitios RAN",
         12060,
         8682,
         "71.99"
        ],
        [
         "Infraestructura",
         12181,
         6468,
         "53.10"
        ]
       ],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "modulo",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "emplazamientos_totales",
            "nullable": false,
            "type": "long"
           },
           {
            "metadata": {},
            "name": "emplaz_disponibles",
            "nullable": true,
            "type": "long"
           },
           {
            "metadata": {},
            "name": "emplaz_disp_p",
            "nullable": true,
            "type": "decimal(27,2)"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 22
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "modulo",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "emplazamientos_totales",
         "type": "\"long\""
        },
        {
         "metadata": "{}",
         "name": "emplaz_disponibles",
         "type": "\"long\""
        },
        {
         "metadata": "{}",
         "name": "emplaz_disp_p",
         "type": "\"decimal(27,2)\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "SELECT \n",
    "    modulo,\n",
    "    COUNT(*) as emplazamientos_totales,\n",
    "    --SUM(CASE WHEN estado in ('activo','Liberado') THEN 1 ELSE 0 END) as activos,\n",
    "    ROUND(SUM(CASE WHEN estado in ('activo','Liberado','liberado') THEN 1 ELSE 0 END), 0) as emplaz_disponibles,\n",
    "    ROUND(\n",
    "        SUM(CASE WHEN estado in ('activo','Liberado','liberado') THEN 1 ELSE 0 END) * 100.0 / COUNT(*), \n",
    "        2\n",
    "    ) as emplaz_disp_p\n",
    "FROM silver_emplazamientos_modulo\n",
    "GROUP BY modulo\n",
    "ORDER BY emplaz_disp_p DESC;"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "2d632647-783f-403d-8c79-27fdd06bed5c",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [],
   "source": []
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "843f3aa9-e28e-41cf-ab1b-a880c82985d8",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "10. Tasa de \"Churn\" de Emplazamientos (Negocio)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 0,
   "metadata": {
    "application/vnd.databricks.v1+cell": {
     "cellMetadata": {
      "byteLimit": 2048000,
      "implicitDf": true,
      "rowLimit": 10000
     },
     "inputWidgets": {},
     "nuid": "a608c109-44f0-4623-a5c0-509d05bfaec1",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "display_data",
     "data": {
      "text/html": [
       "<style scoped>\n",
       "  .table-result-container {\n",
       "    max-height: 300px;\n",
       "    overflow: auto;\n",
       "  }\n",
       "  table, th, td {\n",
       "    border: 1px solid black;\n",
       "    border-collapse: collapse;\n",
       "  }\n",
       "  th, td {\n",
       "    padding: 5px;\n",
       "  }\n",
       "  th {\n",
       "    text-align: left;\n",
       "  }\n",
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>año_mes</th><th>estado</th><th>volumen</th><th>variacion_mensual</th></tr></thead><tbody><tr><td>2025-05</td><td>baja</td><td>1</td><td>0</td></tr><tr><td>2025-03</td><td>No Utilizado</td><td>2</td><td>1</td></tr><tr><td>2024-10</td><td>No Utilizado</td><td>1</td><td>0</td></tr><tr><td>2024-10</td><td>baja</td><td>1</td><td>-1</td></tr><tr><td>2024-09</td><td>No Utilizado</td><td>1</td><td>-8</td></tr><tr><td>2024-08</td><td>baja</td><td>2</td><td>0</td></tr><tr><td>2024-07</td><td>baja</td><td>2</td><td>1</td></tr><tr><td>2024-06</td><td>No Utilizado</td><td>9</td><td>8</td></tr><tr><td>2024-06</td><td>baja</td><td>1</td><td>0</td></tr><tr><td>2024-02</td><td>No Utilizado</td><td>1</td><td>0</td></tr><tr><td>2023-12</td><td>baja</td><td>1</td><td>-4</td></tr><tr><td>2023-11</td><td>No Utilizado</td><td>1</td><td>-1</td></tr><tr><td>2023-11</td><td>baja</td><td>5</td><td>4</td></tr><tr><td>2023-10</td><td>No Utilizado</td><td>2</td><td>0</td></tr><tr><td>2023-09</td><td>No Utilizado</td><td>2</td><td>-2</td></tr><tr><td>2023-08</td><td>No Utilizado</td><td>4</td><td>3</td></tr><tr><td>2023-08</td><td>baja</td><td>1</td><td>0</td></tr><tr><td>2023-07</td><td>No Utilizado</td><td>1</td><td>-3</td></tr><tr><td>2023-06</td><td>No Utilizado</td><td>4</td><td>3</td></tr><tr><td>2023-05</td><td>No Utilizado</td><td>1</td><td>-1</td></tr><tr><td>2023-05</td><td>baja</td><td>1</td><td>0</td></tr><tr><td>2023-02</td><td>baja</td><td>1</td><td>-6</td></tr><tr><td>2022-12</td><td>No Utilizado</td><td>2</td><td>1</td></tr><tr><td>2022-10</td><td>No Utilizado</td><td>1</td><td>0</td></tr><tr><td>2022-09</td><td>No Utilizado</td><td>1</td><td>0</td></tr><tr><td>2022-08</td><td>No Utilizado</td><td>1</td><td>-2</td></tr><tr><td>2022-08</td><td>baja</td><td>7</td><td>-219</td></tr><tr><td>2022-07</td><td>No Utilizado</td><td>3</td><td>-16</td></tr><tr><td>2022-07</td><td>baja</td><td>226</td><td>225</td></tr><tr><td>2022-06</td><td>No Utilizado</td><td>19</td><td>18</td></tr><tr><td>2022-05</td><td>No Utilizado</td><td>1</td><td>0</td></tr><tr><td>2022-01</td><td>No Utilizado</td><td>1</td><td>0</td></tr><tr><td>2021-09</td><td>baja</td><td>1</td><td>0</td></tr><tr><td>2021-07</td><td>baja</td><td>1</td><td>0</td></tr><tr><td>2021-03</td><td>No Utilizado</td><td>1</td><td>0</td></tr><tr><td>2021-02</td><td>No Utilizado</td><td>1</td><td>-1</td></tr><tr><td>2021-02</td><td>baja</td><td>1</td><td>-3</td></tr><tr><td>2021-01</td><td>No Utilizado</td><td>2</td><td>1</td></tr><tr><td>2021-01</td><td>baja</td><td>4</td><td>3</td></tr><tr><td>2020-12</td><td>baja</td><td>1</td><td>0</td></tr><tr><td>2020-10</td><td>No Utilizado</td><td>1</td><td>-1</td></tr><tr><td>2020-09</td><td>No Utilizado</td><td>2</td><td>1</td></tr><tr><td>2020-09</td><td>baja</td><td>1</td><td>0</td></tr><tr><td>2020-08</td><td>No Utilizado</td><td>1</td><td>-2</td></tr><tr><td>2020-07</td><td>No Utilizado</td><td>3</td><td>2</td></tr><tr><td>2020-05</td><td>No Utilizado</td><td>1</td><td>-2</td></tr><tr><td>2020-04</td><td>No Utilizado</td><td>3</td><td>-2</td></tr><tr><td>2020-04</td><td>baja</td><td>1</td><td>-1</td></tr><tr><td>2020-03</td><td>No Utilizado</td><td>5</td><td>3</td></tr><tr><td>2020-02</td><td>No Utilizado</td><td>2</td><td>-1</td></tr><tr><td>2019-12</td><td>No Utilizado</td><td>3</td><td>2</td></tr><tr><td>2019-12</td><td>baja</td><td>2</td><td>1</td></tr><tr><td>2019-11</td><td>No Utilizado</td><td>1</td><td>0</td></tr><tr><td>2019-11</td><td>baja</td><td>1</td><td>-1</td></tr><tr><td>2019-10</td><td>No Utilizado</td><td>1</td><td>0</td></tr><tr><td>2019-09</td><td>No Utilizado</td><td>1</td><td>0</td></tr><tr><td>2019-07</td><td>baja</td><td>2</td><td>-47</td></tr><tr><td>2019-05</td><td>No Utilizado</td><td>1</td><td>-2</td></tr><tr><td>2019-04</td><td>No Utilizado</td><td>3</td><td>2</td></tr><tr><td>2019-03</td><td>No Utilizado</td><td>1</td><td>-2</td></tr><tr><td>2019-03</td><td>baja</td><td>49</td><td>48</td></tr><tr><td>2019-02</td><td>No Utilizado</td><td>3</td><td>2</td></tr><tr><td>2019-01</td><td>No Utilizado</td><td>1</td><td>-1</td></tr><tr><td>2018-12</td><td>No Utilizado</td><td>2</td><td>0</td></tr><tr><td>2018-11</td><td>No Utilizado</td><td>2</td><td>0</td></tr><tr><td>2018-11</td><td>baja</td><td>1</td><td>0</td></tr><tr><td>2018-10</td><td>No Utilizado</td><td>2</td><td>-1</td></tr><tr><td>2018-08</td><td>No Utilizado</td><td>3</td><td>-6</td></tr><tr><td>2018-08</td><td>baja</td><td>1</td><td>-1</td></tr><tr><td>2018-07</td><td>No Utilizado</td><td>9</td><td>3</td></tr><tr><td>2018-06</td><td>No Utilizado</td><td>6</td><td>4</td></tr><tr><td>2018-04</td><td>No Utilizado</td><td>2</td><td>-1</td></tr><tr><td>2018-02</td><td>No Utilizado</td><td>3</td><td>2</td></tr><tr><td>2018-01</td><td>No Utilizado</td><td>1</td><td>-3</td></tr><tr><td>2017-12</td><td>No Utilizado</td><td>4</td><td>-3816</td></tr><tr><td>2017-11</td><td>No Utilizado</td><td>3820</td><td>null</td></tr><tr><td>2016-08</td><td>baja</td><td>2</td><td>-1</td></tr><tr><td>2016-06</td><td>baja</td><td>3</td><td>2</td></tr><tr><td>2016-01</td><td>baja</td><td>1</td><td>-23</td></tr><tr><td>2015-12</td><td>baja</td><td>24</td><td>21</td></tr><tr><td>2015-07</td><td>baja</td><td>3</td><td>2</td></tr><tr><td>2015-02</td><td>baja</td><td>1</td><td>-4</td></tr><tr><td>2014-08</td><td>baja</td><td>5</td><td>-11</td></tr><tr><td>2014-07</td><td>baja</td><td>16</td><td>14</td></tr><tr><td>2014-05</td><td>baja</td><td>2</td><td>null</td></tr></tbody></table></div>"
      ]
     },
     "metadata": {
      "application/vnd.databricks.v1+output": {
       "addedWidgets": {},
       "aggData": [],
       "aggError": "",
       "aggOverflow": false,
       "aggSchema": [],
       "aggSeriesLimitReached": false,
       "aggType": "",
       "arguments": {},
       "columnCustomDisplayInfos": {},
       "data": [
        [
         "2025-05",
         "baja",
         1,
         0
        ],
        [
         "2025-03",
         "No Utilizado",
         2,
         1
        ],
        [
         "2024-10",
         "No Utilizado",
         1,
         0
        ],
        [
         "2024-10",
         "baja",
         1,
         -1
        ],
        [
         "2024-09",
         "No Utilizado",
         1,
         -8
        ],
        [
         "2024-08",
         "baja",
         2,
         0
        ],
        [
         "2024-07",
         "baja",
         2,
         1
        ],
        [
         "2024-06",
         "No Utilizado",
         9,
         8
        ],
        [
         "2024-06",
         "baja",
         1,
         0
        ],
        [
         "2024-02",
         "No Utilizado",
         1,
         0
        ],
        [
         "2023-12",
         "baja",
         1,
         -4
        ],
        [
         "2023-11",
         "No Utilizado",
         1,
         -1
        ],
        [
         "2023-11",
         "baja",
         5,
         4
        ],
        [
         "2023-10",
         "No Utilizado",
         2,
         0
        ],
        [
         "2023-09",
         "No Utilizado",
         2,
         -2
        ],
        [
         "2023-08",
         "No Utilizado",
         4,
         3
        ],
        [
         "2023-08",
         "baja",
         1,
         0
        ],
        [
         "2023-07",
         "No Utilizado",
         1,
         -3
        ],
        [
         "2023-06",
         "No Utilizado",
         4,
         3
        ],
        [
         "2023-05",
         "No Utilizado",
         1,
         -1
        ],
        [
         "2023-05",
         "baja",
         1,
         0
        ],
        [
         "2023-02",
         "baja",
         1,
         -6
        ],
        [
         "2022-12",
         "No Utilizado",
         2,
         1
        ],
        [
         "2022-10",
         "No Utilizado",
         1,
         0
        ],
        [
         "2022-09",
         "No Utilizado",
         1,
         0
        ],
        [
         "2022-08",
         "No Utilizado",
         1,
         -2
        ],
        [
         "2022-08",
         "baja",
         7,
         -219
        ],
        [
         "2022-07",
         "No Utilizado",
         3,
         -16
        ],
        [
         "2022-07",
         "baja",
         226,
         225
        ],
        [
         "2022-06",
         "No Utilizado",
         19,
         18
        ],
        [
         "2022-05",
         "No Utilizado",
         1,
         0
        ],
        [
         "2022-01",
         "No Utilizado",
         1,
         0
        ],
        [
         "2021-09",
         "baja",
         1,
         0
        ],
        [
         "2021-07",
         "baja",
         1,
         0
        ],
        [
         "2021-03",
         "No Utilizado",
         1,
         0
        ],
        [
         "2021-02",
         "No Utilizado",
         1,
         -1
        ],
        [
         "2021-02",
         "baja",
         1,
         -3
        ],
        [
         "2021-01",
         "No Utilizado",
         2,
         1
        ],
        [
         "2021-01",
         "baja",
         4,
         3
        ],
        [
         "2020-12",
         "baja",
         1,
         0
        ],
        [
         "2020-10",
         "No Utilizado",
         1,
         -1
        ],
        [
         "2020-09",
         "No Utilizado",
         2,
         1
        ],
        [
         "2020-09",
         "baja",
         1,
         0
        ],
        [
         "2020-08",
         "No Utilizado",
         1,
         -2
        ],
        [
         "2020-07",
         "No Utilizado",
         3,
         2
        ],
        [
         "2020-05",
         "No Utilizado",
         1,
         -2
        ],
        [
         "2020-04",
         "No Utilizado",
         3,
         -2
        ],
        [
         "2020-04",
         "baja",
         1,
         -1
        ],
        [
         "2020-03",
         "No Utilizado",
         5,
         3
        ],
        [
         "2020-02",
         "No Utilizado",
         2,
         -1
        ],
        [
         "2019-12",
         "No Utilizado",
         3,
         2
        ],
        [
         "2019-12",
         "baja",
         2,
         1
        ],
        [
         "2019-11",
         "No Utilizado",
         1,
         0
        ],
        [
         "2019-11",
         "baja",
         1,
         -1
        ],
        [
         "2019-10",
         "No Utilizado",
         1,
         0
        ],
        [
         "2019-09",
         "No Utilizado",
         1,
         0
        ],
        [
         "2019-07",
         "baja",
         2,
         -47
        ],
        [
         "2019-05",
         "No Utilizado",
         1,
         -2
        ],
        [
         "2019-04",
         "No Utilizado",
         3,
         2
        ],
        [
         "2019-03",
         "No Utilizado",
         1,
         -2
        ],
        [
         "2019-03",
         "baja",
         49,
         48
        ],
        [
         "2019-02",
         "No Utilizado",
         3,
         2
        ],
        [
         "2019-01",
         "No Utilizado",
         1,
         -1
        ],
        [
         "2018-12",
         "No Utilizado",
         2,
         0
        ],
        [
         "2018-11",
         "No Utilizado",
         2,
         0
        ],
        [
         "2018-11",
         "baja",
         1,
         0
        ],
        [
         "2018-10",
         "No Utilizado",
         2,
         -1
        ],
        [
         "2018-08",
         "No Utilizado",
         3,
         -6
        ],
        [
         "2018-08",
         "baja",
         1,
         -1
        ],
        [
         "2018-07",
         "No Utilizado",
         9,
         3
        ],
        [
         "2018-06",
         "No Utilizado",
         6,
         4
        ],
        [
         "2018-04",
         "No Utilizado",
         2,
         -1
        ],
        [
         "2018-02",
         "No Utilizado",
         3,
         2
        ],
        [
         "2018-01",
         "No Utilizado",
         1,
         -3
        ],
        [
         "2017-12",
         "No Utilizado",
         4,
         -3816
        ],
        [
         "2017-11",
         "No Utilizado",
         3820,
         null
        ],
        [
         "2016-08",
         "baja",
         2,
         -1
        ],
        [
         "2016-06",
         "baja",
         3,
         2
        ],
        [
         "2016-01",
         "baja",
         1,
         -23
        ],
        [
         "2015-12",
         "baja",
         24,
         21
        ],
        [
         "2015-07",
         "baja",
         3,
         2
        ],
        [
         "2015-02",
         "baja",
         1,
         -4
        ],
        [
         "2014-08",
         "baja",
         5,
         -11
        ],
        [
         "2014-07",
         "baja",
         16,
         14
        ],
        [
         "2014-05",
         "baja",
         2,
         null
        ]
       ],
       "datasetInfos": [
        {
         "name": "_sqldf",
         "schema": {
          "fields": [
           {
            "metadata": {},
            "name": "año_mes",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "estado",
            "nullable": true,
            "type": "string"
           },
           {
            "metadata": {},
            "name": "volumen",
            "nullable": false,
            "type": "long"
           },
           {
            "metadata": {},
            "name": "variacion_mensual",
            "nullable": true,
            "type": "long"
           }
          ],
          "type": "struct"
         },
         "tableIdentifier": null,
         "typeStr": "pyspark.sql.connect.dataframe.DataFrame"
        }
       ],
       "dbfsResultPath": null,
       "isJsonSchema": true,
       "metadata": {
        "createTempViewForImplicitDf": true,
        "dataframeName": "_sqldf",
        "executionCount": 23
       },
       "overflow": false,
       "plotOptions": {
        "customPlotOptions": {},
        "displayType": "table",
        "pivotAggregation": null,
        "pivotColumns": null,
        "xColumns": null,
        "yColumns": null
       },
       "removedWidgets": [],
       "schema": [
        {
         "metadata": "{}",
         "name": "año_mes",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "estado",
         "type": "\"string\""
        },
        {
         "metadata": "{}",
         "name": "volumen",
         "type": "\"long\""
        },
        {
         "metadata": "{}",
         "name": "variacion_mensual",
         "type": "\"long\""
        }
       ],
       "type": "table"
      }
     },
     "output_type": "display_data"
    }
   ],
   "source": [
    "%sql\n",
    "WITH monthly_counts AS (\n",
    "    SELECT \n",
    "        date_format(fecha_modificacion, 'yyyy-MM') AS `año_mes`,\n",
    "        estado,\n",
    "        COUNT(*) AS volumen\n",
    "    FROM silver_emplazamientos_modulo\n",
    "    WHERE estado IN ('baja','No Utilizado')\n",
    "    GROUP BY \n",
    "        date_format(fecha_modificacion, 'yyyy-MM'),\n",
    "        estado\n",
    ")\n",
    "SELECT \n",
    "    `año_mes`,\n",
    "    estado,\n",
    "    volumen,\n",
    "    volumen - LAG(volumen) OVER (\n",
    "        PARTITION BY estado \n",
    "        ORDER BY `año_mes`\n",
    "    ) AS variacion_mensual\n",
    "FROM monthly_counts\n",
    "ORDER BY `año_mes` DESC;"
   ]
  }
 ],
 "metadata": {
  "application/vnd.databricks.v1+notebook": {
   "computePreferences": null,
   "dashboards": [],
   "environmentMetadata": {
    "base_environment": "",
    "environment_version": "4"
   },
   "inputWidgetPreferences": null,
   "language": "python",
   "notebookMetadata": {
    "mostRecentlyExecutedCommandWithImplicitDF": {
     "commandId": 4711971261023126,
     "dataframes": [
      "_sqldf"
     ]
    },
    "pythonIndentUnit": 4
   },
   "notebookName": "iu_db_emplazamientos",
   "widgets": {}
  },
  "language_info": {
   "name": "python"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 0
}
