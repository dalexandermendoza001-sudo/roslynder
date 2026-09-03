# Inicio de sesión

## 1. Objetivo

Permitir que un usuario con una cuenta en estado **Activa** se autentique mediante sus credenciales y acceda a las funcionalidades correspondientes a los roles que tenga asignados.

El inicio de sesión no modifica los roles del usuario ni el estado de su cuenta.

---

## 2. Datos involucrados

### 2.1 Datos proporcionados por el usuario

* Nombre de usuario o correo electrónico
* Contraseña

### 2.2 Datos gestionados por el sistema

* Identificador de la cuenta
* Estado actual de la cuenta
* Fecha y hora del intento de autenticación
* Resultado del intento de autenticación
* Cantidad de intentos fallidos
* Información necesaria para aplicar un bloqueo de seguridad
* Información relacionada con la sesión
* Información de auditoría y seguridad

---

## 3. Validaciones

### 3.1 Identificador

* Es obligatorio.
* Puede corresponder al nombre de usuario o al correo electrónico registrado.
* Debe estar asociado a una cuenta existente.

### 3.2 Contraseña

* Es obligatoria.
* Debe coincidir con la credencial registrada para la cuenta.
* No debe mostrarse durante su ingreso.
* Su validación debe realizarse en el servidor.

### 3.3 Estado de la cuenta

Antes de validar la contraseña, el sistema debe comprobar el estado actual de la cuenta.

| Estado                    | Inicio de sesión |
| ------------------------- | ---------------- |
| Pendiente de verificación | No permitido     |
| Activa                    | Permitido        |
| Bloqueada                 | No permitido     |
| Suspendida                | No permitido     |

### 3.4 Intentos fallidos

* Cada autenticación incorrecta deberá registrarse.
* Los intentos fallidos deberán acumularse según las reglas de seguridad establecidas.
* Al alcanzar el límite definido, la cuenta podrá pasar a estado **Bloqueada**.

---

## 4. Reglas de negocio

### RN-LOGIN-01 — Cuenta habilitada para iniciar sesión

Solo una cuenta en estado **Activa** puede iniciar sesión normalmente.

### RN-LOGIN-02 — Identificación del usuario

El usuario podrá identificarse mediante su **nombre de usuario o correo electrónico**, junto con su contraseña.

### RN-LOGIN-03 — Cuenta no encontrada

Si no existe una cuenta asociada al identificador proporcionado, el sistema rechazará la autenticación sin revelar información innecesaria sobre la existencia de cuentas.

### RN-LOGIN-04 — Cuenta pendiente de verificación

Una cuenta en estado **Pendiente de verificación** no podrá iniciar sesión.

El sistema deberá informar al usuario que debe verificar su correo electrónico.

### RN-LOGIN-05 — Cuenta bloqueada

Una cuenta en estado **Bloqueada** no podrá iniciar sesión normalmente.

El usuario podrá iniciar el proceso de desbloqueo establecido por el sistema.

### RN-LOGIN-06 — Cuenta suspendida

Una cuenta en estado **Suspendida** no podrá iniciar sesión ni utilizar funcionalidades autenticadas.

La reactivación dependerá de una acción autorizada del administrador.

### RN-LOGIN-07 — Validación de contraseña

La contraseña será validada únicamente después de comprobar que la cuenta existe y se encuentra en un estado que permita la autenticación.

### RN-LOGIN-08 — Credenciales incorrectas

Cuando la contraseña sea incorrecta, el sistema rechazará la autenticación y registrará el intento fallido.

### RN-LOGIN-09 — Control de intentos fallidos

El sistema acumulará los intentos fallidos de autenticación y aplicará el bloqueo de la cuenta cuando se alcance el límite definido.

### RN-LOGIN-10 — Autenticación exitosa

Cuando las credenciales sean correctas y la cuenta esté activa, el sistema permitirá el acceso y creará una **sesión autenticada**.

### RN-LOGIN-11 — Roles y funcionalidades

