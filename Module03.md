# Module 03: Blob storage

## Blob Storage
Solución de almacenamiento de objetos. Está optimizado para el almacenamiento de cantidades masivas de datos no estructurados. Cuenta con varios servicios de almacenamiento que podemos tener en una cuenta, por lo que es necesario crear una cuenta de Storage. Es el tipo más utilizado, para el desarrollo de aplicaciones, por ejemplo imagenes para mostrar en una pagina web. Se guardan en espacios logicos, es decir, en contenedores.

Ofrece dos niveles de rendimiento de cuentas de almacenamiento, *estándar* y *premium*.

- **Estándar:** se trata de la cuenta de uso general v2 estándar y se recomienda para la mayoría de los escenarios en los que se usa Azure Storage.

Dentro del nivel ***Standard***: Cuenta con:
**Files**: similar a File Server, donde se comparten archivos mediante SMB. Tambien se pueden realizar llamadas REST. SE puede sincronizar tanto con servidores OnPremise como con servidores onCloud.

- **Premium:** (son cuentas exclusivas) no se comparte con otros servicios, para ser más efectivo y con mejor renidimiento. Si crea una cuenta Premium, puede elegir entre **tres tipos de cuenta**:

  - **blobs en bloques**: de bloque, se guardan secuencialemente, se usa para videos, imagenes, videos.
  - **blobs en páginas**: utilizados para guardar discos(no administrados) de maquinas virtuales.
  - **recursos compartidos de archivos**: similar a File server(SMB)

**Append**: utilizados guardar archivos secuenciales, por ejemplo guardar logs, en donde se incorpora información a la ya existente.

**Tables**: Base de datos NoSQL, para guardar información de tipo llave:valor. Escalan dinaicamente, son de respuesta rapidas, siempre y cuando la información que se almacene sea especifica, como ser configuracions.

**Queues**: Cola de mensajes, para encolar mensajes y posteriormente ir desencolando. La idea es desacoplar servicios, para tener bajar interdependencia entre aplicaciones o servicios.

### Permisos de Acceso al Blob Storage

- Permisos a nivel contenedor
- Permisos basados RBAC(Role-Based Acces Control)

La seguridad se aplica en transito, con SSL/TLS.

Toda la información se encripta en reposo, y no se puede deshabilitar, solo se permite asignar quien administrara las llaves de des-encriptacion.

### Ciclo de Vida de Blob Storage
Existen 4 capas de acceso:

- Premiun(visto antes): No es posible cambiar a otras capas.

Es posible intercambar entre capas
- **Hot**(*Dentro de Blob Standard*): para acceder a los archivos de manera frecuente. Alto costo de almacenamiento es caro, bajo costo de acceso.
- **Cold**(*Dentro de Blob Standard*): el acceso es para archivos no  tan frecuentes. Menor costo de almacenamiento, y mayor costo de acceso(Comparado a HOT).
- **Archive**(*Dentro de Blob Standard*): sin acceso al archivo, pero se almacena por temas de cumplimiento o auditorias. Es la capa menos costosa de almacenamiento, pero la más costosa de acceder. Para acceder a un archivo se debe "rehidratar" el archivo(proceso que puede horas en base a la prioridad [Alta o Normal]) pasandolo a capa Cold o Hot.


### Detección de directivas de ciclo de vida de Blob Storage
Colección de reglas en un documento JSON. Cada definición de regla incluye un conjunto de filtros y un conjunto de acciones.


```JSON
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

**rules**: Se requiere al menos una regla en una directiva. Puede definir hasta 100 reglas en una directiva.

**name**: String Un nombre de regla puede incluir hasta 256 caracteres alfanuméricos. El nombre de regla  distingue mayúsculas de minúsculas. Debe ser único  dentro de una directiva. True
**enabled**: Boolean Un valor booleano opcional para permitir que una regla se deshabilite de forma temporal. El valor predeterminado es true si no se establece. False
**type**: Un valor de enumeración.El tipo válido actual es Ciclo de vida. True
**definition** Un objeto que define la regla del ciclo de vida Cada definición se compone de un conjunto de filtros y un conjunto de acciones. True

### Acciones de regla
Las acciones se aplican a los blobs filtrados cuando se cumple la condición de ejecución.

**Acción**:

- **tierToCool** Compatible con blockBlob
- **enableAutoTierToHotFromCool** Compatible con blockBlob
- **tierToArchive**: 	Compatible con blockBlob
**delete**Compatible con blockBlob y appendBlob

Para agregar una directiva de administración del ciclo de vida con la CLI de Azure, escriba la directiva en un archivo JSON y, después, llame al comando ```az storage account management-policy create``` para crear la directiva.

### Azure File Sync
Permite mantener copia locales de archivos al mismo tiempo que en la nube. Utiliza protocolos como SMV, NFS, FTPS.

### Rehidratación de blob desde el nivel de archivo
Un blob se encuentra en el nivel de acceso de archivo, se considera que está sin conexión. Para leer o modificar los datos de un blob archivado, primero debe rehidratar el blob en un nivel en línea, ya sea el nivel de acceso frecuente o esporádico. Hay dos opciones para rehidratar un blob que se almacena en el nivel de archivo:

- **Copiar un blob archivado en un nivel en línea** ```Copy blob```
- **Cambio del nivel de acceso de un blob a un nivel en línea** ```Set Blob Tier```

#### Prioridad
Puede establecer la prioridad de la operación de rehidratación a través del encabezado ```x-ms-rehydrate-priority ```

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
