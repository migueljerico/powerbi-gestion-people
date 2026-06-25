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
- **Tablas**: `Empleados_final`, `Departamentos_final`, `Registro formacion_final`.
- **Relaciones**: Relaciones de uno a muchos (1:*) entre `Departamentos_final` $\rightarrow$ `Empleados_final`.

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

## ⚙️ Variables de Entorno

| Variable | Valor de ejemplo | Obligatoria | Descripción |
| :--- | :--- | :--- | :--- |
| `Path_Excel` | `C:\Datos\Empleados.xlsx` | Sí | Ruta local al archivo de origen de datos. |

## 🚀 Guía de Despliegue

1. **Importación**: Abrir el archivo `.pbit` en Power BI Desktop.
2. **Mapeo**: Vincular las columnas del Excel con los campos definidos en Power Query.
3. **Validación**: Verificar que las medidas DAX calculen correctamente los KPIs en la página de Resumen.
4. **Publicación**: Subir el informe al *Power BI Service* (Workspace) para acceso web y configuración de actualización programada.

## ⚠️ Limitaciones y Mejoras
- **Limitación**: Dependencia de un archivo Excel local; cualquier cambio en la estructura de columnas del Excel romperá el ETL.
- **Mejora**: Migrar el origen de datos a una base de datos SQL Server para mayor escalabilidad y seguridad.
- **Mejora**: Implementar RLS (Row Level Security) para restringir la vista de salarios según el rol del usuario.