Después de una autenticación exitosa, las funcionalidades disponibles dependerán de los **roles y permisos asignados al usuario**, no únicamente del estado de la cuenta.

### RN-LOGIN-12 — Múltiples roles

Un mismo usuario podrá tener simultáneamente más de un rol.

Por ejemplo:

**COMPRADOR + VENDEDOR**

### RN-LOGIN-13 — Estado y rol son independientes

Cambiar el estado de una cuenta no elimina ni modifica sus roles.

Mientras la cuenta se encuentre restringida, el usuario no podrá utilizar las funcionalidades autenticadas correspondientes a sus roles.

### RN-LOGIN-14 — Sesión independiente del estado

Cerrar o finalizar una sesión no modifica el estado de la cuenta ni sus roles.

### RN-LOGIN-15 — Restablecimiento del contador

Después de una autenticación exitosa, el contador de intentos fallidos deberá reiniciarse.

### RN-LOGIN-16 — Validación del lado del servidor

Las validaciones realizadas en el navegador tienen como finalidad mejorar la experiencia del usuario.

La autenticación, comprobación de credenciales y validaciones de seguridad deberán realizarse en el servidor.

---

## 5. Flujo principal

```text
Usuario selecciona "Iniciar sesión"
            ↓
Sistema muestra formulario
            ↓
Usuario ingresa identificador + contraseña
            ↓
Validar campos obligatorios
            ↓
¿Datos completos?
     ┌──────┴──────┐
    NO             SÍ
     ↓              ↓
Mostrar error    Buscar cuenta
                     ↓
              ¿Cuenta encontrada?
                ┌────┴────┐
               NO         SÍ
                ↓          ↓
        Rechazar acceso  Revisar estado
                              ↓
                         ¿Estado Activa?
                         ┌────┴─────┐
                        NO          SÍ
                         ↓           ↓
                  Denegar acceso  Validar contraseña
                                      ↓
                              ¿Contraseña correcta?
                               ┌─────┴─────┐
                              NO           SÍ
                               ↓            ↓
                       Registrar intento   Reiniciar contador
                       fallido             ↓
                               ↓        Crear sesión
                       ¿Alcanzó límite?    ↓
                         ┌────┴────┐    Consultar roles
                        NO         SÍ       ↓
                        ↓          ↓    Permitir funciones
                   Rechazar    Bloquear    según roles/permisos
                   acceso       cuenta
```

---

## 6. Comportamiento según el estado de la cuenta

### 6.1 Pendiente de verificación

```text
Inicio de sesión
      ↓
Cuenta encontrada
      ↓
Estado: Pendiente de verificación
      ↓
Acceso denegado
      ↓
Informar que debe verificar el correo
```

Los intentos realizados mientras la cuenta se encuentra pendiente de verificación no deberán considerarse intentos de contraseña incorrecta.

### 6.2 Bloqueada

```text
Inicio de sesión
      ↓
Cuenta encontrada
      ↓
Estado: Bloqueada
      ↓
Acceso denegado
      ↓
Ofrecer proceso de desbloqueo
```

El proceso específico de desbloqueo será definido en un módulo independiente.

### 6.3 Suspendida

```text
Inicio de sesión
      ↓
Cuenta encontrada
      ↓
Estado: Suspendida
      ↓
Acceso denegado
      ↓
Informar que la cuenta se encuentra suspendida
```

La reactivación dependerá de una acción administrativa autorizada.

### 6.4 Activa

```text
Inicio de sesión
      ↓
Cuenta encontrada
      ↓
Estado: Activa
      ↓
Validar contraseña
      ↓
Credenciales correctas
      ↓
Crear sesión
      ↓
Consultar roles y permisos
      ↓
Permitir funcionalidades correspondientes
```

---

## 7. Casos especiales

### 7.1 Cuenta inexistente

El identificador no corresponde a una cuenta registrada.

**Resultado:** autenticación rechazada mediante un mensaje que no revele información innecesaria.

