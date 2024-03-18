# Module 10: Soluciones basadas en mensajes

## Descubriendo colas de mensaje(Message Queues)
Se crean colas de mensajes, para permitir el procesamiento de los mensajes

Un **mensaje** ser procesado de alguna manera, y saber si se proceso correctamente o no.

Existen dos servicios:

- **Azure Service bus**:
  + Solución de tipo FIFO (First-In-First-Out)
  + Detecta duplicados
  + Mensajes de 64Kb hasta 256KB(posibilidad de extenderlo hasta 100MB en Premiun)
  + Soporta hasta 5GB de mensajes en una misma cola
- **Azure Storage Queues**:
  + Colas sencillas para soluciones rápidas
  + Soportan hasta 80 GB de mensajes en una misma cola

## Azure Service bus
Service Bus Queue es una cola de mensajes de nivel empresarial.

### Componentes
- Queues: FIFO
- Modo de recepción:
  + Recepción y borrado
  + Peek Lock
- Tópicos y suscripciones
- Reglas y acciones

## Colas de storge account(Azure Storage Queues)
Almacena gran volumen de mensajes.

### Componentes

- URL Format
- Storage
- Queue
- Message