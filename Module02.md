# Module 02: Implementando Azure Functions

https://learn.microsoft.com/es-es/training/paths/implement-azure-functions/

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

Se puede desarrollar local y luego subirlo dentro de un Function App. Se usan para ejecutar en un periodo muy corto de tiempo.

Existe 3 planes(hosting) indica el tamaño de la VM, como escala y desescala:

- **Consumo (o Serverless)**: paga por lo que usos, Micrososft escala por consumo, y si no se usa, Microsoft lo desescala. Luego de 20 minutos sin ser usado, el servicio se apaga. Al recibir un Request, se levanta el servicio y responde (lo que hace más lenta la  respuesta)
- **Premiun**: se puede elegir (en máquinas virtuales) los recursos, escala y desescala de manera automática según la necesidad, pero, vienen precalentadas (Always On siempre encendido), ya que se paga un precio fijo.
- **Dedicado**: ejecuta las Azure Functions dentro de un App Service Plan(se debe elegir el plan del App Service). Teniendo control sobre el escalado y desescalado (se usan reglas), Always On/Off, precio fijo.

Los planes antes mencionados (Consumo, Premium, Dedicado) son los principales.

También existen 2 planes más de hospedaje:

- **Azure App Service Environment**: o (ASE) entorno completamente aislado y dedicado para ejecutar de forma segura aplicaciones a gran escala.
- **Kubernetes**: (Directo o Azure Arc) entorno aislado y dedicado que se ejecuta sobre la plataforma de Kubernetes.

### Time Out
Por defecto es de 5 minutos, y como máximo puede definirse 10 minutos. Esta propiedad se define en el archivo host.json.
El tiempo puede ser mayor en planes Premium y Dedicados donde por defecto tienen definido 302 minutos.

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
