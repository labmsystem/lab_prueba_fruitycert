# Prueba para Ingeniero de Datos con Conocimientos en Infraestructura en la Nube


## Empresa: FruityCert

**FruityCert** es una empresa especializada en la certificación de calidad de frutas. Su labor principal es inspeccionar diferentes especies de frutas para verificar su condición y calidad para diversos clientes.

- **Condición**: Estado de conservación y deterioro de la fruta a lo largo del tiempo.
- **Calidad**: Excelencia intrínseca de la fruta en términos de sabor, apariencia, tamaño, etc.

### Proceso de Inspección

Para evaluar la condición y calidad de las frutas, FruityCert realiza calificaciones basadas en distintos parámetros de inspección específicos para cada tipo de fruta, por ejemplo: 

- **Manzanas**: Color, firmeza, contenido de azúcar, etc.
- **Uvas**: Nivel de azúcar (brix), firmeza de la piel, tamaño de las bayas, etc.

Algunos parámetros pueden ser comunes entre diferentes especies, mientras que otros son específicos. 
### Tipos de Inspección
Las inspecciones se realizan en distintos puntos del proceso:
- **Puertos de llegada**: Cuando la fruta llega al país de destino.
- **Puertos de salida**: Antes de la exportación.
- **Líneas de trabajo**: Durante el empaque y preparación.

Cada inspección está asociada a una **planilla de inspección (planilla)** realizada en una fecha determinada. Para efectuar la inspección, se selecciona un **pallet (unidad)** específico, que es una unidad de carga que contiene cajas de fruta. 

### Atributos del Pallet
Los pallets tienen varios atributos identificativos, entre ellos: 

| Atributo         | Descripción                |
|------------------|----------------------------|
| Fecha de llegada | `ArrDate`                  |
| Exportador       | `Exporter`                 |
| Importador       | `Importer`                 |
| Productor        | `Grower`                   |
| Mercado destino  | `Market`                   |
| Puerto de llegada| `Port`                     |
| Fecha de empaque | `Packing`                  |
| Almacén          | `Warehouse`                |
| Tienda           | `Store`                    |
| Granja           | `Farm`                     |
| Parcela de Granja| `FarmPlot`                 |
| Variedad         | `Variety`                  |
| Tamaño           | `Size`                     |
| Categoría        | `Cat`                      |
| Etiqueta         | `Label`                    |
| Empaque          | `Package`                  |
| Producto         | `Product`                  |
| Región de origen | `OriginRegion`             |
| Embarcación      | `Vessel`                   |
| Contenedor       | `Container`                |
| Orden de compra  | `PurchaseOrder`            |
| Orden de envío   | `ShippingOrder`            |

---

## Proceso de Calificación
Después de registrar estos atributos, se toma una **muestra** de cada pallet en las que se evalúan sus distintos parámetros de inspección, las que la gran mayoría están relacionadas a su condición y calidad, estas se registran como: 
- **Evaluación por Calidad (Q)**
- **Evaluación por Condición (C)**

Los parámetros más relevantes que aglomeran las evaluaciones de calidad y condición son: 

- **Nota de Calidad (QUAL - SCOR)**: Representa la evaluación general de la calidad de una muestra.
- **Nota de Condición (COND - SCOR)**: Representa la evaluación general de la condición de una muestra.

La combinación de ambas evaluaciones conforma la nota final de la muestra.


### Niveles de Agregación de Calificaciones
FruityCert permite a sus clientes ver las calificaciones agregadas en distintos niveles:
- **Por muestra**: Calificación de una muestra específica del pallet.
- **Por pallet**: Calificación global del pallet como promedio de las calificaciones de las muestras.
- **Por inspección**: Calificación general basada en todos los pallets incluidos en la inspección, ponderada según la cantidad de cajas de cada pallet.
- **Por lote**: Calificación de un conjunto de pallets agrupados según criterios específicos definidos por el cliente.

### Concepto de Lote

El **lote** es fundamental para agrupar notas de diferentes pallets que comparten ciertos atributos, obteniendo una calificación consolidada que ayuda al cliente en la toma de decisiones estratégicas sobre ese grupo de fruta

