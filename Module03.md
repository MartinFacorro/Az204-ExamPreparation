# Module 03: Desarrollo de soluciones que usan Blob Storage

## Exploración de Azure Blob Storage

**Blob Storage**: Solución de almacenamiento de objetos. Está optimizado para el almacenamiento de cantidades masivas de datos no estructurados. Cuenta con varios servicios de almacenamiento que podemos tener en una cuenta, por lo que es necesario crear una cuenta de Storage. Es el tipo más utilizado, para el desarrollo de aplicaciones, por ejemplo imágenes para mostrar en una pagina web. Se guardan en espacios lógicos, es decir, en contenedores.

Blob Storage está diseñado para:

* Visualización de imágenes o documentos directamente en un explorador.
* Almacenamiento de archivos para acceso distribuido.
* Streaming de audio y vídeo.
* Escribir en archivos de registro.
* Almacenamiento de datos para copia de seguridad y restauración, recuperación ante desastres y archivado.
* Almacenamiento de datos para el análisis en local o en un servicio hospedado de Azure.

Los usuarios o las aplicaciones cliente pueden acceder a objetos en Blob Storage a través de HTTP/HTTPS, desde cualquier lugar del mundo. Se puede acceder a los objetos de Blob Storage mediante la API REST de Azure Storage, Azure PowerShell, la CLI de Azure o una biblioteca de cliente de Azure Storage.


### Tipos de Blob Storage

Ofrece dos niveles de rendimiento de cuentas de almacenamiento, *estándar* y *premium*.

- **Estándar:** se trata de la cuenta de uso general v2 estándar y se recomienda para la mayoría de los escenarios en los que se usa Azure Storage.

Dentro del nivel ***Standard***: Cuenta con:
**Files**: similar a File Server, donde se comparten archivos mediante SMB. También se pueden realizar llamadas REST. SE puede sincronizar tanto con servidores OnPremise como con servidores onCloud.

- **Premium:** (son cuentas exclusivas) no se comparte con otros servicios, para ser más efectivo y con mejor rendimiento. Si crea una cuenta Premium, puede elegir entre **tres tipos de cuenta**:

  - **blobs en bloques**: de bloque, se guardan secuencialemente, se usa para videos, imágenes, videos.
  - **blobs en páginas**: utilizados para guardar discos(no administrados) de maquinas virtuales.
  - **recursos compartidos de archivos**: similar a File server(SMB)

**Append**: utilizados guardar archivos secuenciales, por ejemplo guardar logs, en donde se incorpora información a la ya existente.

**Tables**: Base de datos NoSQL, para guardar información de tipo llave:valor. Escalan dinámicamente, son de respuesta rápidas, siempre y cuando la información que se almacene sea especifica, como ser configuraciones.

**Queues**: Cola de mensajes, para encolar mensajes y posteriormente ir desencolando. La idea es desacoplar servicios, para tener bajar interdependencia entre aplicaciones o servicios.

### Detección de los tipos de recursos de Azure Blob Storage

ofrece tres tipos de recursos:

* Cuenta de almacenamiento.
* Un contenedor en la cuenta de almacenamiento
* Un blob en un contenedor

#### Cuentas de almacenamiento

Una cuenta de almacenamiento de Azure es un recurso de nivel superior que se utiliza para almacenar datos. Cada cuenta de almacenamiento tiene un espacio de nombres único y se puede acceder a ella desde cualquier lugar del mundo a través de HTTP o HTTPS. Puede usar cuentas de almacenamiento para almacenar datos de objetos, archivos, tablas y colas.

#### Contenedores

Un contenedor organiza un conjunto de blobs, de forma parecida a un directorio en un sistema de archivos. Una cuenta de almacenamiento puede contener un número ilimitado de contenedores y un contenedor puede almacenar un número ilimitado de blobs.

n nombre de contenedor debe ser un nombre DNS válido, ya que forma parte del URI (identificador uniforme de recursos) único que se usa para direccionar el contenedor o sus blobs. Siga estas reglas al asignar un nombre a un contenedor:

* Los nombres de contenedor pueden tener entre 3 y 63 caracteres.
* Los nombres de contenedor deben comenzar por una letra o un número, y solo pueden contener letras en minúscula, números y el carácter de guión (-).
* En los nombres de contenedor no se permiten dos o más guiones consecutivos.

