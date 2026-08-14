# 📘 Manual Técnico: Gestión People

Sistema de Business Intelligence y Analítica de Recursos Humanos implementado sobre la plataforma Microsoft Power BI, estructurado mediante modelos tabulares, pipelines de extracción y transformación en lenguaje M (Power Query) y formulaciones analíticas en DAX (Data Analysis Expressions).

---

## 🏗️ Arquitectura General

El sistema implementa una arquitectura en capas desacopladas orientada a la ingestión, transformación, modelado relacional y visualización interactiva de métricas del capital humano.

```
┌───────────────────────────────────────────────────────────────────────────────────┐
│                      CAPA DE PRESENTACIÓN (Power BI Report)                       │
│  ┌─────────────────────────┬─────────────────────────┬─────────────────────────┐  │
│  │ Resumen & Dashboard     │ Brecha Salarial Género  │ Formación & Satisfacción│  │
│  │ (Cards, Donut, Columns) │ (ComboChart, Pivots)    │ (Scatter, Bubble Chart) │  │
│  ├─────────────────────────┼─────────────────────────┼─────────────────────────┤  │
│  │ Mapa de Talento         │ Tendencias y Desempeño  │ Paneles Departamentales │  │
│  │ (Filled Map, Shape Map) │ (Line Charts, Matrix)   │ (Cross-filtering & UI)  │  │
│  └─────────────────────────┴─────────────────────────┴─────────────────────────┘  │
└────────────────────────────────────────▲──────────────────────────────────────────┘
                                         │ Contexto de Filtro / Consultas DAX
┌────────────────────────────────────────┴──────────────────────────────────────────┐
│                   CAPA DE LÓGICA (Motor Tabular VertiPaq & DAX)                   │
│  ┌─────────────────────────────────────────────────────────────────────────────┐  │
│  │ Esquema en Estrella / Constelación:                                         │  │
│  │ [Departamentos_final] ──(1:N)──> [Empleados_final] ──(1:N)──> [Formación]   │  │
│  ├─────────────────────────────────────────────────────────────────────────────┤  │
│  │ Medidas Analíticas & KPIs:                                                  │  │
│  │ • Brecha Salarial Genero % • % Indefinidos       • Evaluacion Numerica      │  │
│  │ • Horas Ultimo Año         • Color Fondo Tabla   • % Empleados Formados     │  │
│  ├─────────────────────────────────────────────────────────────────────────────┤  │
│  │ Inteligencia Temporal: Tablas auxiliares LocalDateTable_* & DateTableTemplate│  │
│  └─────────────────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────▲──────────────────────────────────────────┘
                                         │ Tablas Tipadas y Limpias
┌────────────────────────────────────────┴──────────────────────────────────────────┐
│              CAPA DE TRANSFORMACIÓN & ETL (Power Query / Lenguaje M)               │
│  ┌─────────────────────────┬─────────────────────────┬─────────────────────────┐  │
│  │ Consulta: Empleados     │ Consulta: Departamentos │ Consulta: RegFormación  │  │
│  │ • Tipado estricto       │ • Normalización claves  │ • Cálculo duración      │  │
│  │ • Deduplicación ID      │ • Filtro integridad ref │ • Limpieza modalidades  │  │
│  └─────────────────────────┴─────────────────────────┴─────────────────────────┘  │
│  └── Parámetro dinámico de origen: #"Path_Excel"                                  │
└────────────────────────────────────────▲──────────────────────────────────────────┘
                                         │ Carga de Hojas de Cálculo
┌────────────────────────────────────────┴──────────────────────────────────────────┐
│                       CAPA DE ORIGEN (Almacén de Datos)                           │
│  └── Archivo Excel: GestionPeople_Dataset_PowerBI.xlsx                            │
│      ├── Hoja: Empleados (55 registros, 17 columnas de atributos)                 │
│      ├── Hoja: Departamentos (8 registros, 5 columnas de estructura)              │
│      └── Hoja: RegistroFormacion (129 registros, 9 columnas históricas)           │
└───────────────────────────────────────────────────────────────────────────────────┘
```

