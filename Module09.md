# Module 09: Soluciones basadas en eventos 

Application Insights es una extensión de Azure Monitor y proporciona características de Supervisión de rendimiento de aplicaciones (también conocida como "APM"). Útiles para supervisar aplicaciones de desarrollo, pruebas y producción

Instrumentar las aplicaciones para habilitar Application Insights a fin de supervisar el rendimiento y ayudar a solucionar los problemas.

Un **evento** es una situación que sucede en una aplicación o servicio que le hace llegar al mundo que algo esta sucediendo, pero no le interesa si se realiza alguna acción o no. Seria solo una notificación.

## Event Grid
Solución para la gestión de eventos. Concentrara todos los eventos desde diferentes orígenes.

Eventos discretos: se refiere al manejo de poca cantidad de eventos.

Cinco conceptos fundamentales:

- **Eventos**: información pequeña que se genera cuando algo se genera en el sistema.
- **Fuente de Eventos**: quien genera esos eventos, por ejemplo, un storage account.
- **Topics**: se utiliza para contextualizar los eventos, por ejemplo, eventos de servicios, eventos de errores de aplicación.
- **Suscripción**: cuando el event grid recibe eventos, y se desea que los suscriptores escuchen los eventos, por ejemplo una pagina web, como si fuera un Event Handler. Por lo que genera una suscripción para cada uno de esos destinos que recibirán los eventos.
- **Event Handlers**:

## Event Hub
Servicio de gestión de (**muchos**) eventos por segundo. Con un flujo de eventos constantes. Permitiendo manejar grandes volúmenes de información de eventos.
Se utiliza para hacer analítica de Data en tiempo real.
La velocidad de un centro de eventos de Azure está determinada por la cantidad de unidades de rendimiento que reserva para él. Puede establecer entre 1 y 20 unidades de rendimiento para Event Hub.
**1 unidad de rendimiento** para los datos que ingresan a un centro de eventos representa 1 MB por segundo o 1000 eventos por segundo (lo que ocurra primero)