El URI de un contenedor es similar a:

``` BASH
Bash

https://myaccount.blob.core.windows.net/mycontainer

```

#### Blobs

Un blob es un archivo de cualquier tipo y tamaño. Cada blob se almacena como un solo objeto en Azure Storage. Un contenedor puede tener un número ilimitado de blobs. Los blobs se pueden acceder mediante HTTP o HTTPS.

Azure Storage admite tres tipos de blobs:

* Los **blobs en bloques** almacenan texto y datos binarios. Los blobs en bloques se componen de bloques de datos que se pueden administrar de forma individual. Los blobs en bloques pueden almacenar hasta aproximadamente 190,7 TiB.
* Los **blobs en anexos** constan de bloques, como los blobs en bloques, pero están optimizados para operaciones de anexión. Los blobs en anexos resultan muy convenientes para escenarios como el registro de datos de máquinas virtuales.
* Los **blobs en páginas** almacenan archivos de acceso aleatorio con un tamaño de hasta 8 TB. Los blobs en páginas almacenan los archivos del disco duro virtual (VHD) y sirven como discos para las máquinas virtuales de Azure.

### Exploración de las características de seguridad de Azure Storage

Azure Storage proporciona una serie de características de seguridad para proteger los datos almacenados en Azure Storage. Estas características incluyen:


* Todos los datos (incluidos los metadatos) escritos en Azure Storage se cifran automáticamente con Storage Service Encryption (SSE).
* Microsoft Entra ID y el control de acceso basado en roles (RBAC) son compatibles con Azure Storage para las operaciones de administración de recursos y las operaciones de datos, como se indica a continuación:
  * Puede asignar roles de RBAC en el ámbito de la cuenta de almacenamiento para las entidades de seguridad y utilizar Microsoft Entra ID para autorizar las operaciones de administración de recursos, como la administración de claves.
  * La integración con Microsoft Entra se admite para las operaciones de datos de cola y blob. Puede asignar roles de RBAC en el ámbito de una suscripción, un grupo de recursos, una cuenta de almacenamiento, un contenedor individual o una cola a una entidad de seguridad o identidad administrada para los recursos de Azure.
* Los datos se pueden proteger en tránsito entre una aplicación y Azure usando cifrado de cliente, HTTPS o SMB 3.0.
* Se puede establecer el cifrado de los discos de datos y del sistema operativo utilizados por Azure Virtual Machines mediante Azure Disk Encryption.
* Se puede conceder acceso delegado a los objetos de datos de Azure Storage mediante las firmas de acceso compartido.

#### Cifrado de Azure Storage para datos en reposo

Azure Storage cifra automáticamente los datos en reposo mediante Storage Service Encryption (SSE). SSE cifra los datos antes de escribirlos en Azure Storage y descifra los datos antes de devolverlos a la aplicación que los solicita. SSE se aplica a todos los tipos de almacenamiento de Azure: blobs, archivos, tablas y colas.

### Detección del hospedaje de sitios web estáticos en Azure Storage

Azure Storage admite la hospedaje de sitios web estáticos (HTML, CSS, JavaScript y archivos de imagen) directamente desde un contenedor de almacenamiento llamado ```$web```. Cuando hospeda un sitio web en Azure Storage, puede usar un nombre de dominio personalizado con HTTPS, lo que le permite usar un nombre de dominio personalizado para el sitio web en lugar de un nombre de dominio de Azure Storage.

## Administración del ciclo de vida de Azure Blob Storage

Los conjuntos de datos tienen ciclos de vida únicos. Al principio del ciclo de vida, las personas acceden con frecuencia a algunos datos. Pero la necesidad de acceso desciende drásticamente a medida que los datos se hacen más antiguos. Algunos datos permanecen inactivos en la nube y, una vez almacenados, no se suele acceder a ellos.

### Niveles de acceso

Azure Storage ofrece distintos niveles de acceso, lo que permite almacenar datos de objeto de blob de la manera más rentable. Los niveles de acceso disponibles incluyen:

