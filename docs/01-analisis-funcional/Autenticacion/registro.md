# Registro de usuario

## 1. Objetivo

Permitir que una persona cree una cuenta en ROSLYNDER proporcionando sus datos personales y de contacto, aceptando los términos y condiciones y verificando su correo electrónico.

Una vez verificada la cuenta, el usuario podrá iniciar sesión y utilizar las funciones disponibles según su estado y roles asignados.

El registro no permite obtener directamente el rol de administrador ni convierte automáticamente al usuario en vendedor.

---

## 2. Datos solicitados

| Dato                       | Obligatorio | Descripción                                                       |
| -------------------------- | ----------- | ----------------------------------------------------------------- |
| Nombres                    | Sí          | Nombres del usuario.                                              |
| Apellidos                  | Sí          | Apellidos del usuario.                                            |
| Nombre de usuario          | Sí          | Identificador único utilizado para iniciar sesión.                |
| Correo electrónico         | Sí          | Correo utilizado para identificación y verificación de la cuenta. |
| Contraseña                 | Sí          | Credencial de acceso.                                             |
| Confirmación de contraseña | Sí          | Debe coincidir con la contraseña.                                 |
| Provincia                  | Sí          | Provincia de residencia seleccionada de una lista válida.         |
| Ciudad                     | Sí          | Ciudad correspondiente a la provincia seleccionada.               |
| Fecha de nacimiento        | No          | Dato opcional.                                                    |
| Teléfono                   | Sí          | Número telefónico del usuario.                                    |
| Dirección                  | Sí          | Dirección de residencia o contacto.                               |
| Términos y condiciones     | Sí          | El usuario debe aceptar los términos para completar el registro.  |

---

## 3. Validaciones

### 3.1 Nombres y apellidos

* Son campos obligatorios.
* No pueden estar vacíos.
* Deben contener información válida.

### 3.2 Nombre de usuario

* Es obligatorio.
* Debe contener entre **6 y 16 caracteres**.
* Solo se permiten letras y números.
* No se permiten espacios ni caracteres especiales.
* Debe ser único dentro de ROSLYNDER.
* Una vez registrado, el nombre de usuario no podrá modificarse.

### 3.3 Correo electrónico

* Es obligatorio.
* Debe cumplir un formato válido de correo electrónico.
* Debe ser único dentro de ROSLYNDER.
* El acceso al correo se comprobará mediante el proceso de verificación.
* El correo utilizado durante el registro será el correo asociado a la cuenta.

### 3.4 Contraseña

La contraseña:

* Es obligatoria.
* Debe tener entre **6 y 16 caracteres**.
* Debe contener al menos:

  * una letra mayúscula;
  * una letra minúscula;
  * un número;
  * un carácter especial.

### 3.5 Confirmación de contraseña

* Es obligatoria.
* Debe coincidir exactamente con la contraseña ingresada.

### 3.6 Provincia y ciudad

* La provincia es obligatoria.
* La provincia debe seleccionarse de las opciones disponibles.
* La ciudad es obligatoria.
* La ciudad debe corresponder a la provincia seleccionada.

### 3.7 Fecha de nacimiento

* Es opcional.
* Si se proporciona, debe corresponder a una fecha válida.
* En esta versión no se establece una edad mínima, salvo que posteriormente se defina una regla de negocio que lo requiera.

### 3.8 Teléfono

* Es obligatorio.
* Debe contener exactamente **9 dígitos numéricos**, de acuerdo con el formato definido para números móviles en Ecuador.
* No se permiten letras ni caracteres no contemplados por el formato establecido.

### 3.9 Dirección

* Es obligatoria.
* No puede estar vacía.

### 3.10 Términos y condiciones

* El usuario debe aceptar los términos y condiciones.
* Sin aceptación, el registro no puede completarse.

### 3.11 Validación del sistema

Las validaciones realizadas en el navegador sirven para proporcionar una respuesta inmediata al usuario.

La validación definitiva debe realizarse en el servidor antes de crear la cuenta.

---

## 4. Reglas de negocio

### RN-REG-01 — Correo único

Cada cuenta de ROSLYNDER debe tener un correo electrónico único.

### RN-REG-02 — Nombre de usuario único

Cada cuenta debe tener un nombre de usuario único.

### RN-REG-03 — Nombre de usuario permanente

El nombre de usuario no podrá modificarse después del registro.

### RN-REG-04 — Estado inicial de la cuenta

