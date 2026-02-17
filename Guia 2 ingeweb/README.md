# Caracterización del sistema de reservas de zonas comunes

En esta guía se realiza la caracterización de los usuarios del sistema mediante tablas de requerimientos funcionales y no funcionales, tanto para el rol de **Residente** como para el rol de **Administrador**. Adicionalmente, se incluyen los diagramas elaborados en PlantUML que modelan el sistema:

-  Diagrama de clases  
![Diagrama de Clases](Guia 2 ingeweb
/Media/Clases.png)

- Diagrama de casos de uso  
![Diagrama de Casos de Uso](Guia 2 ingeweb
/Media/Casos.png)

- Diagrama de flujo  
![Diagrama de Flujo](Guia 2 ingeweb
/Media/Flujo.png)

>  Documento en linea de las tablas de requerimientos: [Proyecto_ingeweb.docx](https://umbedu-my.sharepoint.com/:w:/g/personal/oscarforero_fd_academia_umb_edu_co/IQCjiM4VZaRKRbrIYC7gN_uYAdt83l4IBn3RhYG3k269cgU?e=3QMGWy)

---

## 1. Descripción del problema

En mi conjunto hay un problema con las reservas de las zonas comunes y espacios de recreación como las canchas, el gimnasio, la piscina o los salones comunales. Actualmente la manera de reservar estos espacios se hace por medio de un formulario de Google lo cual genera confusión entre los residentes del conjunto dado que no se tiene como tal una confirmación de que la reservación se hizo de manera exitosa.

Este es un problema que afecta tanto a los residentes que desean usar los espacios como a la administración que debe de revisar los formularios manualmente y pasar las listas de reservas a la administración de las zonas comunes. Esto genera en ocasiones que los vecinos no sepan si un espacio ya fue reservado, si su solicitud fue aceptada o si alguien más hizo la reserva para el mismo horario.

El sistema actual es deficiente dado que el uso de formularios de Google no permite ver la disponibilidad de los espacios en tiempo real, entonces no se sabe si se excedieron los cupos ni se ofrece una confirmación exitosa de la reserva. Además, los residentes deben esperar a que la administración revise manualmente los formularios lo cual puede generar demoras, errores o reservas duplicadas.

---

## 2. Tabla de usuarios

| Usuarios      | Qué puede hacer                                                |
|---------------|----------------------------------------------------------------|
| Residente     | Reservar espacios, Consultar disponibilidad, Cancelar reservas |
| Administrador | Aprobar reservas, Bloquear espacios, Revisar historial         |

---


## 3. Lógica de acciones

### Reserva de un espacio común

**Acción del usuario:**
El residente selecciona la zona que quiere reservar (cancha, gimnasio, piscina o salón comunal), luego escoge la fecha y el horario, y envía la solicitud de reserva.

**Validaciones:**
- El usuario debe estar registrado e iniciar sesión.
- El espacio debe estar disponible en la fecha y hora seleccionadas.
- El horario debe estar dentro de las normas del conjunto.

**Resultado esperado:**
El sistema registra la reserva y muestra una confirmación automática al residente.

**Qué puede salir mal:**
El espacio ya fue reservado por otro usuario o el horario no está permitido.

---

### Consulta de disponibilidad

**Acción del usuario:**
El residente consulta la disponibilidad de las zonas comunes.

**Validaciones:**
- Fecha y espacio seleccionados correctamente.
- Información actualizada en el sistema.

**Resultado esperado:**
El sistema muestra los horarios disponibles y ocupados.

**Qué puede salir mal:**
La información no está actualizada o hay un error en la carga de datos.

---

### Cancelación de una reserva

**Acción del usuario:**
El residente cancela una reserva previamente realizada.

**Validaciones:**
- La reserva debe existir.
- La cancelación debe hacerse dentro del tiempo permitido.

**Resultado esperado:**
La reserva se elimina y el espacio queda disponible nuevamente.

**Qué puede salir mal:**
La reserva ya pasó o el usuario no tiene permisos para cancelarla.

---

### Gestión por parte de la administración

**Acción del usuario:**
El administrador revisa las reservas y puede aprobar, rechazar o bloquear espacios en base a si el residente está al día o no con los pagos de la administración.

**Validaciones:**
- Usuario con rol de administrador.
- Reglas del conjunto aplicadas correctamente.

**Resultado esperado:**
Las reservas quedan organizadas y los espacios se gestionan correctamente.

**Qué puede salir mal:**
Errores humanos en la gestión o problemas técnicos del sistema.

---

## 4. Reflexión final

Esta actividad permitió identificar un problema real del conjunto residencial y analizar cómo la tecnología puede ayudar a solucionarlo. El uso de formularios de Google no es suficiente para manejar reservas de manera organizada. Diseñar un sistema web facilita la gestión, reduce errores y mejora la comunicación entre residentes y administración. Además, el boceto y la lógica de acciones ayudaron a entender cómo funciona un sistema antes de desarrollarlo. Este ejercicio demuestra la importancia de planear bien una solución digital.