### Flujo de Datos y Ciclo de Vida
1. **Extracción**: El conector nativo de Excel (`Excel.Workbook(File.Contents(Path_Excel))`) lee los libros binarios del origen parametrizado.
2. **Transformación (ETL)**: El motor M normaliza tipos de datos primitivos (`type text`, `Int64.Type`, `Currency.Type`, `type date`), limpia inconsistencias ortográficas y valida claves primarias.
3. **Modelado en Memoria (VertiPaq)**: Se comprimen y almacenan las columnas en memoria columnar, generando relaciones de integridad referencial 1:N entre dimensiones y tablas de hechos.
4. **Cálculo Dinámico**: Las medidas DAX evalúan dinámicamente agregaciones y ratios bajo el contexto de evaluación (Filter Context & Row Context).
5. **Renderizado Visual**: La capa de reporte recibe los resultados calculados y procesa la interacción del usuario mediante segmentación cruzada y formato condicional.

---

## 🧩 Componentes Principales

### 1. Archivo de Plantilla (`GestionPeople_Informe.19062026.pbit`)
- **Ruta**: Raíz del repositorio.
- **Responsabilidad**: Archivo binario empaquetado sin datos cacheados que contiene la metadata del modelo de datos, diseño de reportes visuales, temas corporativos, medidas DAX y scripts Power Query M.
- **Parámetros Exportados**: `Path_Excel` (parámetro de tipo texto para configurar la ruta de origen en el despliegue).

### 2. Dataset Fuente (`GestionPeople_Dataset_PowerBI.xlsx`)
- **Ruta**: Raíz del repositorio (`docs/GestionPeople_Dataset_PowerBI.md`).
- **Responsabilidad**: Base transaccional de pruebas de Recursos Humanos para la empresa GestiónPeople S.L.

#### Esquema de Tablas Fuente
| Tabla | Filas | Columnas Clave | Tipos de Datos Principales |
| :--- | :--- | :--- | :--- |
| **`Empleados`** | 55 | `IDEmpleado` (PK), `IDDepartamento` (FK), `Salario`, `Genero`, `FechaContratacion`, `Evaluacion` | `Text`, `Int64`, `Currency`, `Date` |
| **`Departamentos`** | 8 | `IDDepartamento` (PK), `NombreDepartamento`, `DirectorDpto`, `PresupuestoAnual`, `NumeroObjetivos` | `Text`, `Int64`, `Currency` |
| **`RegistroFormacion`**| 129 | `IDFormacion` (PK), `IDEmpleado` (FK), `NombreCurso`, `Horas`, `Coste`, `Superado` | `Text`, `Int64`, `Currency`, `Date` |

---

### 3. Pipeline ETL (Lenguaje M / Power Query)

```powerquery
// Consulta: Empleados_final
let
    Origen = Excel.Workbook(File.Contents(Path_Excel), null, true),
    Empleados_Sheet = Origen{[Item="Empleados",Kind="Sheet"]}[Data],
    EncabezadosPromovidos = Table.PromoteHeaders(Empleados_Sheet, [PromoteAllScalars=true]),
    TipoCambiado = Table.TransformColumnTypes(EncabezadosPromovidos,{
        {"IDEmpleado", type text},
        {"NombreCompleto", type text},
        {"Genero", type text},
        {"FechaNacimiento", type date},
        {"FechaContratacion", type date},
        {"IDDepartamento", type text},
        {"Cargo", type text},
        {"TipoContrato", type text},
        {"Salario", Currency.Type},
        {"AniosExperiencia", Int64.Type},
        {"Ciudad", type text},
        {"Provincia", type text},
        {"ComunidadAutonoma", type text},
        {"Pais", type text},
        {"Satisfaccion", Int64.Type},
        {"HorasFormacion", Int64.Type},
        {"Evaluacion", type text}
    })
in
    TipoCambiado
```

- **`Departamentos_final`**: Procesa la entidad departamental, tipando `PresupuestoAnual` como moneda y validando el identificador alfanumérico `IDDepartamento`.
- **`Registro formacion_final`**: Estandariza fechas de inicio y fin, convierte el campo `Horas` a entero no negativo y asegura que `Coste` mantenga precisión decimal de moneda.

