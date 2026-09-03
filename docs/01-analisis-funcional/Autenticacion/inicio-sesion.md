# Inicio de sesión

## 1. Objetivo

Permitir que un usuario con una cuenta activa se autentique mediante sus credenciales y acceda de forma segura a las funciones disponibles según su estado y roles asignados.

El inicio de sesión deberá comprobar las credenciales y el estado de la cuenta antes de crear una sesión.

---

## 2. Datos

### 2.1 Datos proporcionados por el usuario

| Dato                                   | Obligatorio | Descripción                                       |
| -------------------------------------- | ----------- | ------------------------------------------------- |
| Nombre de usuario o correo electrónico | Sí          | Identificador utilizado para localizar la cuenta. |
| Contraseña                             | Sí          | Credencial utilizada para autenticar al usuario.  |

El usuario podrá utilizar indistintamente su **nombre de usuario** o **correo electrónico** para iniciar sesión.

### 2.2 Información generada por el sistema

Durante el proceso de autenticación, ROSLYNDER podrá registrar información relacionada con la seguridad:

* Fecha y hora del intento.
* Resultado del intento.
* Cantidad de intentos fallidos.
* Dirección IP.
* Información del navegador o dispositivo, cuando corresponda.
* Fecha y hora del bloqueo, cuando se produzca.

Esta información corresponde a seguridad y auditoría y no forma parte de los datos que el usuario debe introducir en el formulario.

---

## 3. Validaciones

### 3.1 Usuario o correo electrónico

* Es obligatorio.
* Puede corresponder al nombre de usuario o al correo electrónico registrado.
* Debe corresponder a una cuenta registrada.

### 3.2 Contraseña

* Es obligatoria.
* Debe coincidir con la credencial almacenada de forma segura.
* La contraseña no debe almacenarse en texto plano.

### 3.3 Cuenta inexistente

Si no existe una cuenta correspondiente al identificador proporcionado, el acceso será rechazado.

El sistema no deberá revelar información innecesaria que permita determinar si una cuenta específica está registrada.

### 3.4 Contraseña incorrecta

Si la contraseña no coincide:

* El acceso será rechazado.
* El intento podrá registrarse para fines de seguridad.
* Se incrementará el contador de intentos fallidos.

### 3.5 Límite de intentos fallidos

Cuando el número de intentos fallidos alcance el límite definido por ROSLYNDER:

* La cuenta será bloqueada temporalmente.
* El inicio de sesión normal quedará restringido.
* Se aplicará el mecanismo de desbloqueo establecido.

El número exacto de intentos permitidos queda pendiente de definición.

### 3.6 Cuenta pendiente de verificación

Si las credenciales son correctas pero la cuenta se encuentra en estado:

**Pendiente de verificación**

el acceso será rechazado.

El sistema informará al usuario que debe completar la verificación de su correo electrónico.

### 3.7 Cuenta bloqueada

Si la cuenta se encuentra bloqueada debido a intentos fallidos:

* No podrá acceder mediante el inicio de sesión normal.
* Deberá utilizar el mecanismo de desbloqueo establecido por ROSLYNDER.

### 3.8 Cuenta suspendida

Si la cuenta fue suspendida administrativamente:

* El acceso será rechazado.
* El usuario no podrá iniciar sesión mientras la suspensión permanezca activa.

El bloqueo de seguridad y la suspensión administrativa son situaciones diferentes.

### 3.9 Cuenta activa

El acceso podrá completarse cuando:

* Las credenciales sean correctas.
* La cuenta se encuentre activa.
* No exista una restricción de acceso.

En este caso, el sistema creará una sesión y permitirá el acceso al dashboard correspondiente.

### 3.10 Validación frontend y backend

Las validaciones realizadas mediante HTML y JavaScript sirven principalmente para proporcionar una respuesta inmediata al usuario.

La validación definitiva de:

* credenciales;
* estado de cuenta;
* bloqueo;
* suspensión;
* permisos;
* creación de sesión;

deberá realizarse en el servidor.

---

## 4. Reglas de negocio

### RN-LOGIN-01 — Identificación de usuario

El usuario podrá iniciar sesión utilizando su nombre de usuario o correo electrónico.

### RN-LOGIN-02 — Contraseña obligatoria

La contraseña será necesaria para completar la autenticación.

### RN-LOGIN-03 — Cuenta activa

Solo una cuenta en estado **Activa** podrá completar correctamente el inicio de sesión.

### RN-LOGIN-04 — Cuenta pendiente

Una cuenta en estado **Pendiente de verificación** no podrá iniciar sesión.

### RN-LOGIN-05 — Cuenta suspendida

Una cuenta suspendida administrativamente no podrá acceder mientras permanezca suspendida.

### RN-LOGIN-06 — Registro de intentos

