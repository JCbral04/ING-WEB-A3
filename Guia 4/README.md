# Guia 4 - Sprint: Analisis de Puntos Criticos del DOM

En esta guia se realiza la investigacion tecnica correspondiente al sprint de puntos criticos del DOM aplicados al proyecto del sistema de reservas de zonas comunes. El objetivo es identificar las tecnologias mas adecuadas para el proyecto y analizar cada punto critico en relacion con las vulnerabilidades que permite prevenir.

> Documento en linea: [sprint_puntos_criticos_DOM.docx](https://umbedu-my.sharepoint.com/:w:/g/personal/juancabral_eb_academia_umb_edu_co/IQCcxzLRrlX8Rr4q009bJesKAXbr6w1mji1y6tpyjFpTrrU?e=DrnirA)

---

## 1. Proposito

El sistema de reservas requiere una interfaz web que actualice el estado de disponibilidad del calendario en tiempo real, gestione formularios con validaciones dinamicas y controle el acceso por roles (Residente / Administrador). Antes de iniciar el desarrollo del frontend, este sprint establece las bases tecnicas del proyecto analizando cinco puntos criticos relacionados con el rendimiento y la seguridad del DOM, y determina que tecnologias mitigan cada riesgo de forma adecuada.

---

## 2. Tecnologias seleccionadas

| Capa | Tecnologia |
|---|---|
| Framework UI | React 18 + Vite |
| Estilos | Tailwind CSS |
| Enrutamiento | React Router v6 |
| Estado global | Zustand |
| Peticiones HTTP | Axios + React Query |
| Formularios | React Hook Form + Zod |
| Calendario | React Big Calendar / FullCalendar React |
| Backend API | Node.js + Express |
| Base de datos | PostgreSQL |
| Autenticacion | JWT + bcrypt |

---

## 3. Puntos criticos analizados

### Reflow y Repaint
Las actualizaciones frecuentes del calendario (cambio de estado de celdas) pueden provocar recalculos de layout costosos en el navegador. React mitiga esto mediante batching de actualizaciones de estado, uso de `className` en lugar de estilos en linea, y virtualizacion con `react-window` cuando el volumen de celdas lo requiere.

### Event Delegation
Registrar un listener por celda del calendario genera memory leaks y degrada el rendimiento al cambiar de mes. React implementa Event Delegation de forma transparente centralizando todos los eventos en el nodo raiz (`#root`). El equipo debe complementarlo con `useCallback` para memorizar handlers en listas de larga longitud.

### innerHTML y prevencion de XSS
Campos como el motivo de cancelacion de reserva o el motivo de bloqueo de espacio son superficies de ataque para Cross-Site Scripting. React escapa automaticamente el contenido JSX, eliminando el vector principal. Se prohibe el uso de `dangerouslySetInnerHTML` y se requiere validacion de entradas en el backend mas una politica de Content Security Policy (CSP).

### Separacion de responsabilidades
La mezcla de logica de negocio, acceso a datos y presentacion en un mismo componente genera inconsistencias en las reglas de reserva y dificulta las auditorias de seguridad. Se define una arquitectura por capas: `components`, `pages`, `hooks`, `services`, `schemas` y `store`, donde cada capa tiene una responsabilidad unica.

### Acceso al DOM real vs. Virtual DOM
Las actualizaciones concurrentes del estado de disponibilidad pueden generar condiciones de carrera visual si se manipula el DOM real directamente. El algoritmo de reconciliacion de React junto con React Query garantiza que solo los nodos que efectivamente cambiaron de estado sean actualizados en el DOM real. El acceso directo al DOM mediante `useRef` queda reservado para casos justificados como enfoque de campos o integracion de librerias externas.

---

## 4. Relacion con los requerimientos del proyecto

| Punto critico | Requerimientos no funcionales atendidos |
|---|---|
| Reflow / Repaint | RNF-02 Rendimiento, RNF-07 Compatibilidad movil |
| Event Delegation | RNF-03 Rendimiento, RNF-08 Integridad en tiempo real |
| innerHTML / XSS | RNF-01 Seguridad, RNF-04 Privacidad |
| Separacion de responsabilidades | RNF-05 Trazabilidad, RNF-08 Mantenibilidad |
| DOM real vs. Virtual DOM | RNF-02 Rendimiento, RNF-03 Disponibilidad, RNF-08 Integridad |
