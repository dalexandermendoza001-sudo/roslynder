# Cierre de sesión

## 1. Objetivo

Permitir que un usuario autenticado finalice de forma segura su sesión en ROSLYNDER, invalidando el acceso autenticado actual sin modificar su cuenta, estado, roles ni información almacenada.

---

## 2. Datos involucrados

### 2.1 Datos asociados a la sesión

* Identificador de la sesión.
* Usuario asociado.
* Estado de la sesión.
* Fecha y hora de inicio.
* Fecha y hora de cierre.

### 2.2 Datos de seguridad y auditoría

* Evento de cierre de sesión.
* Fecha y hora del evento.
* Información necesaria para seguridad y trazabilidad.

El usuario no necesita proporcionar datos adicionales para cerrar su sesión.

---

## 3. Validaciones

Antes de finalizar la sesión, el sistema deberá comprobar que existe una sesión asociada al usuario autenticado.

La sesión puede encontrarse:

* Activa.
* Ya cerrada.
* Expirada.
* Invalidada por una condición de seguridad.

Una sesión que ya no sea válida no deberá utilizarse para conceder acceso a funcionalidades privadas.

---

## 4. Reglas de negocio

### RN-LOGOUT-01 — Cierre de sesión voluntario

Un usuario autenticado podrá finalizar su sesión mediante la opción **"Cerrar sesión"**.

### RN-LOGOUT-02 — Identificación de la sesión

El sistema deberá identificar la sesión autenticada que está utilizando el usuario antes de finalizarla.

### RN-LOGOUT-03 — Invalidación de la sesión

Al cerrar sesión, la sesión actual deberá quedar invalidada y no podrá utilizarse nuevamente para acceder a funcionalidades privadas.

### RN-LOGOUT-04 — Acceso posterior

Después de cerrar sesión, el usuario podrá continuar utilizando las funcionalidades públicas de ROSLYNDER, pero deberá autenticarse nuevamente para acceder a funciones privadas.

### RN-LOGOUT-05 — Estado de la cuenta

Cerrar sesión no modifica el estado de la cuenta.

Por ejemplo:

```text
ACTIVA + SESIÓN ACTIVA
        ↓
   CERRAR SESIÓN
        ↓
ACTIVA + SESIÓN CERRADA
```

### RN-LOGOUT-06 — Roles

Cerrar sesión no elimina ni modifica los roles asignados al usuario.

### RN-LOGOUT-07 — Datos del usuario

Cerrar sesión no elimina ni modifica los datos personales, comerciales, publicaciones u otra información almacenada del usuario.

### RN-LOGOUT-08 — Registro del evento

El sistema deberá registrar el cierre de sesión cuando corresponda, incluyendo la fecha y hora y la información necesaria para seguridad y auditoría.

### RN-LOGOUT-09 — Sesión inexistente o ya cerrada

Si se solicita cerrar una sesión que ya no existe, expiró o fue invalidada, el sistema no deberá crear una nueva sesión ni conceder acceso.

### RN-LOGOUT-10 — Expiración automática

La expiración automática, el tiempo máximo de sesión y el cierre por inactividad serán tratados en las reglas de **gestión de sesiones**.

---

## 5. Flujo principal

```text
USUARIO AUTENTICADO
        ↓
Selecciona "Cerrar sesión"
        ↓
Identificar sesión actual
        ↓
¿Sesión válida?
   ┌────┴────┐
  NO         SÍ
  ↓           ↓
Sesión ya    Invalidar sesión
inactiva          ↓
              Registrar cierre
                   ↓
          Finalizar autenticación
                   ↓
             Acceso público
```

---

## 6. Resultado del cierre

Antes del cierre:

```text
USUARIO
 ├── Estado: ACTIVA
 ├── Roles: COMPRADOR + VENDEDOR
 └── Sesión: ACTIVA
```

Después del cierre:

```text
USUARIO
 ├── Estado: ACTIVA
 ├── Roles: COMPRADOR + VENDEDOR
 └── Sesión: CERRADA
```

El cierre de sesión únicamente modifica el estado de la **sesión**.

---

## 7. Casos especiales

### 7.1 Sesión activa

El usuario selecciona "Cerrar sesión".

**Resultado:**

* La sesión se invalida.
* Se registra el evento.
* El usuario deja de estar autenticado.
* Puede continuar utilizando las funciones públicas.

### 7.2 Sesión ya cerrada

El usuario intenta cerrar una sesión que ya no está activa.

**Resultado:**

* No se crea una nueva sesión.
* No se concede acceso.
* El sistema puede informar que la sesión ya no está activa.

### 7.3 Sesión expirada

La sesión dejó de ser válida por una condición de expiración.

**Resultado:**

* No se permite utilizar la sesión.
* El usuario deberá autenticarse nuevamente para acceder a funciones privadas.

### 7.4 Problemas de conexión

Si existe una interrupción durante la solicitud de cierre, el sistema deberá manejar la situación sin conceder acceso autenticado a una sesión que no pueda considerarse válida.

Las condiciones técnicas específicas serán definidas posteriormente en la gestión de sesiones.

---

## 8. Relación con otros procesos

```text
INICIO DE SESIÓN
       ↓
SESIÓN ACTIVA
       ↓
CERRAR SESIÓN
       ↓
SESIÓN INVALIDADA
       ↓
FUNCIONES PÚBLICAS
       ↓
NUEVO INICIO DE SESIÓN
```

El cierre de sesión se relaciona principalmente con:

* Inicio de sesión.
* Gestión de sesiones.
* Estado de cuenta.
* Roles y permisos.
* Seguridad y auditoría.

---

## 9. Separación de responsabilidades

El cierre de sesión afecta únicamente a la **sesión actual**.

No debe confundirse con:

| Elemento          | ¿Se modifica al cerrar sesión? |
| ----------------- | ------------------------------ |
| Sesión            | Sí                             |
| Estado de cuenta  | No                             |
| Roles             | No                             |
| Permisos          | No                             |
| Datos personales  | No                             |
| Datos comerciales | No                             |
| Publicaciones     | No                             |

---

## 10. Pendientes

Los siguientes aspectos serán definidos en **Gestión de sesiones**:

* Duración máxima de una sesión.
* Tiempo de inactividad.
* Expiración automática.
* Comportamiento al cerrar el navegador.
* Sesiones simultáneas desde diferentes dispositivos.
* Invalidación de sesiones anteriores.
* Reglas de seguridad relacionadas con sesiones.
* Recuperación cuando una sesión expira durante el uso del sistema.

---

## 11. Resultado esperado

Al cerrar sesión correctamente:

1. La sesión actual queda invalidada.
2. El usuario deja de estar autenticado.
3. No puede utilizar nuevamente esa sesión para acceder a funciones privadas.
4. El usuario puede continuar navegando por las funciones públicas.
5. Para acceder nuevamente a funciones privadas deberá iniciar sesión.
6. El estado de la cuenta permanece sin cambios.
7. Los roles y permisos permanecen sin cambios.
8. La información almacenada del usuario permanece sin cambios.