* **Frecuente**: Optimizado para almacenar datos que se consultan con frecuencia.
* **Esporádico**: optimizado para almacenar datos a los que se accede con poca frecuencia; se almacenan durante un mínimo de 30 días.
* **Nivel de acceso aislado**: está optimizado para almacenar datos a los que se accede con poca frecuencia y al menos durante 90 días. El nivel de acceso aislado tiene menores costes de almacenamiento y mayores costes de acceso en comparación con el nivel de acceso esporádico.
* **Archivo**: optimizado para almacenar datos a los que se accede muy pocas veces y almacenados durante al menos 180 días con requisitos de latencia flexibles, del orden de horas.


### Detección de directivas de ciclo de vida de Blob Storage
Colección de reglas en un documento JSON. Cada definición de regla incluye un conjunto de filtros y un conjunto de acciones.


```JSON

JSON
{
  "rules": [
    {
      "name": "rule1",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {...}
    },
    {
      "name": "rule2",
      "type": "Lifecycle",
      "definition": {...}
    }
  ]
}
```

Una directiva es una colección de reglas:

| Nombre de parámetro | Tipo de parámetro | Notas |
| --- | --- | --- |
| rules | Una matriz de objetos de regla | Se requiere al menos una regla en una directiva. Puede definir hasta 100 reglas en una directiva. |


Cada regla de la directiva tiene varios parámetros:

| Nombre de parámetro | Tipo de parámetro | Notas | Obligatorio |
| --- | --- | --- | --- |
| name | String | Un nombre de regla puede incluir hasta 256 caracteres alfanuméricos. El nombre de regla distingue mayúsculas de minúsculas. Debe ser único dentro de una directiva. | True |
| enabled | Boolean | Un valor booleano opcional para permitir que una regla se deshabilite de forma temporal. El valor predeterminado es true si no se establece. | False |
| type | Un valor de enumeración | El tipo válido actual es Ciclo de vida. | True |
| definition | Un objeto que define la regla del ciclo de vida | Cada definición se compone de un conjunto de filtros y un conjunto de acciones. | True |

#### Reglas

Las reglas de ciclo de vida de Blob Storage se componen de dos partes: 

* **conjunto de filtros** limita las acciones de regla a un determinado conjunto de objetos dentro de un contenedor o nombres de objetos. Los filtros limitan las acciones de regla a un subconjunto de blobs dentro de la cuenta de almacenamiento. Si se define más de un filtro, un operador AND lógico se ejecutará en todos los filtros. Entre los filtros están los siguientes:

| Nombre de filtro | Tipo | Es obligatorio |
| --- | --- | --- |
| blobTypes | Una matriz de valores de enumeración predefinidos. | Sí |
| prefixMatch | Una matriz de cadenas de prefijos con los que debe hacer coincidencias. Cada regla puede definir hasta 10 prefijos. Una cadena de prefijos debe comenzar con el nombre de un contenedor. | No |
| blobIndexMatch | Una matriz de valores de diccionario que se compone de las condiciones de clave y valor de la etiqueta de índice de blobs con las que debe haber coincidencias. Cada regla puede definir hasta 10 condiciones de etiqueta de índice de blobs. | No |


* **conjunto de acciones** aplica las acciones de nivel o eliminación al conjunto filtrado de objetos. Las acciones se aplican a los blobs filtrados cuando se cumple la condición de ejecución.

La administración del ciclo de vida admite el cambio de niveles y la eliminación de blobs e instantáneas de blob. Defina al menos una acción para cada regla en los blobs o las instantáneas de blob.

| Acción | Blob de base | Instantánea | Versión |
| --- | --- | --- | --- |
| tierToCool | Compatible con blockBlob | Compatible | Compatible |
| enableAutoTierToHotFromCool | Compatible con blockBlob | No compatibles | No compatible |
| tierToArchive | Compatible con blockBlob | Compatible | Compatible |
| delete | Compatible con blockBlob y appendBlob | Compatible | Compatible |


```Nota

 Nota
Si define más de una acción en el mismo blob, la administración del ciclo de vida aplica la acción menos cara al blob. Por ejemplo, la acción ```delete``` es más económica que la acción ```tierToArchive```. La acción ```tierToArchive``` es más económica que la acción ```tierToCool```.

```

