# Documentación Técnica: GestionPeople_Informe

## 📋 Resumen
Este documento detalla la estructura del archivo de plantilla de Power BI (`.pbit`) diseñado para la gestión de personas. El informe integra datos de empleados, departamentos y registros de formación para analizar el rendimiento, la brecha salarial, la distribución geográfica y el desarrollo profesional del capital humano.

## 🔑 Puntos clave
- **Análisis Multidimensional**: Incluye vistas ejecutivas, análisis salariales, mapas de talento y seguimiento de formación.
- **Cálculos Avanzados**: Implementa medidas DAX para calcular brechas salariales de género, porcentajes de contratación indefinida y evaluaciones numéricas.
- **Origen de Datos**: Los datos provienen de un archivo Excel externo procesado mediante Power Query.
- **Visualización**: Utiliza una amplia gama de visuales, desde tarjetas de KPI y mapas hasta gráficos de dispersión y tablas dinámicas.

## 📝 Detalle

### 1. Estructura del Informe (Páginas y Visuales)
El informe se organiza en diversas páginas especializadas:

| Página | Visuales Principales | Propósito |
| :--- | :--- | :--- |
| **Resumen ejecutivo** | 12 visuales (Cards, Donut, ColumnChart, Slicers) | Vista general de KPIs principales. |
| **Dashboard ejecutivo** | 10 visuales (Cards, Slicers, ActionButton) | Panel de control operativo. |
| **Análisis Brecha Salarial de Género** | 5 visuales (ComboChart, PivotTable, ScatterChart) | Análisis de equidad salarial. |
| **Informe de Formación y Desarrollo** | 9 visuales (Cards, ColumnChart, ScatterChart, Donut) | Seguimiento de capacitación. |
| **Mapa de Talento** | 4 visuales (Map, FilledMap, Table) | Distribución y ubicación del talento. |
| **Distribución geográfica** | 4 visuales (Map, FilledMap, Slicers) | Análisis espacial de la plantilla. |
| **Tendencias y correlaciones** | 3 visuales (LineChart, ScatterChart) | Análisis de comportamiento de datos. |
| **Formación y Satisfacción** | 4 visuales (ScatterChart, ColumnChart, Table) | Relación entre capacitación y satisfacción. |
| **Análisis salarial / Visual** | 8 visuales (ColumnChart, BarChart, LineChart, PivotTable) | Desglose de compensaciones. |
| **Otras páginas** | Tablas y gráficos simples | Ejercicios 1 y 2, Drill down y Cuadro de mando personal. |

### 2. Modelo de Datos
El modelo se compone de las siguientes tablas principales:

#### Tabla `Empleados_final`
Es la tabla central con 18 columnas (datos demográficos, contractuales y de rendimiento) y 9 medidas DAX clave:
- **% Indefinidos**: Proporción de empleados con contrato indefinido.
- **Brecha Salarial Genero %**: Diferencia porcentual entre el salario promedio de mujeres y hombres.
- **Brecha Salarial Auditoria %**: Cálculo alternativo de la brecha salarial.
- **Evaluacion Numerica**: Convierte categorías ("Excelente", "Bueno", etc.) en valores numéricos (4 a 0).
- **% Evaluacion Excelente/Bueno**: Porcentaje de empleados con alto rendimiento.
- **Horas Ultimo Año**: Suma de horas de formación en los últimos 12 meses.
- **Color Fondo Tabla**: Formato condicional (Verde claro) para empleados con >40 horas de formación y evaluación "Excelente".

#### Tablas Relacionadas
- **Departamentos_final**: Información de departamentos (Director, Presupuesto, Objetivos).
- **Registro formacion_final**: Detalle de cursos, fechas, horas, modalidad y costes.
- **Tablas de Fecha**: Múltiples tablas `LocalDateTable` y `DateTableTemplate` para inteligencia de tiempo.

### 3. Consultas (Power Query / M)
El proceso de extracción y transformación de datos (ETL) se realiza mediante tres consultas que importan datos desde un archivo Excel:
1. **Empleados_final**: Importa la `Tabla1` y define tipos de datos (int64, string, dateTime, double).
2. **Departamentos_final**: Importa la `Tabla2` con datos presupuestarios y de gestión.
3. **Registro formacion_final**: Importa la `Tabla3` con el historial de capacitación.

## ✅ Conclusiones / siguientes pasos
El archivo constituye una herramienta completa de análisis de RRHH. Para su despliegue, es necesario asegurar que la ruta del archivo Excel (`C:\Users\Usuario\Desktop\...`) sea actualizada o parametrizada para que otros usuarios puedan cargar los datos correctamente.
