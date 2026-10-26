# Incident Desk

Servicio para correlacionar alertas, ordenar incidentes por severidad y consultar runbooks con referencias. Mantiene separadas las observaciones y las hipótesis. Una aprobación explícita habilita únicamente una simulación local del recorrido de remediación.

## Ejecutar

Requiere Python 3.10 o posterior. El modo local no necesita credenciales externas.

```powershell
python -m app demo
python -m unittest discover -s tests -v
$env:SERVICE_TOKEN = 'una-clave-local-de-al-menos-24-caracteres'
python -m app serve
```

La consola está en `http://127.0.0.1:8080`. La demo usa métricas sintéticas en una base temporal. Para conservar un incidente:

```powershell
python -m app run --input data/alert.json --key alert-demo-001
```

## Del evento a la revisión

Las alertas se deduplican por tenant e identificador de origen. Las observaciones del mismo servicio, entorno y señal se correlacionan con un incidente abierto observado durante los últimos 15 minutos. El estado conserva el total de alertas y las 20 evidencias más recientes.

La severidad se calcula a partir de métricas estructuradas. El texto descriptivo no ejecuta comandos ni confirma una causa. Los runbooks incluyen identificador y versión; una señal desconocida devuelve evidencia insuficiente y no ofrece simulación.

| Acción | Campos adicionales a `tenant` |
| --- | --- |
| `alert` | `alert_id`, `service`, `environment`, `signal`, `value`, `description` |
| `list` | Ninguno |
| `get` / `diagnose` | `id` |
| `approve` | `id`, `actor`, `reason`, `runbook_id` |
| `simulate` | `id` |
| `resolve` | `id`, `actor`, `reason` |

Señales: `error_rate` (%), `availability` (%), `memory` (%) y `latency` (milisegundos). `unknown` conserva observaciones para revisión humana. Los umbrales son políticas del ejercicio y requieren ajuste al servicio observado.

Para aprobar la propuesta de errores HTTP:

```json
{"action":"approve","tenant":"demo","id":"<id-del-incidente>","actor":"reviewer","reason":"Evidencia y runbook revisados","runbook_id":"RB-ERROR-01"}
```

`simulate` registra `external_actions: 0`. La simulación no resuelve el incidente; `resolve` registra la verificación y el motivo del operador. No hay un ejecutor de comandos, reinicios de servicios o llamadas a infraestructura.

## API, modelo y conectores

`POST /api/run` requiere `Authorization: Bearer <SERVICE_TOKEN>` e `Idempotency-Key`. `GET /health` comprueba el proceso y `GET /api/metrics` informa métricas locales de solicitudes completadas. El flujo n8n incluido puede recibir alertas y enviar sus datos a la API.

`diagnose` utiliza las observaciones y el runbook. Con `MODEL_BACKEND=ollama`, `OLLAMA_URL` y `OLLAMA_MODEL`, añade un resumen y sus incertidumbres mediante salida estructurada. Esa sugerencia no modifica severidad, permisos ni estado. La ejecución con un modelo real requiere tenerlo instalado y disponible.

CloudWatch, PagerDuty, CRM y canales de notificación no están conectados. Este repositorio ejecuta el ciclo local con datos sintéticos; las pruebas no acreditan disponibilidad o MTTR de sistemas empresariales.

## Medición

La auditoría conserva correlaciones, aprobaciones, simulaciones y cierres. Permite calcular volumen de alertas por incidente y tiempo transcurrido hasta cierre en el conjunto observado. Un MTTR comparable exige una definición común de inicio, restablecimiento y verificación sobre incidentes reales.

[Arquitectura](docs/architecture.md) · [Decisiones](docs/decisions.md) · [Operación](docs/operations.md) · [Referencias](docs/references.md)
