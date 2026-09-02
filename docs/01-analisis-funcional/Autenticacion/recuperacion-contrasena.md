1 Objetivo
-Permitir que un usuario que no recuerde su contraseña pueda recuperar el acceso a su cuenta mediante un proceso seguro de restablecimiento, utilizando el correo electrónico asociado a la cuenta.
-El proceso deberá permitir establecer una nueva contraseña sin revelar la contraseña anterior y sin permitir el acceso a la cuenta hasta completar correctamente las validaciones correspondientes.

## 2. Datos

- **Correo electrónico:** dirección asociada a la cuenta que solicita recuperar la contraseña.
- **Solicitud de recuperación:** representa la solicitud realizada por el usuario para restablecer su contraseña.
- **Enlace o código de recuperación:** mecanismo temporal utilizado para autorizar el restablecimiento de la contraseña.
- **Fecha de generación:** fecha y hora en que se generó el enlace o código de recuperación.
- **Fecha de expiración:** fecha y hora hasta la cual podrá utilizarse el mecanismo de recuperación.
- **Nueva contraseña:** nueva credencial que establecerá el usuario.
- **Confirmación de contraseña:** comprobación de que la nueva contraseña fue ingresada correctamente.

  ## 3. Validaciones

### Solicitud de recuperación

- El correo electrónico es obligatorio.
- El correo electrónico deberá tener un formato válido.
- La solicitud deberá corresponder a una cuenta registrada.
- El sistema no deberá revelar información que permita determinar si el correo electrónico está registrado.

### Código de recuperación

- El código es obligatorio para continuar con el restablecimiento.
- El código deberá corresponder a una solicitud de recuperación válida.
- El código deberá encontrarse dentro de su vigencia de 24 horas.
- El código deberá ser de un solo uso.
- Un código reemplazado por uno más reciente no podrá utilizarse.
- Un código utilizado o expirado no podrá utilizarse nuevamente.

### Nueva contraseña

- La nueva contraseña es obligatoria.
- Deberá cumplir las reglas de seguridad establecidas para las contraseñas de ROSLYNDER.
- La confirmación de contraseña es obligatoria.
- La confirmación deberá coincidir exactamente con la nueva contraseña.
- La nueva contraseña no deberá mostrarse en texto visible durante su ingreso.

### Cuenta

- La cuenta deberá existir y encontrarse en un estado que permita recuperar la contraseña.
- El restablecimiento correcto deberá actualizar la contraseña de la cuenta.

  ## 4. Reglas de negocio

### Solicitud de recuperación

1. El usuario podrá solicitar la recuperación de su contraseña utilizando el correo electrónico asociado a su cuenta.
2. El sistema deberá permitir realizar la solicitud sin revelar si el correo electrónico está registrado.
3. Una solicitud válida deberá generar un código temporal de recuperación.
4. El código deberá enviarse al correo electrónico asociado a la cuenta.
5. El código tendrá una vigencia de 24 horas.
6. La generación de un nuevo código deberá invalidar cualquier código anterior asociado a la misma solicitud o cuenta.

### Validación del código

7. El usuario deberá proporcionar un código válido para continuar con el restablecimiento.
8. El código deberá ser de un solo uso.
9. Un código expirado, utilizado, inválido o reemplazado no podrá utilizarse.
10. El usuario dispondrá de un máximo de 3 intentos para ingresar correctamente el código de recuperación. Al alcanzar este límite, el código será invalidado y deberá solicitarse uno nuevo.
11. El código de recuperación no deberá almacenarse de forma que permita recuperar directamente su valor original en caso de una exposición de datos.

### Restablecimiento de contraseña

12. El usuario deberá establecer una nueva contraseña que cumpla las reglas de seguridad definidas para ROSLYNDER.
13. La contraseña anterior no deberá mostrarse ni enviarse al usuario.
14. Una recuperación completada correctamente deberá reemplazar la contraseña anterior por la nueva.
15. El código de recuperación deberá quedar invalidado después de completar correctamente el restablecimiento.
16. Una vez restablecida la contraseña, el usuario podrá iniciar sesión utilizando la nueva contraseña.

### Seguridad y trazabilidad

17. El sistema deberá limitar las solicitudes repetitivas de recuperación de contraseña.
18. Los eventos relevantes del proceso de recuperación deberán poder registrarse para fines de seguridad y trazabilidad.
19. Los errores mostrados al usuario no deberán revelar información técnica ni sensible.
20. El sistema deberá evitar que el proceso de recuperación permita obtener información sobre la contraseña anterior.


- La cuenta deberá existir y encontrarse en un estado que permita recuperar la contraseña.
- El restablecimiento correcto deberá actualizar la contraseña de la cuenta.

 ## 5. Flujo

