# Documentación Técnica: GestionPeople_Informe.19062026.pbit

## 📋 Resumen
Este documento detalla la estructura y configuración del archivo de plantilla de Power BI `GestionPeople_Informe.19062026.pbit`. El informe está diseñado para la gestión de recursos humanos, permitiendo el análisis de datos de empleados, salarios, formación, desempeño y distribución geográfica.

## 🔑 Puntos clave
- **Análisis Multidimensional**: Incluye vistas ejecutivas, análisis de brecha salarial de género, mapas de talento y seguimiento de formación.
- **Modelo de Datos**: Basado en tres tablas principales (`Empleados_final`, `Departamentos_final` y `Registro formacion_final`) con el uso de medidas DAX avanzadas para KPIs.
- **Origen de Datos**: Los datos se extraen de un archivo Excel externo mediante consultas de Power Query.
- **Interactividad**: Implementación de segmentadores (slicers), botones de acción y navegación entre múltiples páginas de análisis.

## 📝 Detalle

### 1. Estructura del Informe (Páginas y Visualizaciones)
El informe se organiza en diversas páginas especializadas según el objetivo del análisis:

| Página | Visuales Principales | Propósito |
| :--- | :--- | :--- |
| **Resumen ejecutivo** | 5 Cards, Donut Chart, Column Chart, Slicers | Vista general de KPIs principales. |
| **Dashboard ejecutivo** | 5 Cards, Slicers, Action Button | Panel de control de alto nivel. |
| **Análisis salarial / Visual** | Column Charts, Line Charts, Pivot Tables | Análisis de compensaciones y salarios. |
| **Análisis Brecha Salarial de Género** | Combo Chart, Pivot Table, Scatter Chart | Comparativa salarial entre géneros. |
| **Mapa de Talento / Distribución Geográfica** | Maps, Filled Maps, Table | Localización de personal y talento. |
| **Formación y Satisfacción** | Scatter Chart, Clustered Column Chart | Relación entre capacitación y satisfacción. |
| **Informe de Formación y Desarrollo** | 3 Cards, Scatter Charts, Donut Chart | Detalle de cursos y capacitación. |
| **Tendencias y correlaciones** | Line Chart, Scatter Chart | Análisis de comportamiento de datos. |
| **Otras páginas** | Tables, Bar Charts | Ejercicios específicos y cuadros de mando personales. |

### 2. Modelo de Datos

#### Tablas Principales
- **Empleados_final**: Tabla central con 18 columnas (datos demográficos, contractuales, salariales y de desempeño).
- **Departamentos_final**: Información organizativa (Director, Presupuesto, Objetivos).
- **Registro formacion_final**: Histórico de cursos, horas, costes y resultados de formación.
- **Tablas de Fecha**: Múltiples tablas `LocalDateTable` y `DateTableTemplate` para el manejo de inteligencia de tiempo.

#### Medidas DAX Implementadas
El modelo incluye cálculos específicos para la toma de decisiones:
- **Gestión de Contratos**: `% Indefinidos` (proporción de empleados con contrato indefinido).
- **Equidad Salarial**: `Brecha Salarial Genero %` y `Brecha Salarial Auditoria %` (comparativa de salarios promedio entre hombres y mujeres).
- **Desempeño y Formación**: 
    - `% de empleados formados`.
    - `Evaluacion Numerica`: Convierte categorías ("Excelente", "Bueno", etc.) a una escala numérica (1-4).
    - `% Evaluacion Excelente/Bueno`: Porcentaje de empleados de alto rendimiento.
- **Cálculos Temporales**: `Horas Ultimo Año` (Suma de horas de formación en los últimos 12 meses).
- **Formato Condicional**: `Color Fondo Tabla` (Asigna color verde `#92D050` si el empleado tiene >40 horas de formación y evaluación "Excelente").

### 3. Consultas (Power Query / M)
El proceso de extracción y transformación de datos (ETL) se realiza mediante tres consultas que importan datos desde un archivo Excel ubicado en ruta local. Todas las consultas aplican una transformación de tipos de datos (`Table.TransformColumnTypes`) para asegurar la integridad de los campos (Enteros, Texto, Fecha y Decimales).

## ✅ Conclusiones / siguientes pasos
El archivo constituye una herramienta completa de análisis de RRHH. Para su despliegue, es necesario asegurar que la ruta del archivo Excel de origen sea accesible o actualizarla a una fuente de datos centralizada (como SharePoint o SQL Server) para evitar errores de conexión en diferentes entornos de usuario.