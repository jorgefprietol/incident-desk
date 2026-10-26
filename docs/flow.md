# Flujo de operación

![Flujo de operación](diagrams/flow.svg)

[Abrir SVG](diagrams/flow.svg) · [Fuente Mermaid](diagrams/flow.mmd)


<details>
<summary>Versión Mermaid</summary>

```mermaid
flowchart TD
    Start["Recibir alerta estructurada"] --> Validate["Validar identidad, señal y métrica"]
    Validate --> Duplicate{"¿Reentrega idéntica?"}
    Duplicate -->|Sí| Existing["Devolver incidente existente"]
    Duplicate -->|No| Correlate["Correlacionar en ventana de quince minutos"]
    Correlate --> Severity["Actualizar severidad y evidencia"]
    Severity --> Retrieve["Recuperar runbook por señal"]
    Retrieve --> Evidence{"¿Existe guía aplicable?"}
    Evidence -->|No| Human["Solicitar revisión y más evidencia"]
    Evidence -->|Sí| Diagnose["Presentar hipótesis y cita versionada"]
    Diagnose --> Approval["Aprobar runbook con actor y motivo"]
    Approval --> Simulate["Simular sin acciones externas"]
    Simulate --> Verify["Verificación y cierre explícito"]
    Human --> Verify
```

</details>

- Una identidad de alerta reutilizada con otra carga se rechaza; una observación nueva puede agregarse al incidente abierto.
- Los runbooks se recuperan por señal estructurada. La causa permanece sin confirmar y una señal desconocida no habilita simulación.
- `simulate` registra `external_actions: 0`; no resuelve el incidente ni ejecuta comandos o reinicios.

Las operaciones del dominio se guardan junto a auditoría y respuesta idempotente en la transacción del servicio local. Un error revierte los cambios de esa solicitud.

[Arquitectura](architecture.md) · [Tecnologías y alcance](technologies.md)
