# Registro de usuario

## 1. Objetivo

Permitir que una persona cree una cuenta en ROSLYNDER proporcionando sus datos personales y credenciales de acceso.

La cuenta creada no queda activa inmediatamente. Después de completar correctamente el registro, queda en estado **Pendiente de verificación** hasta que el usuario confirme su correo electrónico.

---

## 2. Datos del registro

### 2.1 Datos personales

* Nombres
* Apellidos
* Provincia
* Ciudad
* Fecha de nacimiento *(opcional)*
* Teléfono
* Dirección

### 2.2 Datos de acceso

* Nombre de usuario
* Correo electrónico
* Contraseña
* Confirmación de contraseña

### 2.3 Aceptación

* Aceptación de términos y condiciones.

---

## 3. Validaciones

### 3.1 Nombres y apellidos

* Son obligatorios.
* No pueden estar vacíos.

### 3.2 Nombre de usuario

* Es obligatorio.
* Debe contener entre **6 y 16 caracteres**.
* Solo permite letras y números.
* No permite espacios, `_`, `.`, ni otros caracteres especiales.
* Debe ser único.
* No puede coincidir con otro nombre de usuario registrado.
* Una vez creado, no podrá modificarse.

### 3.3 Correo electrónico

* Es obligatorio.
* Debe tener un formato válido.
* Debe ser único.
* Se utilizará para el proceso de verificación de la cuenta.
* El sistema no debe asumir que el correo existe únicamente porque tiene un formato válido; su confirmación se realiza mediante el enlace enviado al correo.

### 3.4 Contraseña

Debe:

* Tener entre **6 y 16 caracteres**.
* Contener al menos una letra mayúscula.
* Contener al menos una letra minúscula.
* Contener al menos un número.
* Contener al menos un carácter especial.

### 3.5 Confirmación de contraseña

* Es obligatoria.
* Debe coincidir exactamente con la contraseña.

### 3.6 Provincia y ciudad

* La provincia es obligatoria.
* La ciudad es obligatoria.
* Las ciudades disponibles deben corresponder a la provincia seleccionada.

### 3.7 Fecha de nacimiento

* Es opcional.
* Si se proporciona, debe corresponder a una fecha válida.
* No se establece actualmente una edad mínima para el registro.

### 3.8 Teléfono

Para la primera versión de ROSLYNDER:

* Es obligatorio.
* Debe contener exactamente **9 dígitos numéricos**, de acuerdo con el formato utilizado para teléfonos móviles en Ecuador.

### 3.9 Dirección

* Es obligatoria.
* No puede estar vacía.

### 3.10 Términos y condiciones

* El usuario debe aceptar los términos y condiciones para completar el registro.

---

## 4. Reglas de negocio

### RN-REG-01 — Datos obligatorios

El sistema debe impedir el registro cuando falte alguno de los datos obligatorios.

### RN-REG-02 — Usuario único

Cada cuenta debe tener un nombre de usuario único.

### RN-REG-03 — Correo único

Cada cuenta debe tener un correo electrónico único.

### RN-REG-04 — Nombre de usuario permanente

El nombre de usuario asignado durante el registro no podrá modificarse posteriormente.

### RN-REG-05 — Validación de credenciales

La contraseña y su confirmación deben cumplir las reglas establecidas antes de crear la cuenta.

### RN-REG-06 — Estado inicial

Una cuenta creada correctamente debe iniciar en estado:

**Pendiente de verificación**

### RN-REG-07 — Verificación obligatoria

El usuario debe verificar su correo electrónico antes de poder iniciar sesión.

### RN-REG-08 — Enlace de verificación

Después de completar correctamente el registro, el sistema debe generar y enviar un enlace único de verificación al correo registrado.

### RN-REG-09 — Vigencia del enlace

El enlace de verificación tendrá una vigencia de **24 horas**.

### RN-REG-10 — Uso único

Un enlace de verificación solamente podrá utilizarse una vez.

### RN-REG-11 — Nuevos enlaces

Cuando se genere un nuevo enlace de verificación, el enlace anterior deberá quedar invalidado.

### RN-REG-12 — Activación

Una verificación válida debe cambiar el estado de la cuenta:

**Pendiente de verificación → Activa**

### RN-REG-13 — Cuenta pendiente

Una cuenta pendiente de verificación no podrá iniciar sesión ni utilizar funcionalidades privadas.

### RN-REG-14 — Función de comprador

Una vez que la cuenta se encuentre **Activa**, el usuario podrá utilizar las funcionalidades correspondientes al rol de **COMPRADOR**.

### RN-REG-15 — Conversión a vendedor

Un usuario activo podrá solicitar utilizar las funciones de vendedor mediante la opción **«Quiero vender»**.

