# Module 02: Implementando Azure Functions

https://learn.microsoft.com/es-es/training/paths/implement-azure-functions/

## Azure Function
Azure Functions es una solución sin servidor que le permite escribir menos código, mantener menos infraestructura y ahorrar costos. En lugar de preocuparse por implementar y mantener servidores, la infraestructura en la nube proporciona todos los recursos actualizados necesarios para mantener las aplicaciones en ejecución.

Azure Functions admite desencadenadores que son formas de iniciar la ejecución del código, y enlaces que son formas de simplificar la codificación para los datos de entrada y salida.

### Comparativa entre Functions y WebJobs
Al igual que Azure Functions, Azure App Service WebJobs con el SDK de WebJobs es un servicio de integración de tipo código primero que está diseñado para desarrolladores. Ambos se basan en Azure App Service y admiten características como la integración del control de código fuente, la autenticación y la supervisión con integración de Application Insights.

Azure Functions se basa en el SDK de WebJobs, por lo que comparte muchos desencadenadores de eventos y conexiones con otros servicios de Azure. Estos son algunos de los factores que se deben tener en cuenta al elegir entre Azure Functions y WebJobs con el SDK de WebJobs:

| Funciones | WebJobs con el SDK de WebJobs |
| --- | --- |
| Modelo de aplicaciones sin servidor con escalado automático | Sí |
| Desarrollo y pruebas en el explorador | Sí |
| Precio de pago por uso | Sí |
| Integración con Logic Apps | Sí |
| Desencadenar eventos | Temporizador<br>Blobs y colas de Azure Storage<br>Colas y temas de Azure Service Bus<br>Azure Cosmos DB<br>Azure Event Hubs<br>HTTP/WebHook (GitHub, Slack)<br>Azure Event Grid |
| Sistema de archivos | Temporizador<br>Blobs y colas de Azure Storage<br>Colas y temas de Azure Service Bus<br>Azure Cosmos DB<br>Azure Event Hubs<br>Sistema de archivos |

### Comparación de las opciones de hospedaje de Azure Functions

Cuando crea una aplicación de funciones en Azure, debe elegir un plan de hospedaje para su aplicación. Hay tres planes de hospedaje básico disponibles para Azure Functions: Plan de consumo, plan Premium y plan App Service (Dedicado). Todos los planes de hospedaje tienen disponibilidad general (GA) en máquinas virtuales Linux y Windows.

Ventajas de los tres planes de hospedaje principales de Functions:

| Plan | Ventajas |
| --- | --- |
| Plan de consumo | Este es el plan de hospedaje predeterminado. Escala de forma automática y pagará los recursos de proceso solo cuando se ejecuten las funciones. Las instancias del host de Functions se agregan y quitan de forma dinámica según el número de eventos entrantes. |
| Plan Premium | Escala automáticamente en función de la demanda mediante trabajos preparados previamente que ejecutan aplicaciones sin ningún retraso después de estar inactivas, ejecuta en instancias más eficaces y se conecta a redes virtuales. |
| Plan dedicado | Ejecute sus funciones en un plan de App Service con las tarifas de plan de App Service normal. Mejor para escenarios de ejecución prolongada en los que no se puede usar Durable Functions. |

Hay otras dos opciones de hospedaje que proporcionan el mayor control y aislamiento posibles con los que ejecutar las aplicaciones de funciones.

| Opción de hospedaje | Detalles |
| --- | --- |
| Azure App Service Environment | App Service Environment (ASE) es una característica de App Service que proporciona un entorno completamente aislado y dedicado para ejecutar de forma segura las aplicaciones de App Service a gran escala. |
| Kubernetes (Directo o Azure Arc) | Kubernetes proporciona un entorno completamente aislado y dedicado que se ejecuta sobre la plataforma de Kubernetes. |


### Planes de hospedaje y escalado

Se comparan los comportamientos de escalado de los distintos planes de hospedaje. El número máximo de instancias se da en función de una aplicación por función (consumo) o por plan (Premium/Dedicado), a menos que se indique lo contrario.

| Opción de hospedaje | Escalado horizontal | N.º máximo de instancias |
| --- | --- | --- |
| Plan de consumo | Basado en eventos. Escale horizontalmente de forma automática, incluso durante períodos de gran carga. La infraestructura de Azure Functions escala los recursos de CPU y memoria mediante la incorporación de más instancias del host de Functions, según el número de eventos de desencadenador entrantes. | Windows: 200, Linux: 100 |
| Plan Premium | Basado en eventos. Escale horizontalmente de forma automática, incluso durante períodos de gran carga. La infraestructura de Azure Functions escala automáticamente los recursos de CPU y la memoria mediante la incorporación de más instancias del host de Functions, según el número de eventos desencadenados por las funciones. | Windows: 100, Linux: 20-100 |
| Plan dedicado | Escalabilidad automática o manual | 10-20 |
| Azure App Service Environment | Escalabilidad automática o manual | 100 |
| Kubernetes | Escalado automático basado en eventos para los clústeres de Kubernetes mediante KEDA. | Varía en función del clúster |


### Duración del tiempo de espera de una aplicación de función

La propiedad ```functionTimeout``` del archivo del proyecto host.json especifica la duración del tiempo de espera de las funciones de una aplicación de funciones. Esta propiedad se aplica específicamente a las ejecuciones de funciones. Una vez que el desencadenador inicia la ejecución de la función, la función debe devolver o responder dentro de la duración del tiempo de espera.


Por defecto es de 5 minutos, y como máximo puede definirse 10 minutos. Esta propiedad se define en el archivo host.json.
El tiempo puede ser mayor en planes Premium y Dedicados donde por defecto tienen definido 302 minutos.

