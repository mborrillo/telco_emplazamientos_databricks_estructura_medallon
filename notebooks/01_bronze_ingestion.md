- `notebooks/01_bronze_ingestion.py`  
  - Lee automáticamente todos los ficheros Excel de un volumen de Databricks.  
  - Fuerza los campos a tipo texto para evitar problemas de tipado.  
  - Evita la conversión automática a `NaN` y crea tablas `raw_emplazamientos_centros_tx`, `raw_emplazamientos_infra` y `raw_emplazamientos_sitios_ran`.
  
[01_bronze_ingestion.py](https://github.com/user-attachments/files/24572979/01_bronze_ingestion.py)
"## CAPA BRONZE\n",
    "\n",
    "Aquí hacemos las primeras vistas de los datos crudos. Cargamos automáticamente todos los ficheros Excel de los tres módulos y los llevamos a tablas raw homogéneas"
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
     "nuid": "d7857dae-3a51-4c88-8e69-4c836d50c745",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
   "outputs": [
    {
     "output_type": "stream",
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "✅ Tabla 'raw_emplazamientos_centros_tx' creada exitosamente\n✅ Tabla 'raw_emplazamientos_infra' creada exitosamente\n✅ Tabla 'raw_emplazamientos_sitios_ran' creada exitosamente\n"
     ]
    }
   ],
   "source": [
    "import pandas as pd\n",
    "import os\n",
    "\n",
    "volume_path = \"/Volumes/workspace/default/iu_db_emplazamientos/bronze/\"\n",
    "\n",
    "files = [f for f in os.listdir(volume_path) if f.endswith('.xlsx')]\n",
    "\n",
    "for file_name in files:\n",
    "    full_path = os.path.join(volume_path, file_name)\n",
    "    \n",
    "    # 1. Leer el Excel sin convertir cadenas a NaN\n",
    "    pdf = pd.read_excel(\n",
    "        full_path,\n",
    "        dtype=str,            # fuerza todo como texto\n",
    "        keep_default_na=False # NO conviertas 'NA', 'null', '' en NaN\n",
    "    )\n",
    "    \n",
    "    # 2. Reemplazar posibles None por cadena vacía para evitar \"nan\"\n",
    "    pdf = pdf.fillna(\"\")     # así no se convierte a \"nan\" en spark\n",
    "    \n",
    "    # 3. Crear DataFrame de Spark\n",
    "    df = spark.createDataFrame(pdf)\n",
    "    \n",
    "    # 4. Crear tabla bronze/raw\n",
    "    table_name = f\"raw_{file_name.replace('.xlsx', '').replace(' ', '_').lower()}\"\n",
    "    df.createOrReplaceTempView(table_name)\n",
    "    \n",
    "    print(f\"✅ Tabla '{table_name}' creada exitosamente\")\n"
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
     "nuid": "c55cb829-75e3-453b-8eb9-1cea0ba716aa",
     "showTitle": false,
     "tableResultSettingsMap": {},
     "title": ""
    }
   },
