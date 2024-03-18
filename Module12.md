# Module 12: Implementación de almacenamiento en caché para soluciones

## Azure Cache for Redis
Redis es una empresa (tercera) que ofrece una base de datos basada en memoria. 

Existe una tecnología Open Source que utiliza Microsoft.

Redis es capaz de procesar grandes volúmenes de solicitudes de aplicación al mantener los datos a los que se accede con frecuencia en la memoria del servidor, donde se pueden realizar operaciones rápidas de lectura y escritura.

El almacenamiento en caché es una técnica que tiene como objetivo mejorar el rendimiento y la escalabilidad de un sistema. se copian temporalmente los datos a los que se accede con mayor frecuencia en almacenamiento rápido ubicado cerca de la aplicación.

El objetivo de esta implementación, es mantener en memoria cierta información que no es modificada frecuentemente. Por lo que, manteniendo la en memoria, se evita consultar en base de datos, evitando consumir tiempo y recursos innecesarios.

### Escenarios principales
Azure Cache for Redis mejora el rendimiento de las aplicaciones, ya que admite patrones de arquitectura de aplicaciones comunes. Estos son algunos de los más comunes:

- **Caché de datos**: Las bases de datos suelen ser demasiado grandes para cargarlas directamente en una caché. Es habitual usar el patrón cache-aside para cargar(o actualizar) datos en la caché solo cuando es necesario. 
- **Caché de contenido**: Muchas páginas web se generan a partir de plantillas que usan contenido estático como encabezados, pies de página y banners. Estos elementos estáticos no deberían cambiar a menudo. El uso de una caché en memoria proporciona acceso rápido a contenido estático en comparación con los almacenes de datos de back-end.
- **Almacén de sesión**: Este patrón se utiliza normalmente con carros de la compra y otros datos del historial de los usuarios que una aplicación web podría asociar con las cookies del usuario. El almacenamiento de demasiados datos en una cookie puede tener un efecto negativo en el rendimiento, ya que aumenta su tamaño y no hay que olvidar que se pasa y se valida con cada solicitud. 
- **Almacenamiento en cola de trabajos y mensajes**: Las aplicaciones agregan a menudo tareas a una cola cuando las operaciones asociadas a la solicitud tardan tiempo en ejecutarse. Las operaciones con ejecuciones más largas se ponen en cola para procesarse en secuencia, a menudo por parte de otro servidor.
- **Distributed transactions**: A veces, las aplicaciones requieren una serie de comandos en un almacén de datos de back-end para ejecutarse como una única operación atómica. El resultado de todos los comandos debe ser satisfactorio, o todos deben revertirse al estado inicial. Azure Cache for Redis admite la ejecución de un lote de comandos como transacción única.

### Niveles de servicio

- **Básico**: Una memoria caché de OSS Redis que se ejecuta en una sola máquina virtual. Este nivel no tiene contrato de nivel de servicio (SLA) y es ideal para las cargas de trabajo de desarrollo y pruebas y no críticas.
- **Estándar**: Una memoria caché de OSS Redis que se ejecuta en dos máquinas virtuales en una configuración replicada.
- **Premium**: Memorias caché de OSS Redis de alto rendimiento. Este nivel ofrece mayor rendimiento, menor latencia, mejor disponibilidad y más características. Las memorias caché Prémium se implementan en máquinas virtuales más eficaces en comparación con las máquinas virtuales de las memorias caché de nivel Básico o Estándar.
- **Enterprise**: Memorias caché de alto rendimiento con la tecnología del software Redis Enterprise de Redis Labs. Este nivel admite módulos de Redis, incluidos RediSearch, RedisBloom y RedisTimeSeries. Además, ofrece una disponibilidad aún mayor que el nivel Prémium.
- **Enterprise Flash**: Memorias caché de gran tamaño y rentabilidad basadas en el software Redis Enterprise de Redis Labs. Este nivel amplía el almacenamiento de datos de Redis en memoria no volátil, que es más barata que DRAM, en una máquina virtual. Reduce el costo total de memoria por GB.

### Creación y Configuración
Se pueden crear desde Azure Portal, la CLI de Azure o Azure PowerShell.

Parámetros para configurar la memoria caché:

- **Nombre**: nombre único global, porque se usa para generar una dirección URL pública para conectarse y comunicarse con el servicio.
- **Ubicación**: La instancia de caché y la aplicación siempre deben colocarse en la misma región.
- **Tipo**: nivel determina el tamaño, rendimiento y características disponibles para la memoria caché.(Basic, Standard, Premium, Enterprise, Enterprise Flash)
- **Compatibilidad con la agrupación en clústeres**: niveles Premium, Enterprise y Enterprise Flash, puede implementar la agrupación en clústeres y dividir automáticamente el conjunto de datos entre varios nodos.

### Linea de comandos
Estos son algunos comandos comunes que puede usar:

| Comando | Descripción |
| --- | --- |
| ping | Hacer ping al servidor. Devuelve "PONG". |
| set [key] [value] | Establece una clave o un valor en la memoria caché. Devuelve "OK" cuando la operación se realiza correctamente. |
| get [key] | Obtiene un valor de la caché. |
| exists [key] | Devuelve "1" si la clave existe en la memoria caché o "0" si no existe. |
| type [key] | Devuelve el tipo asociado con el valor de la clave dada. |
| incr [key] | Incremente el valor dado asociado con la clave en "1". El valor debe ser un número entero o un valor doble. Devuelve el nuevo valor. |
| incrby [key] [amount] | Incremente el valor dado asociado con la clave en la cantidad especificada. El valor debe ser un número entero o un valor doble. Devuelve el nuevo valor. |
| del [key] | Elimina el valor asociado con la clave. |
| flushdb | Elimine todas las claves y valores de la base de datos. |

