# Verificación de correo electrónico

## 1. Objetivo

Confirmar que el usuario tiene acceso al correo electrónico proporcionado durante el registro y permitir la activación de la cuenta mediante un enlace único y temporal.

La verificación es el paso que permite cambiar la cuenta de:

**Pendiente de verificación → Activa**

---

## 2. Datos utilizados

### 2.1 Cuenta

* Identificador de la cuenta.
* Correo electrónico registrado.
* Estado actual de la cuenta.

### 2.2 Token de verificación

* Token único.
* Identificador de la cuenta asociada.
* Fecha y hora de generación.
* Fecha y hora de expiración.
* Estado de utilización.

### 2.3 Control de seguridad

* Fecha y hora de la solicitud.
* Registro de eventos relacionados con la verificación.
* Información necesaria para aplicar límites de solicitudes.

---

## 3. Validaciones

El sistema debe comprobar:

* Que el token exista.
* Que el token corresponda a una cuenta válida.
* Que el token corresponda al proceso de verificación de esa cuenta.
* Que el token no haya expirado.
* Que el token no haya sido utilizado anteriormente.
* Que la cuenta se encuentre en estado **Pendiente de verificación**.
* Que el token sea válido en el servidor.

La validación realizada mediante HTML o JavaScript en el navegador es únicamente complementaria. La validación definitiva debe realizarse en el servidor.

---

## 4. Reglas de negocio

### RN-VER-01 — Estado inicial

Toda cuenta creada correctamente mediante el registro comienza en estado:

**Pendiente de verificación**

### RN-VER-02 — Envío del enlace

El sistema debe enviar al correo registrado un enlace único de verificación después de completar correctamente el registro.

### RN-VER-03 — Vigencia

El enlace de verificación tendrá una vigencia de **24 horas**.

### RN-VER-04 — Uso único

Cada enlace solamente podrá utilizarse una vez.

### RN-VER-05 — Token inválido

Un token inexistente, manipulado o inválido no podrá activar la cuenta.

### RN-VER-06 — Token vencido

Un token que haya superado su vigencia no podrá utilizarse.

### RN-VER-07 — Nuevo enlace

Cuando se genere un nuevo enlace de verificación, el enlace anterior deberá quedar invalidado.

### RN-VER-08 — Activación

Cuando el usuario utilice correctamente un enlace válido:

```text
PENDIENTE DE VERIFICACIÓN
          ↓
        ACTIVA
```

### RN-VER-09 — Inicio de sesión

Una cuenta en estado **Pendiente de verificación** no podrá iniciar sesión.

### RN-VER-10 — Cuenta ya activa

Si la cuenta ya se encuentra activa, un enlace antiguo de verificación no deberá modificar nuevamente su estado.

### RN-VER-11 — Enlace utilizado

Un enlace que ya fue utilizado no podrá utilizarse nuevamente.

### RN-VER-12 — Correo registrado

El enlace debe estar asociado al correo electrónico utilizado para crear la cuenta.

### RN-VER-13 — Registro de verificación

El sistema debe registrar la fecha y hora en que se realizó correctamente la verificación.

### RN-VER-14 — Seguridad

Las solicitudes de generación y reenvío de enlaces deben estar sujetas a mecanismos de control para evitar abuso.

---

## 5. Flujo principal

```text
PENDIENTE DE VERIFICACIÓN
          ↓
Usuario recibe correo
          ↓
Usuario accede al enlace
          ↓
Sistema recibe token
          ↓
Valida token
          ↓
¿Token válido?
   ├── NO
   │    ↓
   │  Mostrar motivo
   │
   └── SÍ
        ↓
   Marcar token como utilizado
        ↓
   Cambiar estado
        ↓
      ACTIVA
        ↓
   Permitir inicio de sesión
```

---

## 6. Flujo de enlace vencido

```text
Usuario utiliza enlace
        ↓
Sistema valida token
        ↓
Token vencido
        ↓
No activar cuenta
        ↓
Cuenta continúa:
PENDIENTE DE VERIFICACIÓN
        ↓
Solicitar nuevo enlace
```

---

## 7. Flujo de nuevo enlace

```text
Cuenta PENDIENTE
       ↓
Usuario solicita nuevo enlace
       ↓
Sistema valida solicitud
       ↓
Invalida enlace anterior
       ↓
Genera nuevo token
       ↓
Establece nueva vigencia
       ↓
Envía nuevo enlace
```

Los límites exactos para solicitar nuevos enlaces quedan pendientes de definición.

---

## 8. Casos especiales

### CE-VER-01 — Correo no recibido

El usuario podrá solicitar un nuevo enlace, sujeto a los límites de seguridad.

### CE-VER-02 — Enlace vencido

El sistema debe informar que el enlace ya no es válido y permitir solicitar uno nuevo.

### CE-VER-03 — Enlace ya utilizado

El sistema debe informar que el enlace ya fue utilizado.

### CE-VER-04 — Cuenta ya activa

Si el usuario accede mediante un enlace antiguo después de haber verificado su cuenta, el sistema no debe modificar nuevamente el estado.

### CE-VER-05 — Token inexistente

El sistema debe rechazar el token y no modificar el estado de la cuenta.

### CE-VER-06 — Token manipulado

El sistema debe rechazar cualquier token que no pueda validarse correctamente.

### CE-VER-07 — Solicitudes repetidas

Las solicitudes de nuevos enlaces deben estar limitadas para evitar abuso.

### CE-VER-08 — Error de envío

Si ocurre un error al enviar el correo, la cuenta debe permanecer en estado **Pendiente de verificación** y debe poder solicitarse posteriormente un nuevo enlace.

### CE-VER-09 — Error del servidor

Si ocurre un error durante la verificación, el sistema no debe cambiar el estado de la cuenta hasta completar correctamente todas las validaciones.

---

## 9. Estados involucrados

La verificación de correo interviene directamente en esta transición:

```text
PENDIENTE DE VERIFICACIÓN
          │
          │ verificación válida
          ▼
        ACTIVA
```

No es responsabilidad de este módulo realizar las transiciones:

```text
ACTIVA → BLOQUEADA
ACTIVA → SUSPENDIDA
```

Estas corresponden respectivamente a los procesos de seguridad y administración.

---

## 10. Relación con otros módulos

```text
REGISTRO
   │
   ▼
PENDIENTE DE VERIFICACIÓN
   │
   ▼
VERIFICACIÓN DE CORREO
   │
   ▼
ACTIVA
   │
   ▼
INICIO DE SESIÓN
```

---

## 11. Pendientes

Queda por definir:

* Límite de solicitudes de verificación.
* Límite de reenvíos.
* Política de retención de tokens.
* Política para cuentas que permanezcan pendientes durante periodos prolongados.
* Mensajes exactos mostrados al usuario.
* Mecanismos específicos de registro y auditoría de los eventos de verificación.
