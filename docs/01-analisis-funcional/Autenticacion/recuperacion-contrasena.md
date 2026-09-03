# Recuperación de contraseña

## 1. Objetivo

Permitir que un usuario que no recuerda su contraseña pueda establecer una nueva contraseña mediante un proceso seguro de verificación utilizando el correo electrónico registrado en su cuenta.

La recuperación de contraseña **no modifica el estado de la cuenta ni sus roles**.

---

## 2. Datos involucrados

### 2.1 Datos proporcionados por el usuario

* Correo electrónico registrado.
* Código o mecanismo de verificación recibido.
* Nueva contraseña.
* Confirmación de la nueva contraseña.

### 2.2 Datos gestionados por el sistema

* Identificador de la cuenta.
* Estado de la cuenta.
* Código o token de recuperación.
* Fecha y hora de generación.
* Fecha y hora de expiración.
* Estado de uso del código o token.
* Cantidad de intentos realizados.
* Fecha y hora del restablecimiento.
* Información necesaria para seguridad y auditoría.

---

## 3. Validaciones

### 3.1 Correo electrónico

* Es obligatorio.
* Debe tener un formato válido.
* El sistema no deberá revelar si el correo está registrado o no.
* La solicitud deberá procesarse mediante un mensaje general.

### 3.2 Código o mecanismo de recuperación

* Debe existir.
* Debe corresponder a una solicitud válida.
* Debe encontrarse vigente.
* No debe haber sido utilizado.
* Debe corresponder a la solicitud más reciente.
* Tendrá un máximo de **3 intentos de validación**.
* Al superar el límite de intentos, deberá invalidarse.

### 3.3 Nueva contraseña

La nueva contraseña deberá cumplir las mismas reglas establecidas durante el registro:

* Longitud mínima y máxima definida para las contraseñas del sistema.
* Al menos una letra mayúscula.
* Al menos una letra minúscula.
* Al menos un número.
* Al menos un carácter especial.

La confirmación deberá coincidir exactamente con la nueva contraseña.

---

## 4. Estados de cuenta

La recuperación de contraseña podrá solicitarse independientemente del estado actual de la cuenta:

| Estado                    | ¿Puede recuperar contraseña? |
| ------------------------- | ---------------------------- |
| Pendiente de verificación | Sí                           |
| Activa                    | Sí                           |
| Bloqueada                 | Sí                           |
| Suspendida                | Sí                           |

La recuperación de contraseña **no cambia el estado de la cuenta**.

Por ejemplo:

```text id="f9c1ka"
BLOQUEADA
   ↓
Recuperación de contraseña
   ↓
Nueva contraseña
   ↓
BLOQUEADA
```

El desbloqueo de una cuenta corresponde a un proceso independiente.

---

## 5. Reglas de negocio

### RN-REC-01 — Solicitud de recuperación

Un usuario podrá solicitar la recuperación de su contraseña proporcionando el correo electrónico asociado a su cuenta.

### RN-REC-02 — Protección de información

El sistema no deberá revelar si el correo electrónico proporcionado corresponde o no a una cuenta registrada.

La respuesta al usuario deberá ser suficientemente general para evitar la enumeración de cuentas.

### RN-REC-03 — Generación del mecanismo de recuperación

Cuando corresponda, el sistema generará un código o mecanismo único de recuperación y lo enviará al correo registrado.

### RN-REC-04 — Vigencia

El código o mecanismo de recuperación tendrá una vigencia máxima de **24 horas**.

Una vez superado ese período, no podrá utilizarse.

### RN-REC-05 — Uso único

Un código o mecanismo de recuperación solo podrá utilizarse una vez.

Después de utilizarse correctamente deberá quedar invalidado.

### RN-REC-06 — Solicitud más reciente

Cuando se genere un nuevo código o mecanismo de recuperación, los anteriores deberán quedar invalidados.

### RN-REC-07 — Límite de intentos

Cada código de recuperación tendrá un máximo de **3 intentos de validación**.

Al superar dicho límite, el código deberá quedar invalidado.