---

### 4. Lógica de Negocio y Medidas DAX

```dax
// 1. Brecha Salarial de Género Porcentual
Brecha Salarial Genero % = 
VAR SalarioHombres = CALCULATE(AVERAGE(Empleados_final[Salario]), Empleados_final[Genero] = "Masculino")
VAR SalarioMujeres = CALCULATE(AVERAGE(Empleados_final[Salario]), Empleados_final[Genero] = "Femenino")
RETURN
    DIVIDE(SalarioHombres - SalarioMujeres, SalarioHombres, 0)

// 2. Proporción de Contratos Indefinidos
% Indefinidos = 
DIVIDE(
    CALCULATE(COUNTROWS(Empleados_final), Empleados_final[TipoContrato] = "Indefinido"),
    COUNTROWS(Empleados_final),
    0
)

// 3. Mapeo de Evaluación Cualitativa a Cuantitativa
Evaluacion Numerica = 
SWITCH(
    SELECTEDVALUE(Empleados_final[Evaluacion]),
    "Excelente", 4,
    "Bueno", 3,
    "Aceptable", 2,
    "Mejorable", 1,
    0
)

// 4. Agregación de Formación en Ventana Móvil
Horas Ultimo Año = 
CALCULATE(
    SUM('Registro formacion_final'[Horas]),
    DATESINPERIOD('Registro formacion_final'[FechaFin], TODAY(), -1, YEAR)
)

// 5. Formato Condicional Dinámico (Semáforo de Desempeño)
Color Fondo Tabla = 
IF(
    SELECTEDVALUE(Empleados_final[HorasFormacion]) >= 40 && 
    SELECTEDVALUE(Empleados_final[Evaluacion]) = "Excelente",
    "#92D050", // Verde Corporativo
    BLANK()
)
```

---

## 🔌 APIs y Consultas de Entrada

Al ser un modelo semántico hospedado sobre el motor VertiPaq en Power BI Desktop/Service, las interfaces de comunicación operan mediante conectores M y expresiones de consulta DAX:

| Método | Ruta / Entrada | Descripción | Parámetros / Esquema |
| :--- | :--- | :--- | :--- |
| `M: File.Contents` | `Path_Excel` | Ingestión binaria del libro estructurado de Excel | `Path_Excel: Text` (Ruta absoluta local/UNC/SharePoint) |
| `M: Excel.Workbook` | `Empleados` | Extracción de la matriz transaccional de personal | `UseHeaders: true`, `InferSheetDimensions: true` |
| `M: Excel.Workbook` | `Departamentos` | Extracción del maestro de departamentos | `UseHeaders: true`, `InferSheetDimensions: true` |
| `M: Excel.Workbook` | `RegistroFormacion` | Extracción del histórico de capacitaciones | `UseHeaders: true`, `InferSheetDimensions: true` |
| `DAX: EVALUATE` | Tablas de Hechos | Evaluación analítica bajo contexto de filtro visual | Contexto de fila, filtros cruzados, jerarquías de fecha |

---

## ⚙️ Variables de Entorno y Parámetros

| Variable / Parámetro | Valor de Ejemplo | Obligatoria | Descripción |
| :--- | :--- | :--- | :--- |
| `Path_Excel` | `C:\Datos\GestionPeople_Dataset_PowerBI.xlsx` | **Sí** | Ruta física o URI del archivo Excel fuente que consume Power Query. |
| `IdiomaInforme` | `es-ES` | No | Configuración regional para la interpretación de separadores decimales `,` y fechas `DD/MM/YYYY`. |
| `FechaRef` | `2026-08-14` / `TODAY()` | No | Parámetro o función base para cálculos de antigüedad e inteligencia temporal. |
| `DirectQueryMode` | `Import (Default)` | No | Modo de almacenamiento del motor de datos (fijado en Importación en memoria). |

---

## 🚀 Guía de Despliegue Paso a Paso

