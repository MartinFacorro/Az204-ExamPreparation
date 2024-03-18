# Module 05: Contenedores (IAAS)

## Azure Container Registry(ACR)
Permite guardar en un repositorio privado imagenes para utilizar en nuestra organizacion. Almacenar las imágenes de contenedor y automatizar compilaciones e implementaciones.. Basado en Docker Registry 2.0, soporte para Docker, Helm, OCI (Open Container Initiative).

### Niveles de ACR
Ofrecen precios predecibles y varias opciones para alinearse con la capacidad y los patrones de uso de su registro de Docker privado:

- **Básico**: optimizado para los costos para que los desarrolladores aprendan sobre Azure Container Registry. Los registros básicos tienen las mismas funcionalidades de programación que Estándar y Premium (como integración de la autenticación de Azure Active Directory, la eliminación de imágenes y webhooks).
- **Estándar**: Mismas funcionalidades del nivel basico, pero con más almacenamiento y mayor rendimiento en las imagenes. Ideal para la mayoria de necesidades en escenarios productivos.
- **Premiun**: Mayor cantidad de almacenamiento incluido y operaciones simultaneas, por lo que permiten trabajar con escenarios de mayor volumen. Agrega replicacion geografica, vinculo privado con puntos de conexion privados.

### Procedimientos recomendados 
- **Implementación cercana a la red**: Cree el registro de contenedor en la misma región de Azure en la que implementa los contenedores.
- **Replicación geográfica en varias regiones** Permite minimizar la latencia mediante la replicación geográfica del registro. ambién puede configurar **webhooks** regionales para que le envíen notificaciones cuando se produzcan eventos en réplicas concretas, como cuando se insertan imágenes.
- **Espacios de nombres del repositorio**: puede permitir el uso compartido de un registro único en varios grupos de su organización. Azure Container Registry admite espacios de nombres anidados, lo que permite el aislamiento del grupo.

### Almacenamiento
ya sea Básico, Estándar o Premium, se gozan de características avanzadas:

**Cifrado en reposo**: Azure cifra automáticamente una imagen antes de almacenarla y la descifra sobre la marcha cuando usted o sus aplicaciones y servicios extraen la imagen.

**Almacenamiento regional**: almacena datos en la región donde se crea el registro, para ayudar a los clientes a cumplir los requisitos de cumplimiento y residencia de datos.

**Redundancia de zona**: característica del nivel Premium y usa zonas de disponibilidad de Azure para replicar el registro en un mínimo de tres zonas independientes en cada región habilitada.

**Almacenamiento escalable**: permite crear tantos repositorios, imágenes, capas o etiquetas como necesite, hasta el límite de almacenamiento del registro.

**Azure Container Registry Tasks** (ACR Tasks) para simplificar la creación, la prueba, el envío de cambios y la implementación de imágenes en Azure.  Cree tareas de varios pasos para automatizar la compilación, prueba y aplicación de revisiones de varias imágenes de contenedor en paralelo en la nube. Configure las tareas de compilación para automatizar el sistema operativo del contenedor y la canalización de aplicaciones de revisión de marcos, y compile imágenes de forma automática cuando el equipo guarde el código en el control de origen.

Escenarios de uso: 

**Tarea rapida**: compile e inserte una sola imagen de contenedor en un registro de contenedor a petición en Azure, sin tener que realizar una instalación local del motor de Docker. 

Para compilar una imagen de contenedor desde el código de ejemplo.

```
az acr build --registry $ACR_NAME --image helloacrtasks:v1 .
```

Para crear un almacen de claves

```
az keyvault create --resource-group $RES_GROUP --name $AKV_NAME
```

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
Permite trabajar con un contenedor o con una aplicación que esta contenida en varias aplicaciones (uso más comun).

- **Container Groups**: recurso de nivel superior de Azure Container Instances es el **grupo de contenedores**. Un grupo de contenedores es una colección de contenedores que se programan en la misma máquina host. Los contenedores de un grupo comparten un ciclo de vida, los recursos, la red local y los volúmenes de almacenamiento. Es similar en concepto a un pod en Kubernetes.

- **Implementación**: dos maneras comunes de implementar un grupo de varios contenedores:

- **plantilla de Resource Manager**: Se recomienda cuando se necesite implementar recursos adicionales de un servicio de Azure (por ejemplo, un recurso compartido de Azure Files) al implementar instancias de contenedor.
- **archivo YAML**: (similar a Docker Compose) Se recomienda cuando la implementación incluya solo instancias de contenedor.

