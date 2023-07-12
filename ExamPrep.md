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
Varios servicios de almacenamiento que podemos tener en una cuenta, por lo que es necesario crear una cuenta de Storage.

Dentro del nivel ***Standard***: Cuenta con:
**Files**: similar a File Server, donde se comparten archivos mediante SMB. Tambien se pueden realizar llamadas REST. SE puede sincronizar tanto con servidores OnPremise como con servidores onCloud.

**Blobs**: para guardar archivos (imagenes, videos, documentos), existen varios tipo de Blogs.
Existen 3 tipos:

- Blob: de bloque, se guardan secuencialemente, se usa para videos, imagenes, videos.

- Page: utilizados para guardar discos(no administrados) de maquinas virtuales.

- Append: utilizados guardar archivos secuenciales, por ejemplo guardar logs, en donde se incorpora informacion a la ya existente.

**Tables**: Base de datos NoSQL, para guardar informacion de tipo llave:valor. Escalan dinaicamente, son de respuesta rapidas, siempre y cuando la informacion que se almacene sea especifica, como ser configuracions.

**Queues**: Cola de mensajes, para encolar mensajes y posteriormente ir desencolando. La idea es desacoplar servicios, para tener bajar interdependencia entre aplicaciones o servicios.

Nivel ***Premiun***: (son cuentas exclusivas) no se comparte con otros servicios, para ser mas efectivo y con mejor renidimiento, se divide en dos tipos de servicio

- Block Blobs: Blog Storage (Imagenes, videos, sonidos, dosc)
- Page Blobs: Solamente Page Blob (Imagenes de disco de maquinas virtuales)
- File Share: similar a File server(SMB)



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





Tipos de Blob
Seguridad, encriptacion, contenedor.
Ciclo de vida.
Rehidratacion de archivos









# Module 04: Develop solutions that use Azure Cosmos DB storage
Cosmos DB
Base distribuida globalmente.
Casandra
Particiones y usos.


# Module 05: Implement infrastructure as a service solutions
Guardar imagenes
instances para ejecutar instancias de contenedores
Azure Container Apps

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
