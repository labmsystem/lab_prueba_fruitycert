# Prueba para Ingeniero de Datos con Conocimientos en Infraestructura en la Nube ☁️

## Empresa: FruityCert 🍈🍇🍌🍋🍒

**FruityCert** es una empresa especializada en la certificación de calidad de frutas. Su labor principal es inspeccionar diferentes especies de frutas para verificar su condición y calidad para diversos clientes.

- **Condición**: Estado de conservación y deterioro de la fruta a lo largo del tiempo.
- **Calidad**: Estado intrínseco de la fruta en términos de sabor, apariencia, tamaño, etc.

### Proceso de Inspección

Para evaluar la condición y calidad de las frutas, FruityCert realiza calificaciones basadas en distintos parámetros de inspección específicos para cada tipo de fruta, por ejemplo: 

- **Manzanas**: Color, firmeza, contenido de azúcar, etc.
- **Uvas**: Nivel de azúcar (brix), firmeza de la piel, tamaño de las bayas, etc.

Algunos parámetros pueden ser comunes entre diferentes especies, mientras que otros son específicos. 
### Tipos de Inspección
Las inspecciones se realizan en distintos puntos del proceso, dependiendo de la necesidad del cliente. Por ejemplo, clientes exportadores inspeccionarán la fruta en origen, mientras que importadores en destino. Algunos ejemplos de tipo de inspección son: 
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
Después de registrar estos atributos, se toma una **muestra** de cada pallet en las que se evalúan sus distintos **parámetros de inspección**, las que la gran mayoría están relacionadas a su condición y calidad, estas se registran como: 
- **Evaluación por Calidad (Q - BQUA)**
- **Evaluación por Condición (C - COND)**

Los parámetros más relevantes que resumen las evaluaciones de calidad y condición son: 

- **Nota de Calidad (QUAL - SCOR)**: Representa la evaluación general de la calidad de una muestra.
- **Nota de Condición (COND - SCOR)**: Representa la evaluación general de la condición de una muestra.

La combinación de ambas evaluaciones conforma la nota final de la muestra.


### Niveles de Agregación de Calificaciones
FruityCert permite a sus clientes ver las calificaciones agregadas en distintos niveles:
- **Por muestra**: Calificación de una muestra específica del pallet.
- **Por pallet**: Calificación global del pallet como promedio de las calificaciones de las muestras.
- **Por inspección**: Calificación general basada en todos los pallets incluidos en la planilla de inspección, ponderada según la cantidad de cajas de cada pallet.
- **Por lote**: Calificación de un conjunto de pallets agrupados según criterios específicos definidos por el cliente.

### Concepto de Lote

El **lote** es fundamental para agrupar notas de diferentes pallets que comparten ciertos atributos, obteniendo una calificación consolidada que ayuda al cliente en la toma de decisiones estratégicas sobre ese grupo de fruta. Estos están definidos por: 

- **Criterios de Agrupación**:Los lotes se definen en la tabla **AtributosLotes** y pueden incluir cualquier atributo del pallet mencionado anteriormente.
- **Atributos Personalizados**: Los clientes pueden definir atributos adicionales para conformar lotes, contenidos en **AtributosCliente** e identificados como "Analysis1" a "Analysis5".

**Ejemplo:** El cliente ExotiFruit agrupa sus inspecciones de paltas utilizando los siguientes atributos: productor, variedad, tamaño, categoría, etiqueta y empaque.

