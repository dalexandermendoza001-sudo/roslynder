# 02.5 — Desactivar / eliminar cuenta

## Objetivo

Permitir que el usuario gestione la desactivación o solicitud de eliminación de su cuenta de forma controlada y segura.

La desactivación y la eliminación son procesos diferentes:

* **Desactivación:** pausa reversible de la cuenta.
* **Eliminación:** proceso controlado que puede finalizar con la eliminación definitiva de la cuenta.

## 02.5.1 — Desactivar cuenta

### Descripción

La desactivación permite al usuario dejar de utilizar temporalmente su cuenta sin eliminar la información almacenada.

La cuenta podrá ser reactivada posteriormente de acuerdo con las condiciones establecidas por ROSLYNDER.

### Reglas de negocio

#### RN-CTA-01 — Solicitud de desactivación

El usuario autenticado puede solicitar la desactivación de su propia cuenta.

#### RN-CTA-02 — Confirmación

La desactivación requiere una confirmación explícita del usuario antes de ejecutarse.

#### RN-CTA-03 — Restricción de acceso

Una cuenta desactivada no podrá iniciar sesión ni utilizar las funciones privadas de ROSLYNDER.

#### RN-CTA-04 — Conservación de información

La desactivación no elimina los datos asociados a la cuenta.

#### RN-CTA-05 — Conservación de roles

La desactivación no elimina ni modifica los roles asignados al usuario.

#### RN-CTA-06 — Reactivación

El usuario podrá solicitar la reactivación de una cuenta desactivada mientras se encuentre dentro de las condiciones establecidas por ROSLYNDER.

#### RN-CTA-07 — Resultado de la reactivación

Una cuenta reactivada volverá a estar disponible conforme a las reglas de estado de cuenta y seguridad aplicables.

#### RN-CTA-08 — Trazabilidad

Las acciones de desactivación y reactivación deberán conservar información suficiente para fines de trazabilidad.

---

## 02.5.2 — Solicitar eliminación de cuenta

### Descripción

La eliminación de una cuenta no se ejecutará inmediatamente después de la solicitud.

La solicitud iniciará un proceso controlado que permitirá al usuario cancelar la eliminación durante un período de gracia previamente establecido.

### Flujo

```text id="p8x4m2"
CUENTA ACTIVA
      ↓
Solicitud de eliminación
      ↓
Confirmación del usuario
      ↓
EN PROCESO DE ELIMINACIÓN
      ↓
Período de gracia
      │
      ├── Cancelar solicitud
      │        ↓
      │      ACTIVA
      │
      └── Finalizar período
               ↓
      Tratamiento de eliminación
               ↓
      ELIMINACIÓN DEFINITIVA
```

### Reglas de negocio

#### RN-CTA-09 — Solicitud de eliminación

El usuario autenticado puede solicitar la eliminación de su propia cuenta.

#### RN-CTA-10 — Confirmación de eliminación

La solicitud de eliminación requiere una confirmación explícita del usuario.

#### RN-CTA-11 — Proceso controlado

La solicitud de eliminación inicia un proceso controlado y no implica la eliminación inmediata de la cuenta.

#### RN-CTA-12 — Período de gracia

La solicitud permanecerá en proceso durante un período de gracia definido por ROSLYNDER.

#### RN-CTA-13 — Cancelación

Durante el período de gracia, el usuario podrá cancelar la solicitud de eliminación.

#### RN-CTA-14 — Cancelación de la solicitud

Una solicitud cancelada permitirá que la cuenta continúe disponible, siempre que no exista otra restricción que impida su funcionamiento.

#### RN-CTA-15 — Registro de la solicitud

El sistema deberá registrar la solicitud de eliminación y los cambios relevantes de su proceso para fines de trazabilidad.

#### RN-CTA-16 — Eliminación definitiva

Una vez finalizado el período de gracia, el sistema podrá ejecutar el tratamiento correspondiente para la eliminación definitiva de la cuenta según las políticas de conservación de datos de ROSLYNDER.

#### RN-CTA-17 — Irreversibilidad

Una vez completada la eliminación definitiva, la cuenta no podrá recuperarse mediante el procedimiento normal de reactivación.

#### RN-CTA-18 — Información asociada

El tratamiento de publicaciones, productos, perfil comercial, comentarios, favoritos y demás información relacionada con el usuario será definido por las reglas correspondientes de cada módulo.

#### RN-CTA-19 — Conservación de información

La eliminación de la cuenta no implica conservar indefinidamente todos los datos personales.

La información que deba mantenerse por razones de trazabilidad, seguridad o cumplimiento de políticas será determinada mediante las reglas de conservación de datos de ROSLYNDER.

#### RN-CTA-20 — Notificación al usuario

El sistema deberá informar al usuario sobre la solicitud de eliminación y los cambios relevantes de su proceso mediante los mecanismos de comunicación disponibles.

---

## Casos especiales

* Usuario no autenticado → no puede solicitar la desactivación o eliminación desde esta función.
* Usuario intenta desactivar una cuenta ya desactivada → informar que la cuenta ya se encuentra desactivada.
* Usuario intenta solicitar eliminación mientras existe una solicitud en proceso → informar sobre el estado actual de la solicitud.
* Usuario cancela la eliminación dentro del período de gracia → cancelar el proceso y mantener la cuenta disponible.
* Período de gracia finalizado → la cancelación mediante el procedimiento normal ya no estará disponible.
* Error durante la solicitud → no iniciar el proceso hasta confirmar correctamente la operación.
* Error durante la eliminación definitiva → registrar el incidente y aplicar el procedimiento de recuperación correspondiente.

## Consideraciones

La desactivación y la eliminación son procesos diferentes.

La desactivación conserva la cuenta para permitir una futura reactivación.

La eliminación comienza como una solicitud y puede finalizar con la eliminación definitiva después del período de gracia.

Los detalles sobre el tratamiento de publicaciones, productos, perfil comercial, comentarios, favoritos y demás información relacionada serán definidos en sus respectivos módulos.

Los períodos de gracia, conservación de datos y eliminación definitiva se definirán posteriormente durante el análisis de datos, reglas de negocio generales y diseño técnico.

No se establece todavía un nuevo estado definitivo de cuenta para representar cada etapa del proceso de eliminación. La representación técnica y los estados necesarios se definirán posteriormente.
