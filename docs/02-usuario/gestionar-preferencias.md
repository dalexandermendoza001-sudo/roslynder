# 02.4 — Gestionar preferencias

## Objetivo

Permitir que el usuario autenticado configure determinadas preferencias relacionadas con la forma en que ROSLYNDER le informa sobre actividades, comunicaciones y funcionalidades relacionadas con su experiencia dentro del sistema.

Las preferencias son independientes de los datos personales, roles, permisos y estado de cuenta.

## Funciones incluidas

La gestión de preferencias comprende:

* Preferencias de notificaciones.
* Preferencias de comunicación.
* Preferencia de ubicación.

---

# 02.4.1 — Preferencias de notificaciones

## Objetivo

Permitir que el usuario configure las notificaciones relacionadas con actividades de ROSLYNDER y determine los canales disponibles mediante los cuales desea recibirlas.

## Tipos de notificaciones

Las notificaciones se clasifican inicialmente en:

### Notificaciones de seguridad y cuenta

Corresponden a eventos importantes relacionados con la seguridad o el funcionamiento de la cuenta.

Ejemplos:

* Cambio de contraseña.
* Cambio de correo electrónico.
* Nuevo inicio de sesión.
* Bloqueo de cuenta.
* Suspensión de cuenta.
* Solicitud de eliminación de cuenta.
* Reactivación de cuenta.
* Otros eventos relevantes de seguridad.

Estas notificaciones no podrán deshabilitarse completamente.

### Notificaciones de actividad

Corresponden a eventos relacionados con la interacción del usuario con las funcionalidades de ROSLYNDER.

Ejemplos:

* Comentarios en una publicación.
* Nuevos "Me gusta".
* Nuevos favoritos o guardados.
* Publicación próxima a vencer.
* Cambio de estado de una publicación.
* Novedades relacionadas con actividades del usuario.

Estas notificaciones podrán configurarse según las opciones disponibles.

## Canales iniciales

Los canales considerados para el MVP son:

* Notificaciones dentro de ROSLYNDER.
* Correo electrónico.

La incorporación de nuevos canales podrá realizarse posteriormente.

## Reglas de negocio

### RN-PREF-01 — Configuración personal

El usuario autenticado puede configurar las preferencias de notificaciones asociadas a su propia cuenta.

### RN-PREF-02 — Notificaciones de actividad

El usuario puede activar o desactivar las notificaciones de actividad que ROSLYNDER permita configurar.

### RN-PREF-03 — Canales de notificación

El usuario puede configurar los canales disponibles para recibir las notificaciones configurables.

### RN-PREF-04 — Notificaciones de seguridad

Las notificaciones relacionadas con eventos críticos de seguridad o estado de la cuenta no pueden deshabilitarse completamente.

### RN-PREF-05 — Preferencias no alteran eventos

Modificar una preferencia de notificación no impide que el evento ocurra ni modifica la información registrada por el sistema.

La preferencia únicamente determina cómo se informa al usuario.

### RN-PREF-06 — Notificaciones según funcionalidad

Las notificaciones disponibles dependerán de los eventos generados por las funcionalidades utilizadas por el usuario.

### RN-PREF-07 — Independencia de roles

Las preferencias de notificaciones son independientes de los roles asignados al usuario.

### RN-PREF-08 — Estado de lectura

Las notificaciones disponibles dentro de ROSLYNDER podrán mantenerse como leídas o no leídas.

### RN-PREF-09 — Consulta de notificaciones

El usuario autenticado puede consultar las notificaciones asociadas a su cuenta.

### RN-PREF-10 — Actualización de preferencias

Los cambios realizados en las preferencias se aplican a las nuevas notificaciones generadas después de la modificación.

### RN-PREF-11 — Conservación de información

Las preferencias de notificación no modifican ni eliminan los datos personales, roles, publicaciones, productos u otra información asociada a la cuenta.

---

# 02.4.2 — Preferencias de comunicación

## Objetivo

Permitir que el usuario configure sus preferencias respecto de las comunicaciones informativas u opcionales enviadas por ROSLYNDER.

Las comunicaciones relacionadas con la seguridad, verificación y funcionamiento de la cuenta se consideran necesarias y no podrán deshabilitarse completamente.

## Comunicaciones obligatorias

ROSLYNDER podrá enviar comunicaciones necesarias para:

* Verificación del correo electrónico.
* Recuperación de contraseña.
* Cambio de contraseña.
* Cambio de correo electrónico.
* Inicio de sesión o actividad de seguridad relevante.
* Bloqueo o suspensión de cuenta.
* Solicitud de eliminación de cuenta.
* Información necesaria para el funcionamiento del servicio.

## Comunicaciones opcionales

El usuario podrá configurar, cuando ROSLYNDER las habilite:

* Novedades de ROSLYNDER.
* Nuevas funcionalidades.
* Información general sobre el servicio.
* Comunicaciones promocionales.
* Campañas comerciales futuras.

Las comunicaciones promocionales y comerciales no forman parte de una obligación del MVP y podrán incorporarse posteriormente.

## Reglas de negocio

### RN-COM-01 — Configuración personal

El usuario autenticado puede configurar las preferencias de comunicación asociadas a su cuenta.

