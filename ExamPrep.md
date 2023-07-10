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

#### App Service plan

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

#### Escalamiento

Cuando una aplicacion tiene muchos request, por lo que se debe incorporar mas recursos.

El **escalamiento horizontal** implica la imcrementacion de mas App Service Plan(mas recursos de computo) con todo lo que tenga.

El **escalado vertical** es otorgar mas o menos recursos, implica para este caso cambiar el plan.

En planes basicos el escalado es manual.

#### Ranuras de implementación 
Son aplicaciones activas con sus propios nombres de host. Los  elementos de contenido y configuraciones de aplicaciones se pueden intercambiar entre dos ranuras de implementación, incluida la ranura de producción. Puede implementar la aplicación en un entorno de ensayo y, a continuación, intercambiar los espacios de ensayo y producción.

#### App Service Web App

Es la aplicacion ejecutandose, dentro de un App Service Plan. (Tipo de aplicacion, Lenguaje usado, es contenedor o no).

[Creación de una aplicación web HTML estática mediante Azure Cloud Shell](./Resource/Module01/CreateStaticWebApp.mkv)

#### Implementación automatizada y manual
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

#### Autenticacion y Autorizacion
Funcionalidad de App Service, implementacion sencilla, que requiere de autenticacion. 
Marcos web incluidos:
- Azure AD
- Facebook
- Google
- Twitter

#### Caracteristicas de Networking
Debe controlar el tráfico de red entrante y saliente.
Dentro de App Service hay dos implementaciones principales
- Multi-tenant: Multi inquilino, muchos clientes diferentes en la misma unidad de escalado.
- Single-Tentant(App Service Environment (ASE) de un único inquilino

### Configurar Web App Settings
como se escalan, como se crean,
ranuras de implementacion, precios
creacion de dominios





# Module 02: Implement Azure Functions
Azure Functions 
codigo que recibe un trigger y ahi se ejecuta el codigo.
se puede desarrollar local y luego subirlo dentro de un Function app
3 precios
	- consumo: paga por lo que usos, Micrososft escala por consumo, y si no se usa, microsoft lo desescala.
	- Premiun: se puede elegir en maquinas virtuales los recursos, pero sigue escalando automaticamente. Para este caso no se desescala automaticamente.
	- Dedicado: ejecuta las azure functions dentro de un App Service Plan. Teniendo controls sobnre el escalado, si se apagan o no

# Module 03: Develop solutions that use Blob storage
Blob storage
Storage acocuint
Blob Storage
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
