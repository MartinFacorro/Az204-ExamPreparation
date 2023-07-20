AZ-204 
# Module 01: Create Azure App Service web apps
## Implementar Azure App Service Web Apps

### Explorar Azure App Service

Servicio basado en HTTP, donde podemos alojar aplicaciones web, RestAPI, BackEnd de aplicaciones moviles.

Escalacion y Desescalacion automatica.

CI/CD

* Azure DevOps
* GitHub
* Bitbucket
* FTP
* Local Git repository
* And others

**Ranuras de implementacion**: Nos permiten crear las implementaciones en distintos ambientes sin tener que modificar las aplicaciones.

### App Service plan

Servicio computacional(por debajo), con diferentes pleanes. (Servicio de computo + Caracteristicas). Es un recurso que se utiliza para alojar las App Services(siempre debe existir este recurso para alojar apps)

App Service Plan, se miden para calcular el costo:
 - CPU
 - Memoria
 - Disco
 - Funcionalida
 
Por cada ASP se pueden crear tantas aplicaciones como el plan permita.

Cada plan de App Service define:

- Sistema operativo (Windows, Linux)
- Región (oeste de EE. UU., este de EE. UU., etc.)
- Número de instancias de VM
- Tamaño de las instancias de VM (pequeño, mediano, grande)
- Plan de tarifa (Gratis, Compartido, Básico, Estándar, Premium, PremiumV2, PremiumV3, Aislado y AisladoV2)

El **plan de tarifa** de un plan de App Service determina qué características de App Service obtendrá y cuánto paga por el plan.

**Categorias Plan de tarifa** (determina qué características de App Service obtendrá y cuánto paga por el plan).

- **Proceso de compartido**: Gratis y Compartido, son los mas basicos, se ejecutan en recursos compartidos con otros usuarios de Azure. No escala horizontalmente.
- **Dedicated compute (Proceso dedicado)**: (Básico, Estándar,Premium, PremiumV2 y PremiumV3) las aplicaciones se ejecutan en recursos dedicados, las aplicaciones del mismo plan comparten recursos.
- **Aislado**: (Aislado y AisladoV2) proporciona aislamiento de red, y aislamiento de procesos. Maximas posibilidades de escalabilidad horizontal.


**Redundancia por zona**: cuando un data center se cae, dentro de la misma zona, se utilizan otros data centers.

### Escalamiento

Cuando una aplicacion tiene muchos request, por lo que se debe incorporar mas recursos.

El **escalamiento horizontal** implica la imcrementacion de mas App Service Plan(mas recursos de computo) con todo lo que tenga.

El **escalado vertical** es otorgar mas o menos recursos, implica para este caso cambiar el plan.

En planes basicos el escalado es manual.

### Ranuras de implementación 
Son aplicaciones activas con sus propios nombres de host. Los  elementos de contenido y configuraciones de aplicaciones se pueden intercambiar entre dos ranuras de implementación, incluida la ranura de producción. Puede implementar la aplicación en un entorno de ensayo y, a continuación, intercambiar los espacios de ensayo y producción.

### App Service Web App

Es la aplicacion ejecutandose, dentro de un App Service Plan. (Tipo de aplicacion, Lenguaje usado, es contenedor o no).

[Creación de una aplicación web HTML estática mediante Azure Cloud Shell](./Resource/Module01/CreateStaticWebApp.mkv)

### Implementación automatizada y manual
*Implementación automatizada*: (o implementacion continua) roceso que se usa para insertar nuevas características y correcciones de errores en un patrón repetitivo y rápido con un efecto mínimo en los usuarios finales.
Se admiten varios origenes:
- **Azure DevOps Services**: inserta el código en Azure DevOps Services, compila, ejecuta pruebas, genera una versión, e inserta el código en una aplicación web de Azure.
- **GitHub**: directamente desde GitHub. Los cambios que se insertan en la rama de producción en GitHub se implementan automáticamente.
- **Bitbucket**: similar a GitHub.

**Implementación manual**
- **Git**: dirección URL de Git que se puede agregar como repositorio remoto. Al insertar en el repositorio remoto, se implementa la aplicación.
- **CLI**: característica de la interfaz de la línea de comandos que empaqueta la aplicación y la implementa. `az webapp up` puede crear una aplicación web de App Service de forma automática si todavía no ha creado una.
- **Implementación desde un archivo Zip**: use `curl` o una utilidad HTTP similar para enviar un archivo ZIP de los archivos de la aplicación a App Service.
- **FTP/S**: FTP o FTPS es una manera tradicional de insertar el código en muchos entornos de hospedaje, incluido App Service.