### RN-COM-02 — Comunicaciones obligatorias

ROSLYNDER podrá enviar comunicaciones necesarias para la seguridad, verificación, recuperación y funcionamiento de la cuenta, independientemente de las preferencias opcionales del usuario.

### RN-COM-03 — Comunicaciones opcionales

El usuario puede activar o desactivar las comunicaciones informativas o promocionales que ROSLYNDER habilite.

### RN-COM-04 — Separación de comunicaciones y notificaciones

Las preferencias de comunicación no modifican las preferencias de notificaciones de actividad.

### RN-COM-05 — Canales disponibles

Las comunicaciones se enviarán únicamente mediante los canales que ROSLYNDER tenga habilitados para cada tipo de comunicación.

### RN-COM-06 — Cambios de preferencias

Los cambios realizados por el usuario se aplicarán a las nuevas comunicaciones generadas después de la modificación.

### RN-COM-07 — Independencia de roles

Las preferencias de comunicación son independientes de los roles asignados al usuario.

### RN-COM-08 — Conservación de información

Modificar las preferencias de comunicación no modifica los datos personales, roles, publicaciones, productos ni demás información de la cuenta.

---

# 02.4.3 — Preferencia de ubicación

## Objetivo

Permitir que el usuario establezca una ubicación preferida que ROSLYNDER pueda utilizar como referencia para personalizar determinadas funcionalidades relacionadas con ubicación.

La ubicación preferida no representa necesariamente la ubicación física actual del usuario.

## Diferencia entre ubicación personal y ubicación preferida

La ubicación registrada como dato personal representa información asociada al usuario.

La ubicación preferida representa el lugar que el usuario desea utilizar como referencia para determinadas funcionalidades.

Por ejemplo:

```text
Datos personales
Provincia: Guayas
Ciudad: Durán

Ubicación preferida
Provincia: Guayas
Ciudad: Guayaquil
```

La ubicación preferida puede ser diferente de la ubicación registrada como dato personal.

## Selección de ubicación

Para el MVP, el usuario podrá seleccionar:

* Provincia.
* Ciudad.

La ciudad seleccionada debe corresponder a la provincia seleccionada.

La utilización de geolocalización automática podrá incorporarse posteriormente como una funcionalidad independiente.

## Reglas de negocio

### RN-UBI-01 — Configuración de ubicación

El usuario autenticado puede establecer una ubicación preferida para personalizar determinadas funcionalidades.

### RN-UBI-02 — Independencia de datos personales

La ubicación preferida se gestiona independientemente de la dirección, provincia y ciudad registradas como datos personales.

### RN-UBI-03 — Relación provincia-ciudad

La ciudad seleccionada debe pertenecer a la provincia seleccionada según las opciones válidas disponibles en ROSLYNDER.

### RN-UBI-04 — Modificación

El usuario puede modificar su ubicación preferida cuando lo considere necesario.

### RN-UBI-05 — No modificación de datos personales

Cambiar la ubicación preferida no modifica la dirección, provincia ni ciudad registradas como datos personales.

### RN-UBI-06 — Uso como referencia

ROSLYNDER podrá utilizar la ubicación preferida como referencia para funcionalidades que dependan de ubicación, como búsqueda o priorización de resultados.

### RN-UBI-07 — No implica geolocalización

Establecer una ubicación preferida no implica que ROSLYNDER conozca la ubicación física actual del usuario.

### RN-UBI-08 — Geolocalización futura

La geolocalización automática podrá incorporarse posteriormente como una funcionalidad independiente y estará sujeta a sus propias reglas de privacidad y permisos.

---

## Flujo general

```text
Usuario inicia sesión
        ↓
Accede a "Preferencias"
        ↓
Sistema valida la sesión
        ↓
Usuario selecciona una categoría
        │
        ├── Notificaciones
        │       ↓
        │   Configura avisos y canales
        │
        ├── Comunicación
        │       ↓
        │   Configura comunicaciones opcionales
        │
        └── Ubicación
                ↓
            Selecciona provincia y ciudad
        ↓
Sistema valida los cambios
        ↓
Guarda las preferencias
```

## Casos especiales

* Sesión no válida → solicitar nuevo inicio de sesión.
* Preferencia inválida → impedir el almacenamiento.
* Provincia y ciudad incompatibles → impedir el almacenamiento.
* Usuario intenta desactivar una comunicación obligatoria → mantenerla habilitada.
* Usuario modifica una preferencia → aplicar el cambio a nuevas notificaciones o comunicaciones.
* Error al guardar preferencias → informar al usuario sin revelar información técnica sensible.

## Consideraciones

Las preferencias representan configuraciones personales del usuario y no deben utilizarse para almacenar información que corresponda a otras áreas del sistema.

Se mantiene la siguiente separación conceptual:

```text
USUARIO
│
├── Datos personales
│
├── Estado de cuenta
│
├── Roles
│
├── Sesiones
│
└── Preferencias
       ├── Notificaciones
       ├── Comunicación
       └── Ubicación preferida
```

La estructura física de almacenamiento de estas preferencias se determinará posteriormente durante el **análisis de datos y diseño de la base de datos**.

La política de conservación de notificaciones y comunicaciones se definirá posteriormente.
