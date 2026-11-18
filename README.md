# Ingeniería Software I — Proyecto 30
## Sistema de Control de Proyectos, Órdenes de Trabajo y Facturación para Empresa de Producción de Software

**UNIVERSIDAD ABIERTA PARA ADULTOS (UAPA)**
- Asignatura: Ingeniería Software I
- Asignación: Semana II
- Facilitadora: Dr.C. Técnicas Zoila Esther Morales Tabares
- Grupo: AD

### Participantes
| Integrante | Rol Operativo Asignado | Responsabilidades Principales |
|---|---|---|
| César Darinel Ortiz Abreu | Responsable de Modelado y Arquitectura | Modelo conceptual de datos (Proyecto, Grupo, Trabajador, Orden de Trabajo, Registro de Horas), lógica de cola priorizada, diagramas técnicos. |
| Jean Luis Báez Reynoso | Analista de Requisitos | Requisitos funcionales (gestión de proyectos, órdenes, 5 consultas gerenciales) y no funcionales. |
| Luis Eduardo García Cegarra | Responsable del Tablero y Apoyo Técnico | Administración de GitHub Projects, columnas Kanban, campos personalizados (prioridad, estado, responsable), soporte técnico. |
| Anyelis Esmeralda López Brito | Responsable de Documentación y UI/UX | Documentación técnica, articulación con prototipos de Diseño de Interfaz de Usuario, calidad en columna En revisión. |

### 1. Identificación y Descripción del Sistema Digital
Plataforma integral de gestión operativa para una Empresa de Producción de Software (continuidad del prototipo de Diseño de Interfaz de Usuario). Controla la ejecución de proyectos asignados a equipos de desarrollo, cada uno liderado por un Jefe de Proyecto que crea, prioriza y distribuye órdenes de trabajo.

**Alcance funcional:**
- Gestión de Proyectos y Equipos: datos maestros de proyecto (identificador, descripción, cliente, fecha terminación, clasificación, sistema operativo) + grupos de trabajo y líderes.
- Administración de Órdenes de Trabajo: ciclo de vida (identificador, prioridad, tiempo estimado, descripción, responsable), cola priorizada FIFO, cierre elimina de lista activa.
- Bitácora de Actividades: avance diario por trabajador (descripción, horas, fecha).
- Consultas y Reportes Gerenciales (5):
  1. Horas diarias por empleado en un período.
  2. Total horas acumuladas por orden.
  3. Facturación por proyecto (órdenes finalizadas, complejidad + horas).
  4. Órdenes pendientes por proyecto.
  5. Morosidad (tiempo real > estimado) ordenada descendentemente.

### 2. Enfoque Metodológico: Kanban en GitHub Projects
- Flujo continuo y transparencia: Backlog | Por hacer | En progreso | En revisión | Finalizado. Visibilidad de responsable y etapa, sin duplicidad.
- Gestión dinámica de prioridades: repriorización del Backlog en cualquier momento, inserción por prioridad sin perder orden FIFO.
- Alineación progresiva: flujo incremental desde requisitos y modelado hasta integración UI, sin ceremonias rígidas de Scrum, con trazabilidad en GitHub.

Justificación vs Scrum: sin Product Owner/Scrum Master formales ni sprints cerrados; ideal para 4 integrantes en paralelo por módulos.

### 3. Tablero Kanban
- **Proyecto:** [Proyecto 30 - Control de Producción de Software](https://github.com/users/cesardarinel/projects/20)
- **Columnas (campo Estado Kanban):** Backlog | Por hacer | En progreso | En revisión | Finalizado
- **Campos personalizados:** Prioridad (Alta/Media/Baja, Single Select), Estado Kanban (Single Select 5 opciones), Responsable / Rol (Text), Status nativo (Todo/In Progress/Done mapeado a Por hacer/Backlog=Todo).
- **Repositorio vinculado:** `cesardarinel/ingenieria-software-i`

### 4. Tarjetas iniciales (6 issues)
| # | Título | Prioridad | Estado | Responsable |
|---|---|---|---|---|
| #1 | Gestión de Proyectos y Grupos de Trabajo - CRUD proyectos (ID, descripción, cliente, fecha, clasificación, SO) + Grupo y Jefe | Alta | Por hacer | Jean Luis Báez Reynoso |
| #2 | Asignación y Cola Priorizada de Órdenes - crear órdenes (ID, prioridad, tiempo estimado, descripción), inserción prioritaria FIFO | Alta | Por hacer | César Darinel Ortiz Abreu |
| #3 | Bitácora de Ejecución y Cierre - registro diario (qué hizo, horas, fecha), cierre saca de lista activa | Alta | Por hacer | César Darinel Ortiz Abreu |
| #4 | Módulo de Facturación - solo terminadas, complejidad + horas | Media | Backlog | Jean Luis Báez Reynoso |
| #5 | Consultas Control de Horas - horas por trabajador/día en rango + total por orden | Media | Backlog | Anyelis Esmeralda López Brito |
| #6 | Reporte Pendientes y Morosidad - no terminadas por proyecto + real>estimado desc | Baja | Backlog | Luis Eduardo García Cegarra |

Ver detalle y criterios de aceptación en la pestaña Issues (cada issue incluye descripción, alcance Proyecto 30, checklist y metadatos Kanban).

### 6. Evidencias Visuales (para capturas)
1. Captura de `https://github.com/users/cesardarinel/projects/20` vista Board agrupada por `Estado Kanban`.
2. Captura de vista Table con columnas Prioridad, Estado Kanban, Responsable / Rol.
3. Captura de Issues list con labels `prioridad-alta/media/baja` + `kanban-backlog/por-hacer`.
4. Captura de un issue abierto mostrando criterios de aceptación.
5. Para el documento Word/PDF: pegar las 4 capturas en la sección 6 con pie de figura.

```bash
# Verificación rápida con gh
gh repo view cesardarinel/ingenieria-software-i --web
gh issue list -R cesardarinel/ingenieria-software-i --limit 20
gh project view 20 --owner cesardarinel --web
gh project item-list 20 --owner cesardarinel
```

### Estructura modelo conceptual (inicio)
```
Proyecto( id, descripcion, cliente, fecha_terminacion, clasificacion, sistema_operativo, grupo_id )
Grupo( id, nombre, jefe_proyecto_id )
Trabajador( id, nombre, grupo_id, rol )
OrdenTrabajo( id, proyecto_id, prioridad, tiempo_estimado, tiempo_real, descripcion, responsable_id, estado [activa|terminada|pendiente] )
RegistroHoras( id, orden_id, trabajador_id, descripcion_labor, horas, fecha )
```
Reglas: cola por trabajador ordenada por prioridad (Alta>Media>Baja) y FIFO; cierre mueve a finalizadas; facturación = SUM(horas_reales * tarifa * factor_complejidad) solo terminadas; morosidad = tiempo_real > tiempo_estimado ORDER BY diferencia DESC.