1. El usuario selecciona la opción `¿Olvidaste tu contraseña?` en ROSLYNDER.
2. El sistema muestra el formulario para solicitar la recuperación.
3. El usuario ingresa el correo electrónico asociado a su cuenta.
4. El usuario envía la solicitud.
5. El sistema valida que el correo tenga un formato válido.
6. El sistema procesa la solicitud sin revelar si el correo electrónico corresponde o no a una cuenta registrada.
7. Si la solicitud corresponde a una cuenta registrada, el sistema genera un código temporal de recuperación.
8. El sistema envía el código al correo electrónico asociado a la cuenta.
9. El sistema informa al usuario que revise su correo electrónico para continuar con el proceso.
10. El usuario ingresa el código recibido.
11. El sistema valida que el código sea correcto, vigente, no haya sido utilizado y continúe siendo el código válido para la cuenta.
12. Si el código no supera alguna validación, el sistema informa que el código no es válido y permite intentar nuevamente de acuerdo con los límites establecidos.
13. Si el código es válido, el sistema permite continuar con el establecimiento de una nueva contraseña.
14. El usuario ingresa la nueva contraseña y su confirmación.
15. El sistema valida que la nueva contraseña cumpla las reglas de seguridad establecidas y que ambas contraseñas coincidan.
16. Si existen errores de validación, el sistema informa al usuario y permite corregir los datos.
17. Si todas las validaciones son correctas, el sistema actualiza la contraseña de la cuenta.
18. El código de recuperación queda invalidado y no podrá utilizarse nuevamente.
19. El sistema registra el restablecimiento de la contraseña para fines de seguridad y trazabilidad.
20. El sistema informa al usuario que la contraseña fue restablecida correctamente.
21. El usuario podrá iniciar sesión utilizando su nueva contraseña.

 ## 6. Casos especiales

### 6.1 Correo electrónico no registrado

- Si el correo ingresado no corresponde a una cuenta registrada, el sistema no deberá revelar esta situación al usuario.
- El sistema deberá mostrar una respuesta general indicando que, si existe una cuenta asociada, se han enviado las instrucciones correspondientes.
- La solicitud podrá registrarse para fines de seguridad y control.

### 6.2 Código incorrecto

- Si el código ingresado no coincide con el código válido, el sistema no permitirá continuar con el restablecimiento.
- El intento deberá registrarse.
- El usuario podrá intentar nuevamente mientras no haya superado los límites establecidos.

### 6.3 Código expirado

- Si el código supera su vigencia de 24 horas, no podrá utilizarse.
- El sistema informará al usuario que el código ha expirado.
- El usuario podrá solicitar un nuevo código cuando corresponda.

### 6.4 Código ya utilizado

- Si el usuario intenta utilizar un código que ya fue utilizado, el sistema no permitirá continuar con el restablecimiento.
- El sistema informará que el código ya no es válido.
- El usuario podrá solicitar un nuevo código cuando corresponda.

### 6.5 Código reemplazado

- Si el usuario solicita un nuevo código, el código anterior quedará invalidado.
- Si posteriormente intenta utilizar el código anterior, el sistema no permitirá continuar con el proceso.
- El usuario deberá utilizar el código más reciente.

### 6.6 Exceso de intentos con el código

- El usuario podrá solicitar un nuevo código cuando corresponda.
- El sistema deberá registrar el evento para fines de seguridad y trazabilidad.
- Si el usuario alcanza los 3 intentos fallidos, el código actual quedará invalidado y no podrá utilizarse nuevamente.

### 6.7 Solicitudes repetitivas de recuperación

- Si el usuario realiza solicitudes de recuperación de manera repetitiva, el sistema deberá aplicar los límites establecidos.
- Las solicitudes que superen dichos límites no deberán generar nuevos códigos durante el periodo de restricción.
- El evento podrá registrarse para fines de seguridad y trazabilidad.

### 6.8 Nueva contraseña no válida

- Si la nueva contraseña no cumple las reglas de seguridad establecidas, el sistema no permitirá completar el restablecimiento.
- El usuario deberá corregir la contraseña antes de continuar.
- La contraseña anterior permanecerá vigente mientras el proceso no se complete correctamente.

### 6.9 Contraseñas no coinciden

- Si la nueva contraseña y su confirmación no coinciden, el sistema no permitirá completar el restablecimiento.
- El usuario deberá ingresar nuevamente ambos valores.

### 6.10 Pérdida de conexión durante el proceso

- Si se pierde la conexión durante la recuperación, el sistema deberá evitar cambios incompletos en la cuenta.
- Si el restablecimiento no pudo completarse, la contraseña anterior deberá permanecer vigente.
- El usuario podrá continuar posteriormente utilizando un código que todavía sea válido.

### 6.11 Error durante el envío del código

- Si ocurre un error al enviar el código de recuperación, el sistema deberá registrar el incidente.
- El usuario deberá recibir un mensaje comprensible sin información técnica.
- La cuenta no deberá modificar su contraseña como consecuencia de un envío fallido.
- El usuario podrá solicitar nuevamente el código cuando corresponda.

### 6.12 Error del servidor

- Si ocurre un error interno durante el proceso de recuperación, el sistema deberá mostrar un mensaje comprensible al usuario.
- No se deberán mostrar detalles técnicos ni información sensible.
- El error deberá registrarse internamente para facilitar su diagnóstico y seguimiento.
- La contraseña no deberá modificarse si el proceso no se completa correctamente.

## 7. Pendientes

- Definir posteriormente el mecanismo técnico para generar, almacenar y validar los códigos de recuperación.
- Definir posteriormente el mecanismo técnico para aplicar los límites de solicitudes y controlar los intentos de ingreso de códigos.
- Definir el comportamiento de las sesiones existentes después de un restablecimiento de contraseña dentro de `06-gestion-sesiones.md`.
- Definir posteriormente los mecanismos técnicos para registrar los eventos de seguridad y trazabilidad.
  
