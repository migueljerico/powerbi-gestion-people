# 📘 Manual Técnico: Gestión People

## 🏗️ Arquitectura General
El flujo de datos sigue un modelo lineal de procesamiento de inteligencia de negocios:

`Origen (Excel)` $\rightarrow$ `ETL (Power Query)` $\rightarrow$ `Modelo de Datos (DAX)` $\rightarrow$ `Capa de Visualización (Power BI)`

### Flujo de Datos:
- **Capa de Datos**: Importación de tablas de Empleados, Departamentos y Formación.
- **Capa de Lógica**: Aplicación de transformaciones (limpieza de nulos, tipado de datos) y cálculo de medidas DAX.
- **Capa de Presentación**: Distribución de la información en 8 páginas especializadas.

## 🧩 Componentes Principales

### 1. Modelo de Datos
- **Tablas**: Empleados, Formación, Departamentos.
- **Relaciones**: Relaciones de uno a muchos (1:*) entre Departamentos $\rightarrow$ Empleados.

### 2. Medidas DAX Clave
- **Brecha Salarial**: Cálculo de la diferencia porcentual de salario promedio entre géneros.
- **Tasa de Contratación**: Porcentaje de empleados con contrato indefinido vs. temporales.
- **Evaluación Numérica**: Promedio de desempeño basado en registros de formación.

## 📊 Endpoints y Visuales

| Página | Visual Clave | Propósito |
| :--- | :--- | :--- |
| Resumen | Cards / Donut | KPIs de conteo de empleados y distribución. |
| Brecha Salarial | ComboChart | Comparativa Salario vs Género. |
| Mapa de Talento | FilledMap | Densidad de talento por región. |
| Formación | ScatterChart | Correlación entre horas de formación y satisfacción. |

## ⚙️ Configuración y Despliegue

### Variables de Entorno / Parámetros
| Parámetro | Valor Ejemplo | Obligatoria | Descripción |
| :--- | :--- | :--- | :--- |
| `Path_Excel` | `C:\Datos\Empleados.xlsx` | Sí | Ruta local al archivo de origen de datos. |

### Pasos de Despliegue
1. **Carga**: Importar el archivo `.pbit` en Power BI Desktop.
2. **Mapeo**: Vincular las columnas del Excel con los campos definidos en Power Query.
3. **Publicación**: Subir el informe al *Power BI Service* (Workspace) para acceso web.
4. **Programación**: Configurar el *Gateway* de datos para actualizaciones automáticas del Excel.

## ⚠️ Limitaciones y Mejoras
- **Limitación**: Dependencia de un archivo plano (Excel), lo que limita la concurrencia de datos en tiempo real.
- **Mejora Futura**: Migración de la fuente de datos a SQL Server para mayor escalabilidad.
- **Mejora Futura**: Implementación de R o Python para análisis predictivo de rotación de personal (Churn Rate).