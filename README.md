# Prueba para Ingeniero de Datos con Conocimientos en Infraestructura en la Nube

## Duración total
**1 hora**

## Objetivo
Evaluar habilidades intermedias en:
- Manipulación y transformación de datos.
- Optimización de consultas analíticas.
- Diseño de arquitectura en la nube (AWS o Azure).
- Desarrollo de infraestructura como código.

---

## Empresa: FruityCert

**FruityCert** es una empresa especializada en la certificación de calidad de frutas destinadas a la exportación. Su labor principal es inspeccionar diferentes especies de frutas para verificar su condición y calidad para diversos clientes.

- **Condición**: Estado de conservación y deterioro de la fruta a lo largo del tiempo.
- **Calidad**: Excelencia intrínseca de la fruta en términos de sabor, apariencia, tamaño, etc.

### Proceso de Inspección

Para evaluar la condición y calidad de las frutas, FruityCert realiza calificaciones basadas en distintos parámetros de inspección específicos para cada tipo de fruta:

- **Manzanas**: Color, firmeza, contenido de azúcar, etc.
- **Uvas**: Nivel de azúcar (brix), firmeza de la piel, tamaño de las bayas, etc.

### Parámetros de Evaluación
- **Nota de Calidad (QUAL - SCOR)**: Representa la evaluación general de la calidad de una muestra.
- **Nota de Condición (COND - SCOR)**: Representa la evaluación general de la condición de una muestra.

### Tipos de Inspección
Las inspecciones se realizan en distintos puntos del proceso:
- **Puertos de llegada**: Cuando la fruta llega al país de destino.
- **Puertos de salida**: Antes de la exportación.
- **Líneas de trabajo**: Durante el empaque y preparación.

Cada inspección está asociada a una planilla y un pallet específico.

### Atributos del Pallet

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

- **Evaluación por Calidad (Q)**
- **Evaluación por Condición (C)**

La combinación de ambas evaluaciones conforma la nota final de la muestra.

### Niveles de Agregación de Calificaciones
- **Por muestra**: Calificación de una muestra específica del pallet.
- **Por pallet**: Calificación global del pallet como promedio de las calificaciones de las muestras.
- **Por inspección**: Calificación general basada en todos los pallets incluidos en la inspección.
- **Por lote**: Calificación de un conjunto de pallets agrupados según criterios específicos.

### Concepto de Lote

El lote agrupa notas de diferentes pallets que comparten ciertos atributos, obteniendo una calificación consolidada.

- **Criterios de Agrupación**: Definidos en la tabla `AtributosLotes`.
- **Atributos Personalizados**: Los clientes pueden definir atributos adicionales como `Analysis1` a `Analysis5`.

---

## Sección 1: Modelamiento de Datos y Construcción Base de Datos Relacional

### Ejercicio 1: Diseño del Modelo de Datos Relacional

**Objetivo**: Dibujar un DER que represente el modelo de datos relacional para FruityCert.

**Instrucciones**:
- Utiliza una herramienta de diagramación o hazlo a mano.
- Incluye atributos clave y cardinalidades de las relaciones.

### Ejercicio 2: Normalización de la Base de Datos

**Objetivo**: Normalizar las tablas para eliminar redundancias y asegurar integridad.

**Instrucciones**:
- Analiza las tablas `tablonInspecciones`, `ParametrosInspeccion`, `AtributosLotes`.
- Determina si están en tercera forma normal (3FN).
- Proporciona estructuras de las tablas normalizadas resultantes.

### Ejercicio 3: Implementación de la Base de Datos

**Objetivo**: Crea una base de datos en PostgreSQL utilizando Docker.

**Instrucciones**:
- Crea un `Dockerfile` que:
  - Use la imagen base `postgres:latest`.
  - Copie los scripts de creación de base de datos.
  - Configure el contenedor para ejecutar los scripts automáticamente.

Ejemplo de `Dockerfile`:

```dockerfile
FROM postgres:latest
COPY ./init.sql /docker-entrypoint-initdb.d/
Sección 2: Construcción de un Pipeline ETL
Construye un ETL que extraiga, transforme y cargue los datos en otra carpeta para la ejecución.

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