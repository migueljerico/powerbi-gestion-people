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

1. **Descargar la plantilla**: Obtén el archivo `GestionPeople_Informe.19062026.pbit` desde el repositorio.
2. **Abrir en Power BI**: Ejecuta el archivo con Power BI Desktop instalado.
3. **Configurar Origen**: Cuando el sistema solicite el parámetro `Path_Excel`, introduce la ruta local del archivo de datos (ej. `C:\Datos\Empleados.xlsx`).
4. **Carga de Datos**: Haz clic en "Cargar" para que Power Query procese las tablas `Empleados_final`, `Departamentos_final` y `Registro formacion_final`.

## 🚀 Uso

Para analizar la equidad salarial, navega a la página **Análisis Brecha Salarial de Género** y utiliza el `ComboChart` para comparar el salario promedio por género. 

Para evaluar la capacitación, accede a la página **Formación y Satisfacción** y analiza la correlación entre horas de formación y satisfacción mediante el `ScatterChart`.

## 📁 Estructura del proyecto
```text
. 
├── README.md
├── MANUAL_TECNICO.md
└── docs/
    ├── GestionPeople_Informe.19062026.md
```

## 🛠️ Tecnologías

| Herramienta | Versión/Detalle | Uso en el proyecto |
| :--- | :--- | :--- |
| **Power BI Desktop** | Latest | Motor de visualización y modelado de datos |
| **Power Query** | M Language | Proceso ETL y limpieza de datos brutos |
| **DAX** | Data Analysis Expressions | Creación de medidas y KPIs complejos |
| **Microsoft Excel** | .xlsx | Almacenamiento de datos de origen |

## 📚 Contexto formativo o motivación del proyecto
Este proyecto surge de la necesidad de profesionalizar la gestión de RRHH mediante el uso de Business Intelligence, sustituyendo reportes estáticos por dashboards interactivos que permitan detectar ineficiencias salariales y optimizar los planes de formación corporativa.

<p align="center">Desarrollado por @migueljerico · 2026</p>