### Autenticacion y Autorizacion
Funcionalidad de App Service, implementacion sencilla, que requiere de autenticacion. 
Marcos web incluidos:
- Azure AD
- Facebook
- Google
- Twitter

### Caracteristicas de Networking
Debe controlar el tráfico de red entrante y saliente.
Dentro de App Service hay dos implementaciones principales
- Multi-tenant: Multi inquilino, muchos clientes diferentes en la misma unidad de escalado.
- Single-Tentant(App Service Environment (ASE) de un único inquilino

### Configurar Web App Settings
como se escalan, como se crean,
ranuras de implementacion, precios
creacion de dominios





# Module 02: Implement Azure Functions
ℹ️ Durable Functions Deprecada

## Azure Functions 
codigo(de diferentes lenguajes) para que se ejecuten en determinado momento. Reciben un trigger(manejado por un Storage Account) que actua como disparador de la ejecucion del codigo.
[Azure Function como nucleo de servicios](/Resource/Module02/AzureFunctions.png)

Se puede desarrollar local y luego subirlo dentro de un Function app. Se usan para ejecutar en un periodo muy corto de tiempo.

Existe 3 planes(hosting) indica el tamaño de la VM, como escala y desescala:
	- **Consumo (o Serverless)**: paga por lo que usos, Micrososft escala por consumo, y si no se usa, microsoft lo desescala. Luego de 20 minutos sin ser usado, el servicio se apaga. Al recibir un request, se levanta el servicio y responde(lo que hace mas lenta la respuesta)-
	- **Premiun**: se puede elegir (en maquinas virtuales) los recursos, escala y desescala de manera automatica segun la necesidad, pero, vienen pre-calentadas(always on siempre encendido), ya que se paga un precio fijo.
	- **Dedicado**: ejecuta las Azure Functions dentro de un App Service Plan(se debe elegir el plan del App Service). Teniendo control sobre el escalado y desescalado(se usan reglas), Always On/Off, precio fijo.

Los planes antes mencionados(Consumo, Premiun, Dedicado) son los principales.
Tambien existen 2 planes mas de hospedaje:

- **Azure App Service Environment**: o (ASE) entorno completamente aislado y dedicado para ejecutar de forma segura aplicaciones a gran escala.
- **Kubernetes**: (Directo o Azure Arc) entorno aislado y dedicado que se ejecuta sobre la plataforma de Kubernetes.

	
### Time Out
Por defecto es de 5 minutos, y como maximo puede definirse 10 minutos. Esta propiedad se define en el archivo host.json.
El tiempo puede ser mayor en planes Premiun y Dedicados donde por defecto tienen definido 302 minutos.

### Cuenta de almacenamiento
Al crear una Function App, <mark>SIEMPRE</mark> se debe crear un Storage Account de uso general, ya que las funciones guardan logs de estados.

### Controlador de escalado
O Scale Controller (solo para Premiun y Por consumo), automaticamente monitorea(revisa la cola de request de) las funciones y determina cual requiere mas recursos(instancias) para atender las peticiones, a medida que disminuyen las peticiones, va realizando el desescalaiento.

El maximo(y por defecto) de instancias que se puede escalar es de 200 instancias. Es posible definirle un numero menor en el archivo de configuracion.

Se debe considerar que las Function App, deben desescalar a todas las funciones contenidas, por lo que se debe considerar que si una funcion convienen colocarla aislada de otras funciones.

### Resumen
La funcion tiene dos componentes principales, la funcion (en un lenguaje determinado, por ejemplo c#)-

Archivo de configuracion llamado function.json, que define cual es el disparador(trigger) de la funcion, y cual es el binding(conexiones a otros servicios) de la funcion.

Una funcion tiene 1 unico trigger, pero puede tener varios bindings.

Los binding pueden ser:
 - IN
 - OUT
 - IN/OUT



# Module 03: Blob storage

## Blob Storage
Solución de almacenamiento de objetos. Está optimizado para el almacenamiento de cantidades masivas de datos no estructurados. Cuenta con varios servicios de almacenamiento que podemos tener en una cuenta, por lo que es necesario crear una cuenta de Storage.

Ofrece dos niveles de rendimiento de cuentas de almacenamiento, *estándar* y *premium*.

- **Estándar:** se trata de la cuenta de uso general v2 estándar y se recomienda para la mayoría de los escenarios en los que se usa Azure Storage.

Dentro del nivel ***Standard***: Cuenta con:
**Files**: similar a File Server, donde se comparten archivos mediante SMB. Tambien se pueden realizar llamadas REST. SE puede sincronizar tanto con servidores OnPremise como con servidores onCloud.

- **Premium:** (son cuentas exclusivas) no se comparte con otros servicios, para ser mas efectivo y con mejor renidimiento. Si crea una cuenta Premium, puede elegir entre **tres tipos de cuenta**:
    - blobs en bloques,
    - blobs en páginas o
    - recursos compartidos de archivos.


- Block Blobs: Blog Storage (Imagenes, videos, sonidos, dosc)
- Page Blobs: Solamente Page Blob (Imagenes de disco de maquinas virtuales)
- File Share: similar a File server(SMB)
**Blobs**: para guardar archivos (imagenes, videos, documentos), existen varios tipo de Blogs.
Existen 3 tipos:

- Blob: de bloque, se guardan secuencialemente, se usa para videos, imagenes, videos.

- Page: utilizados para guardar discos(no administrados) de maquinas virtuales.

- Append: utilizados guardar archivos secuenciales, por ejemplo guardar logs, en donde se incorpora informacion a la ya existente.

**Tables**: Base de datos NoSQL, para guardar informacion de tipo llave:valor. Escalan dinaicamente, son de respuesta rapidas, siempre y cuando la informacion que se almacene sea especifica, como ser configuracions.

**Queues**: Cola de mensajes, para encolar mensajes y posteriormente ir desencolando. La idea es desacoplar servicios, para tener bajar interdependencia entre aplicaciones o servicios.





## Blob Storage
Es el tipo mas utilizado, para el desarrollo de aplicaciones, por ejemplo imagenes para mostrar en una pagina web. Se guardan en espacios logicos, es decir, en contenedores.

### Permisos de Acceso al Blob Storage

- Permisos a nivel contenedor
- Permisos basados RBAC(Role-Based Acces Control)

La seguridad se aplica en transito, con SSL/TLS.

Toda la informacion se encripta en reposo, y no se puede deshabilitar, solo se permite asignar quien administrara las llaves de des-encriptacion.

### Ciclo de Vida de Blob Storage
Existen 4 capas de acceso:

- Premiun(visto antes): No es posible cambiar a otras capas.

Es posible intercambar entre capas
- **Hot**(*Dentro de Blob Standard*): para acceder a los archivos de manera frecuente. Alto costo de almacenamiento es caro, bajo costo de acceso.
- **Cold**(*Dentro de Blob Standard*): el acceso es para archivos no  tan frecuentes. Menor costo de almacenamiento, y mayor costo de acceso(Comparado a HOT).
- **Archive**(*Dentro de Blob Standard*): sin acceso al archivo, pero se almacena por temas de cumplimiento o auditorias. Es la capa menos costosa de almacenamiento, pero la mas costosa de acceder. Para acceder a un archivo se debe "rehidratar" el archivo(proceso que puede horas en base a la prioridad [Alta o Normal]) pasandolo a capa Cold o Hot.


## Trabajar a nivel de Codigo

Libreria cliente ```azure.storage.blob```

Por orden de llamada:

1. **Blob Service Client**: se utiliza para acceder a la StorageAccont, se le proporciona el ConnectionString.
2. **Blob Container Client**: ya conectado a la cuenta de Storage, se utiliza este cliente para trabajar con contenedores.
3. **Blob Client**: para trabajar con archivos, permite eliminar, descargar, crear archivos dentro de un contenedor, que a su vez esta dentro de un Storage Account.
4. **Append Blob Client**: para manipular archivos de tipo log.
5. **Block Blob Client**: para trabajar con archivos de tipo bloque.



# Module 04: Azure Cosmos DB storage

Base de Datos NoSQL, semi estructurada con replicacion global. Es decir, puedo tener multiples replicas en diferentes regiones. Diseñada para proporcionar una latencia baja, una escalabilidad elástica del rendimiento

**Consistencia**: se refiere a la consistencia de la informacion en cada replica. Esto afecta directamente a la performance.

**Baja latencia**: tanto para escritura como para lectura, hay  una respuesta menor a los 10ms en el 99% de los casos. 

### Jerarquía de servicio

**Azure Cosmo DB Account**: unidad fundamental de distribución global y alta disponibilidad, puede agregar y quitar regiones de Azure en su cuenta en cualquier momento.

**Azure Cosmos DB database**: Una base de datos es análoga a un espacio de nombres. Una base de datos es la unidad de administración de un conjunto de contenedores.

**Azure Cosmos DB containers**: Permite la creacion de tablas, Store Procedures, funciones definidas por el usuario, triggers. Es la unidad de escalabilidad del rendimiento y almacenamiento aprovisionados. Un contenedor se divide de forma horizontal y luego se replica en varias regiones.

**Azure Cosmos DB items**:  En función de la API que use, un elemento de Azure Cosmos DB puede representar un documento de una colección, una fila de una tabla, o un nodo o un borde de un grafo. Se refiere a los registros(document, row, node, edge).


**Request Units (RU)**, 3 modos
- Provisioned throughtput
- Serverless
- Autoscale

### API para Cosmos DB

Azure Cosmos DB ofrece varias API de base de datos, entre las que se incluyen las siguientes:

- Azure Cosmos DB para NoSQL
- Azure Cosmos DB for MongoDB
- Azure Cosmos DB para PostgreSQL
- Azure Cosmos DB for Apache Cassandra
- Azure Cosmos DB for Table
- Azure Cosmos DB for Apache Gremlin

### Unidades de solicitud
Azure Cosmos DB normaliza el costo de todas las operaciones de base de datos y este se expresa en unidades de solicitud (o RU, para abreviar). Una unidad de solicitud representa los recursos del sistema, como CPU e IOPS, y la memoria que se necesitan para realizar las operaciones de base de datos

El tipo de cuenta de Azure Cosmos DB que usa determina el modo en que se cobrarán las RU consumidas.

Hay tres modos en los que se puede crear una cuenta:

- **Modo de rendimiento aprovisionado**:se realiza por segundos en incrementos de 100 RU/segundo. Para escalar el rendimiento aprovisionado para la aplicación, puede aumentar o disminuir el número de RU en cualquier momento, en incrementos o decrementos de 100 RU.
- **Modo sin servidor**: no es necesario aprovisionar rendimiento al crear recursos, factura el número de unidades de solicitud consumidas por las operaciones de base de datos.
- **Modo de escalabilidad automática**: puede escalar de forma automática e instantánea el rendimiento (RU/s) de la base de datos o del contenedor en función de su uso. Adecuado para cargas de trabajo críticas que tienen patrones de tráfico variables o imprevisibles y requieren SLA para el alto rendimiento y la escala.


[Creación de CosmosDB Account + DB + Container + Item](./Resource/Module04/CreateCosmosDBforNoSQL.mkv)



# Module 05: Contenedores (IAAS)

## Azure Container Registry(ACR)
Permite guardar en un repositorio privado imagenes para utilizar en nuestra organizacion. Almacenar las imágenes de contenedor y automatizar compilaciones e implementaciones.. Basado en Docker Registry 2.0, soporte para Docker, Helm, OCI (Open Container Initiative).

### Niveles de ACR
Ofrecen precios predecibles y varias opciones para alinearse con la capacidad y los patrones de uso de su registro de Docker privado:

- **Básico**: optimizado para los costos para que los desarrolladores aprendan sobre Azure Container Registry. Los registros básicos tienen las mismas funcionalidades de programación que Estándar y Premium (como integración de la autenticación de Azure Active Directory, la eliminación de imágenes y webhooks).
- **Estándar**: Mismas funcionalidades del nivel basico, pero con mas almacenamiento y mayor rendimiento en las imagenes. Ideal para la mayoria de necesidades en escenarios productivos.
- **Premiun**: Mayor cantidad de almacenamiento incluido y operaciones simultaneas, por lo que permiten trabajar con escenarios de mayor volumen. Agrega replicacion geografica, vinculo privado con puntos de conexion privados.

### Almacenamiento
ya sea Básico, Estándar o Premium, se gozan de características avanzadas:

**Cifrado en reposo**: Azure cifra automáticamente una imagen antes de almacenarla y la descifra sobre la marcha cuando usted o sus aplicaciones y servicios extraen la imagen.

**Almacenamiento regional**: almacena datos en la región donde se crea el registro, para ayudar a los clientes a cumplir los requisitos de cumplimiento y residencia de datos.

**Redundancia de zona**: característica del nivel Premium y usa zonas de disponibilidad de Azure para replicar el registro en un mínimo de tres zonas independientes en cada región habilitada.

**Almacenamiento escalable**: permite crear tantos repositorios, imágenes, capas o etiquetas como necesite, hasta el límite de almacenamiento del registro.


**Azure Container Registry Tasks** (ACR Tasks) para simplificar la creación, la prueba, el envío de cambios y la implementación de imágenes en Azure.  Cree tareas de varios pasos para automatizar la compilación, prueba y aplicación de revisiones de varias imágenes de contenedor en paralelo en la nube. Configure las tareas de compilación para automatizar el sistema operativo del contenedor y la canalización de aplicaciones de revisión de marcos, y compile imágenes de forma automática cuando el equipo guarde el código en el control de origen.

Escenarios de uso: 

**Tarea rapida**: compile e inserte una sola imagen de contenedor en un registro de contenedor a petición en Azure, sin tener que realizar una instalación local del motor de Docker. Considere que ` docker build `, es ` docker push ` en la nube.


**Tareas desencadenadas automáticamente**: habilite uno o varios desencadenadores para compilar una imagen:

- Desencadenar al actualizar el código fuente
- Desencadenar al actualizar la imagen base
- Desencadenar de acuerdo con una programación


**Tarea de varios pasos**: amplíe la funcionalidad de inserción y compilación de una única imagen de ACR Tasks con flujos de trabajo basados en varios contenedores y varios pasos.

## DockerFile
es un script que contiene una serie de instrucciones que se usan para compilar una imagen de Docker. Suelen incluir la siguiente información:

- La imagen base o primaria que usamos para crear la nueva imagen.
- Los comandos para actualizar el sistema operativo base e instalar otro software.
- Los artefactos de compilación que se incluirán, como una aplicación desarrollada.
- Los servicios que se expondrán, como la configuración de red y del almacenamiento.
- El comando que se ejecutará cuando se inicie el contenedor.

**WebHooks**

## Container Instances
Permite trabajar con un contenedor o con una aplicacion que esta contenida en varias aplicaciones(uso mas comun).

- **Container Groups**: recurso de nivel superior de Azure Container Instances es el **grupo de contenedores**. Un grupo de contenedores es una colección de contenedores que se programan en la misma máquina host. Los contenedores de un grupo comparten un ciclo de vida, los recursos, la red local y los volúmenes de almacenamiento. Es similar en concepto a un pod en Kubernetes.

- **Implementación**: dos maneras comunes de implementar un grupo de varios contenedores:

- **plantilla de Resource Manager**: Se recomienda cuando se necesite implementar recursos adicionales de un servicio de Azure (por ejemplo, un recurso compartido de Azure Files) al implementar instancias de contenedor.
- **archivo YAML**: (similar a Docker Compose) Se recomienda cuando la implementación incluya solo instancias de contenedor.


- **Asignación de recursos**: Si bien es un modo serverless se especifica CPUs, memoria y opcionalmente GPUs, a un grupo de contenedores mediante la adición de las solicitudes de recursos de las instancias del grupo.

- **Redes**: se comparten direcciones IP y puertos.

- **Storage**: se utilizan "volumenes" concepto de contenedores, es un recurso de file server, como un espacio de disco. Que contienen informacion de aplicaciones. Sirve para tener un espacio de almacenamiento persistente. En Azure puede ser dentro de una Maquina Virtual, o tambien, puede ser un Azure Files dentro de un Storage Account.

- **Escenarios frecuentes**: útiles en los casos en los que se quiere dividir una sola tarea funcional en varias imágenes de contenedor. Luego, estas imágenes las pueden entregar diferentes equipos y pueden tener diversos requisitos de recursos. Considerar a ```docker build``` o ```docker push``` para compilar e insertar una sola imagen de contenedor(sin instalacion local).

## Comandos utilizados en CLiShell

``` az group create ``` para crear un grupo de recusos.
``` az container create ``` crea un contenedor.
``` az container show ``` comprueba el estado del contenedor ya creado.
``` az group delete ```  quita el grupo de recursos, el registro de contenedor y las imágenes de contenedor almacenadas allí.

### Directivas de reinicio
Cuando se crea un grupo de contenedores, se puede especificar una de tres configuraciones:

**Always**:	Los contenedores del grupo de contenedores siempre se reinician. (valor de configuración predeterminado).

**Never**: Nunca se reinician los contenedores del grupo de contenedores. Los contenedores se ejecutan al menos una vez.

**OnFailure**:	Los contenedores del grupo de contenedores se reinician solo cuando se produce un error en el proceso ejecutado en el contenedor (cuando se cierra con un código de salida distinto de cero). Los contenedores se ejecutan al menos una vez.

Se especifica con el parámetro ``` --restart-policy ``` al llamar a ``` az container create ```.



## Azure Container Apps
Es un cluster ya preparado(por debajo se ejecuta Azure Kubernetes Services), que requiere de cierta condiguracion para funcionar. Permite aplicar microsevicios o aplicaciones contenerizadas en modos ServerLess. Admite cualquier imagen de contenedor x86-64 (linux/amd64) basada en Linux. No hay ninguna imagen de contenedor base necesaria y, si un contenedor se bloquea, se reinicia de forma automática.

Las aplicaciones basadas en Azure Container Apps se pueden escalar de forma dinámica en función del tráfico HTTP, el procesamiento controlado por eventos, la CPU o la carga de memoria

**Autenticación** integrada con proveedores de identidades federados. Esta característica solo se debe usar con HTTPS.

El componente de **middleware de autenticación y autorización** es una característica de la plataforma que se ejecuta como contenedor sidecar en cada réplica de la aplicación. Cuando está habilitado, cada solicitud HTTP entrante pasa por el nivel de seguridad antes de que la aplicación lo controle. El módulo de autenticación y autorización se ejecuta en un contenedor independiente, aislado del código de la aplicación.

Existen dos flujos de inicio de sesion:

- **Sin el SDK del proveedor**: la aplicación delega el inicio de sesión federado a Container Apps.
- **Con el SDK del proveedor**: la aplicación inicia manualmente la sesión del usuario en el proveedor y, luego, envía el token de autenticación a Container Apps para la validación.

### Microservicios
Las arquitecturas de microservicios permiten desarrollar, actualizar, controlar versiones y escalar áreas básicas de funcionalidad de forma independiente en un sistema general.

**Distributed Application Runtime (Dapr)**: simplifican la creación de aplicaciones distribuidas basadas en microservicios. Incluye características como la observabilidad, la publicación/suscripción y la invocación de servicio a servicio con TLS mutuo, reintentos, etc. Proporciona funcionalidades para habilitar la intercomunicación de aplicaciones mediante la mensajería a través de pub/sub o llamadas de servicio a servicio confiables y seguras.

**Container App Envaironment**: ambiente donde se crean y ejecuta Container Apps.

**Administración de Revisiones**: Una revisión es una instantánea inmutable de una versión de la aplicación de contenedor.`az containerapp update` para generar versiones, que permiten volver a revisiones anteriores, tambien se puede ejecutar 2 revisiones a la vez, hasta validar el funcionamiento de la nueva revision, y asi apagar la mas antigua. Puede controlar qué revisiones están activas y el tráfico externo que se dirige a cada revisión activa. Los nombres de revisión se usan para identificar una revisión, y en la URL de la revisión.

```az containerapp revision list``` : enumera todas las revisiones asociadas a la aplicación de contenedor.

# Module 06: Implement user authentication and authorization
Plataforma de identidad
Auth2.0
Microsoft Auth Library
Microsoft Graph


# Module 07:Implement secure cloud solutions
Identidades administradas
AppConfiguration


# Module 08: Implement API Management
Servicio de gfestion de APIs, politicas, parametros, suscripcion para desarrolladores.


# Module 09: Develop event-based solutions
Event Grid gestion de eventos de menor flujo
Event Hub gestion de muchos eventos por segundo




# Module 10: Develop message-based solutions
Colas de storge account
Service boss

# Module 11: Instrument solutions to support monitoring and logging

Application insight
telemetria.

# Module 12: Integrate caching and content delivery within solutions