Cuando el registro se completa correctamente, la cuenta se crea en estado:

**Pendiente de verificación**

### RN-REG-05 — Verificación de correo

El sistema enviará al correo registrado un enlace único para verificar la cuenta.

### RN-REG-06 — Vigencia del enlace

El enlace de verificación tendrá una vigencia de **24 horas** y será de un solo uso.

### RN-REG-07 — Cuenta pendiente

Mientras la cuenta permanezca en estado **Pendiente de verificación**, el usuario no podrá iniciar sesión.

### RN-REG-08 — Activación

Cuando el usuario complete correctamente la verificación del correo, la cuenta cambiará de:

**Pendiente de verificación → Activa**

### RN-REG-09 — Rol inicial

Una cuenta activa podrá utilizar ROSLYNDER como **COMPRADOR**.

No es necesario que el usuario seleccione manualmente el rol de comprador durante el registro.

### RN-REG-10 — Una cuenta para diferentes funciones

Un mismo usuario podrá utilizar la misma cuenta para las funciones de comprador y vendedor.

No será necesario crear una segunda cuenta para vender.

### RN-REG-11 — Habilitación de funciones de vendedor

Para habilitar las funciones de vendedor, un usuario activo deberá seleccionar la opción correspondiente y completar la información comercial requerida.

### RN-REG-12 — Validación de información comercial

Las funciones de vendedor se habilitarán una vez que la información comercial requerida haya sido completada y validada según las reglas establecidas para el perfil comercial.

### RN-REG-13 — Roles comprador y vendedor

Un mismo usuario podrá tener simultáneamente las funciones correspondientes a:

* COMPRADOR
* VENDEDOR

La asignación de estas funciones se gestionará mediante el sistema de roles.

### RN-REG-14 — Administrador

El rol **ADMINISTRADOR** no podrá obtenerse mediante el registro público.

Un usuario no podrá asignarse a sí mismo este rol.

La asignación del rol de administrador deberá realizarse mediante el mecanismo administrativo correspondiente.

### RN-REG-15 — Navegación pública

Las personas podrán navegar por las publicaciones disponibles sin necesidad de registrarse.

Las acciones que requieran una cuenta deberán solicitar al usuario que se registre o inicie sesión.

### RN-REG-16 — Información de seguridad

Los eventos relacionados con seguridad, autenticación y registro podrán generar información de auditoría, como fecha, hora, dirección IP, navegador y resultado de la operación.

Esta información corresponde al control de seguridad y no forma parte de los datos personales básicos del usuario.

---

## 5. Flujo principal

```text
VISITANTE
    │
    ▼
Explora ROSLYNDER
    │
    ▼
Intenta realizar una acción que requiere cuenta
    │
    ▼
¿Tiene una cuenta?
    ├── NO ──► Registro
    │
    └── SÍ ──► Inicio de sesión
```

### Registro

```text
Registro
   │
   ▼
Completar formulario
   │
   ▼
Validar datos
   │
   ├── Datos incorrectos
   │       │
   │       ▼
   │   Mostrar errores
   │       │
   │       └────► Corregir datos
   │
   └── Datos correctos
           │
           ▼
     Crear cuenta
           │
           ▼
Pendiente de verificación
           │
           ▼
Enviar correo de verificación
           │
           ▼
Usuario abre enlace
           │
           ▼
¿Enlace válido?
      ├── NO ──► Mostrar motivo
      │
      └── SÍ
           │
           ▼
     Cuenta ACTIVA
           │
           ▼
      Iniciar sesión
           │
           ▼
       COMPRADOR
```

### Conversión a vendedor

El proceso de registro no convierte automáticamente al usuario en vendedor.

El proceso posterior será:

```text
CUENTA ACTIVA
      │
      ▼
   COMPRADOR
      │
      ▼
"QUIERO VENDER"
      │
      ▼
Completar información comercial
      │
      ▼
Validar información
      │
      ▼
   VENDEDOR
      │
      ▼
Funciones comerciales
```

---

## 6. Casos especiales

### CE-REG-01 — Correo ya registrado

Si el correo ingresado ya pertenece a una cuenta:

* El sistema informará que el correo ya está registrado.
* No se creará una nueva cuenta con ese correo.
* El usuario podrá utilizar el inicio de sesión o recuperación de contraseña según corresponda.

El evento podrá registrarse como evento de seguridad para fines de control y auditoría.

### CE-REG-02 — Nombre de usuario ya registrado

Si el nombre de usuario ya existe:

