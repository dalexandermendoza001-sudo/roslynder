# Desbloqueo de cuenta

## 1. Objetivo

Permitir que un usuario cuya cuenta se encuentra en estado **Bloqueada** por una condición de seguridad pueda recuperar el acceso mediante un proceso de validación seguro.

El desbloqueo modifica únicamente el estado de la cuenta y no modifica la contraseña, los roles ni los datos almacenados del usuario.

---

## 2. Datos involucrados

### 2.1 Datos proporcionados por el usuario

* Correo electrónico asociado a la cuenta.
* Código o mecanismo de desbloqueo recibido.

### 2.2 Datos gestionados por el sistema

* Identificador de la cuenta.
* Estado actual de la cuenta.
* Código o token de desbloqueo.
* Fecha y hora de generación.
* Fecha y hora de expiración.
* Estado de uso del código.
* Cantidad de intentos de validación.
* Información relacionada con el bloqueo.
* Fecha y hora del desbloqueo.
* Información de seguridad y auditoría.

---

## 3. Validaciones

### 3.1 Correo electrónico

* Es obligatorio.
* Debe tener un formato válido.
* El sistema no deberá revelar si el correo corresponde a una cuenta bloqueada.
* La solicitud deberá procesarse mediante un mensaje general.

### 3.2 Estado de la cuenta

El proceso de desbloqueo está destinado exclusivamente a cuentas en estado:

**Bloqueada**

Los demás estados se gestionan mediante procesos diferentes:

| Estado                    | Proceso correspondiente |
| ------------------------- | ----------------------- |
| Pendiente de verificación | Verificación de correo  |
| Activa                    | No requiere desbloqueo  |
| Bloqueada                 | Desbloqueo de cuenta    |
| Suspendida                | Administración          |

### 3.3 Código o mecanismo de desbloqueo

El sistema deberá comprobar que:

* Existe.
* Corresponde a una solicitud válida.
* Es el mecanismo más reciente.
* Se encuentra vigente.
* No ha sido utilizado.
* No ha superado el límite de intentos.

---

## 4. Reglas de negocio

### RN-UNLOCK-01 — Cuenta bloqueada

El proceso de desbloqueo estará destinado a cuentas que se encuentren en estado **Bloqueada** por una condición de seguridad.

### RN-UNLOCK-02 — Estados que no utilizan desbloqueo

Las cuentas `Pendiente de verificación`, `Activa` y `Suspendida` no utilizarán este proceso.

* `Pendiente de verificación` → requiere verificación de correo.
* `Activa` → no necesita desbloqueo.
* `Suspendida` → requiere una acción administrativa.

### RN-UNLOCK-03 — Solicitud de desbloqueo

El usuario podrá solicitar el desbloqueo proporcionando el correo asociado a su cuenta.

### RN-UNLOCK-04 — Protección de información

El sistema no deberá revelar si el correo proporcionado corresponde a una cuenta bloqueada.

### RN-UNLOCK-05 — Código o enlace de desbloqueo

Cuando corresponda, el sistema generará un mecanismo temporal de desbloqueo y lo enviará al correo registrado.

### RN-UNLOCK-06 — Vigencia

El mecanismo de desbloqueo tendrá una vigencia máxima de **24 horas**.

Una vez superado ese período, no podrá utilizarse.

### RN-UNLOCK-07 — Uso único

Un código o enlace de desbloqueo podrá utilizarse una sola vez.

Después de utilizarse correctamente deberá quedar invalidado.

### RN-UNLOCK-08 — Nuevo mecanismo

Cuando se genere un nuevo código o enlace, cualquier mecanismo anterior quedará invalidado.

### RN-UNLOCK-09 — Intentos de validación

Cada mecanismo tendrá un máximo de **3 intentos** de validación.

Al superar el límite, el mecanismo deberá quedar invalidado y será necesario solicitar uno nuevo.

### RN-UNLOCK-10 — Desbloqueo exitoso

Cuando el mecanismo sea válido, el sistema cambiará el estado de la cuenta:

```text
BLOQUEADA → ACTIVA
```

### RN-UNLOCK-11 — Restablecimiento del control de intentos

Después de un desbloqueo exitoso, el contador de intentos fallidos de inicio de sesión deberá quedar restablecido.

### RN-UNLOCK-12 — Contraseña independiente

Desbloquear la cuenta no modifica la contraseña.

Si el usuario olvidó su contraseña, deberá utilizar el proceso de **Recuperación de contraseña**.

### RN-UNLOCK-13 — Roles independientes

Desbloquear la cuenta no modifica los roles ni permisos asignados al usuario.

### RN-UNLOCK-14 — Datos independientes

El desbloqueo no modifica ni elimina los datos personales, comerciales, publicaciones u otra información almacenada.

### RN-UNLOCK-15 — Sesiones existentes

Las sesiones anteriores que hayan quedado invalidadas por el bloqueo no deberán recuperarse automáticamente.

El usuario deberá iniciar sesión nuevamente.

### RN-UNLOCK-16 — Registro de seguridad

El sistema deberá registrar el desbloqueo exitoso y los eventos relevantes del proceso para fines de seguridad y auditoría.

### RN-UNLOCK-17 — Mensajes de seguridad

Los mensajes mostrados al usuario no deberán revelar información técnica, credenciales ni información innecesaria sobre la cuenta.

