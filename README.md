# Automatización ETL diaria con Pentaho: Carga de datos desde SFTP a SQL Server

## 📝 Descripción

Este proyecto implementa un proceso de automatización diaria utilizando **Pentaho Data Integration (PDI)** para extraer, transformar y cargar datos desde archivos remotos ubicados en un servidor **SFTP** hacia una base de datos **SQL Server**.

Se trata de información operativa sobre llamadas y eventos de agentes en un entorno de **call center**, consolidada en la tabla `tb_tm_agent_detail`.

## 🎯 Objetivo del Proceso

- Descargar archivos diarios y retroactivos (`Cisco_Agent*.csv`) desde SFTP.
- Validar duplicidad, transformar columnas y estandarizar formato de fechas.
- Consolidar y cargar los datos a la tabla destino en SQL Server.
- Eliminar archivos descargados para evitar reprocesamiento.

---

## 🧱 Arquitectura del Job

El job principal en Pentaho contiene los siguientes pasos:

1. `DESCARGA_AGENTEVENTDETAIL`: transformación que configura y obtiene variables (fechas, ruta remota, etc.).
2. `DESCARGA_AGENTEVENTDETAIL_2`: transformación que descarga archivos múltiples desde el SFTP.
3. `AgentEventDetail`: transformación principal de carga:
    - Lee los archivos `.csv` (uno por fecha).
    - Realiza un `Stream Lookup` con una tabla de control para evitar duplicados.
    - Filtra registros ya procesados.
    - Carga los datos nuevos a `tb_tm_agent_detail`.

4. `Delete files`: elimina archivos temporales descargados localmente.
5. `Success`: paso final para marcar ejecución correcta.


---

## 🔧 Herramientas utilizadas

- 🛠️ **Pentaho Data Integration (Spoon)**
- 🧾 **SFTP** (múltiples archivos con nombres dinámicos)
- 💾 **SQL Server**
- 🗃️ Archivos CSV delimitados por coma
- ⚙️ Variables de entorno y mapeo de campos

---

## 📊 Detalles técnicos

- **Nombre de archivo**: `Cisco_AgentYYYYMMDD.csv`
- **Frecuencia**: ejecución **diaria**
- **Ventana de datos**: cada ejecución carga archivos de los últimos **20 días**
- **Tabla destino**: `dbo.tb_tm_agent_detail`
- **Transformaciones clave**:
    - Renombrado de columnas (`row Date` → `row_date`, etc.)
    - Cambios regionales en fechas y horas
    - Validación para evitar duplicados (por fecha y llave compuesta)
    - Inserción en SQL Server

---

## 💡 Resultados e impacto

- ✔️ Automatización completa del proceso de carga
- ✔️ Validación previa que garantiza consistencia
- ✔️ Reducción de errores humanos
- ✔️ Proceso flexible ante nuevos archivos o cambios estructurales menores

---

## 📂 Archivos del repositorio

| Archivo/Carpeta        | Descripción                            |
|------------------------|----------------------------------------|
| `README.md`            | Este archivo con la documentación      |
| `capturas/`            | Carpeta con capturas del flujo en PDI  |
| `estructura_tabla.sql` | Estructura ficticia de la tabla destino|
| `ejemplo.csv`          | CSV de ejemplo con datos dummy         |

---

## 📌 Notas

- Este job puede adaptarse fácilmente a otros procesos de extracción con diferentes estructuras.
- Está pensado para escalarse y manejar grandes volúmenes de datos operativos diarios.