Si define más de una acción en el mismo blob, la administración del ciclo de vida aplica la acción menos cara al blob. Por ejemplo, la acción delete es más económica que la acción tierToArchive. La acción tierToArchive es más económica que la acción tierToCool.

## Rehidratación de blob desde el nivel de archivo
Un blob se encuentra en el nivel de acceso de archivo, se considera que está sin conexión y no se puede leer ni modificar. Para leer o modificar los datos de un blob archivado, primero debe rehidratar el blob en un nivel en línea, ya sea el nivel de acceso frecuente o esporádico. Hay dos opciones para rehidratar un blob que se almacena en el nivel de archivo:

- **Copiar un blob archivado en un nivel en línea** ```Copy blob```
- **Cambio del nivel de acceso de un blob a un nivel en línea** ```Set Blob Tier```

La rehidratación de un blob de un nivel de acceso de archivo puede tardar varias horas en completarse. Microsoft recomienda rehidratar blobs más grandes para obtener un rendimiento óptimo. La rehidratación de varios blobs pequeños de forma simultánea puede requerir tiempo adicional.


#### Prioridad de la rehidratación
Puede establecer la prioridad de la operación de rehidratación a través del encabezado ```x-ms-rehydrate-priority ```

Las opciones de prioridad de rehidratación incluyen:

* **Prioridad estándar**: La solicitud de rehidratación se procesa en el orden en que se recibió y puede tardar hasta 15 horas.
* **Prioridad alta**: La solicitud de rehidratación tiene prioridad con respecto a las solicitudes de prioridad estándar y puede completarse en menos de una hora para objetos con un tamaño inferior a 10 GB.

## Trabajo con Azure Blob Storage a través de código

## Exploración de la biblioteca cliente de Azure Blob Storage

Las bibliotecas cliente de Azure Storage para .NET ofrecen una interfaz práctica para realizar llamadas a Azure Storage. La versión más reciente de la biblioteca cliente de Azure Storage es la versión 12.x. Microsoft recomienda usar la versión 12.x para las nuevas aplicaciones.

Esta tabla enumera las clases básicas con una breve descripción:

| Clase | Descripción |
| --- | --- |
| ```BlobServiceClient``` | Representa la cuenta de almacenamiento y proporciona operaciones para recuperar y configurar las propiedades de la cuenta y para trabajar con contenedores de blobs en la cuenta de almacenamiento. |
| ```BlobContainerClient``` | Representa un contenedor de blobs específico y proporciona operaciones para trabajar con el contenedor y los blobs dentro. |
| ```BlobClient``` | Representa un blob específico y proporciona operaciones generales para trabajar con el blob, incluidas las operaciones para cargar, descargar, eliminar y crear instantáneas. |
| ```AppendBlobClient``` | Representa un blob en anexos y proporciona operaciones específicas de blobs en anexos, como anexar datos de registro. |
| ```BlockBlobClient``` | Representa un blob en bloques y proporciona operaciones específicas de blobs en bloques, como el almacenamiento provisional y luego la confirmación de bloques de datos. |

Los siguientes paquetes contienen las clases que se usan para trabajar con recursos de datos de Blob Storage:

* Azure.Storage.Blobs: contiene las clases principales (objetos de cliente) que puede usar para operar en el servicio, los contenedores y los blobs.
* Azure.Storage.Blobs.Specialized: contiene clases que puede usar para llevar a cabo operaciones específicas de un tipo de blob, como blobs en bloques.
* Azure.Storage.Blobs.Models: todas las demás clases de utilidad, estructuras y tipos de enumeración.

### Creación de un objeto de cliente de Blob Service

Trabajar con cualquier recurso de Azure mediante el SDK comienza con la creación de un objeto de cliente.

Aprenderá a crear objetos de cliente para interactuar con los tres tipos de recursos en el servicio de almacenamiento: 

* cuentas de almacenamiento, 
* contenedores y 
* blobs.

### Creación de un objeto de cliente de Blob Service

Un objeto ```BlobServiceClient``` autorizado permite a su aplicación interactuar con los recursos a nivel de la cuenta de almacenamiento. ```BlobServiceClient``` proporciona métodos para recuperar y configurar propiedades de cuenta, así como enumerar, crear y eliminar contenedores dentro de la cuenta de almacenamiento. Este objeto de cliente es el punto de partida para interactuar con los recursos de la cuenta de almacenamiento.

