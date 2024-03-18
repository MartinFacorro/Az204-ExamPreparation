# Module 08: Implementación de API Management
Es un servicio que esta en el medio, entre las aplicación y las API, sirve para protección y limitación de las API. Es un gateway ya que enruta las peticiones hacia las API correspondientes. Por lo que gestiona APIs, políticas, parámetros, suscripción para desarrolladores.
Cada API consta de una o varias operaciones y se puede agregar a uno o varios productos.

### Componentes
Se compone de una puerta de enlace API, un plano de administración y un portal para desarrolladores.

### Productos
Se usan para administrar la visibilidad de productos a los desarrolladores. Tiene los siguientes grupos invariables del sistema:

- **Administradores**: controlan las instancias del servicio API Management y crean las API, las operaciones y los productos que los desarrolladores usan.
- **Desarrolladores**: usuarios autenticados del portal para desarrolladores que compilan aplicaciones mediante las API.
- **Invitados**: usuarios del portal para desarrolladores no autenticados.  Se les concede determinado acceso de solo lectura con la posibilidad de ver API pero no llamarlas.

### Puertas de enlace de API
La puerta de enlace de API Management (también denominada plano de datos o tiempo de ejecución) es el componente de servicio responsable de las solicitudes de API de proxy, la aplicación de directivas y la recopilación de telemetría. Se ubica entre los clientes y los servicios. Actúa como un proxy inverso, enrutando las solicitudes de los clientes a los servicios. También puede realizar diversas tareas transversales como la autenticación, la terminación SSL y la limitación de velocidad.

### Puertas de enlace Administradas y autohospedadas

- **Administrada**: es el componente de puerta de enlace predeterminado que se implementa en Azure para cada instancia de API Management en cada nivel de servicio. Todo el tráfico de API fluye a través de Azure, independientemente de dónde se hospeden los back-end que implementan las API.
- **Autohospedada** : versión opcional y en contenedores de la puerta de enlace administrada predeterminada. Útil para escenarios híbridos y de varias nubes en los que es necesario ejecutar las puertas de enlace fuera de Azure en los mismos entornos donde se hospedan los back-end de API.

### Directivas / Politicas
Permiten al publicador cambiar el comportamiento de la API a través de la configuración. Las directivas son una colección de declaraciones que se ejecutan secuencialmente en la solicitud o respuesta de una API. Se aplican en la puerta de enlace que se encuentra entre el consumidor de la API y la API administrada. La puerta de enlace recibe todas las solicitudes y normalmente las reenvía sin modificar a la API subyacente. Una directiva puede aplicar cambios a la solicitud de entrada y a la respuesta de salida.

### Directivas Avanzadas
Esta unidad proporciona una referencia para las siguientes directivas de API Management:

- **Flujo de control:** aplica condicionalmente instrucciones de directiva basadas en los resultados de la evaluación de expresiones booleanas.
- **Reenviar solicitud** : reenvía la solicitud al servicio back-end.
- **Limitar la simultaneidad**: evita que las directivas delimitadas las ejecute simultáneamente un número de solicitudes mayor que el especificado.
- **Registro en centro de eventos:** envía mensajes en el formato especificado a un centro de eventos definido por una entidad de registrador.
- **Mock response (Simular respuesta):** anula la ejecución de la canalización y devuelve la respuesta ficticia directamente al llamador.
- **Reintentar :** reintenta ejecutar las instrucciones de directiva adjuntas, si y hasta que se cumple la condición. La ejecución se repite en los intervalos de tiempo especificados y hasta el número de reintentos indicado.
