# Sistema Integrado de Visualización y Automatización de Datos

Proyecto académico de **Gestión de Datos I** que implementa un **Cuadro de Mando Integral (CMI / Balanced Scorecard)** para *NTM Producciones*, una productora audiovisual ficticia. El sistema integra los datos operativos de la empresa en un modelo dimensional, los visualiza en un dashboard interactivo de **Power BI** y define reglas de alerta automática con **Power Automate** cuando un indicador estratégico queda bajo su meta.

---

## Objetivo

Dar a la gerencia una vista única y actualizada del desempeño de la productora en las cuatro perspectivas del CMI:

| Perspectiva | Objetivo estratégico | Indicador principal | Meta |
|---|---|---|---|
| Financiera | Maximizar la rentabilidad de los proyectos | Margen de utilidad neta | ≥ 13,8 % |
| Cliente | Elevar la satisfacción del cliente | % de proyectos con NPS ≥ 9 | ≥ 60 % |
| Procesos internos | Optimizar la eficiencia operativa | % de proyectos entregados en plazo | ≥ 85 % |
| Aprendizaje y crecimiento | Desarrollar competencias técnicas | % de colaboradores con ≥ 40 h de capacitación | ≥ 80 % |

---

## Contenido del repositorio

```
Gestion-de-datos-I/
├── Base_CMI.xlsx            # Fuente de datos: modelo dimensional (hechos + dimensiones)
└── Dashboard Terminado.pbix # Dashboard de Power BI conectado a Base_CMI.xlsx
```

---

## Modelo de datos

`Base_CMI.xlsx` está organizado como un **modelo en estrella**, con una tabla de hechos central y tablas de dimensión relacionadas por claves foráneas.

### Tabla de hechos

| Hoja | Descripción |
|---|---|
| `Fact_Gestion_NTM` | Un registro por proyecto (40 proyectos, año 2026): servicio, nicho, días planificados vs. reales, ingresos, costos y utilidad (CLP), NPS del proyecto, residuos generados, técnicas aplicadas y estado. |

### Dimensiones

| Hoja | Descripción |
|---|---|
| `Dim_Clientes` | 50 clientes con su NPS y su condición de cliente 2025, activo 2026 y retenido. |
| `Dim_Empleados` | 10 colaboradores con su rol y total de horas de capacitación. |
| `Dim_Tiempo_Finanzas` | Calendario 2026 por mes y trimestre. |
| `Dim_Capacitacion` | Registro de cursos realizados por cada colaborador (curso, trimestre, horas). |
| `Dim_Tecnicas` | Catálogo de técnicas audiovisuales innovadoras aplicadas en los proyectos. |
| `Dim_Ref2025` | Valores de referencia 2025 de los 24 indicadores del CMI (F, C, P, A), usados como línea base. |

### Tablas auxiliares

| Hoja | Descripción |
|---|---|
| `Empleado_Proyecto` | Tabla puente (muchos a muchos) que asigna colaboradores y roles a cada proyecto. |
| `Resumen_CMI_Automatizacion` | Resumen de los 4 indicadores clave con meta, valor actual, umbral, condición de activación, destinatario del correo y estado de verificación. Es la tabla que alimenta las alertas de Power Automate. |

```
                 Dim_Clientes      Dim_Tiempo_Finanzas
                        \               /
Dim_Empleados ──── Fact_Gestion_NTM ──── Dim_Ref2025
      |                 |
Dim_Capacitacion   Empleado_Proyecto     Dim_Tecnicas
```

---

## Dashboard (Power BI)

El reporte tiene dos páginas con botones de navegación entre ellas.

**Página 1 – Desempeño financiero y operativo**
- Tarjetas: utilidad total e ingreso promedio por cliente.
- Embudo de ingresos totales por tipo de servicio.
- Volumen de servicios por trimestre.
- Matriz de ingresos por servicio y trimestre.
- Comparación de días planificados vs. días reales de entrega por servicio.
- Segmentador por tipo de servicio.

**Página 2 – Clientes, procesos y aprendizaje**
- Tarjetas: servicio estrella actual y crecimiento mensual de servicios.
- Medidor de NPS promedio de los proyectos.
- Clientes retenidos de 2025 a 2026.
- Evolución mensual de fechas previstas vs. efectivas de entrega (2026).
- Horas de capacitación por curso.

---

## Automatización (Power Automate)

La hoja `Resumen_CMI_Automatizacion` define una regla por indicador:

- **Condición:** se dispara una alerta si `Valor < Umbral`.
- **Destinatario:** el área responsable de cada perspectiva (gerencia, atención al cliente, producción o RR.HH.).
- **Estado:** `OK - cumple meta` o `ALERTA - bajo meta`.

El flujo de Power Automate lee esta tabla y envía un correo al área correspondiente cuando un indicador está bajo la meta. Con los datos actuales, quedan en alerta los indicadores de **satisfacción del cliente** (55 % vs. meta 60 %) y **entregas en plazo** (55 % vs. meta 85 %).

---

## Tecnologías

- **Microsoft Excel** – almacenamiento y estructuración del modelo dimensional.
- **SQL** – consultas de extracción y limpieza de datos.
- **Power BI Desktop** – modelado, medidas DAX y visualización.
- **Power Automate** – alertas automáticas por correo según umbrales del CMI.

---

## Cómo usarlo

1. Clonar o descargar el repositorio:
   ```bash
   git clone https://github.com/sofia-espinosa/Gestion-de-datos-I.git
   ```
2. Abrir `Dashboard Terminado.pbix` con **Power BI Desktop** (Windows).
3. Si Power BI no encuentra la fuente de datos, ir a **Transformar datos → Configuración de origen de datos → Cambiar origen** y seleccionar la ruta local de `Base_CMI.xlsx`.
4. Presionar **Actualizar** para recargar los datos.

---

## Autores

- **Sofía Espinosa Solís** 
- **Gustavo Espinoza Fuentes**
- **Jairo Albornoz Figeroa**
- **Benjamín Garrido Jara**
- **Iván Sánchez Valenzuela** 

Proyecto desarrollado para la asignatura **Gestión de Datos I** – Ingeniería Civil Informática, Universidad Católica del Maule.

> Los datos de *NTM Producciones* son ficticios y se crearon con fines académicos.