Ejemplo sobre como crear un objeto de cliente de Blob Service:

```C#

C#
using Azure.Identity;
using Azure.Storage.Blobs;

public BlobServiceClient GetBlobServiceClient(string accountName)
{
    BlobServiceClient client = new(
        new Uri($"https://{accountName}.blob.core.windows.net"),
        new DefaultAzureCredential());

    return client;
}
```

### Creación de un objeto BlobContainerClient

Puede usar un objeto ```BlobServiceClient``` para crear un nuevo objeto ```BlobContainerClient```. Un objeto ```BlobContainerClient``` permite interactuar con un recurso de contenedor específico. ```BlobContainerClient``` proporciona métodos para crear, eliminar o configurar un contenedor, e incluye métodos para enumerar, cargar y eliminar los blobs que contiene.

En el ejemplo siguiente se muestra cómo crear un cliente de contenedor a partir de un objeto ```BlobServiceClient``` para interactuar con un recurso de contenedor específico:

```C#

C#
public BlobContainerClient GetBlobContainerClient(
    BlobServiceClient blobServiceClient,
    string containerName)
{
    // Create the container client using the service client object
    BlobContainerClient client = blobServiceClient.GetBlobContainerClient(containerName);
    return client;
}

```

Si el trabajo está limitado a un único contenedor, puede optar por crear un objeto BlobContainerClient directamente sin usar BlobServiceClient.

```C#
C#
public BlobContainerClient GetBlobContainerClient(
    string accountName,
    string containerName,
    BlobClientOptions clientOptions)
{
    // Append the container name to the end of the URI
    BlobContainerClient client = new(
        new Uri($"https://{accountName}.blob.core.windows.net/{containerName}"),
        new DefaultAzureCredential(),
        clientOptions);

    return client;
}
```

### Creación de un objeto BlobClient

Para interactuar con un recurso de blob específico, cree un objeto ```BlobClient``` a partir de un cliente de servicio o un cliente de contenedor. Un objeto ```BlobClient``` permite interactuar con un recurso de blob específico.

En el ejemplo siguiente se muestra cómo crear un cliente de blobs para interactuar con un recurso de blob específico:

```C#
C#

public BlobClient GetBlobClient(
    BlobServiceClient blobServiceClient,
    string containerName,
    string blobName)
{
    BlobClient client =
        blobServiceClient.GetBlobContainerClient(containerName).GetBlobClient(blobName);
    return client;
}

```

### Ejercicio: Creación de recursos de Blob Storage mediante la biblioteca cliente de .NET

https://learn.microsoft.com/es-es/training/modules/work-azure-blob-storage/4-develop-blob-storage-dotnet

### Administración de metadatos y propiedades de contenedor mediante .NET

Los contenedores de blobs admiten propiedades del sistema y metadatos definidos por el usuario, además de los datos que contienen.

* **Propiedades del sistema**: en cada recurso de almacenamiento de blobs existen propiedades del sistema. Algunas se pueden leer o establecer, mientras que otras son de solo lectura. En segundo plano, algunas propiedades del sistema corresponden a ciertos encabezados HTTP estándar. La biblioteca cliente de Azure Storage para .NET mantiene estas propiedades automáticamente.

* **Metadatos definidos por el usuario**: los metadatos definidos por el usuario se componen de uno o más pares nombre-valor que especifica para un recurso de almacenamiento de blobs. Puede usar metadatos para almacenar valores adicionales con el recurso. Los valores de metadatos se proporcionan para uso personal y no afectan a cómo se comporta el recurso.

Los pares de nombre/valor de metadatos son encabezados HTTP válidos y, por tanto, deben cumplir todas las restricciones que controlan los encabezados HTTP. Los nombres de los metadatos deben ser nombres válidos de encabezado HTTP e identificadores de C# válidos, y solo pueden contener caracteres ASCII y no distinguir entre mayúsculas y minúsculas. Los valores de los metadatos que contengan caracteres distintos de ASCII deben estar codificados en Base64 o con direcciones URL.

### Recuperación de las propiedades del contenedor

Para recuperar las propiedades de contenedor, llame a uno de los métodos siguientes de la clase ```BlobContainerClient```:

