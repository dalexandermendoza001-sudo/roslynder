# 02.2 — Modificar información personal

## Objetivo

Permitir que un usuario autenticado modifique la información personal asociada a su propia cuenta, aplicando las validaciones y controles correspondientes según el tipo de dato.

## Información modificable

La información personal se divide según las condiciones de modificación:

### Datos de modificación directa

El usuario podrá modificar:

* Nombres
* Apellidos
* Fecha de nacimiento
* Dirección
* Provincia
* Ciudad
* Teléfono, de acuerdo con las reglas de validación y verificación establecidas por ROSLYNDER.

### Datos que requieren verificación

El cambio de correo electrónico requiere verificar la nueva dirección antes de considerarla como correo electrónico verificado de la cuenta.

### Datos restringidos

El nombre de usuario no podrá modificarse normalmente después de su registro.

La contraseña no forma parte de esta función. Su modificación corresponde a una función independiente de seguridad.

## Reglas funcionales

### RF-USU-08 — Modificación de información propia

El usuario autenticado puede modificar únicamente la información personal asociada a su propia cuenta.

### RF-USU-09 — Campos modificables

El sistema debe permitir modificar únicamente los campos habilitados para modificación.

### RF-USU-10 — Validación de información

Los nuevos valores deben cumplir las validaciones establecidas para cada tipo de dato antes de ser almacenados.

### RF-USU-11 — Modificación de nombres y apellidos

El usuario puede solicitar modificaciones de sus nombres y apellidos.

Los cambios realizados deben conservar información suficiente para permitir su trazabilidad.

### RF-USU-12 — Independencia de información comercial

Modificar nombres o apellidos no modifica automáticamente:

* Nombre comercial.
* Perfil comercial.
* Publicaciones.
* Productos.
* Menús.
* Otra información asociada a las actividades comerciales del usuario.

### RF-USU-13 — Provincia y ciudad

La provincia y ciudad seleccionadas deben corresponder entre sí según las opciones válidas disponibles en ROSLYNDER.

### RF-USU-14 — Dirección

La dirección ingresada debe cumplir las validaciones establecidas por ROSLYNDER.

### RF-USU-15 — Teléfono

El número telefónico debe cumplir el formato establecido por ROSLYNDER.

### RF-USU-16 — Cambio de correo electrónico

El cambio de correo electrónico requiere la verificación de la nueva dirección.

### RF-USU-17 — Correo electrónico anterior

Mientras la nueva dirección de correo electrónico no haya sido verificada, el correo electrónico actualmente verificado continuará siendo válido para la cuenta.

### RF-USU-18 — Correo electrónico único

El nuevo correo electrónico no puede estar asociado a otra cuenta registrada en ROSLYNDER.

### RF-USU-19 — Trazabilidad del cambio de correo

Los cambios de correo electrónico deben conservar información suficiente para fines de seguridad y trazabilidad.

### RF-USU-20 — Nombre de usuario

El nombre de usuario no podrá modificarse mediante la función normal de modificación de información personal.

### RF-USU-21 — Contraseña independiente

La modificación de la contraseña se realizará mediante una función independiente de seguridad.

### RF-USU-22 — Conservación de historial

Cuando corresponda conservar información histórica de un cambio, esta no reemplaza los datos actuales de la cuenta.

El sistema debe mantener separados los datos vigentes de los registros necesarios para trazabilidad.

### RF-USU-23 — Independencia de roles

Modificar información personal no modifica los roles asignados al usuario.

### RF-USU-24 — Independencia del estado de cuenta

Modificar información personal no modifica el estado actual de la cuenta.

## Flujo principal

```text
Usuario inicia sesión
        ↓
Accede a su perfil
        ↓
Selecciona "Modificar información personal"
        ↓
Sistema valida la sesión
        ↓
Usuario modifica los campos permitidos
        ↓
Sistema valida la información
        ↓
¿Existe algún dato que requiere verificación?
        │
       ├── No → Guardar cambios
       │
       └── Sí → Iniciar proceso de verificación
                    ↓
             Confirmar nueva información
                    ↓
               Guardar cambio
                    ↓
              Registrar trazabilidad
```

## Cambio de correo electrónico

```text
Usuario solicita cambiar correo
        ↓
Ingresa nueva dirección
        ↓
Sistema verifica que no pertenezca a otra cuenta
        ↓
Sistema registra la solicitud
        ↓
Envía verificación a la nueva dirección
        ↓
Usuario verifica el nuevo correo
        ↓
Sistema confirma el cambio
        ↓
Registra la modificación
```

## Casos especiales

* Sesión no válida → solicitar nuevo inicio de sesión.
* Información inválida → informar el campo que debe corregirse.
* Provincia y ciudad incompatibles → impedir el almacenamiento.
* Correo electrónico ya registrado → impedir el cambio.
* Nueva dirección de correo no verificada → mantener el correo actualmente verificado.
* Error durante la actualización → no aplicar cambios incompletos.
* Usuario intenta modificar un campo restringido → impedir la modificación.
* Usuario intenta modificar información de otra cuenta → denegar el acceso.

## Consideraciones

La información personal representa los datos del usuario como persona dentro de ROSLYNDER.

La información comercial se administra independientemente mediante **04 — Perfil comercial**.

Los cambios personales tampoco deben modificar automáticamente publicaciones, productos, menús u otros elementos comerciales asociados al usuario.

El almacenamiento de historial y trazabilidad se definirá posteriormente durante el **análisis de datos y diseño de la base de datos**.