---

## 5. Flujo principal

```text
CUENTA BLOQUEADA
       ↓
Usuario selecciona "Desbloquear cuenta"
       ↓
Ingresa correo electrónico
       ↓
Validar solicitud
       ↓
Procesar solicitud
       ↓
Generar código/enlace
       ↓
Enviar al correo registrado
       ↓
Usuario proporciona código
       ↓
Validar código
       ↓
¿Código válido?
   ┌────┴────┐
  NO         SÍ
  ↓           ↓
Registrar   Cambiar estado
intento     BLOQUEADA → ACTIVA
  ↓           ↓
¿3 intentos? Restablecer contador
  ↓           ↓
Invalidar   Invalidar código
código          ↓
             Registrar evento
                  ↓
          Sesiones anteriores
             permanecen
             invalidadas
                  ↓
          Iniciar sesión nuevamente
```

---

## 6. Validación del mecanismo de desbloqueo

```text
Código recibido
      ↓
¿Existe?
      ↓ SÍ
¿Corresponde a una solicitud válida?
      ↓ SÍ
¿Es el mecanismo más reciente?
      ↓ SÍ
¿Está vigente?
      ↓ SÍ
¿No ha sido utilizado?
      ↓ SÍ
¿No superó 3 intentos?
      ↓ SÍ
Mecanismo válido
```

Si alguna validación falla, el mecanismo deberá ser rechazado.

---

## 7. Resultado del desbloqueo

Antes:

```text
USUARIO
 ├── Estado: BLOQUEADA
 ├── Roles: COMPRADOR + VENDEDOR
 └── Sesión: INVALIDADA
```

Después:

```text
USUARIO
 ├── Estado: ACTIVA
 ├── Roles: COMPRADOR + VENDEDOR
 └── Sesión: INVALIDADA
```

El usuario deberá realizar un nuevo inicio de sesión.

El desbloqueo modifica únicamente el **estado de la cuenta**.

---

## 8. Casos especiales

### 8.1 Cuenta no bloqueada

Si la cuenta se encuentra `Activa`, `Pendiente de verificación` o `Suspendida`, no corresponde ejecutar el proceso de desbloqueo.

### 8.2 Código incorrecto

Se registra el intento y se permite continuar mientras no se haya alcanzado el límite de 3 intentos.

### 8.3 Tercer intento incorrecto

El mecanismo queda invalidado y el usuario deberá solicitar uno nuevo.

### 8.4 Código expirado

El código no podrá utilizarse y el usuario podrá solicitar un nuevo mecanismo.

### 8.5 Código ya utilizado

El código será rechazado y no podrá reutilizarse.

### 8.6 Código anterior

Si existe un mecanismo más reciente, cualquier mecanismo anterior será rechazado.

### 8.7 Problema de envío del correo

El sistema deberá informar que no fue posible completar la solicitud sin revelar información técnica.

### 8.8 Problema de conexión

Si se pierde la conexión durante el proceso, el desbloqueo no deberá considerarse completado hasta que el servidor confirme la operación.

### 8.9 Error del servidor

El sistema deberá mostrar un mensaje general y no exponer información técnica o sensible.

---

## 9. Relación con otros procesos

```text
INICIO DE SESIÓN
       ↓
Intentos fallidos
       ↓
Límite alcanzado
       ↓
BLOQUEADA
       ↓
DESBLOQUEO DE CUENTA
       ↓
ACTIVA
       ↓
INICIO DE SESIÓN
```

También se relaciona con:

* Estado de cuenta.
* Seguridad.
* Correo electrónico.
* Sesiones.
* Auditoría.

---

## 10. Separación entre procesos

El desbloqueo no debe confundirse con otros procesos:

| Situación                 | Proceso                    | Resultado                   |
| ------------------------- | -------------------------- | --------------------------- |
| Usuario olvidó contraseña | Recuperación de contraseña | Nueva contraseña            |
| Cuenta bloqueada          | Desbloqueo de cuenta       | `BLOQUEADA → ACTIVA`        |
| Cuenta suspendida         | Administración             | Reactivación administrativa |
| Correo no verificado      | Verificación de correo     | `PENDIENTE → ACTIVA`        |

---

## 11. Pendientes

Los siguientes aspectos podrán definirse posteriormente:

* Límite de solicitudes de desbloqueo.
* Frecuencia máxima de solicitudes.
* Mecanismo definitivo: código, enlace o combinación.
* Formato y longitud del código.
* Política de almacenamiento del mecanismo.
* Política de retención de registros de seguridad.
* Reglas técnicas para invalidar sesiones.
* Duración y condiciones exactas del bloqueo.
* Integración con el sistema de notificaciones.

---

## 12. Resultado esperado

Al completar correctamente el desbloqueo:

1. La cuenta cambia de `BLOQUEADA` a `ACTIVA`.
2. El contador de intentos fallidos queda restablecido.
3. El mecanismo utilizado queda invalidado.
4. Las sesiones anteriores permanecen invalidadas.
5. El usuario deberá iniciar sesión nuevamente.
6. La contraseña permanece sin cambios.
7. Los roles y permisos permanecen sin cambios.
8. Los datos personales, comerciales y publicaciones permanecen sin cambios.
9. El evento queda registrado para seguridad y auditoría.