### RN-REC-08 — Nueva solicitud

Cuando un código expire, sea invalidado o se alcance el límite de intentos, el usuario podrá solicitar un nuevo mecanismo de recuperación, sujeto a los límites de seguridad establecidos.

### RN-REC-09 — Restablecimiento correcto

Cuando el usuario proporcione un mecanismo válido y una nueva contraseña que cumpla las validaciones, el sistema deberá actualizar la contraseña de la cuenta.

### RN-REC-10 — No modificación del estado

El restablecimiento de contraseña no deberá modificar el estado actual de la cuenta.

Una cuenta suspendida permanecerá suspendida y una cuenta bloqueada permanecerá bloqueada hasta que se ejecute el proceso correspondiente.

### RN-REC-11 — No modificación de roles

El restablecimiento de contraseña no modifica los roles ni permisos asignados al usuario.

### RN-REC-12 — Invalidación de sesiones

Después de cambiar correctamente la contraseña, el sistema deberá invalidar las sesiones existentes asociadas a la cuenta.

El usuario deberá iniciar sesión nuevamente para acceder a funcionalidades privadas.

### RN-REC-13 — Registro del restablecimiento

El sistema deberá registrar el evento de restablecimiento de contraseña para fines de seguridad y auditoría.

### RN-REC-14 — Protección de credenciales

La contraseña no deberá almacenarse ni registrarse en texto plano.

### RN-REC-15 — Mensajes de error

Los mensajes mostrados al usuario no deberán revelar información técnica, credenciales ni datos sensibles de la cuenta.

---

## 6. Flujo principal

```text id="m5w7x2"
Usuario selecciona "¿Olvidaste tu contraseña?"
                    ↓
          Sistema muestra formulario
                    ↓
       Usuario proporciona correo
                    ↓
          Validar formato del correo
                    ↓
             Procesar solicitud
                    ↓
       ¿Existe una cuenta asociada?
             ┌──────┴──────┐
            NO             SÍ
             │              │
             │       Generar código/token
             │              ↓
             │       Enviar al correo
             │              ↓
             └──────→ Mostrar mensaje general
                            ↓
                 Usuario proporciona código
                            ↓
                    Validar código/token
                            ↓
                   ¿Es válido y vigente?
                     ┌──────┴──────┐
                    NO             SÍ
                     ↓              ↓
              Registrar intento   Solicitar
                     ↓            contraseña
               ¿Alcanzó límite?      ↓
                ┌────┴────┐    Validar contraseña
               NO         SÍ        ↓
               ↓           ↓    ¿Datos válidos?
          Permitir       Invalidar  ┌────┴────┐
          nuevo intento   código    NO        SÍ
                                    ↓          ↓
                                  Error     Actualizar
                                             contraseña
                                                ↓
                                        Invalidar sesiones
                                                ↓
                                         Registrar evento
                                                ↓
                                        Proceso completado
```

---

## 7. Validación del código

El sistema deberá comprobar:

```text id="w4d8jc"
Código recibido
      ↓
¿Existe?
      ↓ SÍ
¿Corresponde a una solicitud válida?
      ↓ SÍ
¿Es el código más reciente?
      ↓ SÍ
¿Está vigente?
      ↓ SÍ
¿No ha sido utilizado?
      ↓ SÍ
¿No superó 3 intentos?
      ↓ SÍ
Código válido
```

Si alguna validación falla, el sistema deberá rechazar el código.

---

## 8. Cambio de contraseña

Cuando el mecanismo de recuperación sea válido:

```text id="b3x6qp"
Código válido
     ↓
Nueva contraseña
     ↓
Confirmación
     ↓
Validaciones correctas
     ↓
Actualizar contraseña
     ↓
Invalidar código utilizado
     ↓
Invalidar sesiones existentes
     ↓
Registrar evento
     ↓
Informar finalización
```

El usuario deberá iniciar sesión nuevamente para acceder a funcionalidades privadas.

---

## 9. Casos especiales

### 9.1 Correo no registrado

