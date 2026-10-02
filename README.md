# Mesa de Ayuda IT - Automatización Inteligente

Proyecto final de automatización inteligente para la gestión de incidencias de una Mesa de Ayuda IT, desarrollado con n8n, Telegram, Airtable y OpenRouter.

## Objetivo

Diseñar e implementar un sistema automatizado capaz de recibir solicitudes de usuarios, interpretar el mensaje mediante inteligencia artificial, clasificar la incidencia, determinar prioridad e impacto, generar y consultar tickets, asignar el equipo responsable y registrar la información para su posterior seguimiento y análisis.

## Tecnologías utilizadas

- n8n
- Telegram
- Airtable
- OpenRouter
- Modelo de IA: `openai/gpt-4.1-mini`
- Structured Output Parser
- Airtable Interface para dashboard
- JavaScript para expresiones y validaciones de n8n

## Arquitectura general

El flujo principal comienza con la recepción de mensajes mediante Telegram.

```text
Telegram
   ↓
Normalización del mensaje
   ↓
Filtro de mensajes
   ↓
Clasificación de intención mediante IA
   ↓
Enrutamiento
   ├── SALUDO
   ├── INCIDENTE
   ├── SOLICITUD
   ├── CONSULTAR_ESTADO
   ├── NO_VALIDO
   └── CONVERSACION
          ↓
    Gestión de solicitud
          ↓
    Creación / consulta de ticket
          ↓
       Airtable
          ↓
    Respuesta al usuario

## Funcionalidades principales
### Recepción de incidencias
El sistema recibe mensajes desde Telegram y normaliza la información necesaria para procesarlos.
### Clasificación mediante IA
La inteligencia artificial analiza el contenido de la solicitud y determina:
- Categoría
- Subcategoría
- Prioridad
- Impacto
- Urgencia
- Resumen
### Categorías
El sistema utiliza ocho categorías cerradas:
1. HARDWARE
2. SOFTWARE
3. REDES_CONECTIVIDAD
4. ACCESOS_SEGURIDAD
5. IMPRESORAS
6. SOLICITUDES Y SERVICIOS
7. TELEFONIA_COMUNICACIONES
8. INFRAESTRUCTURA_SERVIDORES
### Asignación de equipos
| Categoría | Equipo responsable |
|---|---|
| HARDWARE | Soporte Técnico |
| SOFTWARE | Soporte Técnico |
| REDES_CONECTIVIDAD | Operaciones |
| ACCESOS_SEGURIDAD | Operaciones |
| IMPRESORAS | Soporte Técnico |
| SOLICITUDES Y SERVICIOS | Mesa de Ayuda |
| TELEFONIA_COMUNICACIONES | Operaciones |
| INFRAESTRUCTURA_SERVIDORES | Operaciones |


### Gestión de tickets
Los tickets contienen información de identificación, usuario, solicitud, clasificación, prioridad, estado y equipo responsable.
Estados utilizados:
- NUEVO
- ASIGNADO
- EN_PROCESO
- RESUELTO
- CERRADO
- PENDIENTE_APROBACION
```text
### Consulta de estado

El usuario puede consultar un ticket utilizando su identificador.

```text
estado TKT-YYYYMMDD-HHMMSS-ID

### Protección contra duplicados
El flujo incorpora una validación de idempotencia utilizando:
- chat_id
- telegram_message_id
Esto evita registrar nuevamente un mismo mensaje procesado.
```text
### Human-in-the-loop

Las solicitudes clasificadas como críticas requieren aprobación humana antes de continuar con la gestión normal del ticket.

Se utilizan las acciones:

- APROBAR_TICKET
- RECHAZAR_TICKET

Los tickets pendientes de aprobación utilizan el estado:

```text
PENDIENTE_APROBACION

## Gestión de errores
El proyecto utiliza un workflow independiente:

Mesa de Ayuda - Gestión de Errores

Este workflow recibe los errores mediante Error Trigger y registra la información en una tabla específica de Airtable.
Los errores registrados incluyen:
- Identificador del error
- Fecha
- Workflow
- Nodo
- Tipo de error
- Mensaje
- Ticket relacionado
- Severidad
- Estado de resolución
Severidades:
- BAJA
- MEDIA
- ALTA
- CRITICA
  
## Persistencia de información
La información se almacena en Airtable.
### Tabla Tickets
Contiene información relacionada con:
- ticket_id
- fecha_creacion
- nombre
- email
- mensaje
- chat_id
- telegram_message_id
- estado
- categoria
- subcategoria
- prioridad
- impacto
- urgencia
- resumen
- equipo_responsable
### Tabla Errores
Permite registrar y realizar seguimiento de los errores generados durante la ejecución de los workflows.

## Dashboard
El proyecto incluye un dashboard desarrollado mediante Airtable Interface.
Indicadores utilizados:
- Total de tickets
- Tickets abiertos
- Tickets críticos
- Tickets cerrados
- Errores registrados
- Errores no resueltos
- Distribución por equipo
- Distribución por estado
El dashboard se encuentra implementado como parte del proyecto. El acceso público mediante la web depende de las funcionalidades y permisos disponibles en el plan de Airtable utilizado.

## Evidencias y documentación
El repositorio contiene la documentación técnica y las evidencias utilizadas para la entrega del proyecto.
La documentación incluye:
- Arquitectura del sistema
- Modelo de datos
- Esquemas JSON
- Optimización y costos de tokens
- Seguridad y resiliencia
- Gestión de errores
- Pruebas y validación
- Documento final de entrega

```text
## Workflow de n8n

El workflow exportado se encuentra en:

```text
workflow/
└── Mesa de Ayuda - Recepción de Incidencias_FINAL.json

El archivo permite disponer del blueprint del workflow utilizado en el proyecto.

## Seguridad
Por razones de seguridad, este repositorio no contiene:
- Contraseñas
- API Keys
- Tokens de Telegram
- Credenciales de Airtable
- Credenciales de OpenRouter
- Credenciales de n8n
- Variables de entorno con información sensible
Las credenciales deben configurarse directamente en el entorno de ejecución de n8n.

## Resultado
El proyecto integra automatización, inteligencia artificial, persistencia de datos, clasificación automática, gestión de tickets, aprobación humana para casos críticos, consulta de estados, control de duplicados y gestión centralizada de errores.
El objetivo es demostrar una arquitectura de automatización aplicable a un escenario real de Mesa de Ayuda IT.

```text
## Autor

J.J.LOPEZ

Tecnologías principales: n8n · Telegram · Airtable · OpenRouter · IA
