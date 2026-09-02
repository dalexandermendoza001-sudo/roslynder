
- La cuenta debe existir.
- La cuenta debe encontrarse en estado `Pendiente de activación` para completar la verificación.
- La cuenta no deberá activarse mediante un enlace inválido, expirado o ya utilizado.

 3.2Enlace de activación
## 1 Objetivo
-Permitir que un usuario confirme su dirección de correo electrónico mediante un enlace de verificación enviado por ROSLYNDER, para validar la cuenta y cambiar su estado de Pendiente de activación a Activa.
-El sistema deberá impedir que una cuenta pendiente de activación acceda a las funciones que requieren autenticación hasta completar correctamente la verificación.


## 2. Datos

- **Cuenta:** corresponde a la cuenta que se encuentra en estado `Pendiente de activación`.
- **Correo electrónico:** dirección asociada a la cuenta a la que se enviará el enlace de verificación.
- **Enlace de activación:** enlace generado por el sistema para verificar la cuenta.
- **Fecha de generación:** fecha y hora en que se generó el enlace.
- **Fecha de expiración:** fecha y hora hasta la cual el enlace podrá utilizarse.
- **Estado de verificación:** indica si la cuenta continúa pendiente o ya fue verificada.

## 3. Validaciones

 3.1Cuenta

- El enlace debe corresponder a una cuenta registrada.
- El enlace debe encontrarse dentro de su vigencia de 24 horas.
- El enlace debe ser válido y no haber sido utilizado previamente.
- Si existe un enlace más reciente para la misma cuenta, los enlaces anteriores deberán considerarse inválidos.

 3.3Verificación

- La verificación deberá realizarse correctamente antes de cambiar el estado de la cuenta.
- Una verificación exitosa deberá cambiar el estado de `Pendiente de activación` a `Activa`.


## 4. Reglas de negocio

1. Toda cuenta creada mediante el registro deberá permanecer en estado `Pendiente de activación` hasta verificar el correo electrónico.
2. El sistema deberá enviar un enlace de activación al correo registrado.
3. El enlace de activación tendrá una vigencia de 24 horas.
4. El enlace de activación será de un solo uso.
5. La utilización correcta del enlace cambiará el estado de la cuenta de `Pendiente de activación` a `Activa`.
6. Un enlace expirado no podrá utilizarse para activar la cuenta.
7. Un enlace ya utilizado no podrá utilizarse nuevamente.
8. La generación de un nuevo enlace deberá invalidar el enlace anterior.
9. Una cuenta activa no deberá volver a activarse mediante el proceso de verificación.
10. Una cuenta que no haya completado la verificación no podrá iniciar sesión.
11. La verificación deberá registrarse para fines de seguridad y trazabilidad.
12. Los errores durante la verificación no deberán revelar información técnica ni sensible.


## 5. Flujo

1. El usuario completa correctamente el proceso de registro.
2. El sistema crea la cuenta en estado `Pendiente de activación`.
3. El sistema genera un enlace de activación.
4. El sistema envía el enlace al correo electrónico registrado.
5. El usuario recibe el correo y selecciona el enlace de activación.
6. El sistema recibe la solicitud de verificación.
7. El sistema valida que el enlace corresponda a una cuenta existente.
8. El sistema verifica que el enlace sea válido y no haya expirado.
9. El sistema verifica que el enlace no haya sido utilizado y que continúe siendo el enlace vigente para la cuenta.
10. Si alguna validación falla, el sistema informa al usuario que el enlace no es válido o ha expirado.
11. Si todas las validaciones son correctas, el sistema confirma la dirección de correo.
12. El sistema cambia el estado de la cuenta de `Pendiente de activación` a `Activa`.
13. El sistema invalida el enlace utilizado.
14. El sistema registra la verificación para fines de seguridad y trazabilidad.
15. El sistema informa al usuario que su cuenta ha sido activada correctamente.
16. El usuario podrá iniciar sesión utilizando sus credenciales.


## 6. Casos especiales

### 6.1 Enlace de activación expirado

- Si el usuario intenta utilizar un enlace cuya vigencia de 24 horas ha finalizado, el sistema no permitirá activar la cuenta.
- El sistema informará que el enlace ha expirado.
- La cuenta permanecerá en estado `Pendiente de activación`.
- El usuario podrá solicitar un nuevo enlace de activación cuando corresponda.

### 6.2 Enlace de activación inválido

- Si el enlace no corresponde a una cuenta registrada o no contiene una referencia válida para realizar la verificación, el sistema no permitirá activar la cuenta.
- El sistema informará que el enlace no es válido.
- La cuenta no deberá modificar su estado como consecuencia de un enlace inválido.

### 6.3 Enlace de activación ya utilizado

- Si el usuario intenta utilizar nuevamente un enlace que ya fue utilizado, el sistema no realizará una nueva activación.
- El sistema informará que el enlace ya no es válido.
- Si la cuenta ya se encuentra activa, el usuario podrá continuar con el inicio de sesión.

### 6.4 Enlace anterior invalidado

- Si el usuario solicita un nuevo enlace de activación, el enlace anterior dejará de ser válido.
- Si posteriormente intenta utilizar el enlace anterior, el sistema no permitirá la activación.
- El usuario deberá utilizar el enlace más reciente.

### 6.5 Cuenta ya activa

- Si el usuario intenta utilizar un enlace de activación correspondiente a una cuenta que ya se encuentra `Activa`, el sistema no deberá modificar nuevamente el estado de la cuenta.
- El sistema podrá informar que la cuenta ya se encuentra activada.
- El usuario podrá continuar con el inicio de sesión.

### 6.6 Error durante la verificación

- Si ocurre un error interno durante el proceso de verificación, el sistema no deberá activar la cuenta de forma parcial o incorrecta.
- El sistema deberá mostrar un mensaje comprensible al usuario.
- No se deberán mostrar detalles técnicos ni información sensible.
- El error deberá registrarse internamente para facilitar su diagnóstico y seguimiento.
- El usuario podrá intentar nuevamente cuando el servicio se encuentre disponible.

### 6.7 Pérdida de conexión durante la verificación

- Si se pierde la conexión durante el proceso de verificación, el sistema deberá evitar cambios incompletos en el estado de la cuenta.
- Si la verificación no pudo completarse, la cuenta deberá permanecer en el estado que tenía antes del intento.
- El usuario podrá volver a utilizar un enlace válido mientras este continúe vigente.


## 7. Pendientes

- Definir el mecanismo para solicitar nuevamente el enlace de activación.
- Definir el límite de solicitudes de nuevos enlaces de activación y el intervalo entre solicitudes.
- Definir qué información relacionada con la verificación se registrará para fines de seguridad y trazabilidad.
- Definir posteriormente el mecanismo técnico para generar, almacenar y validar los enlaces de activación.
