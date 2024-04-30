# Module 04: Azure Cosmos DB storage

## Exploración de Azure Cosmos DB

### Identificación de las principales ventajas de Azure Cosmos DB

Base de Datos NoSQL, semi estructurada con replicacion global. Es decir, puedo tener múltiples replicas en diferentes regiones. Diseñada para proporcionar una latencia baja, una escalabilidad elástica del rendimiento

#### Ventajas clave de distribución global



**Consistencia**: se refiere a la consistencia de la información en cada replica. Esto afecta directamente a la performance.

Azure Cosmos DB ofrece cinco niveles bien definidos. De más fuerte a más débil, los niveles son:

- **Fuerte**:  capacidad de servir solicitudes simultáneamente. Se garantiza que las lecturas devuelven la versión más reciente de un elemento. 
- **Uso vinculado**: 
- **Sesión**: as escrituras se replican en un mínimo de tres réplicas (en un conjunto de cuatro réplicas) en la región local, con replicación asincrónica en todas las demás regiones.
- **Prefijo coherente**: la escritura se realiza en lotes separados. Solo al leer el lote en conjunto se obtiene la coherencia completa.
- **Ocasional**: 

**Baja latencia**: tanto para escritura como para lectura, hay  una respuesta menor a los 10ms en el 99% de los casos. 

### Jerarquía de servicio

**Azure Cosmo DB Account**: unidad fundamental de distribución global y alta disponibilidad, puede agregar y quitar regiones de Azure en su cuenta en cualquier momento.

**Azure Cosmos DB database**: Una base de datos es análoga a un espacio de nombres. Una base de datos es la unidad de administración de un conjunto de contenedores.

**Azure Cosmos DB containers**: Permite la creacion de tablas, Store Procedures, funciones definidas por el usuario, triggers. Es la unidad de escalabilidad del rendimiento y almacenamiento aprovisionados. Un contenedor se divide de forma horizontal y luego se replica en varias regiones.

**Azure Cosmos DB items**:  En función de la API que use, un elemento de Azure Cosmos DB puede representar un documento de una colección, una fila de una tabla, o un nodo o un borde de un grafo. Se refiere a los registros(document, row, node, edge).

### API para Cosmos DB

Azure Cosmos DB ofrece varias API de base de datos, entre las que se incluyen las siguientes:

- **Azure Cosmos DB para NoSQL** almacena datos en formato de documento, permiten realizar consultas mediante la sintaxis del Lenguaje de consulta estructurado (SQL).
- **Azure Cosmos DB for MongoDB** almacena los datos en una estructura de documentos a través del formato BSON.
- **Azure Cosmos DB para PostgreSQL** Almacena datos en un único nodo o distribuidos en una configuración de varios nodos.
- **Azure Cosmos DB for Apache Cassandra** almacena los datos en el esquema orientado a columnas, escalado horizontal y altamente distribuido para almacenar grandes volúmenes de datos
- **Azure Cosmos DB for Table** almacena datos en formato clave-valor.
- **Azure Cosmos DB for Apache Gremlin** permite a los usuarios realizar consultas de grafos y almacenar datos como bordes y vértices.

### Unidades de solicitud (Request Units [RU])
Azure Cosmos DB normaliza el costo de todas las operaciones de base de datos y este se expresa en unidades de solicitud (o RU, para abreviar). Una unidad de solicitud representa los recursos del sistema, como CPU e IOPS, y la memoria que se necesitan para realizar las operaciones de base de datos

El tipo de cuenta de Azure Cosmos DB que usa determina el modo en que se cobrarán las RU consumidas.

Hay tres modos en los que se puede crear una cuenta:

- **Modo de rendimiento aprovisionado**:se realiza por segundos en incrementos de 100 RU/segundo. Para escalar el rendimiento aprovisionado para la aplicación, puede aumentar o disminuir el número de RU en cualquier momento, en incrementos o decrementos de 100 RU.
- **Modo sin servidor**: no es necesario aprovisionar rendimiento al crear recursos, factura el número de unidades de solicitud consumidas por las operaciones de base de datos.
- **Modo de escalabilidad automática**: puede escalar de forma automática e instantánea el rendimiento (RU/s) de la base de datos o del contenedor en función de su uso. Adecuado para cargas de trabajo críticas que tienen patrones de tráfico variables o imprevisibles y requieren SLA para el alto rendimiento y la escala.

[Creación de CosmosDB Account + DB + Container + Item](./Resource/Module04/CreateCosmosDBforNoSQL.mkv)
