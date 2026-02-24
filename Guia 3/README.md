# Guía 3 - Definición de Arquitectura de Software

En esta guía se define y justifica la arquitectura de software seleccionada para el sistema de reservas de zonas comunes del conjunto residencial. La decisión arquitectónica se tomó con base en los requerimientos funcionales y no funcionales levantados en la Guía 2, los diagramas UML elaborados (clases, casos de uso y flujo), y el presupuesto de tiempo real disponible del equipo.

>  Documento completo en línea: [Definicion de Arquitectura Proyecto.docx](https://umbedu-my.sharepoint.com/:w:/g/personal/juancabral_eb_academia_umb_edu_co/IQBglvsxD4UrSby4IOGnkohmAery2tzUSUeQZ9v9J0cuZ6A?e=z3wDVQ)

---

## Contexto del Proyecto

El sistema busca reemplazar el proceso actual de reservas mediante formularios de Google, el cual no ofrece confirmaciones en tiempo real, permite reservas duplicadas y requiere revisión manual por parte de la administración. La arquitectura definida en esta guía es la base sobre la que se construirá toda la solución.

---

## Arquitectura Elegida: Capas 

Se adoptó la **arquitectura de tres capas**, que divide el sistema en niveles independientes con responsabilidades claramente separadas. La regla fundamental es que ninguna capa se salta a la siguiente: la presentación nunca accede directamente a los datos.

| Capa | Responsabilidad | Tecnología |
|---|---|---|
| **Presentación** | Interfaz de usuario: calendario, formulario de reserva, panel admin | HTML5, CSS3, JavaScript |
| **Negocio** | Reglas y validaciones: disponibilidad, pagos, duplicados, auditoría | Node.js + Express.js |
| **Datos** | Persistencia: lectura y escritura en base de datos | MySQL / PostgreSQL |

---

## Por Qué Elegimos Esta Arquitectura

### Porque el diagrama de clases la confirma
Las clases definidas en la Guía 2 ya están naturalmente agrupadas por responsabilidad: `Usuario`, `Residente` y `Administrador` son entidades de datos; `Reserva`, `EspacioComun`, `Horario` y `ReglaReserva` encapsulan lógica de negocio; `Notificacion` y `RegistroAuditoria` son servicios transversales. Esta organización coincide exactamente con la separación que propone la arquitectura de capas.

### Porque el diagrama de flujo la exige
El flujo de reserva define una cadena de validaciones secuenciales (¿bloqueado? → ¿disponible? → ¿reglas ok? → ¿pagos ok? → decisión del admin → auditoría → notificación). Centralizar esta lógica en una capa de negocio dedicada evita que sea frágil o manipulable desde el frontend.

### Porque el diagrama de casos de uso lo respalda
Los casos de uso `Validar residente (pagos)` y `Registrar auditoría` son compartidos por múltiples actores (Residente y Administrador). Una capa de negocio centralizada permite reutilizar esa lógica sin duplicarla.

### Porque los requerimientos no funcionales la requieren
- **RNF-01 Seguridad por roles** → la capa de negocio valida el rol antes de ejecutar cada operación.
- **RNF-04 No duplicados** → la capa de negocio valida antes de insertar; la capa de datos aplica restricciones de unicidad.
- **RNF-05 Trazabilidad** → el `RegistroAuditoria` es gestionado exclusivamente por la capa de negocio.
- **RNF-08 Mantenibilidad** → cambiar reglas solo afecta la capa de negocio, sin tocar presentación ni datos.

### Porque el tiempo disponible la hace viable
Con 10 semanas y entre 90 y 180 horas totales de trabajo en equipo, la arquitectura de capas permite dividir el trabajo entre los tres integrantes por capa o por funcionalidad, reduciendo dependencias y conflictos de integración.

---

## Estructura de Carpetas del Proyecto

```
/ING-WEB-A3
├── /frontend                  ← Capa de Presentación
│   ├── index.html
│   ├── /css/
│   ├── /js/
│   └── /pages/
│       ├── login.html
│       ├── calendario.html
│       ├── mis-reservas.html
│       └── admin-panel.html
│
├── /backend                   ← Capa de Negocio
│   ├── server.js
│   ├── /routes/
│   ├── /controllers/
│   └── /middleware/
│       └── authMiddleware.js
│
└── /database                  ← Capa de Datos
    └── esquema.sql
```