Valores predeterminados y máximos (en minutos) para planes específicos:

| Plan | Valor predeterminado | Máximo |
| --- | --- | --- |
| Plan de consumo | 5 | 10 |
| Plan Premium | 30 | Sin límite |
| Plan dedicado | 30 | Sin límite |



### Requisitos de la cuenta de almacenamiento

En cualquier plan, una aplicación de funciones requiere una cuenta de Azure Storage general que admita almacenamiento de Azure en blobs, colas, archivos y tablas. Esto es porque Functions se basa en Azure Storage para operaciones como la administración de desencadenadores y el registro de las ejecuciones de funciones, pero algunas cuentas de almacenamiento no admiten colas ni tablas.

Los desencadenadores y enlaces para almacenar los datos de la aplicación también pueden usar la misma cuenta de almacenamiento que usa la aplicación de función. Sin embargo, para las operaciones que consumen muchos recursos de almacenamiento, debe usar una cuenta de almacenamiento independiente.

### Escalado de Azure Functions

En los planes Consumo y Premium, Azure Functions escala los recursos de CPU y memoria mediante la incorporación de instancias adicionales del host de Functions.

Cada instancia del host de Functions del plan de consumo tiene una limitación de 1.5 GB de memoria y una CPU. Una instancia del host es la aplicación de funciones completa, lo que significa que todas las funciones de una aplicación de funciones comparten recursos al mismo tiempo en una instancia y escala determinadas. Las aplicaciones de funciones que comparten el mismo plan de consumo se escalan de manera independiente.

https://learn.microsoft.com/es-es/training/modules/explore-azure-functions/4-scale-azure-functions














## Durable Functions 
Extensión de Azure Functions que permite escribir funciones con estado en un entorno de proceso sin servidor. La extensión permite definir flujos de trabajo con estado mediante la escritura de funciones del orquestador y entidades con estado mediante la escritura de funciones de entidad con el modelo de programación de Azure Functions. 

### Patones utilizados

- **Patrón nº 1: Encadenamiento de funciones** una secuencia de funciones se ejecuta en un orden específico. La salida de una función se aplica a la entrada de otra función. 
- **Patrón nº 2: Distribución ramificada de salida y de entrada** se ejecutan en paralelo varias funciones y después se espera a que todas finalicen. 
- **Patrón nº 3: Las API de HTTP asincrónico** soluciona el problema de coordinar el estado de las operaciones de larga duración con los clientes externos.
- **Patrón 4: Supervisión** proceso flexible y periódico en un flujo de trabajo, hasta que se cumplen condiciones específicas. Puede usarse cuando se necesita que una API externa modifique su valor de retorno.
- **Patrón nº 5: Interacción humana** Un proceso de aprobación es un ejemplo de un proceso empresarial que implica la interacción humana.
- **Patrón 6: Agregador (entidades con estado)** trata de agregar datos de eventos durante un período de tiempo en una solaentidad direccionable. En este patrón, los datos que se agreguen pueden proceder de varios orígenes. Es posible que el agregador tome medidas según datos de eventos a medida que llegan, y puede que los clientes externos necesiten consultar los datos agregados.

## Azure Functions 
Código de diferentes lenguajes) para que se ejecuten en determinado momento. Reciben un trigger(manejado por un Storage Account) que actúa como disparador de la ejecución del código.
[Azure Function como núcleo de servicios](/Resource/Module02/AzureFunctions.png)




### Cuenta de almacenamiento
Al crear una Function App, <mark>SIEMPRE</mark> se debe crear un Storage Account de uso general, ya que las funciones guardan logs de estados.
Se permiten hasta 3 copias en zona redundante en la zona primaria.

### Controlador de escalado
O Scale Controller (solo para Premium y Por consumo), automáticamente monitorea (revisa la cola de request de) las funciones y determina cual requiere más recursos(instancias) para atender las peticiones, a medida que disminuyen las peticiones, va realizando el desescalaiento.

El máximo (y por defecto) de instancias que se puede escalar es de 200 instancias. Es posible definirle un número menor en el archivo de configuración.

Se debe considerar que las Function App, deben desescalar a todas las funciones contenidas, por lo que se debe considerar que si una función convienen colocarla aislada de otras funciones.

### Controles Personalizados
Los controladores personalizados son servidores web ligeros que reciben eventos del host de Functions. Cualquier lenguaje que admita primitivas de HTTP puede implementar un controlador personalizado.

Los controladores personalizados son más adecuados para las situaciones en las que desea:

* Implemente una aplicación de funciones en un lenguaje que actualmente no se ofrece de manera inmediata, como Go o Rust.
* Implemente una aplicación de funciones en un runtime que actualmente no se incluye de manera predeterminada, como Deno.

### Trigger
CRON utiliza un formato de "{segundo} {minuto} {hora} {día} {mes} {día de la semana}" para las expresiones. El primer "0" significa que corre cuando el segundo es igual a 0. El segundo "15,30,45" significa cuando los minutos son igual a 15, 30 y 45. El tercer "0" significa a la medianoche. Así que la respuesta es a las 12:15, 12:30 y 12:45 todos los días. 

### Resumen
La función tiene dos componentes principales, la función (en un lenguaje determinado, por ejemplo, c#)

Archivo de configuración llamado function.json, que define cual es el disparador(trigger) de la función, y cuál es el binding(conexiones a otros servicios) de la función.

Una función tiene 1 único trigger, pero puede tener varios bindings.

Los binding pueden ser:
 - IN
 - OUT
 - IN/OUT
