# 02.3 — Gestionar cuenta

## Objetivo

Permitir que el usuario autenticado consulte y gestione aspectos generales de su propia cuenta, sin modificar directamente sus roles ni las reglas de seguridad que controlan el estado de la cuenta.

## Funciones incluidas

La gestión de cuenta comprende:

* Consultar estado de cuenta.
* Consultar roles asignados.
* Consultar información general de la cuenta.
* Gestionar la sesión actual.
* Consultar información de sesiones recientes, cuando corresponda.

Las acciones de desactivación y eliminación de cuenta se encuentran definidas en **02.5 — Desactivar / eliminar cuenta**.

## Información general de la cuenta

El usuario podrá consultar información como:

* Estado actual de la cuenta.
* Rol o roles asignados.
* Fecha de registro.
* Estado de verificación del correo electrónico.
* Información relacionada con la sesión actual.

## Reglas funcionales

### RF-CTA-01 — Consulta de cuenta propia

El usuario autenticado puede consultar la información general asociada exclusivamente a su propia cuenta.

### RF-CTA-02 — Consulta del estado

El usuario puede consultar el estado actual de su cuenta.

El estado no puede ser modificado directamente mediante esta función.

### RF-CTA-03 — Consulta de roles

El usuario puede consultar los roles que tiene asignados.

Los roles no pueden ser modificados directamente mediante esta función.

### RF-CTA-04 — Independencia entre estado y roles

El estado de la cuenta y los roles son conceptos independientes.

Un cambio en el estado de la cuenta no implica modificar los roles asignados.

### RF-CTA-05 — Acceso según estado

Las funciones disponibles para el usuario estarán determinadas por el estado de su cuenta y los roles que tenga asignados.

### RF-CTA-06 — Gestión de sesión

El usuario autenticado puede cerrar su sesión actual mediante la función de cierre de sesión.

El cierre de sesión no modifica el estado, los roles ni la información almacenada de la cuenta.

### RF-CTA-07 — Una sesión activa

Para el MVP, una cuenta podrá mantener únicamente una sesión activa simultáneamente.

### RF-CTA-08 — Nuevo inicio de sesión

Cuando una cuenta que ya posee una sesión activa inicia sesión correctamente desde otro dispositivo, el sistema debe invalidar la sesión anterior y establecer la nueva sesión como sesión activa.

```text id="u8k4n2"
DISPOSITIVO A
     ↓
INICIO DE SESIÓN
     ↓
SESIÓN A ACTIVA
     
DISPOSITIVO B
     ↓
INICIO DE SESIÓN CORRECTO
     ↓
INVALIDAR SESIÓN A
     ↓
CREAR SESIÓN B
     ↓
SESIÓN B ACTIVA
```

### RF-CTA-09 — Notificación de nuevo inicio de sesión

Cuando una nueva sesión sustituya a una sesión anterior, ROSLYNDER podrá informar al usuario sobre el evento mediante los mecanismos de notificación disponibles.

La seguridad de la sesión no dependerá de que el usuario reciba o consulte dicha notificación.

### RF-CTA-10 — Invalidación de sesión anterior

Una sesión invalidada no podrá continuar realizando acciones autenticadas dentro de ROSLYNDER.

### RF-CTA-11 — Cambio de contraseña

El cambio exitoso de contraseña invalida las sesiones existentes de la cuenta.

El usuario deberá autenticarse nuevamente.

### RF-CTA-12 — Sesión y estado de cuenta

Cerrar, expirar o invalidar una sesión no modifica el estado de la cuenta.

### RF-CTA-13 — Sesión y roles

Cerrar, expirar o invalidar una sesión no modifica los roles asignados al usuario.

### RF-CTA-14 — Sesión y datos

Cerrar, expirar o invalidar una sesión no elimina ni modifica la información personal, comercial, publicaciones, productos u otros datos asociados a la cuenta.

### RF-CTA-15 — Sesiones recientes

ROSLYNDER podrá mostrar información de sesiones recientes con fines de consulta y seguridad.

La consulta de sesiones recientes no implica que varias sesiones permanezcan activas simultáneamente.

### RF-CTA-16 — Información técnica de sesión

Los detalles técnicos relacionados con identificación de dispositivos, tokens, direcciones IP, tiempos de expiración y mecanismos de invalidación serán definidos durante el diseño técnico y de seguridad.

## Flujo de consulta de cuenta

```text id="c2m7p1"
Usuario inicia sesión
        ↓
Accede a "Mi cuenta"
        ↓
Sistema valida sesión
        ↓
Muestra información general
        ↓
Usuario puede consultar:
        ├── Estado de cuenta
        ├── Roles
        ├── Información de cuenta
        └── Sesión actual / sesiones recientes
```

## Flujo de sustitución de sesión

```text id="a6r9x3"
Cuenta con sesión activa
        ↓
Nuevo inicio de sesión correcto
        ↓
Sistema identifica sesión anterior
        ↓
Invalida sesión anterior
        ↓
Crea nueva sesión
        ↓
Nueva sesión queda activa
        ↓
Sistema puede generar una notificación de seguridad
```

## Casos especiales

* Sesión no válida → solicitar nuevo inicio de sesión.
* Sesión anterior ya invalidada → no puede utilizarse para acceder a funciones privadas.
* Inicio de sesión desde otro dispositivo → invalidar la sesión anterior después de una autenticación correcta.
* Cambio de contraseña → invalidar las sesiones existentes.
* Cuenta bloqueada o suspendida → impedir nuevas sesiones según las reglas de autenticación.
* Error al consultar información → informar al usuario sin revelar información técnica sensible.

## Consideraciones

La gestión de cuenta no permite al usuario cambiar directamente:

* Estado de cuenta.
* Roles.
* Permisos.
* Configuraciones administrativas.

El estado puede cambiar como consecuencia de procesos específicos, como verificación de correo, bloqueo de seguridad, suspensión administrativa o desbloqueo.

Los roles se gestionan mediante las reglas correspondientes a **03 — Roles y permisos**.

El cierre de sesión se encuentra documentado como una función específica de autenticación.

La desactivación y eliminación de cuenta se encuentran documentadas en **02.5 — Desactivar / eliminar cuenta**.

Los detalles técnicos de sesiones serán definidos posteriormente y no forman parte del análisis funcional.
