# Registro de usuario
   
## 1. Objetivo
-Permitir que una persona cree una cuenta en ROSLYNDER proporcionando sus datos personales y credenciales de acceso, aceptando     los términos y condiciones y verificando su correo electrónico para activar la cuenta.
-La cuenta permanecerá en estado "Pendiente de activación" hasta que el usuario verifique correctamente su correo electrónico.
## 2. Datos
| Dato                                 | ¿Obligatorio? | Observación                    |
| ------------------------------------ | ------------- | ------------------------------ |
| Nombre                               | Sí            | Identificación personal        |
| Apellido                             | Sí            | Identificación personal        |
| Nombre de usuario                    | Sí            | Identificador para acceso      |
| Correo electrónico                   | Sí            | Verificación y recuperación    |
| Contraseña                           | Sí            | Credencial de acceso           |
| Confirmación de contraseña           | Sí            | Validación del registro        |
| Provincia                            | Sí            | Ubicación                      |
| Ciudad                               | Sí            | Ubicación                      |
| Dirección                            | Sí            | Ubicación/domicilio            |
| Teléfono                             | Sí            | Contacto                       |
| Fecha de nacimiento                  | No            | Dato opcional                  |
| Aceptación de términos y condiciones | Sí            | Requisito para crear la cuenta |

## 3. Validaciones


### Datos personales

- **Nombre:** obligatorio.
- **Apellido:** obligatorio.
- **Dirección:** obligatoria.
- **Provincia:** obligatoria.
- **Ciudad:** obligatoria y debe corresponder a la provincia seleccionada.
- **Teléfono:** obligatorio y debe contener exactamente 9 dígitos numéricos para Ecuador.
- **Fecha de nacimiento:** opcional. Si se proporciona, debe corresponder a una fecha válida.

### Credenciales

- **Nombre de usuario:** obligatorio.
- Debe tener una longitud mínima de 6 y máxima de 16 caracteres.
- Debe cumplir con el formato de caracteres permitido.
- Debe ser único dentro de ROSLYNDER.
- **Correo electrónico:** obligatorio.
- Debe cumplir con un formato válido de correo electrónico.
- Debe ser único dentro de ROSLYNDER.
- **Contraseña:** obligatoria.
- Debe tener una longitud mínima de 6 y máxima de 16 caracteres.
- Debe contener al menos una letra mayúscula, una letra minúscula, un número y un carácter especial.
- **Confirmación de contraseña:** obligatoria.
- Debe coincidir exactamente con la contraseña ingresada.

### Términos y condiciones

- El usuario debe aceptar los términos y condiciones para completar el registro.
- 
## 4. Reglas de negocio

### Creación de la cuenta

1. El usuario podrá crear una cuenta proporcionando los datos requeridos.
2. El correo electrónico debe ser único dentro de ROSLYNDER.
3. El nombre de usuario debe ser único dentro de ROSLYNDER.
4. Una cuenta nueva se creará inicialmente en estado `Pendiente de activación`.
5. Una vez completado correctamente el registro, el sistema enviará un correo de verificación a la dirección proporcionada.
6. La cuenta permanecerá en estado `Pendiente de activación` hasta que el usuario verifique correctamente su correo electrónico.
7. Una cuenta en estado `Pendiente de activación` no podrá iniciar sesión.

### Verificación de la cuenta

8. El sistema generará un enlace de activación asociado a la cuenta registrada.
9. El enlace de activación tendrá una vigencia de 24 horas.
10. La verificación correcta cambiará el estado de la cuenta de `Pendiente de activación` a `Activa`.
11. Un enlace de activación utilizado, inválido o expirado no podrá utilizarse nuevamente.
12. El usuario podrá solicitar un nuevo enlace de activación cuando corresponda.
13. La generación de un nuevo enlace invalidará el enlace anterior.


### Roles y capacidades

