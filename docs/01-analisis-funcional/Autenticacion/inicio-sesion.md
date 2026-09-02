# Inicio de sesión

## 1. Objetivo
-Permitir que un usuario con una cuenta activa se autentique de forma segura en ROSLYNDER mediante su nombre de usuario o correo electrónico y contraseña, para acceder a las funciones correspondientes a su cuenta, estado y permisos.
-El sistema deberá impedir el acceso a cuentas que se encuentren pendientes de activación, bloqueadas o que no cumplan las condiciones necesarias para iniciar sesión.

 ## 2. Datos

- **Identificador de acceso:** obligatorio. El usuario podrá ingresar su nombre de usuario o correo electrónico.
- **Contraseña:** obligatoria. Corresponde a la credencial utilizada para autenticar la cuenta.

## 3. Validaciones

### Identificador de acceso

- El campo es obligatorio.
- Debe contener un nombre de usuario o un correo electrónico.
- Si se utiliza un nombre de usuario, debe corresponder a una cuenta registrada.
- Si se utiliza un correo electrónico, debe corresponder a una cuenta registrada.

### Contraseña

- El campo es obligatorio.
- Debe coincidir con la contraseña registrada para la cuenta.
- La contraseña no deberá mostrarse en texto visible durante su ingreso.

### Estado de la cuenta

- La cuenta debe encontrarse en estado `Activa` para permitir el inicio de sesión.
- Una cuenta en estado `Pendiente de activación` no podrá iniciar sesión.
- Una cuenta bloqueada no podrá iniciar sesión hasta completar el proceso de desbloqueo correspondiente.

### Intentos de acceso

- Cada intento fallido de autenticación deberá registrarse.
- El sistema deberá contabilizar los intentos fallidos asociados a la cuenta.
- Al superar el límite de intentos establecido, la cuenta podrá quedar temporalmente bloqueada.
- Los intentos realizados durante un bloqueo no deberán permitir el acceso a la cuenta.

## 4. Reglas de negocio

### Acceso a la cuenta

1. Solo una cuenta en estado `Activa` podrá iniciar sesión.
2. El usuario podrá autenticarse utilizando su `nombre_usuario` o `correo_electronico`.
3. Una autenticación correcta permitirá acceder al panel correspondiente a las funciones y permisos del usuario.
4. Una autenticación incorrecta no permitirá crear una sesión válida.

### Intentos fallidos y bloqueo

5. Cada intento fallido de autenticación deberá registrarse.
6. El sistema deberá contabilizar los intentos fallidos asociados a la cuenta.
7. Cuando se supere el límite establecido de intentos fallidos, la cuenta será bloqueada temporalmente.
8. Una cuenta bloqueada no podrá iniciar sesión hasta completar el proceso de desbloqueo correspondiente.
9. El desbloqueo podrá realizarse mediante un código temporal de un solo uso.
10. El código de desbloqueo tendrá una vigencia de 24 horas.
11. El código quedará invalidado después de utilizarse.
12. La generación de un nuevo código invalidará el código anterior.
13. El sistema deberá limitar las solicitudes repetitivas de códigos de desbloqueo.
14. Una autenticación correcta deberá restablecer el contador de intentos fallidos.

### Seguridad y comportamiento

15. El sistema no deberá revelar información innecesaria sobre las credenciales durante los intentos fallidos.
16. Las validaciones realizadas en el navegador serán complementarias; la validación real de autenticación deberá realizarse en el servidor.
17. Una vez autenticado correctamente, el usuario accederá únicamente a las funciones que correspondan a su estado y permisos.
18. Un usuario que tenga funciones de `COMPRADOR` y `VENDEDOR` utilizará la misma cuenta para ambas.
19. Las funciones administrativas estarán disponibles únicamente para cuentas con el rol `ADMINISTRADOR`.

## 5. Flujo

1. El usuario selecciona la opción de inicio de sesión en ROSLYNDER.

2. El sistema muestra el formulario de autenticación.

3. El usuario ingresa su nombre de usuario o correo electrónico y su contraseña.

4. El usuario envía el formulario.

5. El sistema valida que los campos requeridos hayan sido proporcionados.

6. Si existen errores de validación, el sistema informa al usuario y permite corregir los datos.

7. Si los datos tienen el formato esperado, el sistema busca la cuenta correspondiente al nombre de usuario o correo electrónico proporcionado.

8. Si no existe una cuenta correspondiente, el sistema informa que las credenciales no son válidas y permite intentar nuevamente.