* El sistema informará al usuario.
* No se creará la cuenta.
* El usuario deberá seleccionar otro nombre de usuario.

### CE-REG-03 — Correo de verificación no recibido

El usuario podrá solicitar el reenvío del correo de verificación.

El sistema deberá aplicar límites para evitar solicitudes excesivas.

### CE-REG-04 — Enlace de verificación vencido

Si el enlace supera las 24 horas:

* No podrá utilizarse para verificar la cuenta.
* El sistema informará que el enlace ha vencido.
* El usuario podrá solicitar un nuevo enlace.

### CE-REG-05 — Enlace ya utilizado

Si el usuario intenta utilizar nuevamente un enlace de verificación:

* El sistema rechazará el enlace.
* Si la cuenta ya está activa, informará que la cuenta ya fue verificada.

### CE-REG-06 — Nuevo enlace de verificación

Cuando se genere un nuevo enlace:

* El enlace anterior quedará invalidado.
* Solo el enlace vigente podrá utilizarse para verificar la cuenta.

### CE-REG-07 — Pérdida de conexión durante el registro

Si se pierde la conexión:

* El sistema deberá informar al usuario.
* Los datos que todavía no hayan sido enviados al servidor no deben considerarse una cuenta creada.
* Cuando sea técnicamente posible, el formulario podrá conservar temporalmente la información para evitar que el usuario tenga que ingresarla nuevamente.

### CE-REG-08 — Abandono del registro

Si el usuario abandona el formulario antes de enviarlo:

* No se crea una cuenta.
* La información podrá conservarse temporalmente en el navegador para facilitar la continuación del proceso.

Si el formulario ya fue enviado correctamente:

* La cuenta sí existe.
* Su estado permanecerá como **Pendiente de verificación** hasta completar la verificación.

### CE-REG-09 — Error del servidor

Si ocurre un error durante el registro:

* El sistema mostrará un mensaje comprensible.
* No deberá crear una cuenta incompleta.
* El error podrá registrarse para fines técnicos y de auditoría.
* Cuando sea posible, el usuario podrá volver a intentar la operación sin perder la información ingresada.

### CE-REG-10 — Error en el envío del correo

Si ROSLYNDER no puede enviar el correo de verificación:

* El error deberá registrarse.
* El sistema podrá realizar nuevos intentos de envío.
* El usuario podrá solicitar posteriormente un nuevo enlace.

### CE-REG-11 — Intentos repetidos

Las solicitudes repetidas de registro, verificación o reenvío de enlaces podrán estar sujetas a límites de frecuencia y controles de seguridad.

---

## 7. Estados relacionados con el registro

El registro utiliza inicialmente los siguientes estados de cuenta:

| Estado                    | Descripción                                                         |
| ------------------------- | ------------------------------------------------------------------- |
| Pendiente de verificación | La cuenta fue creada, pero el correo todavía no ha sido verificado. |
| Activa                    | El correo fue verificado y la cuenta puede utilizarse.              |

Los estados relacionados con bloqueos de seguridad o suspensiones administrativas se gestionarán en las reglas correspondientes de **Inicio de sesión**, **Gestión de sesiones** y **Administración de usuarios**.

---

## 8. Relación con otros módulos

El registro de usuario se relaciona posteriormente con:

* **Verificación de correo:** confirma el acceso al correo registrado.
* **Inicio de sesión:** permite acceder a la cuenta una vez activa.
* **Recuperación de contraseña:** permite recuperar el acceso mediante el correo registrado.
* **Gestión de sesiones:** controla las sesiones activas.
* **Roles y permisos:** determina las funciones disponibles para el usuario.
* **Perfil comercial:** permite habilitar las funciones de vendedor.
* **Publicaciones:** permite al vendedor gestionar sus productos.
* **Administración:** permite la supervisión de usuarios y vendedores.

---

## 9. Pendientes

Los siguientes puntos no son necesarios para cerrar el funcionamiento básico del registro y podrán definirse posteriormente:

* Política de eliminación de cuentas que permanezcan indefinidamente en estado **Pendiente de verificación**.
* Cantidad máxima exacta de reenvíos del correo de verificación.
* Tiempo mínimo entre solicitudes de reenvío.
* Reglas detalladas de auditoría y conservación de eventos de seguridad.
* Política específica para datos almacenados temporalmente durante un registro abandonado.
* Reglas adicionales relacionadas con la fecha de nacimiento, si posteriormente fueran necesarias.