14. Una cuenta activa podrá utilizar las funciones correspondientes al rol de `COMPRADOR`.
15. Un usuario podrá habilitar posteriormente las funciones de `VENDEDOR` sin crear una segunda cuenta.
16. Para habilitar las funciones de vendedor, el usuario deberá completar la información comercial requerida y superar las validaciones establecidas.
17. Un mismo usuario podrá desempeñar las funciones de `COMPRADOR` y `VENDEDOR`.
18. El rol `ADMINISTRADOR` no podrá obtenerse mediante el registro público.
19. La asignación del rol `ADMINISTRADOR` será realizada mediante un proceso administrativo.

### Protección y control

20. Si el correo electrónico ya se encuentra registrado, el sistema no permitirá crear una nueva cuenta con dicho correo.
21. Si el nombre de usuario ya se encuentra registrado, el sistema no permitirá crear una nueva cuenta con dicho nombre.
22. El sistema deberá limitar solicitudes repetitivas relacionadas con la activación de cuentas, como el reenvío del correo de verificación.
23. Los eventos relevantes del proceso de registro y activación deberán poder registrarse para fines de seguridad y trazabilidad.
24. Ante un error del servidor, el sistema deberá mostrar al usuario un mensaje comprensible sin revelar información técnica interna.
25. Los errores del proceso deberán registrarse internamente cuando sea necesario para facilitar su diagnóstico y seguimiento.


## 5. Flujo


1. El visitante selecciona la opción de registro en ROSLYNDER.

2. El sistema muestra el formulario de registro.

3. El usuario completa los datos requeridos y, opcionalmente, la fecha de nacimiento.

4. El usuario acepta los términos y condiciones.

5. El usuario envía el formulario.

6. El sistema valida los datos ingresados.

7. Si existen errores de validación, el sistema informa los campos que deben corregirse y permite volver a enviar el formulario.

8. Si los datos son válidos, el sistema verifica que el correo electrónico y el nombre de usuario no estén registrados previamente.

9. Si el correo electrónico o el nombre de usuario ya están registrados, el sistema informa la situación y no crea una nueva cuenta.

10. Si toda la información es válida y no existen duplicados, el sistema crea la cuenta en estado `Pendiente de activación`.

11. El sistema genera un enlace de verificación y lo envía al correo electrónico registrado.

12. El usuario accede al enlace recibido.

13. El sistema valida el enlace de verificación.

14. Si el enlace es válido y se encuentra vigente, el sistema activa la cuenta.

15. La cuenta cambia de `Pendiente de activación` a `Activa`.

16. El usuario podrá iniciar sesión utilizando sus credenciales.

17. Si el enlace es inválido, ya fue utilizado o expiró, el sistema no modifica el estado de la cuenta y permite solicitar un nuevo enlace cuando corresponda.
18. VISITANTE
   │
   ▼
Formulario de registro
   │
   ▼
Completa datos
   │
   ▼
Acepta términos
   │
   ▼
Enviar registro
   │
   ▼
Validaciones
   │
   ├── ❌ Error ──→ Corregir datos
   │
   ▼
¿Correo/usuario disponibles?
   │
   ├── ❌ No ──→ Informar al usuario
   │
   ▼
Crear cuenta
   │
   ▼
PENDIENTE DE ACTIVACIÓN
   │
   ▼
Enviar correo de verificación
   │
   ▼
Usuario abre enlace
   │
   ▼
¿Enlace válido?
   │
   ├── ❌ No ──→ Solicitar nuevo enlace
   │
   ▼
ACTIVA
   │
   ▼
INICIAR SESIÓN

## 6. Casos especiales

### 6.1 Correo electrónico ya registrado

- Si el correo electrónico ingresado ya pertenece a una cuenta registrada, el sistema no permitirá crear una nueva cuenta con dicho correo.
- El sistema deberá informar al usuario que el correo ya se encuentra registrado.
- Por razones de seguridad, la información mostrada no deberá revelar datos de la cuenta existente.
- El intento podrá registrarse para fines de seguridad y trazabilidad.