Para ello deberá completar la información comercial requerida y cumplir las validaciones correspondientes.

### RN-REG-16 — Múltiples roles

Un mismo usuario puede tener simultáneamente los roles:

* COMPRADOR
* VENDEDOR

El rol de ADMINISTRADOR no se obtiene mediante el registro público.

### RN-REG-17 — Separación de estado y rol

El estado de la cuenta no determina los roles del usuario.

Por ejemplo, un usuario puede ser:

**ACTIVA + COMPRADOR + VENDEDOR**

### RN-REG-18 — Seguridad

Los datos relacionados con intentos de acceso, IP, fecha/hora, navegador, dispositivo y otros eventos de seguridad deben tratarse como información de control y auditoría, separada conceptualmente de los datos básicos del usuario.

---

## 5. Flujo principal

```text
Usuario accede al registro
        ↓
Completa los datos
        ↓
Sistema valida la información
        ↓
¿Datos correctos?
   ├── NO → Mostrar errores
   │
   └── SÍ
         ↓
Crear cuenta
         ↓
Estado = PENDIENTE DE VERIFICACIÓN
         ↓
Generar enlace de verificación
         ↓
Enviar enlace al correo
         ↓
Esperar verificación
```

---

## 6. Flujo de verificación

```text
PENDIENTE DE VERIFICACIÓN
          ↓
Usuario recibe enlace
          ↓
Accede al enlace
          ↓
Sistema valida token
          ↓
¿Token válido?
   ├── NO → Mostrar motivo
   │
   └── SÍ
         ↓
Estado = ACTIVA
         ↓
Usuario puede iniciar sesión
```

---

## 7. Flujo de usuario hacia vendedor

```text
CUENTA ACTIVA
      ↓
Usuario utiliza funciones de COMPRADOR
      ↓
Selecciona "Quiero vender"
      ↓
Completa información comercial
      ↓
Sistema valida información
      ↓
Funciones de VENDEDOR habilitadas
```

El cambio a vendedor no crea una nueva cuenta ni modifica el estado de la cuenta.

---

## 8. Casos especiales

### CE-REG-01 — Correo ya registrado

El sistema debe informar que el correo electrónico ya está asociado a una cuenta.

### CE-REG-02 — Nombre de usuario ya registrado

El sistema debe solicitar un nombre de usuario diferente.

### CE-REG-03 — Correo no recibido

El usuario podrá solicitar el reenvío del enlace de verificación, sujeto a los límites de seguridad establecidos.

### CE-REG-04 — Enlace vencido

El enlace vencido no podrá utilizarse.

La cuenta permanecerá en estado **Pendiente de verificación** y el usuario podrá solicitar un nuevo enlace.

### CE-REG-05 — Enlace utilizado

Un enlace utilizado no podrá volver a utilizarse.

### CE-REG-06 — Registro abandonado

La información temporal de un registro incompleto podrá conservarse durante el tiempo definido por la política de retención del sistema.

### CE-REG-07 — Pérdida de conexión

El sistema debe informar al usuario que no fue posible completar la operación y evitar crear registros incompletos.

### CE-REG-08 — Error del servidor

El sistema debe informar que la operación no pudo completarse y evitar duplicar la cuenta.

### CE-REG-09 — Error de envío de correo

Si la cuenta fue creada pero el correo no pudo enviarse, el sistema debe permitir posteriormente solicitar un nuevo enlace de verificación.

### CE-REG-10 — Intentos repetidos

Las solicitudes de registro, verificación y reenvío de enlaces podrán estar sujetas a límites de seguridad.

### CE-REG-11 — Registro administrativo

El rol ADMINISTRADOR no se obtiene mediante el registro público.

---

## 9. Estados relacionados con el registro

El registro participa principalmente en la creación del estado inicial:

```text
REGISTRO
   ↓
PENDIENTE DE VERIFICACIÓN
```

La transición posterior a **ACTIVA** corresponde al proceso de verificación del correo.

Los estados **BLOQUEADA** y **SUSPENDIDA** corresponden a procesos posteriores de seguridad y administración.

---

## 10. Relaciones con otros módulos

```text
REGISTRO
   ↓
VERIFICACIÓN DE CORREO
   ↓
INICIO DE SESIÓN
   ↓
GESTIÓN DE SESIONES
   ↓
ROLES Y PERMISOS
   ↓
PERFIL COMERCIAL
```

---

## 11. Pendientes

Quedan por definir en fases posteriores:

* Límite de solicitudes de registro.
* Límite de reenvíos del correo de verificación.
* Política de eliminación de cuentas que permanezcan pendientes durante un periodo prolongado.
* Política de retención de información temporal del registro.
* Políticas específicas de seguridad y auditoría.
* Reglas completas para la activación de funciones de vendedor.
