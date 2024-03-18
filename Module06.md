# Module 06: Implementación de la autenticación y autorización de usuarios

## Plataforma de identidad(v2.0)
Es un conjunto de herramientas que incluye un servicio de autenticación, bibliotecas de código abierto y herramientas de administración de aplicaciones.

Permite realizar configuraciones desde el portal. Soporta Auth2.0 y OpenAI lo que permite autenticarse desde múltiples servicios corporativos, de servicios federados.

La plataforma esta formada por:

- Servicio de autenticación compatible con los estándares OAuth 2.0 y OpenID Connect, permite autenticar: 
  + Cuentas profesionales o educativas
  + Cuentas personales de Microsoft
  + Cuentas sociales o locales
- Bibliotecas de código abierto
- Portal de administración de aplicaciones
- PowerShell y API de configuración de aplicaciones

**Service Principal**: objeto dentro del Active Directory. Al referirnos a aplicaciones creamos un Service Principal para registrar aplicaciones. 

**Objeto de aplicación**: se usa como plantilla o plano técnico para crear uno o varios objetos de entidad de servicio. La entidad de servicio se crea en todos los inquilinos en los que se utiliza la aplicación. De forma similar a una clase en la programación orientada a objetos, el objeto de aplicación tiene algunas propiedades estáticas que se aplican a todas las entidades de servicio creadas (o instancias de aplicación).

El objeto de aplicación describe **tres aspectos** de una aplicación: 

- la forma en que el servicio emite tokens para tener acceso a la aplicación 
- los recursos a los que la aplicación podría necesitar tener acceso
- las acciones que puede realizar la aplicación

Al crear una aplicación en el portar se crea:
ObjectID: contiene toda la configuración de seguridad en el tenant de la aplicación.
Application ID: Id dentro del Azure Active Directory.

1 Object Id = 1 aplicación

1 aplicación = múltiples aplication ID

## Permisos
Se representa como un valor de cadena. Admite dos tipos de permisos:

**Delegados**: se utilizan en aplicaciones que tienen un usuario con la sesión iniciada, el usuario o un administrador dan su consentimiento para los permisos que la aplicación requiere.

**Aplicación**: los usan las aplicaciones que se ejecutan sin la presencia de un usuario con la sesión iniciada, aplicaciones que se ejecutan como demonios o servicios en segundo plano. Solo un administrador puede dar su consentimiento para los permisos de acceso exclusivo de aplicación.

**Consentimientos**:

- **Usuario estático**: debe especificar todos los permisos que necesita en la configuración de la aplicación en Azure Portal.
- **Incremental y dinámico**: solicita permisos de forma incremental, puede solicitar un conjunto mínimo de permisos por adelantado y solicitar más con el tiempo, a medida que el cliente use características de aplicación adicionales.
- **Administrador**: si la aplicación necesita acceder a determinados permisos con privilegios elevados.

## Acceso condicional
Permite que los desarrolladores y clientes empresariales protejan los servicios de diversas formas, entre las que se incluyen las siguientes:

- Autenticación multifactor
- Autorización para que solo los dispositivos inscritos en Intune accedan a servicios específicos
- Restricción de ubicaciones de usuario e intervalos IP

## MSAL (Microsoft Auth Library)
Liberia que se utiliza dentro de la aplicación a nivel de código. Soporta .net, JavaScript, Python, Android y iOS. Permite que los desarrolladores adquieran tokens desde la Plataforma de identidad de Microsoft para autenticar usuarios y acceder a API web protegidas.

es posible adquirir un token desde muchos tipos de aplicación: aplicaciones web, API web, aplicaciones de una sola página (JavaScript), aplicaciones móviles y nativas, así como aplicaciones del lado servidor y demonios.

### Flujos de autenticación