El sistema deberá mostrar un mensaje general que no permita determinar si el correo existe.

### 9.2 Código incorrecto

Se registra el intento y se permite continuar mientras no se haya alcanzado el límite de 3 intentos.

### 9.3 Tercer intento incorrecto

El código queda invalidado y deberá solicitarse uno nuevo.

### 9.4 Código expirado

El código no podrá utilizarse y el usuario podrá solicitar uno nuevo.

### 9.5 Código ya utilizado

El código será rechazado y no podrá reutilizarse.

### 9.6 Código anterior

Si existe un código más reciente, cualquier código anterior será rechazado.

### 9.7 Contraseña no válida

El sistema deberá indicar que la nueva contraseña no cumple los requisitos establecidos.

### 9.8 Contraseñas diferentes

Si la confirmación no coincide con la nueva contraseña, el cambio no podrá completarse.

### 9.9 Problema de envío del correo

Si el sistema no puede enviar el mecanismo de recuperación, deberá informar que no fue posible completar la solicitud sin exponer información técnica.

### 9.10 Problema de conexión

Si se pierde la conexión durante el proceso, el sistema deberá evitar confirmar el cambio hasta que este haya sido procesado correctamente por el servidor.

### 9.11 Cuenta bloqueada

La contraseña podrá recuperarse, pero la cuenta continuará en estado **Bloqueada**.

El desbloqueo deberá realizarse mediante su proceso independiente.

### 9.12 Cuenta suspendida

La contraseña podrá recuperarse, pero la cuenta continuará en estado **Suspendida**.

El cambio de contraseña no elimina la suspensión.

---

## 10. Relación con otros procesos

```text id="j4s8zn"
REGISTRO
   ↓
Cuenta
   ↓
RECUPERACIÓN DE CONTRASEÑA
   ↓
Nueva contraseña
   ↓
INICIO DE SESIÓN
   ↓
SESIÓN
```

La recuperación también se relaciona con:

* Estado de cuenta.
* Seguridad.
* Sesiones.
* Roles y permisos.
* Correo electrónico.
* Auditoría.

El proceso de **desbloqueo de cuenta** se mantiene separado.

---

## 11. Separación entre recuperación y desbloqueo

Estos procesos tienen objetivos diferentes:

| Proceso                    | Objetivo                                              | ¿Cambia estado?        |
| -------------------------- | ----------------------------------------------------- | ---------------------- |
| Recuperación de contraseña | Establecer una nueva contraseña                       | No                     |
| Desbloqueo de cuenta       | Permitir nuevamente el acceso de una cuenta bloqueada | Sí, cuando corresponda |

Ejemplo:

```text id="q6x2rm"
CUENTA BLOQUEADA
      │
      ├── Recuperar contraseña
      │       ↓
      │   Sigue BLOQUEADA
      │
      └── Desbloquear cuenta
              ↓
           ACTIVA
```

---

## 12. Pendientes

Los siguientes aspectos deberán definirse posteriormente:

* Límites de solicitudes de recuperación.
* Frecuencia máxima de solicitudes.
* Mecanismo definitivo: código, enlace o combinación de ambos.
* Formato y longitud del código, si se utiliza.
* Política de almacenamiento del mecanismo de recuperación.
* Política de retención de registros de seguridad.
* Reglas técnicas de invalidación de sesiones.
* Plantilla y contenido definitivo del correo de recuperación.

---

## 13. Resultado esperado

Al completar correctamente la recuperación:

1. La identidad del usuario habrá sido validada mediante el mecanismo establecido.
2. La nueva contraseña cumplirá las reglas de seguridad.
3. La contraseña anterior dejará de ser válida.
4. El mecanismo de recuperación utilizado quedará invalidado.
5. Las sesiones existentes serán invalidadas.
6. El estado de la cuenta permanecerá sin cambios.
7. Los roles y permisos permanecerán sin cambios.
8. El evento quedará registrado para seguridad y auditoría.
9. El usuario deberá iniciar sesión nuevamente para acceder a funcionalidades privadas.
