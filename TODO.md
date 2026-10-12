# TODO · TechCRA

Análisis de `CRA_Operator.html` y tareas pendientes, ordenadas por prioridad.
Leyenda: 🔴 crítico · 🟠 importante · 🟡 mejora · ⚪ pendiente de conectar

---

## 🔴 1. Errores de lógica

### 1.1 Una señal nueva no escala una alarma existente
**Dónde:** función `signal()`.
**Problema:** solo se crea alarma nueva si el abonado está en estado `ok` o `susp`. Si ya está en `aver` (avería), `com` (sin comunicación), `alarm` o `proc` (en gestión), la señal solo se registra en el log. Ejemplo: un abonado con «Fallo 220V» recibe un «Robo» y no pasa a alarma de robo, ni sube de prioridad.
**Solución:** al llegar una señal, comparar su prioridad con la actual y escalar si es mayor (actualizar `s.st`, `s.ev`, `s.pri`, `s.at`, y marcar como no vista). Si el abonado está en gestión, avisar al operador de la señal nueva sin perder el progreso del patrón.

### 1.2 Suspensión con datos obsoletos
**Dónde:** `signal()` y `unsus()`.
**Problema:** cuando una señal nueva reactiva un abonado suspendido, se cambia el estado pero `s.susp` queda con los datos antiguos. Después puede mostrarse información de suspensión incorrecta o cancelarse mal.
**Solución:** al reactivar por señal nueva, limpiar `s.susp` y registrar en el log que la suspensión terminó por señal nueva.

---

## 🔴 2. Cumplimiento normativo (revisar con asesoría legal)

> Referencias: Ley 5/2014, Reglamento (RD 2364/1994) y Orden INT/316/2011. Verificar los artículos exactos en el texto consolidado del BOE.

### 2.1 Comunicación de alarmas reales a la policía
**Problema:** el patrón de acción solo incluye «Avisar a servicio de acuda/vigilante». El Reglamento obliga a comunicar sin demora al servicio policial las alarmas reales verificadas.
**Solución:** añadir un paso explícito «Comunicar a Fuerzas y Cuerpos de Seguridad» al patrón de intrusión y pánico, con hora, interlocutor y referencia, que quede registrado.

### 2.2 Libro-registro de alarmas
**Problema:** el registro de eventos está en memoria, es editable desde el código y no se puede exportar. No equivale a un libro-registro (el Reglamento exige un formato aprobado por el Ministerio, tratable e informatizable).
**Solución:** registro persistente, de solo añadir (append-only), con sello de tiempo, usuario y exportación (CSV/PDF). Añadir también libro-registro de contratos.

### 2.3 Datos de la instalación
**Problema:** el modelo de abonado no incluye grado de seguridad (Grado 2/3), certificado de instalación ni certificado de conexión a CRA.
**Solución:** añadir esos campos por sistema, con fecha de emisión, empresa instaladora y alertas de caducidad o falta de documento.

### 2.4 Verificación y mínimo de operadores
**Problema:** no hay modelado de procedimientos técnicos de verificación (audio, vídeo, secuencial) ni control del mínimo de dos operadores en turno.
**Solución:** registrar el método de verificación usado en cada alarma y un control de operadores en servicio.

### 2.5 Normas técnicas
**Pendiente:** definir qué normas UNE-EN aplican al software y a la transmisión (50131, 50136, 50518) y mapearlas a requisitos verificables.

---

## 🟠 3. Seguridad

### 3.1 Sin autenticación ni roles
**Problema:** el usuario «OP1» está fijo en el código.
**Solución:** login real, roles (operador, supervisor, técnico, admin) y registro de quién hace qué.

### 3.2 Credenciales de CCTV en el cliente
**Problema:** usuario y contraseña de grabadores están en el JavaScript, con botón «Ver».
**Solución:** guardarlas cifradas en el servidor, mostrarlas solo con permiso y registrar cada consulta (el log ya anota la consulta; falta control de acceso real).

### 3.3 Campos sin escapar en `innerHTML`
**Dónde:** por ejemplo `s.panel`, `s.com` y otros valores introducidos por el usuario.
**Problema:** con datos reales y un backend, es un riesgo de XSS almacenado.
**Solución:** escapar siempre con `esc()`, o usar `textContent` y plantillas seguras. Añadir una Content-Security-Policy.

### 3.4 Datos personales
**Pendiente:** definir base legal, minimización, retención y acceso a datos de abonados y contactos (RGPD/LOPDGDD).

---

## 🟠 4. Arquitectura

### 4.1 Backend y persistencia
**Problema:** todo vive en memoria; al recargar se pierde.
**Solución:** API + base de datos (por ejemplo PostgreSQL), con eventos persistentes y transacciones.

### 4.2 Receptor de señales real
**Problema:** las señales se simulan con botones.
**Solución:** módulo receptor para protocolos reales (Contact ID, SIA DC-09 sobre IP, etc.), con supervisión de vías y alta disponibilidad (receptoras principal y respaldo).

### 4.3 Tiempo real
**Pendiente:** WebSocket o similar para sincronizar varios operadores y evitar que dos gestionen la misma alarma.

### 4.4 Código en un solo archivo
**Problema:** 645 líneas, estado global mutable y código muy compacto, difícil de mantener.
**Solución:** separar en módulos (datos, lógica de alarmas, vistas, diálogos, PBX) y valorar un framework o un sistema de build.

---

## 🟡 5. Calidad y pruebas

- Tests unitarios de la lógica de cola, prioridades, suspensión, pruebas y señales (cubren los bugs 1.1 y 1.2).
- Tests de interfaz para los flujos principales (tomar, gestionar y cerrar alarma).
- Linter y formateador.
- Datos de demo con fechas relativas (hoy están fijadas en 2026).
- Accesibilidad: revisar foco, contrastes y etiquetas.

---

## ⚪ 6. Funciones pendientes de conectar

- Telefonía PBX/SIP real (hoy simulada; sustituir por SIP.js o JsSIP).
- Canal inverso real a las centrales (armar, desarmar, estado).
- Visor de cámaras (el botón «Ver» está pendiente).
- Visor de planos con detectores y cámaras.
- «Alarma manual» (acción sin implementar).
- Envío real de SMS al titular.
- «Informe del abonado» (hoy apunta al mismo informe que «Informe de alarma»).
- Imprimir ficha con formato propio.
- Facturación: cálculo ya existe, falta generar documento y exportar.

---

## 📄 7. Documentación y proyecto

- Mantener README y licencia coherentes (AGPL-3.0 ya reflejada).
- Añadir un archivo `SECURITY.md` con el canal de contacto.
- Documentar el modelo de datos y los estados de alarma.
- Cabeceras de copyright/AGPL en los archivos fuente.
- Si se ofrece como servicio en red, enlazar el código fuente (obligación AGPL, sección 13).
- Definir un `CHANGELOG.md` y versionado.

---

## Orden sugerido

1. Corregir bugs 1.1 y 1.2 y añadir sus tests.
2. Backend + persistencia + autenticación (4.1, 3.1).
3. Registro de eventos inmutable y exportable (2.2).
4. Paso de aviso a policía y campos de grado/certificados (2.1, 2.3).
5. Receptor de protocolos reales (4.2).
6. Revisión normativa y de protección de datos con asesoría especializada.