9. Si la cuenta existe, el sistema verifica su estado.

10. Si la cuenta se encuentra en estado `Pendiente de activación`, el sistema informa que debe verificarse el correo electrónico antes de iniciar sesión.

11. Si la cuenta se encuentra bloqueada, el sistema informa que no puede iniciar sesión y proporciona la opción de iniciar el proceso de desbloqueo.

12. Si la cuenta está activa, el sistema verifica la contraseña.

13. Si la contraseña es incorrecta, el sistema registra el intento fallido y actualiza el contador correspondiente.

14. Si se supera el límite de intentos establecido, el sistema bloquea temporalmente la cuenta y proporciona la opción de desbloqueo.

15. Si las credenciales son correctas, el sistema autentica al usuario y crea una sesión válida.

16. El sistema determina las funciones y permisos correspondientes al usuario.

17. El usuario es dirigido al panel o área correspondiente según sus funciones y permisos.

18. Una autenticación correcta restablece el contador de intentos fallidos.

## 6. Casos especiales

### 6.1 Cuenta inexistente

- Si el identificador proporcionado no corresponde a una cuenta registrada, el sistema no permitirá iniciar sesión.
- El sistema deberá mostrar un mensaje que no revele si el identificador corresponde o no a una cuenta existente.
- El intento podrá registrarse para fines de seguridad y trazabilidad.

### 6.2 Contraseña incorrecta

- Si la contraseña no coincide con la registrada, el sistema no permitirá iniciar sesión.
- El intento fallido deberá registrarse.
- El contador de intentos fallidos deberá actualizarse.
- Si se supera el límite establecido, la cuenta será bloqueada temporalmente.

### 6.3 Cuenta pendiente de activación

- Si la cuenta se encuentra en estado `Pendiente de activación`, el sistema no permitirá iniciar sesión.
- El usuario deberá ser informado de que debe verificar su correo electrónico.
- El sistema podrá ofrecer la opción de solicitar nuevamente el enlace de activación.

### 6.4 Cuenta bloqueada

- Si la cuenta se encuentra bloqueada, el sistema no permitirá iniciar sesión mediante las credenciales habituales.
- El usuario deberá recibir la opción de iniciar el proceso de desbloqueo correspondiente.
- El bloqueo permanecerá activo hasta completar correctamente dicho proceso.

### 6.5 Código de desbloqueo expirado

- Si el código de desbloqueo ha superado su vigencia de 24 horas, no podrá utilizarse.
- El sistema informará que el código ha expirado.
- El usuario podrá solicitar un nuevo código.

### 6.6 Código de desbloqueo incorrecto o inválido

- Si el código ingresado no coincide con el código válido asociado a la cuenta, el desbloqueo no se realizará.
- El sistema deberá verificar que el código no haya sido utilizado, que no haya expirado y que no haya sido reemplazado por uno más reciente.
- El sistema deberá controlar los intentos de ingreso del código para evitar abusos.

### 6.7 Solicitudes repetidas de código de desbloqueo

- El sistema deberá limitar la cantidad de solicitudes de códigos de desbloqueo.
- Las solicitudes podrán registrarse para fines de seguridad y trazabilidad.
- Ante un comportamiento anormal, el sistema podrá aplicar medidas adicionales de protección.

### 6.8 Código de desbloqueo no recibido

- Si el usuario no recibe el código de desbloqueo, podrá solicitar uno nuevo cuando corresponda.
- La solicitud deberá respetar los límites establecidos por el sistema.
- La generación de un nuevo código invalidará el código anterior.

### 6.9 Error del servidor

- Si ocurre un error interno durante el inicio de sesión, el sistema deberá mostrar un mensaje comprensible al usuario.
- No se deberán mostrar detalles técnicos ni información sensible.
- El error deberá registrarse internamente para facilitar su diagnóstico y seguimiento.
- El usuario deberá poder intentar nuevamente cuando el servicio se encuentre disponible.

  ## 7. Pendientes

- Definir el límite exacto de intentos fallidos de autenticación antes de bloquear temporalmente una cuenta.
- Definir la duración del bloqueo temporal de la cuenta.
- Definir el mecanismo exacto para solicitar y recibir el código de desbloqueo.
- Definir el límite de solicitudes de códigos de desbloqueo y el intervalo entre solicitudes.
- Definir qué información de seguridad se registrará para los intentos de inicio de sesión y bloqueos.
- Definir posteriormente el mecanismo técnico para la gestión de sesiones.

## 6. Casos especiales

## 7. Pendientes
