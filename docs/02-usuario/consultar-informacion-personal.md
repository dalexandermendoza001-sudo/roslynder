# 02.1 — Consultar información personal

## Objetivo

Permitir que un usuario autenticado consulte la información personal asociada a su propia cuenta.

La consulta debe mostrar la información registrada y vigente del usuario, manteniendo separada la información personal de los roles, el estado de cuenta y las sesiones.

## Información consultable

El usuario podrá consultar:

* Nombres
* Apellidos
* Fecha de nacimiento
* Teléfono
* Dirección
* Provincia
* Ciudad
* Correo electrónico
* Nombre de usuario

También podrá consultar un resumen de la cuenta:

* Estado de cuenta
* Rol o roles asignados
* Fecha de registro

## Reglas funcionales

### RF-USU-01 — Consulta de información propia

El usuario autenticado puede consultar únicamente la información personal asociada a su propia cuenta.

### RF-USU-02 — Información vigente

La información mostrada debe corresponder a los datos actuales registrados en la cuenta.

### RF-USU-03 — Separación de información

Los datos personales se gestionan de forma independiente de:

* Estado de cuenta.
* Roles.
* Permisos.
* Sesiones.

### RF-USU-04 — Información de roles

El usuario puede consultar los roles que tiene asignados, pero no puede modificarlos desde esta función.

### RF-USU-05 — Información del estado de cuenta

El usuario puede consultar el estado actual de su cuenta, pero no puede modificarlo directamente desde esta función.

### RF-USU-06 — Acceso autenticado

La consulta de información personal requiere una sesión autenticada y válida.

### RF-USU-07 — Protección de información

La información personal de un usuario no debe quedar disponible para otros usuarios mediante esta función.

## Flujo principal

```text
Usuario inicia sesión
        ↓
Accede a su perfil
        ↓
Selecciona "Información personal"
        ↓
Sistema valida la sesión
        ↓
Sistema obtiene información de la cuenta
        ↓
Muestra información personal
        ↓
Muestra estado y roles como información complementaria
```

## Casos especiales

* Sesión no válida → solicitar nuevo inicio de sesión.
* Cuenta sin información opcional → mostrar únicamente los datos disponibles.
* Error al consultar información → informar al usuario sin revelar información técnica sensible.
* Usuario intenta consultar información de otra cuenta → denegar el acceso.

## Consideraciones

Esta función permite consultar la información actual, pero no modificarla.

La modificación de los datos personales corresponde a **02.2 — Modificar información personal**.

La información comercial del usuario no forma parte de esta función y será definida posteriormente dentro de **04 — Perfil comercial**.