- **Criterios de Agrupación**:Los lotes se definen en la tabla **AtributosLotes** y pueden incluir cualquier atributo del pallet mencionado anteriormente.
- **Atributos Personalizados**: Los clientes pueden definir atributos adicionales para conformar lotes, contenidos en **AtributosCliente** e identificados como "Analysis1" a "Analysis5".

Ejemplo: El cliente ExotiFruit agrupa sus inspecciones de paltas utilizando los siguientes atributos: productor, variedad, tamaño, categoría, etiqueta y empaque.

---

## Sección 1: Modelamiento de Datos y Construcción Base de Datos Relacional
**Instrucciones:** Basándote en el contexto y las tablas proporcionadas de FruityCert, responde a las siguientes preguntas y tareas relacionadas con el modelamiento de datos y diseño de base de datos.

### Ejercicio 1: Diseño del Modelo de Datos Relacional

**Objetivo**: Diseñar un DER que represente el modelo de datos relacional para FruityCert.

**Instrucciones**:
- Utiliza una herramienta de diagramación (puede ser a mano alzada si no tienes acceso a una) para crear un DER que incluya todas las entidades identificadas y sus relaciones.
- Asegúrate de incluir atributos clave de cada entidad y las cardinalidades de las relaciones.

### Ejercicio 2: Normalización de la Base de Datos

**Objetivo**: Normalizar las tablas para eliminar redundancias y asegurar la integridad de los datos..

**Instrucciones**:
- Analiza las tablas proporcionadas (**tablonInspecciones**, **ParametrosInspeccion**, **AtributosLotes**, **AtributoLoteCliente**) y determina si están en tercera forma normal (3FN).
- Si encuentras que no están en 3FN, explica las anomalías y cómo las corregirías.
- roporciona las estructuras de las tablas normalizadas resultantes.

### Ejercicio 3: Implementación de la Base de Datos