* ```GetProperties```
* ```GetPropertiesAsync```

En el ejemplo de código siguiente se capturan las propiedades del sistema de un contenedor y se escriben algunos valores de propiedad en una ventana de la consola:

```C#
C#

private static async Task ReadContainerPropertiesAsync(BlobContainerClient container)
{
    try
    {
        // Fetch some container properties and write out their values.
        var properties = await container.GetPropertiesAsync();
        Console.WriteLine($"Properties for container {container.Uri}");
        Console.WriteLine($"Public access level: {properties.Value.PublicAccess}");
        Console.WriteLine($"Last modified time in UTC: {properties.Value.LastModified}");
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine($"HTTP error code {e.Status}: {e.ErrorCode}");
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}

```

### Establecimiento y recuperación de metadatos de contenedor

Puede especificar metadatos como uno o más pares nombre-valor en un recurso de blob o contenedor. Para establecer metadatos, agregue pares nombre-valor a un objeto ```IDictionary``` y, a continuación, llame a uno de los métodos siguientes de la clase BlobContainerClient para escribir los valores:

* ```SetMetadata```
* ```SetMetadataAsync```

El nombre de los metadatos debe cumplir las convenciones de nomenclatura para los identificadores de C#. Los nombres de los metadatos conservan las mayúsculas y minúsculas con las que se crearon, pero no las distinguen cuando se establecen o se leen. Si se envían dos o más encabezados de metadatos con el mismo nombre para un recurso, Blob Storage separa con comas y concatena los dos valores y devuelve el código de respuesta HTTP ```200 (OK)```.

El ejemplo de código siguiente establece los metadatos en un contenedor.

``` C#

C#

public static async Task AddContainerMetadataAsync(BlobContainerClient container)
{
    try
    {
        IDictionary<string, string> metadata =
           new Dictionary<string, string>();

        // Add some metadata to the container.
        metadata.Add("docType", "textDocuments");
        metadata.Add("category", "guidance");

        // Set the container's metadata.
        await container.SetMetadataAsync(metadata);
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine($"HTTP error code {e.Status}: {e.ErrorCode}");
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}

```

Los métodos ```GetProperties``` y ```GetPropertiesAsync``` se usan para recuperar metadatos además de propiedades como se ha mostrado anteriormente.

En el ejemplo de código siguiente se recuperan los metadatos de un contenedor.

```C#
C#

public static async Task ReadContainerMetadataAsync(BlobContainerClient container)
{
    try
    {
        var properties = await container.GetPropertiesAsync();

        // Enumerate the container's metadata.
        Console.WriteLine("Container metadata:");
        foreach (var metadataItem in properties.Value.Metadata)
        {
            Console.WriteLine($"\tKey: {metadataItem.Key}");
            Console.WriteLine($"\tValue: {metadataItem.Value}");
        }
    }
    catch (RequestFailedException e)
    {
        Console.WriteLine($"HTTP error code {e.Status}: {e.ErrorCode}");
        Console.WriteLine(e.Message);
        Console.ReadLine();
    }
}
```


### Establecimiento y recuperación de propiedades y metadatos para recursos de blob mediante REST

Para establecer y recuperar propiedades y metadatos para recursos de blob, use las operaciones REST de Azure Storage. Puede usar estas operaciones para establecer y recuperar propiedades y metadatos para contenedores y blobs.

Los contenedores y los blobs admiten metadatos personalizados, representados como encabezados HTTP. Los encabezados de metadatos se pueden establecer en una solicitud que crea un nuevo recurso de contenedor o de blob, o en una solicitud que crea explícitamente una propiedad en un recurso existente.

```x-ms-meta-name:string-value```

### Operaciones con metadatos

Los metadatos de un recurso de blob o de un recurso contenedor se pueden recuperar o establecer directamente, sin devolver ni modificar el contenido del recurso.

Tenga en cuenta que los valores de metadatos solo se pueden leer o escribir en su totalidad; no se admiten actualizaciones parciales. Cuando se establecen los metadatos de un recurso, se sobrescriben los valores de metadatos existentes para dicho recurso.

### Recuperación de propiedades y metadatos

