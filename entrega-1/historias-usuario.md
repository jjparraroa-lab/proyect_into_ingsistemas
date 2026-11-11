# Historias de usuario

## Resumen

| ID | Historia | Prioridad | Puntos | Sprint | Issue |
|---|---|---|---|---|---|
| HU01 | Iniciar y cerrar sesión | Debe tener | 5 | 1 | [enlace] |
| HU02 | Gestionar acceso por roles | Debe tener | 3 | 1 | [enlace] |
| HU03 | Registrar depósitos y rangos | Debe tener | 3 | 1 | [enlace] |
| HU04 | Registrar y consultar expedientes | Debe tener | 5 | 1 | [enlace] |
| HU05 | Actualizar y archivar expedientes | Debería tener | 3 | 1 | [enlace] |
| HU06 | Registrar inspecciones | Debe tener | 5 | 2 | [enlace] |
| HU07 | Consultar el historial | Debe tener | 3 | 2 | [enlace] |
| HU08 | Alertas de estado crítico | Debe tener | 3 | 2 | [enlace] |
| HU09 | Alertas de inspecciones vencidas | Debe tener | 3 | 2 | [enlace] |
| HU10 | Ingesta de temperatura y humedad | Debe tener | 5 | 2 | [enlace] |
| HU11 | Simulación de sensores | Debe tener | 2 | 2 | [enlace] |
| HU12 | Tablero priorizado | Debe tener | 5 | 2 | [enlace] |

## HU01: Autenticación
**Como** personal del CLENA **quiero** iniciar y cerrar sesión **para** acceder de manera segura.
- Dado que soy un usuario activo y escribo mis credenciales correctas, cuando presiono Iniciar sesión, entonces entro al sistema.
- Dado que escribo un usuario o una contraseña incorrectos, cuando presiono Iniciar sesión, entonces veo un mensaje de error que no indica cuál de los dos datos falló.
- Dado que tengo la sesión iniciada, cuando presiono Cerrar sesión, entonces vuelvo a la pantalla de inicio y no puedo ver el inventario sin entrar de nuevo.

## HU02: Roles
**Como** administrador **quiero** gestionar el acceso por roles **para** proteger el inventario.
- Dado que soy administrador, cuando creo una cuenta institucional, entonces la cuenta queda con el rol que elegí.
- Dado que soy personal del CLENA, cuando intento crear cuentas o configurar depósitos, entonces el sistema no me lo permite.
- Dado que soy usuario público, cuando escribo directamente la dirección del inventario, entonces no veo ningún expediente.
- Dado que veo la lista de usuarios, cuando reviso sus datos, entonces no aparece ninguna contraseña.

## HU03: Depósitos
**Como** administrador **quiero** registrar depósitos y rangos **para** contextualizar el monitoreo.
- Dado que registro un depósito, cuando dejo vacío el nombre o los rangos, entonces veo qué campo falta y no se guarda.
- Dado que escribo un mínimo mayor o igual que el máximo, cuando presiono Guardar, entonces veo un mensaje de error.
- Dado que escribo una humedad fuera de 0 a 100 %, cuando presiono Guardar, entonces veo un mensaje de error.
- Dado que cambio los rangos de un depósito, cuando se hace la siguiente lectura, entonces se aplican los nuevos y las lecturas anteriores no cambian.

## HU04: Inventario
**Como** archivista **quiero** registrar y consultar expedientes **para** localizar su caja y estantería.
- Dado que lleno código, título, depósito, estantería, caja y periodicidad (1 a 365 días), cuando presiono Guardar, entonces el expediente aparece con estado «Pendiente de primera inspección».
- Dado que escribo un código que ya existe, cuando presiono Guardar, entonces veo un mensaje de código repetido y no se crea el expediente.
- Dado que escribo un código o parte del título, cuando presiono Buscar, entonces veo los expedientes que coinciden en una lista paginada.
- Dado que abro un expediente, cuando veo su ficha, entonces aparecen el depósito, la estantería y la caja.

## HU05: Actualización y archivo
**Como** archivista **quiero** actualizar datos y retirar expedientes del inventario activo **para** no perder su historia.
- Dado que soy personal del CLENA, cuando cambio la ubicación o los datos de un expediente, entonces se guardan y sus inspecciones anteriores siguen disponibles.
- Dado que soy administrador, cuando archivo un expediente, entonces desaparece del listado activo.
- Dado que un expediente está archivado, cuando intento registrar una inspección, entonces el sistema no me deja, pero sí puedo ver su historial.

