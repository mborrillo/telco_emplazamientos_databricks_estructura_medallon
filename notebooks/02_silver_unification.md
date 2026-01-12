- `notebooks/02_silver_unification.py`  
  - Selecciona y renombra columnas comunes (código único, nombre de sitio, estado, fecha, cabecera, área, etc.).  
  - Añade una columna `modulo` para identificar el origen (TX, RAN, Infraestructura).  
  - Unifica los tres orígenes en una vista única de emplazamientos, permitiendo análisis consistentes entre módulos.
 [02_silver_unification.py](https://github.com/user-attachments/files/24573006/02_silver_unification.py)
"## CAPA SILVER\n",
    "\n",
    "Normalizamos nombres de columnas, tipos de datos y unificamos los tres mundos de la capa Bronze, en una vista única de emplazamientos. "
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
     "nuid": "da68246f-c3ef-4fb2-a08d-c622feabf876",
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
        "executionCount": 2
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
    "-- Capa SILVER: emplazamientos por módulo, conservando estado y fecha de modificación de cada modulo\n",
    "\n",
    "DROP TABLE IF EXISTS silver_emplazamientos_modulo;\n",
    "\n",
    "CREATE OR REPLACE TABLE silver_emplazamientos_modulo\n",
    "USING DELTA\n",
    "AS\n",
    "\n",
    "-- 1. Centros TX\n",
    "SELECT\n",
    "  'Centros TX'                         AS modulo,\n",
    "  CAST(emplaz_TX AS STRING)            AS codigo_unico,\n",
    "  UPPER(CAST(emplaz_TX AS STRING))     AS nombre_sitio,\n",
    "  CAST(estado_centro AS STRING)        AS estado,\n",
    "  CAST(creationDate AS TIMESTAMP)      AS fecha_modificacion,\n",
    "  cabeceraOYM as cabeceraOYM,\n",
    "  areaOYM as areaOYM\n",
    "\n",
    "FROM raw_emplazamientos_centros_tx\n",
    "WHERE\n",
    "  TRIM(emplaz_TX)       <> ''      -- excluye los registros con código vacío\n",
    "  AND estado_centro IS NOT NULL\n",
    "  AND TRIM(estado_centro) <> ''   -- excluye los regustros con estado vacío\n",
    "\n",
    "UNION ALL\n",
    "\n",
    "-- 2. Sitios RAN\n",
    "SELECT\n",
    "  'Sitios RAN'                         AS modulo,\n",
    "  CAST(emplaz_RAN AS STRING)           AS codigo_unico,\n",
    "  UPPER(CAST(emplaz_RAN AS STRING))    AS nombre_sitio,\n",
    "  CAST(estado_sitio AS STRING)         AS estado,\n",
    "  CAST(lastChangeDate AS TIMESTAMP)    AS fecha_modificacion,\n",
    "  cabecera as cabeceraOYM,\n",
    "  region as areaOYM\n",
    "FROM raw_emplazamientos_sitios_ran\n",
    "\n",
    "UNION ALL\n",
    "\n",
    "-- 3. Infraestructura\n",
    "SELECT\n",
    "  'Infraestructura'                    AS modulo,\n",
    "  CAST(emplaz_INFRA AS STRING)         AS codigo_unico,\n",
    "  UPPER(CAST(emplaz_INFRA AS STRING))  AS nombre_sitio,\n",
    "  CAST(estado_infra AS STRING)         AS estado,\n",
    "  CAST(creationDate AS TIMESTAMP)      AS fecha_modificacion,\n",
    "  cabeceraOYM as cabeceraOYM,\n",
    "  areaOYM as areaOYM\n",
    "FROM raw_emplazamientos_infra\n",
    "--WHERE estado_infra <> \"Baja de Necesidad\"\n",
    "WHERE estado_infra NOT IN ('Baja de Necesidad','Reserva estrategica');\n",
    "\n"
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
     "nuid": "2f62b1e5-1f5f-4e30-974e-a6a1baca169c",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "Verificamos que cada modulo tiene sus emplazamientos individualizados"
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
     "nuid": "9b94c528-ed6f-47bf-bbff-4ba64cf6377b",
     "showTitle": false,
     "tableResultSettingsMap": {
      "0": {
       "dataGridStateBlob": "{\"version\":1,\"tableState\":{\"columnPinning\":{\"left\":[\"#row_number#\"],\"right\":[]},\"columnSizing\":{},\"columnVisibility\":{}},\"settings\":{\"columns\":{}},\"syncTimestamp\":1768166034426}",
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
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>modulo</th><th>emplazamientos</th></tr></thead><tbody><tr><td>Sitios RAN</td><td>12060</td></tr><tr><td>Centros TX</td><td>11264</td></tr><tr><td>Infraestructura</td><td>12181</td></tr></tbody></table></div>"
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
         12060
        ],
        [
         "Centros TX",
         11264
        ],
        [
         "Infraestructura",
         12181
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
        "executionCount": 3
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
    "SELECT modulo, COUNT(*) AS emplazamientos\n",
    "FROM silver_emplazamientos_modulo\n",
    "GROUP BY modulo;"
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
     "nuid": "2d0bc694-1ee5-48ec-8970-57b527e8664f",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "Chequeamos que cada modulo tiene sus emplazamientos por separado"
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
     "nuid": "63e87dd2-342f-45ac-a044-d0da18a134fb",
     "showTitle": false,
     "tableResultSettingsMap": {
      "0": {
       "dataGridStateBlob": "{\"version\":1,\"tableState\":{\"columnPinning\":{\"left\":[\"#row_number#\"],\"right\":[]},\"columnSizing\":{},\"columnVisibility\":{}},\"settings\":{\"columns\":{}},\"syncTimestamp\":1768138438266}",
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
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>modulo</th><th>codigo_unico</th><th>nombre_sitio</th><th>estado</th><th>fecha_modificacion</th><th>cabeceraOYM</th><th>areaOYM</th></tr></thead><tbody><tr><td>Centros TX</td><td>ARCF1447</td><td>ARCF1447</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_SUR</td><td>CUYO</td></tr><tr><td>Centros TX</td><td>ARCF0576</td><td>ARCF0576</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_SUR</td><td>CUYO</td></tr><tr><td>Centros TX</td><td>ARCF0248</td><td>ARCF0248</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_SUR</td><td>CUYO</td></tr><tr><td>Centros TX</td><td>ARCF0332</td><td>ARCF0332</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_SUR</td><td>CUYO</td></tr><tr><td>Centros TX</td><td>ARCF0030</td><td>ARCF0030</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_SUR</td><td>CUYO</td></tr></tbody></table></div>"
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
         "ARCF1447",
         "ARCF1447",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_SUR",
         "CUYO"
        ],
        [
         "Centros TX",
         "ARCF0576",
         "ARCF0576",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_SUR",
         "CUYO"
        ],
        [
         "Centros TX",
         "ARCF0248",
         "ARCF0248",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_SUR",
         "CUYO"
        ],
        [
         "Centros TX",
         "ARCF0332",
         "ARCF0332",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_SUR",
         "CUYO"
        ],
        [
         "Centros TX",
         "ARCF0030",
         "ARCF0030",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_SUR",
         "CUYO"
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
        "executionCount": 4
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
    "SELECT *\n",
    "FROM silver_emplazamientos_modulo\n",
    "WHERE modulo = 'Centros TX'\n",
    "LIMIT 5;\n"
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
     "nuid": "8c5fa5aa-2a07-445d-b787-0d698a8b254f",
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
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>modulo</th><th>codigo_unico</th><th>nombre_sitio</th><th>estado</th><th>fecha_modificacion</th><th>cabeceraOYM</th><th>areaOYM</th></tr></thead><tbody><tr><td>Sitios RAN</td><td>ARBA0433</td><td>ARBA0433</td><td>liberado</td><td>2024-06-06T00:00:00.000Z</td><td>ATLÁNTICA</td><td>INT</td></tr><tr><td>Sitios RAN</td><td>ARBA0428</td><td>ARBA0428</td><td>liberado</td><td>2024-06-06T00:00:00.000Z</td><td>ATLÁNTICA</td><td>INT</td></tr><tr><td>Sitios RAN</td><td>ARBA0411</td><td>ARBA0411</td><td>liberado</td><td>2024-06-06T00:00:00.000Z</td><td>ATLÁNTICA</td><td>INT</td></tr><tr><td>Sitios RAN</td><td>ARBA0387</td><td>ARBA0387</td><td>liberado</td><td>2024-06-06T00:00:00.000Z</td><td>ATLÁNTICA</td><td>INT</td></tr><tr><td>Sitios RAN</td><td>ARBA0370</td><td>ARBA0370</td><td>liberado</td><td>2024-06-06T00:00:00.000Z</td><td>ATLÁNTICA</td><td>INT</td></tr></tbody></table></div>"
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
         "ARBA0433",
         "ARBA0433",
         "liberado",
         "2024-06-06T00:00:00.000Z",
         "ATLÁNTICA",
         "INT"
        ],
        [
         "Sitios RAN",
         "ARBA0428",
         "ARBA0428",
         "liberado",
         "2024-06-06T00:00:00.000Z",
         "ATLÁNTICA",
         "INT"
        ],
        [
         "Sitios RAN",
         "ARBA0411",
         "ARBA0411",
         "liberado",
         "2024-06-06T00:00:00.000Z",
         "ATLÁNTICA",
         "INT"
        ],
        [
         "Sitios RAN",
         "ARBA0387",
         "ARBA0387",
         "liberado",
         "2024-06-06T00:00:00.000Z",
         "ATLÁNTICA",
         "INT"
        ],
        [
         "Sitios RAN",
         "ARBA0370",
         "ARBA0370",
         "liberado",
         "2024-06-06T00:00:00.000Z",
         "ATLÁNTICA",
         "INT"
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
        "executionCount": 5
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
    "SELECT *\n",
    "FROM silver_emplazamientos_modulo\n",
    "WHERE modulo = 'Sitios RAN'\n",
    "LIMIT 5;"
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
     "nuid": "7d92a7ea-fd93-4c18-be67-5685e10f7402",
     "showTitle": false,
     "tableResultSettingsMap": {
      "0": {
       "dataGridStateBlob": "{\"version\":1,\"tableState\":{\"columnPinning\":{\"left\":[\"#row_number#\"],\"right\":[]},\"columnSizing\":{},\"columnVisibility\":{}},\"settings\":{\"columns\":{}},\"syncTimestamp\":1768166037857}",
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
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>modulo</th><th>codigo_unico</th><th>nombre_sitio</th><th>estado</th><th>fecha_modificacion</th><th>cabeceraOYM</th><th>areaOYM</th></tr></thead><tbody><tr><td>Infraestructura</td><td>ARBA10886</td><td>ARBA10886</td><td>No Utilizado</td><td>2017-11-24T20:20:09.000Z</td><td>AMBA_OESTE</td><td>AMBA_OESTE</td></tr><tr><td>Infraestructura</td><td>ARCF9289</td><td>ARCF9289</td><td>No Utilizado</td><td>2017-11-24T20:20:09.000Z</td><td>AMBA_SUR</td><td>AMBA_SUR</td></tr><tr><td>Infraestructura</td><td>ARCF9290</td><td>ARCF9290</td><td>No Utilizado</td><td>2017-11-24T20:20:09.000Z</td><td>AMBA_SUR</td><td>AMBA_SUR</td></tr><tr><td>Infraestructura</td><td>ARCF9291</td><td>ARCF9291</td><td>No Utilizado</td><td>2017-11-24T20:20:09.000Z</td><td>AMBA_SUR</td><td>AMBA_SUR</td></tr><tr><td>Infraestructura</td><td>ARBA10914</td><td>ARBA10914</td><td>No Utilizado</td><td>2017-11-24T20:20:09.000Z</td><td>BUENOS AIRES</td><td>JUNIN</td></tr></tbody></table></div>"
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
         "Infraestructura",
         "ARBA10886",
         "ARBA10886",
         "No Utilizado",
         "2017-11-24T20:20:09.000Z",
         "AMBA_OESTE",
         "AMBA_OESTE"
        ],
        [
         "Infraestructura",
         "ARCF9289",
         "ARCF9289",
         "No Utilizado",
         "2017-11-24T20:20:09.000Z",
         "AMBA_SUR",
         "AMBA_SUR"
        ],
        [
         "Infraestructura",
         "ARCF9290",
         "ARCF9290",
         "No Utilizado",
         "2017-11-24T20:20:09.000Z",
         "AMBA_SUR",
         "AMBA_SUR"
        ],
        [
         "Infraestructura",
         "ARCF9291",
         "ARCF9291",
         "No Utilizado",
         "2017-11-24T20:20:09.000Z",
         "AMBA_SUR",
         "AMBA_SUR"
        ],
        [
         "Infraestructura",
         "ARBA10914",
         "ARBA10914",
         "No Utilizado",
         "2017-11-24T20:20:09.000Z",
         "BUENOS AIRES",
         "JUNIN"
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
        "executionCount": 6
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
    "SELECT *\n",
    "FROM silver_emplazamientos_modulo\n",
    "WHERE modulo = 'Infraestructura'\n",
    "LIMIT 5;"
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
     "nuid": "c0d624be-6c2c-4908-be73-b241e67d6d82",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "source": [
    "Verificamos que la tabla final Silver, este creada correctamente"
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
     "nuid": "c068d0f1-c5fc-4c3c-8674-0ef7adc38ba3",
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
       "</style><div class='table-result-container'><table class='table-result'><thead style='background-color: white'><tr><th>modulo</th><th>codigo_unico</th><th>nombre_sitio</th><th>estado</th><th>fecha_modificacion</th><th>cabeceraOYM</th><th>areaOYM</th></tr></thead><tbody><tr><td>Centros TX</td><td>ARBA8254</td><td>ARBA8254</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>OLAVARRIA</td></tr><tr><td>Centros TX</td><td>ARBA1102</td><td>ARBA1102</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>OLAVARRIA</td></tr><tr><td>Centros TX</td><td>ARBA1459</td><td>ARBA1459</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>OLAVARRIA</td></tr><tr><td>Centros TX</td><td>ARBA1462</td><td>ARBA1462</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>PERGAMINO</td></tr><tr><td>Centros TX</td><td>ARBA1474</td><td>ARBA1474</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>PERGAMINO</td></tr><tr><td>Centros TX</td><td>ARBA1486</td><td>ARBA1486</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>PERGAMINO</td></tr><tr><td>Centros TX</td><td>ARBA4049</td><td>ARBA4049</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>PERGAMINO</td></tr><tr><td>Centros TX</td><td>ARBA1037</td><td>ARBA1037</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>PERGAMINO</td></tr><tr><td>Centros TX</td><td>ARBA8863</td><td>ARBA8863</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>MAR DE AJO</td></tr><tr><td>Centros TX</td><td>ARBA3991</td><td>ARBA3991</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>NECOCHEA</td></tr><tr><td>Centros TX</td><td>ARBA11364</td><td>ARBA11364</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>PERGAMINO</td></tr><tr><td>Centros TX</td><td>ARBA0508</td><td>ARBA0508</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>PERGAMINO</td></tr><tr><td>Centros TX</td><td>ARBA0564</td><td>ARBA0564</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>MAR DE AJO</td></tr><tr><td>Centros TX</td><td>ARBA1292</td><td>ARBA1292</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>MAR DE AJO</td></tr><tr><td>Centros TX</td><td>ARBA0499</td><td>ARBA0499</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>MAR DE AJO</td></tr><tr><td>Centros TX</td><td>ARBA0193</td><td>ARBA0193</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>GLEW-CANUELAS</td></tr><tr><td>Centros TX</td><td>ARBA1290</td><td>ARBA1290</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>TANDIL</td></tr><tr><td>Centros TX</td><td>ARBA0650</td><td>ARBA0650</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>OLAVARRIA</td></tr><tr><td>Centros TX</td><td>ARBA9047</td><td>ARBA9047</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>TANDIL</td></tr><tr><td>Centros TX</td><td>ARBA1460</td><td>ARBA1460</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>TANDIL</td></tr><tr><td>Centros TX</td><td>ARBA6272</td><td>ARBA6272</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>CHIVILCOY</td></tr><tr><td>Centros TX</td><td>ARBA0151</td><td>ARBA0151</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>MAR DEL PLATA</td></tr><tr><td>Centros TX</td><td>ARSF0026</td><td>ARSF0026</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>PERGAMINO</td></tr><tr><td>Centros TX</td><td>ARBA0753</td><td>ARBA0753</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>CHIVILCOY</td></tr><tr><td>Centros TX</td><td>ARBA1044</td><td>ARBA1044</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>CHIVILCOY</td></tr><tr><td>Centros TX</td><td>ARBA0674</td><td>ARBA0674</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>TANDIL</td></tr><tr><td>Centros TX</td><td>ARBA0866</td><td>ARBA0866</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>BUENOS AIRES</td><td>JUNIN</td></tr><tr><td>Centros TX</td><td>ARBA3607</td><td>ARBA3607</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARBA0906</td><td>ARBA0906</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARBA4065</td><td>ARBA4065</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARBA9838</td><td>ARBA9838</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td></tr><tr><td>Centros TX</td><td>ARBA8975</td><td>ARBA8975</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td></tr><tr><td>Centros TX</td><td>ARBA5662</td><td>ARBA5662</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARBA1519</td><td>ARBA1519</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARBA8124</td><td>ARBA8124</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARBA0678</td><td>ARBA0678</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARBA1081</td><td>ARBA1081</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARBA1491</td><td>ARBA1491</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LUJAN</td></tr><tr><td>Centros TX</td><td>ARBA6038</td><td>ARBA6038</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td></tr><tr><td>Centros TX</td><td>ARBA1354</td><td>ARBA1354</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARBA11197</td><td>ARBA11197</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td></tr><tr><td>Centros TX</td><td>ARCF0657</td><td>ARCF0657</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td></tr><tr><td>Centros TX</td><td>ARCF0998</td><td>ARCF0998</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td></tr><tr><td>Centros TX</td><td>ARCF0747</td><td>ARCF0747</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>AMBA_NORTE</td></tr><tr><td>Centros TX</td><td>ARBA11482</td><td>ARBA11482</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARCF0546</td><td>ARCF0546</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARBA1246</td><td>ARBA1246</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARBA1248</td><td>ARBA1248</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr><tr><td>Centros TX</td><td>ARBA4037</td><td>ARBA4037</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LUJAN</td></tr><tr><td>Centros TX</td><td>ARBA1348</td><td>ARBA1348</td><td>activo</td><td>2015-12-01T00:00:00.000Z</td><td>AMBA_NORTE</td><td>LOS POLVORINES</td></tr></tbody></table></div>"
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
         "ARBA8254",
         "ARBA8254",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "OLAVARRIA"
        ],
        [
         "Centros TX",
         "ARBA1102",
         "ARBA1102",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "OLAVARRIA"
        ],
        [
         "Centros TX",
         "ARBA1459",
         "ARBA1459",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "OLAVARRIA"
        ],
        [
         "Centros TX",
         "ARBA1462",
         "ARBA1462",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "PERGAMINO"
        ],
        [
         "Centros TX",
         "ARBA1474",
         "ARBA1474",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "PERGAMINO"
        ],
        [
         "Centros TX",
         "ARBA1486",
         "ARBA1486",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "PERGAMINO"
        ],
        [
         "Centros TX",
         "ARBA4049",
         "ARBA4049",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "PERGAMINO"
        ],
        [
         "Centros TX",
         "ARBA1037",
         "ARBA1037",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "PERGAMINO"
        ],
        [
         "Centros TX",
         "ARBA8863",
         "ARBA8863",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "MAR DE AJO"
        ],
        [
         "Centros TX",
         "ARBA3991",
         "ARBA3991",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "NECOCHEA"
        ],
        [
         "Centros TX",
         "ARBA11364",
         "ARBA11364",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "PERGAMINO"
        ],
        [
         "Centros TX",
         "ARBA0508",
         "ARBA0508",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "PERGAMINO"
        ],
        [
         "Centros TX",
         "ARBA0564",
         "ARBA0564",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "MAR DE AJO"
        ],
        [
         "Centros TX",
         "ARBA1292",
         "ARBA1292",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "MAR DE AJO"
        ],
        [
         "Centros TX",
         "ARBA0499",
         "ARBA0499",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "MAR DE AJO"
        ],
        [
         "Centros TX",
         "ARBA0193",
         "ARBA0193",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "GLEW-CANUELAS"
        ],
        [
         "Centros TX",
         "ARBA1290",
         "ARBA1290",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "TANDIL"
        ],
        [
         "Centros TX",
         "ARBA0650",
         "ARBA0650",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "OLAVARRIA"
        ],
        [
         "Centros TX",
         "ARBA9047",
         "ARBA9047",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "TANDIL"
        ],
        [
         "Centros TX",
         "ARBA1460",
         "ARBA1460",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "TANDIL"
        ],
        [
         "Centros TX",
         "ARBA6272",
         "ARBA6272",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "CHIVILCOY"
        ],
        [
         "Centros TX",
         "ARBA0151",
         "ARBA0151",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "MAR DEL PLATA"
        ],
        [
         "Centros TX",
         "ARSF0026",
         "ARSF0026",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "PERGAMINO"
        ],
        [
         "Centros TX",
         "ARBA0753",
         "ARBA0753",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "CHIVILCOY"
        ],
        [
         "Centros TX",
         "ARBA1044",
         "ARBA1044",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "CHIVILCOY"
        ],
        [
         "Centros TX",
         "ARBA0674",
         "ARBA0674",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "TANDIL"
        ],
        [
         "Centros TX",
         "ARBA0866",
         "ARBA0866",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "BUENOS AIRES",
         "JUNIN"
        ],
        [
         "Centros TX",
         "ARBA3607",
         "ARBA3607",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARBA0906",
         "ARBA0906",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARBA4065",
         "ARBA4065",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARBA9838",
         "ARBA9838",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "AMBA_NORTE"
        ],
        [
         "Centros TX",
         "ARBA8975",
         "ARBA8975",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "AMBA_NORTE"
        ],
        [
         "Centros TX",
         "ARBA5662",
         "ARBA5662",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARBA1519",
         "ARBA1519",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARBA8124",
         "ARBA8124",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARBA0678",
         "ARBA0678",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARBA1081",
         "ARBA1081",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARBA1491",
         "ARBA1491",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LUJAN"
        ],
        [
         "Centros TX",
         "ARBA6038",
         "ARBA6038",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "AMBA_NORTE"
        ],
        [
         "Centros TX",
         "ARBA1354",
         "ARBA1354",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARBA11197",
         "ARBA11197",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "AMBA_NORTE"
        ],
        [
         "Centros TX",
         "ARCF0657",
         "ARCF0657",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "AMBA_NORTE"
        ],
        [
         "Centros TX",
         "ARCF0998",
         "ARCF0998",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "AMBA_NORTE"
        ],
        [
         "Centros TX",
         "ARCF0747",
         "ARCF0747",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "AMBA_NORTE"
        ],
        [
         "Centros TX",
         "ARBA11482",
         "ARBA11482",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARCF0546",
         "ARCF0546",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARBA1246",
         "ARBA1246",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARBA1248",
         "ARBA1248",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
        ],
        [
         "Centros TX",
         "ARBA4037",
         "ARBA4037",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LUJAN"
        ],
        [
         "Centros TX",
         "ARBA1348",
         "ARBA1348",
         "activo",
         "2015-12-01T00:00:00.000Z",
         "AMBA_NORTE",
         "LOS POLVORINES"
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
        "executionCount": 7
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
    "select * from silver_emplazamientos_modulo\n",
    "limit 50"
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
     "nuid": "e8be69b8-e798-49de-9709-9e1c8958fc9d",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