Cada intento de autenticación incorrecto podrá registrarse para fines de seguridad y auditoría.

### RN-LOGIN-07 — Contador de intentos

Los intentos fallidos se contabilizarán para determinar si corresponde aplicar un bloqueo de seguridad.

### RN-LOGIN-08 — Bloqueo por intentos fallidos

Al alcanzar el límite de intentos fallidos definido por ROSLYNDER, la cuenta será bloqueada temporalmente.

### RN-LOGIN-09 — Restricción de cuenta bloqueada

Una cuenta bloqueada no podrá acceder mediante el proceso normal de inicio de sesión.

### RN-LOGIN-10 — Desbloqueo

El usuario podrá utilizar el mecanismo de desbloqueo establecido por ROSLYNDER para recuperar el acceso.

### RN-LOGIN-11 — Código de desbloqueo

El mecanismo de desbloqueo utilizará un código temporal de un solo uso.

### RN-LOGIN-12 — Vigencia del código

El código de desbloqueo tendrá una vigencia máxima de **24 horas**.

### RN-LOGIN-13 — Invalidación de códigos anteriores

La generación de un nuevo código de desbloqueo invalidará cualquier código anterior asociado al mismo proceso.

### RN-LOGIN-14 — Código de un solo uso

Un código utilizado correctamente no podrá volver a utilizarse.

### RN-LOGIN-15 — Restablecimiento de intentos

Una autenticación exitosa restablecerá el contador de intentos fallidos de la cuenta.

### RN-LOGIN-16 — Creación de sesión

Después de una autenticación exitosa, el sistema creará una sesión para el usuario.

### RN-LOGIN-17 — Acceso según roles

Las funciones disponibles después del inicio de sesión dependerán de los roles y permisos asignados al usuario.

### RN-LOGIN-18 — Una cuenta para comprador y vendedor

Un usuario que tenga funciones de comprador y vendedor utilizará la misma cuenta para ambas funciones.

No será necesario crear una segunda cuenta.

### RN-LOGIN-19 — Funciones administrativas

Las funciones administrativas estarán disponibles únicamente para usuarios que posean el rol correspondiente.

### RN-LOGIN-20 — Validación en servidor

Las decisiones de autenticación y seguridad deberán ser realizadas por el servidor y no depender exclusivamente de las validaciones del navegador.

---

## 5. Flujo principal

```text
USUARIO
   │
   ▼
Selecciona "Iniciar sesión"
   │
   ▼
Ingresa usuario/correo + contraseña
   │
   ▼
Validación del formulario
   │
   ├── Campos incompletos
   │        │
   │        ▼
   │   Mostrar errores
   │        │
   │        └────► Corregir datos
   │
   └── Datos completos
            │
            ▼
     Validación en servidor
            │
            ▼
       ¿Credenciales
          correctas?
        │           │
       NO           SÍ
        │           │
        ▼           ▼
Registrar       Verificar
intento         estado de cuenta
        │           │
        │      ┌────┴──────────┐
        │      │               │
        │   No activa         Activa
        │      │               │
        │      ▼               ▼
        │  Rechazar        Crear sesión
        │                      │
        ▼                      ▼
¿Alcanzó límite?           Dashboard
   │       │                   │
  NO      SÍ                   ▼
   │       │              Identificar
   │       ▼              roles/permisos
   │    Bloquear
   │       │
   ▼       ▼
Reintentar / desbloqueo
```

---

## 6. Flujo de autenticación correcta

Cuando las credenciales sean correctas:

```text
Credenciales correctas
        │
        ▼
Comprobar estado de cuenta
        │
        ├── Pendiente de verificación
        │       └──► Acceso denegado
        │
        ├── Bloqueada
        │       └──► Acceso denegado
        │
        ├── Suspendida
        │       └──► Acceso denegado
        │
        └── Activa
                │
                ▼
          Crear sesión
                │
                ▼
            Dashboard
                │
                ▼
       Identificar roles
          y permisos
```

---

## 7. Flujo de credenciales incorrectas

```text
Credenciales incorrectas
          │
          ▼
    Registrar intento
          │
          ▼
Incrementar contador
          │
          ▼
   ¿Alcanzó el límite?
       │          │
      NO          SÍ
       │          │
       ▼          ▼
Nuevo intento   Bloquear
                cuenta
                   │
                   ▼
             Desbloqueo
```

Mientras no se alcance el límite, el usuario podrá volver a intentar iniciar sesión.

Cuando se alcance el límite, el inicio de sesión normal quedará restringido.

---

## 8. Acceso al dashboard

Después de una autenticación correcta:

1. Se crea la sesión.
2. Se identifica el usuario.
3. Se consultan sus roles y permisos.
4. Se determina qué funciones puede utilizar.
5. Se permite el acceso al dashboard correspondiente.

