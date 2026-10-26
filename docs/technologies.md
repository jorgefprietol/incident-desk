# Tecnologías y alcance

Python 3.10+, SQLite y runbooks JSON versionados. Ollama es opcional para resumir diagnósticos; se incluyen n8n, Docker y CI.

| Tecnología | Estado | Uso en este proyecto |
| --- | --- | --- |
| Python 3.10+ / datetime / math | Implementado | Validación de señales, umbrales de severidad y ventana de correlación. |
| SQLite / SHA-256 | Implementado | Incidentes, reentregas, evidencia reciente, estados y auditoría por tenant. |
| Runbooks JSON versionados | Implementado | Recurso del paquete con hipótesis, verificaciones y simulación permitida por señal. |
| Ollama / salida estructurada | Opcional | Resumen y dudas en diagnose; no altera la severidad, aprobación o estado. |
| API HTTP / consola / CLI | Implementado | Ingreso, consulta, diagnóstico, aprobación, simulación y cierre. |
| n8n / Docker Compose | Archivos incluidos | Puente de alertas y empaquetado; requiere configurar credenciales y validar runtime. |
| unittest / GitHub Actions | Implementado | Correlación, estados, límites, simulaciones y contrato HTTP. |
| CloudWatch, PagerDuty y notificaciones | Por conectar | No hay conector de monitoreo ni ejecutor sobre infraestructura real. |

“Implementado” describe código disponible y comprobable en el repositorio. “Opcional” requiere un proveedor configurado; “incluido” identifica un archivo o adaptador entregado, sin afirmar una integración externa ejecutada.

[Arquitectura](architecture.md) · [Flujo de operación](flow.md) · [Integraciones](integrations.md)
