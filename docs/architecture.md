# Arquitectura

![Arquitectura del servicio](diagrams/architecture.svg)

[Abrir SVG](diagrams/architecture.svg) · [Fuente Mermaid](diagrams/architecture.mmd)


Relaciona alertas estructuradas con un incidente y un runbook versionado. La revisión del operador gobierna la simulación y el cierre; el diagnóstico mantiene evidencia e hipótesis separadas.

<details>
<summary>Versión Mermaid</summary>

```mermaid
flowchart TB
    Client["CLI o consola web"] --> API["API con token e idempotencia"]
    subgraph Local["Servicio local implementado"]
        API --> Correlate["Deduplicación y correlación"]
        Correlate --> Severity["Severidad por señal y umbral"]
        Severity --> DB["Incidentes y auditoría en SQLite"]
        DB --> Runbook["Runbook JSON por señal"]
        Runbook --> Diagnose["Evidencia, hipótesis y cita"]
        Diagnose --> Approval["Aprobación explícita"]
        Approval --> Simulation["Simulación local permitida"]
        Simulation --> DB
        DB --> Closure["Cierre verificado por operador"]
    end
    subgraph Optional["Componentes opcionales por configurar"]
        N8N["Workflow n8n incluido"]
        Model["Resumen de diagnóstico Ollama"]
    end
    N8N -.-> API
    Diagnose -.-> Model
    External["Monitoreo y notificaciones por conectar"] -.-> N8N
```

</details>

Las líneas continuas muestran el recorrido implementado dentro de cada límite. Las líneas discontinuas indican componentes opcionales, entregados para configurar o aún por conectar; su estado se precisa en [tecnologías](technologies.md).

La clave de correlación combina tenant, servicio, entorno y señal. Cada alerta de origen tiene además una clave única que permite distinguir reentrega y observación nueva. Una alerta repetida con otro contenido se rechaza.

Los runbooks son datos versionados en `app/resources/runbooks.json` y se distribuyen como recurso del paquete. La aplicación recupera la guía correspondiente a una señal estructurada y devuelve citas. No realiza una búsqueda semántica ni infiere una causa raíz confirmada.

Ollama puede resumir el diagnóstico mediante un contrato de salida. Su respuesta se presenta como sugerencia independiente. No se usa para aprobar operaciones ni construir llamadas ejecutables.

La simulación utiliza una lista fija de nombres permitidos. Devuelve un resultado local sin acciones externas. Un ejecutor real necesitaría permisos por usuario, validación de alcance, idempotencia del destino, verificación posterior y reversión.

[Flujo de operación](flow.md) · [Tecnologías y alcance](technologies.md)
