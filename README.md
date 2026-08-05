# 📊 Power BI Gestión People

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=microsoft-power-bi&logoColor=white) ![DAX & M](https://img.shields.io/badge/DAX%20%26%20M-0078D4?style=for-the-badge) ![Estado](https://img.shields.io/badge/Estado-Publicado-green?style=for-the-badge) ![Licencia](https://img.shields.io/badge/Licencia-MIT-lightgrey?style=for-the-badge)

*Sistema de inteligencia de negocios para el análisis multidimensional del capital humano y gestión de talento.*

## 🔗 Acceso / Demo
El proyecto se distribuye como una plantilla interactiva de Power BI (`.pbit`) que permite la carga dinámica de datos mediante un parámetro de ruta local. No requiere instalación de servidor; funciona directamente en **Power BI Desktop** y puede publicarse posteriormente en el servicio web de Microsoft.

## 📋 Descripción
**Power BI Gestión People** es una solución de Business Intelligence diseñada específicamente para departamentos de Recursos Humanos y gestión estratégica del talento. El proyecto resuelve la fragmentación típica de los datos de RRHH, centralizando métricas dispersas en hojas de cálculo estáticas y transformándolas en un modelo relacional dinámico capaz de generar insights accionables sobre equidad, rendimiento y desarrollo profesional.

La arquitectura del informe conecta tres fuentes de información críticas: perfiles de empleados, estructura departamental y registros históricos de formación. Mediante transformaciones ETL automatizadas y medidas DAX avanzadas, el sistema permite a los gestores identificar brechas salariales por género, visualizar la distribución geográfica de la plantilla, correlacionar horas de capacitación con niveles de satisfacción y detectar patrones temporales de contratación y rotación.

Esta herramienta está pensada para directivos de RRHH, analistas de datos corporativos y equipos de planificación estratégica que necesitan sustituir reportes manuales por paneles de control interactivos, segmentables y actualizables bajo demanda.

## ✨ Funcionalidades

| Funcionalidad | Descripción |
| :--- | :--- |
| **Resumen Ejecutivo** | Panel de control con 5+ Cards de KPIs críticos, gráficos Donut y Columnas, junto a segmentadores dinámicos para filtrado cruzado instantáneo. |
| **Análisis de Brecha Salarial** | Comparativa de equidad salarial por género utilizando `ComboChart` y tablas pivot, calculando automáticamente la diferencia porcentual mediante DAX. |
| **Mapa de Talento** | Visualización geoespacial de la ubicación de los empleados mediante `FilledMaps` y tablas de densidad por Comunidad Autónoma y Provincia. |
| **Seguimiento de Formación** | Análisis de capacitación y satisfacción mediante `ScatterChart` que correlaciona horas acumuladas, coste por curso y nivel de satisfacción (escala 1-10). |
| **Tendencias y Correlaciones** | Identificación de patrones temporales de contratación, evaluación de desempeño y evolución de objetivos departamentales a lo largo del tiempo. |

## ⚙️ Instalación

1. **Descargar la plantilla**: Obtén el archivo `GestionPeople_Informe.19062026.pbit` desde la raíz del repositorio.
2. **Abrir en Power BI Desktop**: Ejecuta el archivo con la versión más reciente de Power BI Desktop instalada.
3. **Configurar Origen de Datos**: Cuando el asistente solicite el parámetro `Path_Excel`, introduce la ruta absoluta al archivo Excel de práctica:
   ```powershell
   C:\Datos\GestionPeople_Dataset_PowerBI.xlsx
   ```
4. **Validar Transformaciones**: Haz clic en "Cargar" para que Power Query ejecute las consultas `Empleados_final`, `Departamentos_final` y `Registro formacion_final`, aplicando automáticamente el tipado de columnas (`Table.TransformColumnTypes`) y estableciendo las relaciones 1:N.

## 🚀 Uso

Una vez cargados los datos, navega por las pestañas inferiores para acceder a los módulos analíticos. Para evaluar la equidad organizacional, dirígete a la página **Análisis Brecha Salarial de Género** y utiliza el `ComboChart` para comparar el salario promedio entre géneros. La medida DAX `Brecha Salarial Genero %` calculará automáticamente la desviación porcentual.

Para auditar el impacto de la capacitación, accede a **Formación y Satisfacción** y analiza la nube de puntos generada por el `ScatterChart`. Observa cómo la medida `Evaluacion Numerica` transforma las categorías cualitativas ("Excelente", "Bueno", etc.) en una escala cuantitativa (1-4), permitiendo correlacionar directamente las horas de formación con el desempeño registrado.

Los segmentadores superiores permiten filtrar por departamento, tipo de contrato o año de contratación en tiempo real, actualizando todos los visuales conectados simultáneamente.

## 📁 Estructura del proyecto
```text
. 
├── README.md
├── MANUAL_TECNICO.md
└── docs/
    ├── GestionPeople_Dataset_PowerBI.md
    └── GestionPeople_Informe.19062026.md
```

## 🛠️ Tecnologías

| Herramienta | Versión/Detalle | Uso en el proyecto |
| :--- | :--- | :--- |
| **Power BI Desktop** | Última versión estable | Motor de visualización, modelado de datos y publicación |
| **Power Query (Lenguaje M)** | Nativo | ETL, limpieza de nulos, transformación de tipos y parametrización de rutas |
| **DAX** | Data Analysis Expressions | Creación de medidas KPI, inteligencia temporal y formato condicional |
| **Microsoft Excel** | .xlsx (Office 365) | Almacenamiento estructurado de datos fuente (3 hojas relacionales) |
| **Tablas de Fecha** | LocalDateTable / DateTableTemplate | Habilitación de cálculos YoY, MoM y agregaciones temporales precisas |

## 📚 Contexto formativo o motivación del proyecto
Este repositorio fue desarrollado como material práctico para el módulo **IFCT153 "Análisis de Datos con Excel: Power Query, Power Pivot y Power BI"**, impartido por Daniela Laborda en Izquierdo FP Zaragoza. Su objetivo pedagógico es simular un entorno corporativo real (GestiónPeople S.L.) donde los estudiantes apliquen técnicas avanzadas de diseño de informes, normalización de datos y creación de medidas DAX complejas. Más allá del ámbito académico, el proyecto responde a la necesidad empresarial de profesionalizar la gestión de RRHH, reemplazando flujos de trabajo manuales por dashboards escalables, auditables y listos para integrarse con fuentes centralizadas como SharePoint o SQL Server.

<p align="center">Creado por @migueljerico y documentado por BazaarLink (Qwen 3.7 Flash (free)) · 2026</p>