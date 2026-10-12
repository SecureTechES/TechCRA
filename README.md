# TechCRA

> ⚠️ **Proyecto en desarrollo · Laboratorio de pruebas · NO apto para producción**

**TechCRA** es un prototipo de **puesto de operador de una Central Receptora de Alarmas (CRA)**, desarrollado como parte de un laboratorio de pruebas de **SecureTech | TechECS_EU**.

🔗 Demo: <https://secureteches.github.io/TechCRA/CRA_Operator.html>

---

## Estado del proyecto

| Aspecto | Estado |
|---|---|
| Fase | Prototipo de frontend |
| Entorno | Laboratorio de pruebas |
| Producción | ❌ No |
| Backend / persistencia | ❌ No (todo en memoria; se pierde al recargar) |
| Datos | Ficticios (8 abonados de demostración) |
| Certificación / homologación | ❌ No certificado ni autorizado |

Varias acciones están simuladas o pendientes de conectar (telefonía PBX/SIP, canal inverso, visor de cámaras, visor de planos, alarma manual).

## Aviso importante

Este software **no** está autorizado ni homologado para operar como CRA real. Una CRA en España requiere, entre otros:

- Ley 5/2014, de 4 de abril, de Seguridad Privada.
- Real Decreto 2364/1994 (Reglamento de Seguridad Privada).
- Orden INT/316/2011 (funcionamiento de los sistemas de alarma) y Orden INT/317/2011.
- Normas UNE-EN aplicables (serie 50131, 50136, 50518, etc.).
- Autorización e inscripción como empresa de seguridad ante el Ministerio del Interior.
- RGPD y LOPDGDD.

Usar este proyecto no sustituye ninguno de estos requisitos. Esto no es asesoramiento legal.

---

## Qué hace

Interfaz web de una sola página que simula la operativa de un operador de CRA:

**Gestión de alarmas**
- Cola de alarmas por prioridad, contadores y mensajes de estado.
- Patrón de acción paso a paso (verificar cámaras, contactar con el cliente y contactos, avisar a acuda/vigilante, registrar resolución).
- Resolución con códigos configurables, palabra clave y notas.
- Suspensión de alarmas con motivo y duración, con vencimiento automático.
- Cierre rápido para averías y sin comunicación, con «deshacer».
- Modo «En pruebas» por abonado o zonas, con caducidad, que omite señales sin generar alarma.
- Seguimiento de alarmas y registro de eventos por abonado (códigos tipo Contact ID: robo, pánico, incendio, fallo 220V, batería baja, fallo de comunicación, armado/desarmado, test periódico).

**Abonados y sistemas**
- Ficha de abonado: datos, zonas, contactos, horarios, comentarios y estado de zonas.
- Sistemas y comunicaciones: intrusión, CCTV, incendio, accesos.
- Vías de comunicación (IP, GPRS/4G, PSTN) con prioridad, supervisión y receptoras asociadas.
- Acceso remoto por IP y datos de grabadores CCTV.

**Servicio técnico**
- Mantenimientos contratados con periodicidad y revisiones programadas.
- Órdenes de trabajo y partes (técnico, tiempos, desplazamiento, material, facturación con IVA).

**Otros**
- Asistente de telefonía PBX/SIP (simulado), atajos de teclado, tema claro/oscuro y diseño adaptable a móvil.

## Tecnología

- HTML + CSS + JavaScript puro, en un único archivo: `CRA_Operator.html`.
- Sin dependencias ni proceso de compilación.

## Uso

```bash
git clone https://github.com/SecureTechES/TechCRA.git
cd TechCRA
# Abre CRA_Operator.html en el navegador
```

Usa siempre datos de prueba. No conectes instalaciones ni clientes reales.

## Limitaciones conocidas

Consulta [`TODO.md`](TODO.md) para el análisis detallado. En resumen:

- Lógica de señales: una señal nueva no escala una alarma si el abonado ya tiene avería, sin comunicación o alarma en gestión.
- El registro de eventos no es inmutable ni exportable (no equivale a un libro-registro).
- Sin autenticación ni roles; credenciales de CCTV visibles en el cliente.
- Sin backend, persistencia ni receptor real de protocolos de alarma.

## Hoja de ruta

Ver [`TODO.md`](TODO.md).

## Licencia

Este proyecto se distribuye bajo la **GNU Affero General Public License v3.0 (AGPL-3.0)**. Consulta el archivo [`LICENSE`](LICENSE).

Puntos clave de la AGPL-3.0:

- Puedes usar, modificar y redistribuir el software bajo las mismas condiciones.
- Si ofreces una versión modificada como servicio accesible por red, debes ofrecer su código fuente a los usuarios (sección 13).
- El software se proporciona sin garantía.

## Contribuciones

El proyecto está en fase temprana. Puedes abrir un *issue* con sugerencias o errores. Las contribuciones se aceptarán bajo la misma licencia (AGPL-3.0).

## Seguridad

Si detectas una vulnerabilidad, no la publiques en un issue abierto; contacta directamente con el autor.

## Autor

**Adan Nahir Abad Mora**
SecureTech | TechECS_EU
<https://github.com/SecureTechES/TechCRA>

## Descargo de responsabilidad

Software proporcionado «tal cual», sin garantías de ningún tipo. Los autores no se hacen responsables de daños derivados de su uso fuera del entorno de laboratorio.
