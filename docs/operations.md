# Operación

Configure el token mediante el entorno y mantenga la API en loopback durante las pruebas. El token compartido autentica al cliente del servicio; `actor` es un dato de auditoría aportado por ese cliente. La separación por tenant en consultas no reemplaza autorización por identidad.

Los identificadores de alerta deben ser estables ante reentregas. Una observación nueva usa un identificador nuevo. El mismo `Idempotency-Key` conserva exactamente la respuesta original; use una clave distinta para `approve`, `simulate` y `resolve`.

Los porcentajes deben estar entre 0 y 100 y la latencia debe expresarse en milisegundos no negativos. Valores no finitos, booleanos y señales no admitidas se rechazan. La hora de recepción la establece el servicio en UTC.

Una simulación repetida devuelve su registro anterior. `resolve` registra el cierre pero no comprueba métricas externas. El motivo debe indicar la verificación realizada por el operador; no utilice el ejercicio para publicar tiempos de recuperación de clientes.

Antes de conectar un sistema de monitoreo, pruebe ráfagas de alertas, cambio de severidad, entornos separados, alertas tras el cierre y señales sin runbook. Configure retención para descripciones y evidencias que pudieran contener información sensible. No publique la base operativa en Git.
