# Decisiones

| Decisión | Motivo | Límite |
| --- | --- | --- |
| Métricas estructuradas para severidad | Evitar que el texto de alerta decida acciones | Requiere normalizar el proveedor de monitoreo |
| Correlación de 15 minutos | Agrupar ruido del mismo problema operativo | No reconstruye causalidad entre servicios |
| Evidencia separada de hipótesis | Mostrar qué está observado y qué falta comprobar | El operador debe validar la causa |
| Runbooks con id y versión | Vincular una propuesta con su guía | La recuperación local depende de la señal, no de embeddings |
| Simulación con aprobación | Revisar el recorrido sin modificar infraestructura | No demuestra remediación real |
| Cierre explícito | Registrar la verificación independiente | No consulta automáticamente la salud del servicio |

El diseño toma como referencia el recorrido alerta, consulta de conocimiento y recomendación. La implementación mantiene deliberadamente explícitos el alcance de la evidencia y el permiso para actuar.