Ejemplo:

```text
USUARIO
   │
   ▼
USUARIO_ROL
   │
   ├────► COMPRADOR
   │
   └────► VENDEDOR
```

Un usuario que posteriormente obtenga el rol de vendedor seguirá utilizando la misma cuenta.

---

## 9. Casos especiales

### CE-LOGIN-01 — Contraseña incorrecta

El sistema:

* Rechazará el acceso.
* Registrará el intento para fines de seguridad.
* Incrementará el contador de intentos fallidos.
* Comprobará si corresponde bloquear la cuenta.

### CE-LOGIN-02 — Usuario o correo no encontrado

El sistema rechazará el acceso.

El mensaje mostrado no deberá revelar información innecesaria sobre la existencia de la cuenta.

### CE-LOGIN-03 — Cuenta pendiente de verificación

El sistema rechazará el acceso e informará que el usuario debe verificar su correo electrónico.

### CE-LOGIN-04 — Cuenta bloqueada

El sistema rechazará el inicio de sesión normal y permitirá utilizar el mecanismo de desbloqueo correspondiente.

### CE-LOGIN-05 — Cuenta suspendida

El sistema rechazará el acceso mientras la suspensión administrativa permanezca activa.

### CE-LOGIN-06 — Límite de intentos alcanzado

La cuenta será bloqueada temporalmente y el usuario deberá utilizar el mecanismo de desbloqueo establecido.

### CE-LOGIN-07 — Código de desbloqueo vencido

Si el código supera las 24 horas:

* Será rechazado.
* No permitirá desbloquear la cuenta.
* El usuario podrá solicitar un nuevo código según las reglas establecidas.

### CE-LOGIN-08 — Código de desbloqueo utilizado

Un código utilizado anteriormente será rechazado y no podrá volver a utilizarse.

### CE-LOGIN-09 — Nuevo código de desbloqueo

La generación de un nuevo código invalidará el código anterior.

### CE-LOGIN-10 — Error del servidor

Si ocurre un error durante el proceso:

* El sistema mostrará un mensaje comprensible.
* No permitirá el acceso hasta completar correctamente la autenticación.
* El error podrá registrarse para revisión técnica.

### CE-LOGIN-11 — Sesión no creada

Si las credenciales son correctas pero no es posible crear la sesión:

* No se deberá conceder acceso al área privada.
* El usuario deberá recibir un mensaje apropiado.
* El evento podrá registrarse para revisión técnica.

---

## 10. Seguridad

El proceso de inicio de sesión deberá considerar:

* Protección de las credenciales.
* Almacenamiento seguro de contraseñas.
* Control de intentos fallidos.
* Bloqueo temporal ante exceso de intentos.
* Códigos de desbloqueo temporales y de un solo uso.
* Validación de autenticación en el servidor.
* Registro de eventos relevantes de seguridad.
* Protección de las áreas privadas mediante comprobación de sesión.

La información de seguridad deberá mantenerse separada conceptualmente de los datos básicos del usuario.

---

## 11. Estados que afectan al inicio de sesión

| Estado                    | Acceso                                          |
| ------------------------- | ----------------------------------------------- |
| Pendiente de verificación | ❌ No permitido                                  |
| Activa                    | ✅ Permitido                                     |
| Bloqueada                 | ❌ No permitido mediante inicio de sesión normal |
| Suspendida                | ❌ No permitido                                  |

**Nota:** El estado `Bloqueada` corresponde a una restricción de seguridad, mientras que `Suspendida` corresponde a una acción administrativa. No representan necesariamente la misma condición dentro del sistema.

---

## 12. Relación con otros módulos

El inicio de sesión se relaciona directamente con:

* **Registro de usuario:** proporciona la cuenta y las credenciales iniciales.
* **Verificación de correo:** determina cuándo una cuenta puede pasar a estado activo.
* **Recuperación de contraseña:** permite recuperar el acceso cuando el usuario no recuerda su contraseña.
* **Cierre de sesión:** finaliza la sesión activa.
* **Gestión de sesiones:** controla la duración y condiciones de las sesiones.
* **Roles y permisos:** determina las funciones disponibles para el usuario.
* **Administración de usuarios:** permite aplicar suspensiones y otras acciones administrativas.

---

## 13. Pendientes

Los siguientes puntos quedan pendientes de definición:

* Número máximo de intentos fallidos antes del bloqueo.
* Duración exacta del bloqueo temporal.
* Cantidad máxima de solicitudes de código de desbloqueo.
* Tiempo mínimo entre solicitudes de nuevos códigos.
* Reglas detalladas de conservación de registros de seguridad.
* Información exacta del dispositivo que se almacenará durante la autenticación.
* Política definitiva para recuperación de una cuenta bloqueada.