---
A partir del contexto anterior, utiliza las tablas dispuestas en el siguiente link [fruitycert](https://prueba-fruitycert.s3.us-east-1.amazonaws.com/) para realizar la evaluación. Acá están contenidas las diferentes tablas con la información señalada
- `AtributosCliente.csv`
- `TablonInspecciones.csv`
- `ParametrosInspeccion.csv`
- `AtributosLotes.csv`

Para acceder a cada uno de estos `.csv`, debes conectarte a dicho bucket en AWS y consumir la información dispuesta. 

**Recomendaciones:**
- Navega por las tablas, comprende cómo se asocia el contexto a cada columna de cada tabla y cómo se relacionan.
- Comprende bien la agregación por muestra, pallet, lote e inspección, ahí está la clave al éxito 😉.
- La prueba está armada de tal forma que también te ayude a orientarte, utiliza eso en tu beneficio.
- Si tienes dudas sobre el enunciado, crea un `Issue` que se entienda lo que necesitas aclarar:
  - Titulo: Descripción breve y precisa del problema.
  - Etiquetas: `error` | `documentacion`
  - Descripción precisa.
- La tabla `TablonInspecciones.csv` es grande! Parte del desafío es que sepas como manejar y eficientar su consumo. Puedes utilizar **diferentes tipos de estructura** para esto. 

## Sección 1: Modelamiento de Datos y Construcción Base de Datos Relacional
**Instrucciones:** Basándote en el contexto y las tablas proporcionadas de FruityCert, responde a las siguientes preguntas y tareas relacionadas con el modelamiento de datos y diseño de base de datos.

### Ejercicio 1: Diseño del Modelo de Datos Relacional

**Objetivo**: Diseñar un DER que represente el modelo de datos relacional para FruityCert.

**Instrucciones**:
- Utiliza una herramienta de diagramación para crear un DER que incluya todas las entidades identificadas y sus relaciones.
- Asegúrate de incluir atributos clave de cada entidad y las cardinalidades de las relaciones.

### Ejercicio 2: Normalización de la Base de Datos

**Objetivo**: Normalizar las tablas para eliminar redundancias y asegurar la integridad de los datos.

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
1. Extraiga los datos desde el bucket S3 público prueba-fruitycert,  específicamente los archivos:
- `AtributosCliente.csv`
- `TablonInspecciones.csv`
- `ParametrosInspeccion.csv`
- `AtributosLotes.csv`

2. Transforme los datos aplicando procesos de calidad:
- Manejo de valores nulos o faltantes.
- Conversión de tipos de datos (fechas, números).
- Eliminación de duplicados.
- Validación de rangos y consistencia.
- Cargue los datos limpios en la base de datos PostgreSQL creada anteriormente.

3. Crea un protocolo y código para recuperación ante desastres (DR) en que:
- Se copie la información almacenada en la base de datos en un formato eficiente como **Parquet** y guardarlos en un directorio accesible.
- Generar un código de carga a la base de datos los más óptimo y veloz posible.
- 
**Requisitos:**

- El pipeline debe ser ejecutable mediante un script o contenedor Docker.
- Documenta brevemente los pasos y decisiones tomadas.
- El código debe permitir ejecutar tanto la carga como el DR con un solo comando, y debe ser replicable.


## Sección 3: Consultas Analíticas y Optimización
**Instrucciones:** Utiliza las tablas proporcionadas y el contexto de FruityCert para realizar las siguientes consultas analíticas. En cada uno de los ejercicios, se darán **puntos extra** por implementar cada consulta en el pipeline y base de datos. 

### Ejercicio 1: Cálculo de Calificaciones Promedio por Lote y Pallet
- **Objetivo:** Escribir una consulta que calcule la calificación promedio de calidad y condición por inspección (planilla), lote y por pallet en base al COND - SCOR y BQUA - SCOR de cada muestra, donde un lote se define según los criterios especificados en la tabla AtributosLotes para cada cliente. 
- **Salida esperada:** La misma tabla tablonInspecciones, agregando las columnas NotaInspQ, NotaInspC, NotaPalletQ, NotaPalletC. NotaLoteQ, NotaLoteC, NotaPalletQ, NotaPalletC. 

### Ejercicio 2: Análisis de Rendimiento por Variedad y Mercado
- **Objetivo:** Identificar qué variedades (Variety) por cada fruta (Especie) que tienen el mejor rendimiento en términos de calidad en diferentes mercados (Market).
- **Salida esperada:** Tabla con columnas Market, Variety, calidad_promedio, rank.

### Ejercicio 3: Monitoreo de Desempeño de Parámetros Específicos
- **Objetivo:** Evaluar cómo varía cada parámetro de inspección de las UVAS en PACK para GreenHarvest Exports a lo largo del tiempo y detecta cuáles tienen mayor variación. Utiliza funciones de ventana. 
- **Salida esperada:** Tabla con columnas IdCliente, IdTipoInspeccion, IdEspecie, IdPlanilla, IdUnidad, Fecha, ValorParametroInspeccion, valor_anterior, diferencia, alerta_variacion.

### Ejercicio 4: Identificación de Outliers en Parámetros de Inspección
- **Objetivo:** Detectar valores atípicos en los parámetros de inspección de cada cliente que puedan indicar problemas de calidad o condición.
- **Salida esperada:** Tabla con columnas IdCliente, IdTipoInspeccion, IdEspecie, IdPlanilla, IdUnidad, NumeroMuestra, CodigoParametroInspeccion, ValorParamet

## Sección 3: Diseño de Arquitectura en la Nube y Desarrollo de Infraestructura como Código
Instrucciones: Responde a los siguientes escenarios, detallando los servicios específicos de AWS o Azure que utilizarías y explicando los motivos detrás de tus elecciones. En el caso de la infraestructura como código, puedes proporcionar fragmentos de código o describir los recursos que implementarías. Considera que la base de datos puede llegar a contener 100.000.000 de inspecciones. 
### Pregunta 1: Diseño de Arquitectura de Datos Escalable
**Escenario:**
FruityCert desea implementar una nueva plataforma de datos que les permita:
- Ingerir datos de inspecciones en tiempo real desde diferentes puertos y líneas de trabajo.
- Almacenar todos los datos brutos para análisis histórico y cumplimiento regulatorio.
- Proveer a los clientes informes y dashboards en tiempo real para tomar decisiones estratégicas basadas en las calificaciones de los lotes.

**Tareas:**
Diseña la arquitectura que implementarías en AWS o Azure para este flujo de trabajo, incluyendo:
- **Ingesta en tiempo real:** Indica qué servicio usarías para recoger y procesar los datos de inspección en tiempo real.
- **Almacenamiento a largo plazo:** Especifica el servicio donde almacenarías datos brutos de manera económica y segura.
- **Data Warehouse:** Explica cómo estructurarías los datos en un data warehouse para facilitar el análisis y la generación de informes.
- **Entrega de informes:** Indica qué servicio o herramientas usarías para proporcionar dashboards e informes a los clientes.
- **Optimización y escalabilidad:** ¿Cómo manejarías el escalado automático y la alta disponibilidad de los servicios para asegurar un rendimiento consistente?

### Pregunta 2: Desarrollo de Infraestructura como Código
**Escenario:**
Como parte de la implementación de la nueva plataforma de datos, necesitas automatizar la creación de la infraestructura en la nube usando Infraestructura como Código (IaC).
**Tareas:**
- **Elige una herramienta de IaC** (por ejemplo, AWS CloudFormation, Terraform, Azure Resource Manager).
- **Especifica los recursos** que necesitarías definir en tu código para implementar la arquitectura diseñada, incluyendo:
  - Servicios de ingesta de datos en tiempo real.
  - Almacenamiento de datos brutos.
  - Servicios de procesamiento y transformación de datos.
  - Data Warehouse.
  - Servicios para la entrega de informes y dashboards.
- Proporciona ejemplos de código o fragmentos que muestren cómo definirías al menos dos de estos recursos.


### Pregunta 3: Seguridad y Gobernanza en la Nube
- **Accesos y permisos:** ¿Qué políticas o roles implementarías en AWS o Azure para asegurar que solo personal autorizado acceda a datos sensibles, incluyendo datos de clientes y calificaciones?
- **Costos:** Explica dos estrategias específicas que podrías utilizar para reducir costos en un sistema que maneja grandes volúmenes de datos en almacenamiento y procesamiento en la nube.

## Entrega
Para la entrega de esta evaluación, clona el repositorio y luego cárgalo en un lugar que podamos acceder. Acá debe estar contenido todo lo solicitado. Al finalizar, envía un correo a lab@microsystem.cl señalando tu nombre y link del repo. Suerte! 💪