**Código de autorización**	Las aplicaciones nativas y web obtienen tokens de forma segura en el nombre del usuario.
**Credenciales de cliente**	Las aplicaciones de servicio se ejecutan sin interacción del usuario.
**En nombre de**	La aplicación llama a una API web o de servicio, que a su vez llama a Microsoft Graph.
**Implícita**	Se usa en aplicaciones basadas en explorador.
**Código del dispositivo**	Habilita el inicio de sesión en un dispositivo mediante otro dispositivo que tiene un explorador.
**Integrado en Windows**	Los equipos Windows adquieren de forma silenciosa un token de acceso cuando están unidos a un dominio.
**Interactive**	Las aplicaciones móviles y de escritorio llaman a Microsoft Graph nombre de un usuario.
**Nombre de usuario y contraseña**	La aplicación inicia la sesión de un usuario con su nombre de usuario y contraseña.


**Aplicaciones cliente públicas**: aplicaciones que se ejecutan en dispositivos o equipos de escritorio o en un explorador web. No son de confianza para mantener de manera segura secretos de aplicación, por lo que solo tienen acceso a API web en nombre del usuario (solo admiten flujos de cliente públicos).

**Aplicaciones cliente confidenciales**: se ejecutan en servidores (aplicaciones web, aplicaciones de API web o incluso aplicaciones de servicio o demonio). Se consideran de acceso difícil pueden mantener un secreto de aplicación.

## Firmas compartidas / Identidad Administrada
Una Firma de acceso compartido (SAS) es un URI que concede derechos de acceso restringidos a recursos de Azure Storage. Eliminan la necesidad de administrar las credenciales para los desarrolladores. 

Existen tres tipos de firmas de acceso compartido

- **SAS de Delegación de Usuario**: la más recomendable. se protege con las credenciales de Azure Active Directory y también por los permisos especificados para la SAS. Una SAS de delegación de usuarios solo se aplica a Blob Storage.
- **SAS de Servicio**: se protege con la clave de cuenta de almacenamiento. Una SAS de servicio delega el acceso a un recurso en los servicios de Azure Storage siguientes: Blob Storage, Queue Storage, Table Storage o Azure Files.
- **SAS de Cuenta**: se protege con la clave de cuenta de almacenamiento. Delega el acceso a los recursos en uno o varios de los servicios de almacenamiento. Proporcionan una identidad administrada automáticamente en Azure Active Directory (Azure AD) para que las aplicaciones la utilicen al conectarse a los recursos que admiten la autenticación de Azure AD. Las aplicaciones pueden usar identidades administradas para obtener tokens de Azure AD sin necesidad de administrar credenciales.

## Microsoft Graph
Nos brinda información sobre todo el ecosistema de Microsoft 365, proporciona un modelo de programación unificado para acceder ad atos en Microsoft 365, Windows 10 y Enterprise Mobility + Security. Facilita el acceso y el flujo de datos y cómo crear consultas mediante REST y código. Crear aplicaciones para organizaciones y consumidores que interactúen con millones de usuarios.

Tres componentes principales facilitan el acceso y el flujo de datos:

- **Microsoft Graph API** ofrece un único punto de conexión. Puede usar las API REST o los SDK para acceder al punto de conexión. También incluye un conjunto eficaz de servicios que permiten administrar la identidad, el acceso, el cumplimiento y la seguridad de usuarios y dispositivos, y ayudan a proteger a las organizaciones frente a la filtración o la pérdida de datos.
- **Conectores de Microsoft Graph** funcionan en la dirección de entrada y entregan datos externos en la nube de Microsoft en aplicaciones y servicios, con el fin de mejorar las experiencias de Microsoft 365, como Búsqueda de Microsoft. Existen conectores para muchos orígenes de datos de uso frecuente, como Box, Google Drive, Jira y Salesforce.
- **Microsoft Graph Data Connect** conjunto de herramientas para simplificar la entrega de datos de Microsoft Graph en almacenes de datos de Azure conocidos de forma segura y escalable.