### 7.2 Contraseña incorrecta

La cuenta existe y está activa, pero la contraseña no coincide.

**Resultado:**

* Rechazar autenticación.
* Registrar intento fallido.
* Incrementar contador.
* Comprobar si se alcanzó el límite de seguridad.

### 7.3 Cuenta pendiente de verificación

**Resultado:**

* No crear sesión.
* No considerar el intento como contraseña incorrecta.
* Informar que debe verificarse el correo.

### 7.4 Cuenta bloqueada

**Resultado:**

* No crear sesión.
* No permitir autenticación normal.
* Permitir iniciar el proceso de desbloqueo.

### 7.5 Cuenta suspendida

**Resultado:**

* No crear sesión.
* No permitir acceso a funcionalidades autenticadas.
* Informar la restricción correspondiente.

### 7.6 Se alcanza el límite de intentos

**Resultado:**

* Cambiar la cuenta al estado **Bloqueada**.
* Rechazar el acceso.
* Permitir posteriormente el proceso de desbloqueo.

### 7.7 Problemas de conexión

Si la comunicación con el servidor no puede completarse, el sistema deberá informar que no fue posible procesar el inicio de sesión.

No deberá crear una sesión localmente como si la autenticación hubiera sido exitosa.

### 7.8 Error del servidor

Si ocurre un error interno durante la autenticación, el sistema deberá mostrar un mensaje general y no exponer información técnica o sensible.

---

## 8. Relación con otros procesos

```text
REGISTRO
   ↓
Pendiente de verificación
   ↓
VERIFICACIÓN DE CORREO
   ↓
Activa
   ↓
INICIO DE SESIÓN
   ↓
SESIÓN
   ↓
ROLES Y PERMISOS
   ↓
FUNCIONALIDADES
```

El inicio de sesión utiliza información generada previamente durante el registro y la verificación de correo.

También se relaciona con:

* Estado de cuenta
* Roles
* Permisos
* Sesiones
* Seguridad y control de intentos
* Desbloqueo de cuenta

---

## 9. Separación de responsabilidades

Para mantener el modelo del sistema correctamente definido:

### Estado de cuenta

Determina si la cuenta puede acceder al sistema.

```text
Pendiente de verificación
Activa
Bloqueada
Suspendida
```

### Roles

Determinan qué tipo de funciones puede tener el usuario.

```text
COMPRADOR
VENDEDOR
ADMINISTRADOR
```

### Sesión

Representa el acceso autenticado actual del usuario.

```text
Sesión creada
Sesión activa
Sesión cerrada/expirada
```

Por lo tanto:

```text
USUARIO
 ├── ESTADO DE CUENTA
 │      └── Activa
 │
 ├── ROLES
 │      ├── Comprador
 │      └── Vendedor
 │
 └── SESIONES
        └── Sesión autenticada
```

---

## 10. Pendientes

Los siguientes aspectos se definirán posteriormente:

* Número máximo de intentos fallidos.
* Condiciones exactas para aplicar el bloqueo.
* Duración del bloqueo, si corresponde.
* Proceso completo de desbloqueo.
* Vigencia y límites del código/enlace de desbloqueo.
* Límites de solicitudes de desbloqueo.
* Reglas detalladas de administración de sesiones.
* Registro y auditoría de eventos de seguridad.

Estos elementos no deben definirse arbitrariamente dentro del inicio de sesión; serán tratados en sus respectivos módulos.

---

## 11. Resultado esperado

Al finalizar correctamente el inicio de sesión:

1. La cuenta debe encontrarse en estado **Activa**.
2. Las credenciales deben haber sido validadas correctamente.
3. El intento fallido acumulado debe reiniciarse.
4. Debe crearse una sesión autenticada.
5. El sistema debe identificar los roles y permisos del usuario.
6. El usuario debe acceder únicamente a las funcionalidades permitidas.

El inicio de sesión **no crea roles, no modifica el estado de la cuenta y no convierte al usuario en vendedor**.