## HU06: Inspecciones
**Como** restaurador **quiero** registrar inspecciones **para** documentar el estado y los hallazgos.
- Dado que registro una inspección, cuando dejo sin elegir el estado o escribo observaciones de menos de 10 o más de 2000 caracteres, entonces veo un mensaje de error.
- Dado que marco humedad, plagas y desgaste como presentes o ausentes, cuando presiono Guardar, entonces la inspección aparece en el historial con mi nombre como inspector, sin que yo lo escriba.
- Dado que guardo una inspección, cuando termina el registro, entonces el expediente muestra su nuevo estado y la fecha de la próxima revisión.

## HU07: Historial
**Como** archivista **quiero** consultar el historial **para** conocer la evolución del expediente.
- Dado que abro la ficha de un expediente, cuando veo su historial, entonces aparecen sus inspecciones ordenadas por fecha y en páginas.
- Dado que veo una inspección ya registrada, cuando busco cómo editarla o borrarla, entonces esa opción no existe.

## HU08: Riesgo crítico
**Como** restaurador **quiero** recibir alertas de estado crítico **para** priorizar la atención.
- Dado que guardo una inspección en estado crítico, cuando reviso las alertas, entonces veo una alerta de prioridad 1 para ese expediente.
- Dado que el expediente ya tiene una alerta abierta, cuando registro otra inspección crítica, entonces no se crea una alerta duplicada.
- Dado que guardo una inspección que no es crítica, cuando reviso las alertas, entonces la alerta pasa a resuelta y queda guardada en el historial de alertas.

## HU09: Vencimientos
**Como** archivista **quiero** recibir alertas de inspecciones vencidas **para** evitar revisiones olvidadas.
- Dado que la fecha de próxima inspección de un expediente activo ya pasó, cuando reviso las alertas, entonces veo una alerta de prioridad 3 para ese expediente.
- Dado que el sistema revisa los vencimientos automáticamente cada minuto, cuando un expediente se vence, entonces su alerta aparece sin que yo haga nada.
- Dado que el sistema vuelve a revisar los vencimientos, cuando el expediente ya tiene su alerta, entonces no se crea una alerta repetida.
- Dado que registro una nueva inspección del expediente vencido, cuando guardo, entonces la alerta de vencimiento se resuelve.

## HU10: Ingesta y alertas ambientales
**Como** personal del CLENA **quiero** recibir temperatura y humedad mediante API **para** detectar cambios ambientales.
- Dado que envío una lectura con depósito, sensor, temperatura, humedad y fecha, cuando la lectura es válida, entonces aparece en el historial de mediciones del depósito.
- Dado que envío una humedad fuera de 0 a 100 %, una temperatura fuera de −50 a 80 °C o una fecha de hace más de 24 horas o del futuro, cuando intento enviarla, entonces el sistema la rechaza con un mensaje de error.
- Dado que una lectura sale del rango configurado del depósito, cuando se registra, entonces se crea una alerta de prioridad 2; si está justo en el límite, no hay alerta.
- Dado que hay una alerta ambiental abierta, cuando llega una lectura normal posterior, entonces la alerta se resuelve; una lectura atrasada no cambia el estado actual.

## HU11: Simulación
**Como** [rol] **quiero** simular sensores **para** demostrar el flujo sin hardware.
- Dado que ejecuto el simulador con una cuenta autorizada, cuando envía lecturas, entonces estas llegan a la aplicación real y aparecen en el historial.
- Dado que configuro la cantidad y el intervalo de lecturas, cuando lo ejecuto, entonces envía esa cantidad con ese intervalo.
- Dado que el simulador envía lecturas fuera de rango y luego normales, cuando reviso las alertas, entonces se abren y se resuelven según corresponde.

## HU12: Dashboard
**Como** coordinador **quiero** un tablero priorizado **para** identificar riesgos abiertos.
- Dado que abro el tablero, cuando carga, entonces veo cuántos expedientes activos, alertas abiertas, tareas pendientes e inspecciones vencidas hay.
- Dado que hay varias alertas abiertas, cuando las veo en el tablero, entonces están ordenadas por prioridad y, a igual prioridad, por antigüedad.
- Dado que veo una alerta, cuando la abro, entonces llego al expediente, y desde el monitoreo puedo llegar a su historial.
- Dado que no hay datos o ocurre un error, cuando abro el tablero, entonces veo un mensaje de estado vacío o de error, sin cifras inventadas.