### 6.2 Nombre de usuario ya registrado

- Si el nombre de usuario ingresado ya se encuentra registrado, el sistema no permitirá utilizarlo nuevamente.
- El sistema informará al usuario que debe seleccionar otro nombre de usuario.
- El intento podrá registrarse para fines de seguridad y trazabilidad.

### 6.3 Correo de activación no recibido

- Si el usuario no recibe el correo de activación, podrá solicitar el reenvío del enlace.
- El sistema deberá limitar la cantidad de solicitudes de reenvío para evitar abusos.
- El nuevo enlace deberá invalidar el enlace de activación anterior.
- La cuenta permanecerá en estado `Pendiente de activación` hasta completar correctamente la verificación.

### 6.4 Enlace de activación expirado

- Si el usuario intenta utilizar un enlace cuya vigencia ha finalizado, el sistema deberá informar que el enlace ha expirado.
- El sistema no deberá modificar el estado de la cuenta.
- El usuario podrá solicitar un nuevo enlace de activación cuando corresponda.

### 6.5 Enlace de activación ya utilizado

- Si el usuario intenta utilizar nuevamente un enlace que ya fue utilizado, el sistema deberá informar que el enlace ya no es válido.
- El sistema no deberá modificar nuevamente el estado de la cuenta.
- Si la cuenta ya se encuentra activa, el usuario podrá continuar con el inicio de sesión.

### 6.6 Pérdida de conexión durante el registro

- Si se pierde la conexión mientras el usuario completa el formulario, la información ingresada deberá conservarse temporalmente cuando sea técnicamente posible.
- Si la pérdida de conexión ocurre durante el envío, el sistema deberá evitar la creación de registros duplicados cuando el usuario intente nuevamente.
- El usuario deberá recibir una indicación clara de que debe verificar o reintentar el envío.

### 6.7 Registro abandonado

- Si el usuario abandona el formulario antes de completar el registro, los datos ingresados podrán conservarse temporalmente en el navegador para permitir continuar posteriormente.
- La información temporal de un registro no enviado no deberá considerarse una cuenta creada.
- Si el usuario completa posteriormente el registro, los datos deberán volver a validarse antes de crear la cuenta.
- El mecanismo utilizado para conservar temporalmente los datos se definirá durante la implementación.

### 6.8 Error del servidor

- Si ocurre un error interno durante el registro, el sistema deberá mostrar un mensaje comprensible al usuario.
- No se deberán mostrar detalles técnicos, mensajes internos ni información sensible.
- El error deberá registrarse internamente para facilitar su diagnóstico y seguimiento.
- Cuando sea posible, el usuario deberá poder reintentar el proceso sin perder la información ingresada.

### 6.9 Error en el envío del correo de activación

- Si la cuenta fue creada correctamente pero el correo de activación no pudo enviarse, la cuenta deberá permanecer en estado `Pendiente de activación`.
- El sistema deberá registrar el error del envío.
- El usuario deberá poder solicitar nuevamente el correo de activación.
- El sistema podrá realizar nuevos intentos de envío de acuerdo con las reglas de control establecidas.
- La cuenta no deberá activarse hasta que el usuario complete correctamente la verificación.
## 7. Pendientes
## 7. Pendientes

- Definir el formato exacto permitido para `nombre_usuario`, especialmente si se permitirán caracteres como `_` (guion bajo) o `.` (punto), además de letras y números.
- Definir si ROSLYNDER establecerá una edad mínima para crear una cuenta.
- Definir el mecanismo y los límites para solicitar nuevamente el correo de activación.
- Definir las condiciones y el tiempo de conservación de los datos de un registro abandonado.
- Definir posteriormente los mecanismos técnicos para registrar eventos de seguridad y trazabilidad.