### 1. Requisitos Previos
- **Power BI Desktop**: Versión actualizada (2024 o superior / compilación vigente en 2026).
- **Sistema Operativo**: Windows 10/11 x64 o Windows Server 2019/2022.
- **Acceso a Datos**: Permisos de lectura en el sistema de archivos donde resida `GestionPeople_Dataset_PowerBI.xlsx`.

### 2. Despliegue Local (Desarrollo y Auditoría)
1. **Clonar el repositorio**:
   ```bash
   git clone https://github.com/migueljerico/powerbi-gestion-people.git
   cd powerbi-gestion-people
   ```
2. **Ubicación del dataset**:
   Copie el archivo `GestionPeople_Dataset_PowerBI.xlsx` en una ruta estandarizada (ej. `C:\Datos\GestionPeople_Dataset_PowerBI.xlsx`).
3. **Inicialización de la plantilla**:
   - Abra el archivo `GestionPeople_Informe.19062026.pbit`.
   - En la ventana modal emergente **Parámetros**, introduzca la ruta completa al archivo Excel en el campo `Path_Excel`.
   - Presione **Cargar**.
4. **Validación del Modelo**:
   - Vaya a la vista de **Modelo** y compruebe que las relaciones están activas:
     - `Departamentos_final[IDDepartamento]` (1) ──> `Empleados_final[IDDepartamento]` (N)
     - `Empleados_final[IDEmpleado]` (1) ──> `Registro formacion_final[IDEmpleado]` (N)
   - Guarde el archivo con extensión `.pbix` (`GestionPeople_Produccion.pbix`).

### 3. Publicación en Power BI Service (Producción)
1. **Publicar el Reporte**:
   - En la pestaña *Inicio* de Power BI Desktop, haga clic en **Publicar**.
   - Seleccione el Área de Trabajo corporativa de destino (Workspace).
2. **Configuración de Conectividad (Gateway)**:
   - Si el archivo Excel reside en una unidad de red local, configure un **On-Premises Data Gateway** estándar.
   - En `Configuración del Conjunto de Datos` -> `Credenciales de origen de datos`, valide los accesos del Gateway con permisos de lectura.
3. **Programación de Actualizaciones**:
   - Active la **Actualización programada** (diaria/semanal según el flujo de RRHH).

---

## ⚠️ Limitaciones Conocidas y Posibles Mejoras Futuras

### Limitaciones Actuales
- **Conexión a Archivo Estático**: El origen de datos basado en Excel genera riesgo de bloqueo de archivos por concurrencia y no soporta inserción masiva en tiempo real.
- **Seguridad a Nivel de Fila (RLS)**: El modelo actual carece de roles RLS (`Row Level Security`), lo que expone salarios y evaluaciones a cualquier usuario que consuma el reporte.
- **Acoplamiento de Esquema**: Modificaciones en los encabezados o nombres de columna del Excel provocarán fallos de evaluación en el paso `Table.TransformColumnTypes` de Power Query.

### Mejoras Recomendadas
1. **Migración a Base de Datos Centralizada**:
   Migrar el origen a Azure SQL Database, PostgreSQL o SharePoint Lists corporativo para soporte multiusuario transaccional.
2. **Implementación de RLS Basado en Jerarquías**:
   Definir roles de seguridad DAX como:
   ```dax
   [IDDepartamento] = LOOKUPVALUE(UsuariosDpto[IDDepartamento], UsuariosDpto[Email], USERPRINCIPALNAME())
   ```
3. **Optimización DAX con Variables y Tablas Desconectadas**:
   Refactorizar medidas complejas para almacenar contextos intermedios mediante bloques `VAR / RETURN`, reduciendo el escaneo de memoria en el motor VertiPaq.
4. **Implementación de Calendario Dedicado**:
   Sustituir las `LocalDateTable` automáticas por una tabla `Dim_Calendario` centralizada generada con `CALENDARAUTO()`, optimizando el consumo de RAM.

<p align="center">Creado por <a href="https://github.com/migueljerico">@migueljerico</a> y documentado por Google Gemini (gemini-3.7-flash) desde la App Asistente de IA · 2026</p>