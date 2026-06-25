# 📊 Power BI Gestión People

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=microsoft-power-bi&logoColor=white) ![Data Analysis](https://img.shields.io/badge/Data%20Analysis-blue?style=for-the-badge) ![Status](https://img.shields.io/badge/Estado-Publicado-green?style=for-the-badge) ![License](https://img.shields.io/badge/Licencia-MIT-lightgrey?style=for-the-badge)

*Sistema de inteligencia de negocios para el análisis multidimensional del capital humano y gestión de talento.*

## 🔗 Acceso / Demo
El proyecto se distribuye como una plantilla de Power BI (`.pbit`) que permite la carga dinámica de datos de empleados y formación.

## 📋 Descripción
**Power BI Gestión People** es una solución de análisis de datos diseñada para departamentos de Recursos Humanos y gestión de personas. El proyecto resuelve la necesidad de centralizar métricas dispersas de capital humano, transformando datos brutos de Excel en insights accionables sobre el rendimiento y la equidad organizacional.

El sistema permite a los gestores de talento identificar brechas salariales, analizar la distribución geográfica de la plantilla y monitorear el progreso de la formación profesional, facilitando la toma de decisiones basada en datos para mejorar la retención y el desarrollo del personal.

## ✨ Funcionalidades

| Funcionalidad | Descripción |
| :--- | :--- |
| **Resumen Ejecutivo** | Panel de control con 12 KPIs críticos y segmentadores dinámicos para una vista global. |
| **Análisis de Brecha Salarial** | Comparativa de equidad salarial por género mediante gráficos de dispersión y tablas dinámicas. |
| **Mapa de Talento** | Visualización geoespacial de la ubicación de los empleados mediante Filled Maps. |
| **Seguimiento de Formación** | Análisis de capacitación y satisfacción mediante correlaciones y gráficos de columnas. |
| **Análisis de Tendencias** | Identificación de patrones de comportamiento y correlaciones temporales de la plantilla. |

## ⚙️ Instalación

1. **Requisitos**: Instalar [Power BI Desktop](https://powerbi.microsoft.com/desktop/).
2. **Descarga**: Clonar el repositorio o descargar el archivo `.pbit`.
3. **Carga de Datos**: 
   - Abrir el archivo `GestionPeople.pbit`.
   - Cuando el sistema solicite el origen de datos, seleccionar el archivo Excel con la estructura de empleados y formación.
4. **Actualización**: Hacer clic en el botón `Actualizar` en la cinta de opciones superior para procesar los datos a través de Power Query.

## 🚀 Uso

- **Filtrado Dinámico**: Utilice los *Slicers* laterales para filtrar por departamento, género o ubicación.
- **Navegación**: Use los `ActionButton` integrados en el Dashboard Ejecutivo para saltar entre las páginas de análisis.
- **Análisis de Brecha**: Acceda a la página de *Brecha Salarial* y observe el `ScatterChart` para identificar anomalías salariales por rango de edad y cargo.

## 📁 Estructura del proyecto
```text
powerbi-gestion-people/
├── docs/
│   └── GestionPeople_Informe_19062026.md  # Documentación técnica detallada
└── README.md                                          # Guía de inicio rápido
```

## 🛠️ Tecnologías

| Herramienta | Versión/Detalle | Uso en el proyecto |
| :--- | :--- | :--- |
| **Power BI** | Desktop | Motor de visualización y modelado de datos. |
| **DAX** | Data Analysis Expressions | Creación de medidas complejas (brechas salariales, % contratación). |
| **Power Query** | M Language | ETL para limpieza y transformación de datos de Excel. |
| **Excel** | .xlsx | Fuente de datos primaria. |

<p align="center">Desarrollado por @migueljerico · 2024</p>
