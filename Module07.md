# Module 07:Implementar seguridad en soluciones en la nube

## Azure Key Vault
Permite gestionar secretos. Asiste en proteger mejor sus aplicaciones y cómo configurar y recuperar secretos mediante la CLI de Azure.

Un **secreto** es todo aquello cuyo acceso desea controlar de forma estricta, como las claves API, las contraseñas, los certificados o las claves criptográficas.

Para realizar cualquier operación con Key Vault, deberá autenticarse. Hay tres formas de autenticarse en Key Vault:

- **Identidades administradas** de recursos de Azure: cuando implementa una aplicación en una máquina virtual,puede asignar una identidad a la máquina virtual que tiene acceso a Key Vault. También puede asignar identidades a otros recursos de Azure. Azure rota automáticamente el secreto de cliente de la entidad de servicio asociado con la identidad. Este enfoque es un *procedimiento recomendado*.
- **Entidad de servicio y certificado**: puede usar una entidad de servicio y un certificado asociado que tenga acceso a Key Vault. *No se recomienda este enfoque porque el propietario o el desarrollador de la aplicación debe girar el certificado*.
- Entidad de servicio y secreto:puede usar una entidad de servicio y un secreto para autenticarse en Key Vault, *No se recomienda este enfoque porque Es difícil girar automáticamente el secreto de arranque*

### Cifrado en transito
Key Vault aplica el protocolo Seguridad de la capa de transporte (TLS) para proteger los datos en el tránsito entre Azure Key Vault y los clientes. Los clientes negocian una conexión TLS con Azure Key Vault. TLS proporciona una autenticación sólida, privacidad de mensajes e integridad (lo que permite la detección de la manipulación, interpretación y falsificación de mensajes), interoperabilidad, flexibilidad de algoritmo, y facilidad de implementación y uso.

**Confidencialidad directa total (PFS)** protege las conexiones entre los sistemas cliente de los clientes y los servicios en la nube de Microsoft mediante claves únicas.

### Procedimientos recomendados

- **Uso de almacenes de claves distintos**: usar un almacén por aplicación y por entorno (desarrollo, preproducción y producción).
- **Control del acceso al almacén**: proteger el acceso a sus almacenes de claves permitiéndoselo solo a aplicaciones y usuarios autorizados.
- **Copia de seguridad**: cree copias de seguridad periódicas del almacén al actualizar, eliminar o crear objetos dentro de este.
- **Registro**: activar el registro y las alertas.
- **Opciones de recuperación**: active la eliminación temporal y la protección de purga si desea protegerse contra la eliminación forzada del secreto.

### Autenticación
(Azure Key Vault)Funciona junto con Azure Active Directory, que es responsable de autenticar la identidad de cualquier entidad de seguridad determinada.

En el caso de las aplicaciones, hay dos maneras de obtener una entidad de servicio:

- Habilite una **identidad administrada** asignada por el sistema para la aplicación, lo que permite la administración interna entre la entidad de servicio de la aplicación y la autentica automáticamente con otros servicios.
- (Si nos es posible usar identidad administrada) registre la aplicación con su inquilino de Azure AD. El registro también crea un segundo objeto de aplicación que identifica la  aplicación en todos los inquilinos.

## Identidades administradas
Se utilizan cuando dos servicios se tienen que comunicar entre si, y un servicio necesita permisos del otro, sin necesidad de administrar las credenciales.

La aplicación puede tener dos tipos de identidades:

- **Identidad asignada por el sistema** está asociada al almacén de configuración. Solo puede tener una identidad asignada por el sistema.
- **Identidad asignada por el usuario** puede tener varias identidades asignadas por el usuario.

## Implementando App Configuration
Es un servicio donde se concentran todas las configuraciones de nuestras aplicaciones. Con lo que se evita que cada aplicación busque sus propias configuraciones, en cambio, se puede mediante el uso de un único App Config, centralizar todas las configuraciones.
Usa una librería especifica, que se usa mediante código

Existe para:
- .NET
- JavaSpring
- Para otros lenguajes se puede usar una APIREST

### Clave-Valor
Las **claves** sirven como nombre de los pares clave-valor y se usan para almacenar y recuperar los valores correspondientes.

Los datos de configuración, se cifran tanto en reposo como en tránsito.