### Tiempo de expiración
Es preciso tener una forma de que los valores expiren cuando estén obsoletos. En Redis, para hacerlo es preciso aplicar un período de vida (TTL) a una clave.
Cuando transcurra el TTL, la clave se elimina automáticamente, exactamente como si se hubiera emitido el comando DEL. Estas son algunas notas acerca de las expiraciones de TTL.

- Las expiraciones se pueden establecer con una precisión de segundos o mili segundos.
- La resolución de tiempo de expiración es siempre 1 mili segundo.
- La información sobre las expiraciones se replica y se conserva en el disco. El tiempo pasa virtualmente mientras el servidor de Redis permanece detenido (lo que significa que Redis guarda la fecha en que expiran las claves).


### StackExchange.Redis
Librería que permite la conexión con Redis y poder enviar comandos desde una aplicación en .NET.

Usamos la dirección del host, el número de puerto y una clave de acceso para conectarse a un servidor de Redis.

Ejemplo de conexión:

```c#
//Creating a connection

using StackExchange.Redis;
...
var redisConnection = "[cache-name].redis. cache.windows.net:6380,password=[password-here],ss1=True,abortConnect=Fa1se";

var connectionString = ConnectionMu1tip1exer.Connect(connectionString);

```

## Develop for Storage on CDNs
Los CDNs pueden ser aplicados en App Services y Storage Account.

red de entrega de contenido (CDN) es una red distribuida de servidores que puede proporcionar contenido web a los usuarios de manera eficaz. Las redes CDN guardan contenido almacenado en caché en servidores perimetrales en ubicaciones de **puntos de presencia o Puntos de distribución(POP)** que están cerca de los usuarios finales, para minimizar la latencia.

### Ventajas

- Mejor rendimiento y experiencia
- Gran escalado para mejorar la administración de cargas instantáneas pesadas
- Distribución de las solicitudes de usuario y entrega del contenido directamente desde los servidores perimetrales, de forma que se envía menos tráfico al servidor de origen.

### Controlar el comportamiento de almacenamiento en caché
Dos mecanismos para almacenar en caché los archivos.

- **Reglas de caché**: pueden ser globales (se aplica a todo el contenido desde un punto de conexión especificado) o personalizadas. Las reglas personalizadas se aplican a rutas de acceso y extensiones de archivo específicas.
- **Almacenamiento en caché de cadena de consulta**: permite configurar cómo responde Azure CDN en una cadena de consulta.

### Filtrado geográfico
permite autorizar o bloquear contenido en países o regiones específicos, según el código de país o región.

### Proveedores

- Microsoft
- Akamai
- Verizon

# Maquinas virtuales
El uso de **Azure Spot Virtual Machines** le permite aprovechar nuestra capacidad no utilizada con un ahorro de costos significativo. En cualquier momento en que Azure necesite recuperar la capacidad, la infraestructura de Azure expulsará las máquinas virtuales al contado de Azure. Por lo tanto, Azure Spot Virtual Machines es ideal para cargas de trabajo que pueden manejar interrupciones como trabajos de procesamiento por lotes, entornos de desarrollo/prueba, grandes cargas de trabajo de cómputo y más. Consulte el documento de Microsoft

# Plantillas de Resource Manager
Para implementar repetidamente sus soluciones en la nube y saber que su infraestructura se encuentra en un estado confiable. Puede automatizar las implementaciones y usar la práctica de infraestructura como código. En el código, debe definir la infraestructura que va a implementar. Así pues, el código de infraestructura se convierte en parte del proyecto. Al igual que el código de la aplicación, puede almacenar el código de infraestructura en un repositorio de origen y agregarle un número de versión. 

Para implementar la infraestructura como código para las soluciones de Azure, use las plantillas de Azure Resource Manager (plantillas de ARM). La plantilla es un archivo de notación de objetos JavaScript (JSON) que contiene la infraestructura y la configuración del proyecto. La plantilla usa sintaxis declarativa, lo que permite establecer lo que pretende implementar sin tener que escribir la secuencia de comandos de programación para crearla. En la plantilla se especifican los recursos que se van a implementar y las propiedades de esos recursos.

## Caracteristicas:

- **Sintaxis declarativa**: permiten crear e implementar una infraestructura de Azure completa de forma declarativa.
- **Resultados repetibles**: puede implementar la misma plantilla varias veces y obtener los mismos tipos de recursos en el mismo estado.
- **Orquestación**: Resource Manager se encarga de gestionar la implementación de recursos interdependientes para que se creen en el orden correcto. 

## Archivo de plantilla

Dentro de la plantilla, puede escribir expresiones de plantilla que aumentan las capacidades de JSON. Estas expresiones hacen uso de las funciones que proporciona Resource Manager.

La plantilla contiene las secciones siguientes:

- **Parámetros**: proporcione valores durante la implementación que permitan usar la misma plantilla con entornos diferentes.

- **Variables**: defina los valores que se reutilizan en las plantillas. Se pueden crear a partir de valores de parámetro.

- **Funciones definidas por el usuario**: cree funciones personalizadas que simplifiquen la plantilla.

- **Recursos**: especifique los recursos que se van a implementar.

- **Salidas**: devuelva valores de los recursos implementados.