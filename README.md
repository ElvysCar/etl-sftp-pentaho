# Automatización ETL diaria con Pentaho: Carga de datos desde SFTP a SQL Server

## 📝 Descripción

Este proyecto implementa un proceso de automatización diaria utilizando **Pentaho Data Integration (PDI)** para extraer, transformar y cargar datos desde archivos remotos ubicados en un servidor **SFTP** hacia una base de datos **SQL Server**.

Se trata de información operativa sobre llamadas y eventos de agentes en un entorno de **call center**, consolidada en la tabla `tb_tm_agent_detail`.

## 🎯 Objetivo del Proceso

- Descargar archivos diarios y retroactivos (`Cisco_Agent*.csv`) desde SFTP.
- Validar duplicidad, transformar columnas y estandarizar formato de fechas.
- Consolidar y cargar los datos a la tabla destino en SQL Server.
- Eliminar archivos descargados para evitar reprocesamiento.

## 🧱 Arquitectura del Job

1. `DESCARGA_AGENTEVENTDETAIL`: configuración de variables.
2. `DESCARGA_AGENTEVENTDETAIL_2`: descarga múltiples archivos del SFTP.
3. `AgentEventDetail`: consolidación, validación y carga a base de datos.
4. `Delete files`: eliminación de archivos temporales.
5. `Success`: validación de ejecución correcta.

## 🔧 Herramientas utilizadas

- Pentaho Data Integration (Spoon)
- SFTP
- SQL Server
- CSV delimitado por coma
- Variables y mapeo de campos

## 📊 Detalles técnicos

- **Archivo**: `Cisco_AgentYYYYMMDD.csv`
- **Frecuencia**: diaria
- **Ventana de datos**: 20 días atrás
- **Tabla destino**: `dbo.tb_tm_agent_detail`
- **Transformaciones**:
  - Renombrado de columnas
  - Cambio regional de fechas
  - Validación para evitar duplicados
  - Carga a SQL Server

## 💡 Resultados

- Automatización completa
- Validación y consistencia de datos
- Escalable y confiable

## 📂 Archivos

| Archivo         | Descripción                                |
|-----------------|--------------------------------------------|
| `README.md`     | Documentación del proyecto                 |
| `ejemplo.csv`   | CSV de ejemplo con datos dummy             |
| `estructura_tabla.sql` | SQL de ejemplo de la tabla destino |
| `capturas/`     | Imágenes del flujo de Pentaho              |