Las operaciones GET y HEAD recuperan los encabezados de metadatos para el contenedor o el blob especificado. Estas operaciones devuelven solo los encabezados; no devuelven ningún cuerpo de respuesta. La sintaxis del URI para recuperar los encabezados de metadatos de un contenedor es la siguiente:

``` GET/HEAD https://myaccount.blob.core.windows.net/mycontainer?restype=container```

La sintaxis del URI para recuperar los encabezados de metadatos de un blob es la siguiente:

```GET/HEAD https://myaccount.blob.core.windows.net/mycontainer/myblob?comp=metadata```

### Establecer encabezados de metadatos

La operación PUT establece los encabezados de metadatos del contenedor o blob especificado, sobrescribiendo los metadatos existentes en el recurso. Cuando se llama a PUT sin incluir encabezados en la solicitud, se borran todos los metadatos existentes en el recurso.

La sintaxis del URI para establecer los encabezados de metadatos de un contenedor es la siguiente:

```PUT https://myaccount.blob.core.windows.net/mycontainer?comp=metadata&restype=container```

La sintaxis del URI para establecer los encabezados de metadatos de un blob es la siguiente:

```PUT https://myaccount.blob.core.windows.net/mycontainer/myblob?comp=metadata```

### Propiedades HTTP estándar para contenedores y blobs

Los nombres de los encabezados de metadatos están formados por el prefijo de encabezado ```x-ms-meta-``` y un nombre personalizado.

Los nombres de propiedades utilizan nombres de encabezado HTTP estándar HTTP/1.1

Los encabezados HTTP estándar admitidos en los contenedores son los siguientes:

- `ETag`
- `Last-Modified`

Los encabezados HTTP estándar admitidos en los contenedores son los blobs:

- `ETag`
- `Last-Modified`
- `Content-Length`
- `Content-Type`
- `Content-MD5`
- `Content-Encoding`
- `Content-Language`
- `Cache-Control`
- `Origin`
- `Range`







### Permisos de Acceso al Blob Storage

- Permisos a nivel contenedor
- Permisos basados RBAC(Role-Based Acces Control)

La seguridad se aplica en transito, con SSL/TLS.

Toda la información se encripta en reposo, y no se puede deshabilitar, solo se permite asignar quien administrara las llaves de des-encriptacion.

### Ciclo de Vida de Blob Storage
Existen 4 capas de acceso:

- Premiun(visto antes): No es posible cambiar a otras capas.

Es posible intercambiar entre capas
- **Hot**(*Dentro de Blob Standard*): para acceder a los archivos de manera frecuente. Alto costo de almacenamiento es caro, bajo costo de acceso.
- **Cold**(*Dentro de Blob Standard*): el acceso es para archivos no  tan frecuentes. Menor costo de almacenamiento, y mayor costo de acceso(Comparado a HOT).
- **Archive**(*Dentro de Blob Standard*): sin acceso al archivo, pero se almacena por temas de cumplimiento o auditorias. Es la capa menos costosa de almacenamiento, pero la más costosa de acceder. Para acceder a un archivo se debe "rehidratar" el archivo(proceso que puede horas en base a la prioridad [Alta o Normal]) pasándolo a capa Cold o Hot.




### Azure File Sync
Permite mantener copia locales de archivos al mismo tiempo que en la nube. Utiliza protocolos como SMV, NFS, FTPS.



## Trabajar a nivel de Código

Libreria cliente ```azure.storage.blob```

Por orden de llamada:

1. **Blob Service Client**: se utiliza para acceder a la StorageAccont, se le proporciona el ConnectionString.
2. **Blob Container Client**: ya conectado a la cuenta de Storage, se utiliza este cliente para trabajar con contenedores.
3. **Blob Client**: para trabajar con archivos, permite eliminar, descargar, crear archivos dentro de un contenedor, que a su vez esta dentro de un Storage Account.
4. **Append Blob Client**: para manipular archivos de tipo log.
5. **Block Blob Client**: para trabajar con archivos de tipo bloque.

### Operaciones con metadatos

Los metadatos de un recurso de blob o de un recurso contenedor se pueden recuperar o establecer directamente, sin devolver ni modificar el contenido del recurso.

**Recuperación de propiedades y metadatos** 

```GET/HEAD```

**Establecer encabezados de metadatos**

```PUT ```