**Objetivo**: Crea una base de datos en PostgreSQL utilizando una[ imagen Docker]([url](https://hub.docker.com/_/postgres)). Utiliza el diagrama entidad-relación que planteaste anteriormente para definir las tablas y relaciones. Debes proporcionar un Dockerfile que, al ejecutarlo, levante la base de datos con las instrucciones DDL necesarias para crear las tablas y establecer las relaciones.

**Instrucciones**:
- Crea un `Dockerfile` que:
  - Use la imagen base de PostgreSQL (por ejemplo, `postgres:latest`.
  - Copie los scripts de creación de la base de datos (archivos .sql con las sentencias DDL) al contenedor.
  - Asegúrate de que el contenedor Docker, al iniciarse, cree la base de datos y las tablas automáticamente sin intervención manual.

## Sección 2: Construcción de un Pipeline ETL con Procesos de Calidad de Datos (20 minutos)
**Instrucciones:**

Desarrolla un pipeline ETL que:
- Extraiga los datos desde el bucket S3 público prueba-fruitycert (ARN: arn:aws:s3:::prueba-fruitycert), específicamente los archivos:
| Archivo                  |
|--------------------------|
| `AtributoCliente.csv`    |
| `Inspecciones.csv`       |
| `ParametrosInspeccion.csv`|
| `AtributosLotes.csv`     |

Transforme los datos aplicando procesos de calidad:

Manejo de valores nulos o faltantes.
Conversión de tipos de datos (fechas, números).
Eliminación de duplicados.
Validación de rangos y consistencia.
Cargue los datos limpios en la base de datos PostgreSQL creada anteriormente.

Requisitos:

El pipeline debe ser ejecutable mediante un script o contenedor Docker.
Documenta brevemente los pasos y decisiones tomadas.
El código debe permitir ejecutar el pipeline con un solo comando.
Entrega:

Código fuente del pipeline.
Instrucciones claras en un archivo README.

Modelamiento de Datos y Construcción Base de Datos Relacional
**Instrucciones:** Basándote en el contexto y las tablas proporcionadas de FruityCert, responde a las siguientes preguntas y tareas relacionadas con el modelamiento de datos y diseño de base de datos.

### Ejercicio 1: Diseño del Modelo de Datos Relacional

**Objetivo**: Diseñar un DER que represente el modelo de datos relacional para FruityCert.

**Instrucciones**:
- Utiliza una herramienta de diagramación (puede ser a mano alzada si no tienes acceso a una) para crear un DER que incluya todas las entidades identificadas y sus relaciones.
- Asegúrate de incluir atributos clave de cada entidad y las cardinalidades de las relaciones.

### Ejercicio 2: Normalización de la Base de Datos

**Objetivo**: Normalizar las tablas para eliminar redundancias y asegurar la integridad de los datos..

**Instrucciones**:
- Analiza las tablas proporcionadas (**tablonInspecciones**, **ParametrosInspeccion**, **AtributosLotes**, **AtributoLoteCliente**) y determina si están en tercera forma normal (3FN).
- Si encuentras que no están en 3FN, explica las anomalías y cómo las corregirías.
- roporciona las estructuras de las tablas normalizadas resultantes.

### Ejercicio 3: Implementación de la Base de Datos

**Objetivo**: Crea una base de datos en PostgreSQL utilizando una[ imagen Docker]([url](https://hub.docker.com/_/postgres)). Utiliza el diagrama entidad-relación que planteaste anteriormente para definir las tablas y relaciones. Debes proporcionar un Dockerfile que, al ejecutarlo, levante la base de datos con las instrucciones DDL necesarias para crear las tablas y establecer las relaciones.

**Instrucciones**:
- Crea un `Dockerfile` que:
  - Use la imagen base de PostgreSQL (por ejemplo, `postgres:latest`.
  - Copie los scripts de creación de la base de datos (archivos .sql con las sentencias DDL) al contenedor.
  - Asegúrate de que el contenedor Docker, al iniciarse, cree la base de datos y las tablas automáticamente sin intervención manual.



Consultas Analíticas y Optimización
Ejercicio 1: Cálculo de Calificaciones Promedio por Lote y Pallet
Objetivo: Consulta para calcular la calificación promedio de calidad y condición por lote y por pallet.

sql
Copiar código
SELECT 
    IdLote,
    IdPallet,
    AVG(calidad) AS promedio_nota_lote_calidad,
    AVG(condicion) AS promedio_nota_lote_condicion
FROM tablonInspecciones
GROUP BY IdLote, IdPallet;
Ejercicio 2: Determinación de Productores con Mayor Variabilidad en Calificaciones
Objetivo: Consulta para identificar productores con mayor desviación estándar.

sql
Copiar código
SELECT 
    Grower,
    STDDEV(calidad) AS desviacion_estandar_calidad
FROM tablonInspecciones
GROUP BY Grower
ORDER BY desviacion_estandar_calidad DESC;
Ejercicio 3: Segmentación de Pallets por Cuartiles de Condición
Objetivo: Segmentar pallets en cuartiles basados en su calificación de condición promedio.

sql
Copiar código
SELECT 
    IdCliente,
    IdTipoInspeccion,
    IdEspecie,
    IdPlanilla,
    IdUnidad,
    PERCENTILE_CONT(0.25) WITHIN GROUP (ORDER BY condicion_promedio) AS cuartil
FROM tablonInspecciones;
Sección 3: Diseño de Arquitectura en la Nube y Desarrollo de Infraestructura como Código
Pregunta 1: Diseño de Arquitectura de Datos Escalable
Escenario
Ingesta en tiempo real: Utilizar AWS Kinesis o Azure Event Hubs.
Almacenamiento a largo plazo: AWS S3 o Azure Blob Storage.
Data Warehouse: AWS Redshift o Azure Synapse Analytics.
Entrega de informes: Tableau, Power BI, o AWS QuickSight.
Pregunta 2: Desarrollo de Infraestructura como Código
Herramienta de IaC
Utiliza Terraform para definir recursos. Ejemplo de definición de un bucket S3 en Terraform:

hcl
Copiar código
resource "aws_s3_bucket" "data_bucket" {
  bucket = "fruitycert-data-bucket"
  acl    = "private"
}
Pregunta 3: Seguridad y Gobernanza en la Nube
Accesos y permisos: Implementar políticas de acceso mediante IAM Roles en AWS o Azure AD en Azure.
Costos:
Estrategia 1: Utilizar almacenamiento escalonado en S3 (por ejemplo, S3 Glacier para datos históricos).
Estrategia 2: Optimización de consultas y uso de instancias spot para procesamiento en lotes.