- **Asignación de recursos**: Si bien es un modo serverless se especifica CPUs, memoria y opcionalmente GPUs, a un grupo de contenedores mediante la adición de las solicitudes de recursos de las instancias del grupo.

- **Redes**: se comparten direcciones IP y puertos.

- **Storage**: se utilizan "volumenes" concepto de contenedores, es un recurso de file server, como un espacio de disco. Que contienen información de aplicaciones. Sirve para tener un espacio de almacenamiento persistente. En Azure puede ser dentro de una Maquina Virtual, o tambien, puede ser un Azure Files dentro de un Storage Account.

- **Escenarios frecuentes**: útiles en los casos en los que se quiere dividir una sola tarea funcional en varias imágenes de contenedor. Luego, estas imágenes las pueden entregar diferentes equipos y pueden tener diversos requisitos de recursos. Considerar a ```docker build``` o ```docker push``` para compilar e insertar una sola imagen de contenedor(sin instalacion local).

## Comandos utilizados en CliShell

``` az group create ``` para crear un grupo de recusos.
``` az container create ``` crea un contenedor.
``` az container show ``` comprueba el estado del contenedor ya creado.
``` az group delete ```  quita el grupo de recursos, el registro de contenedor y las imágenes de contenedor almacenadas allí.

### Directivas de reinicio
Cuando se crea un grupo de contenedores, se puede especificar una de tres configuraciones:

**Always**: Los contenedores del grupo de contenedores siempre se reinician. (valor de configuración predeterminado).

**Never**: Nunca se reinician los contenedores del grupo de contenedores. Los contenedores se ejecutan al menos una vez.

**OnFailure**: Los contenedores del grupo de contenedores se reinician solo cuando se produce un error en el proceso ejecutado en el contenedor (cuando se cierra con un código de salida distinto de cero). Los contenedores se ejecutan al menos una vez.

Se especifica con el parámetro ``` --restart-policy ``` al llamar a ``` az container create ```.

## Azure Container Apps
Es un cluster ya preparado(por debajo se ejecuta Azure Kubernetes Services), que requiere de cierta condiguracion para funcionar. Permite aplicar microsevicios o aplicaciones contenerizadas en modos ServerLess. Admite cualquier imagen de contenedor x86-64 (linux/amd64) basada en Linux. No hay ninguna imagen de contenedor base necesaria y, si un contenedor se bloquea, se reinicia de forma automática.

Las aplicaciones basadas en Azure Container Apps se pueden escalar de forma dinámica en función del tráfico HTTP, el procesamiento controlado por eventos, la CPU o la carga de memoria

**Autenticación** integrada con proveedores de identidades federados. Esta característica solo se debe usar con HTTPS.

El componente de **middleware de autenticación y autorización** es una característica de la plataforma que se ejecuta como contenedor sidecar en cada réplica de la aplicación. Cuando está habilitado, cada solicitud HTTP entrante pasa por el nivel de seguridad antes de que la aplicación lo controle. El módulo de autenticación y autorización se ejecuta en un contenedor independiente, aislado del código de la aplicación.

Existen dos flujos de inicio de sesión:

- **Sin el SDK del proveedor**: la aplicación delega el inicio de sesión federado a Container Apps.
- **Con el SDK del proveedor**: la aplicación inicia manualmente la sesión del usuario en el proveedor y, luego, envía el token de autenticación a Container Apps para la validación.

### Microservicios
Las arquitecturas de microservicios permiten desarrollar, actualizar, controlar versiones y escalar áreas básicas de funcionalidad de forma independiente en un sistema general.

**Distributed Application Runtime (Dapr)**: simplifican la creación de aplicaciones distribuidas basadas en microservicios. Incluye características como la observabilidad, la publicación/suscripción y la invocación de servicio a servicio con TLS mutuo, reintentos, etc. Proporciona funcionalidades para habilitar la intercomunicación de aplicaciones mediante la mensajería a través de pub/sub o llamadas de servicio a servicio confiables y seguras.

**Container App Envaironment**: ambiente donde se crean y ejecuta Container Apps.

**Administración de Revisiones**: Una revisión es una instantánea inmutable de una versión de la aplicación de contenedor.`az containerapp update` para generar versiones, que permiten volver a revisiones anteriores, tambien se puede ejecutar 2 revisiones a la vez, hasta validar el funcionamiento de la nueva revision, y asi apagar la más antigua. Puede controlar qué revisiones están activas y el tráfico externo que se dirige a cada revisión activa. Los nombres de revisión se usan para identificar una revisión, y en la URL de la revisión.

```az containerapp revision list``` : enumera todas las revisiones asociadas a la aplicación de contenedor.
