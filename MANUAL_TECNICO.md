# 📘 Manual Técnico: Gestión People

## 🏗️ Arquitectura General

El sistema sigue un flujo de procesamiento de Inteligencia de Negocios (BI) de cuatro capas, típico del ecosistema Microsoft Power Platform. La arquitectura es monolítica a nivel de cliente (Power BI Desktop) y se distribuye mediante plantillas `.pbit` para consumo en servicio.

```
┌─────────────────────────────────────────────────────────────────┐
│  CAPA DE PRESENTACIÓN (Power BI Report / Service)              │
│  └── 8 Páginas especializadas, Segmentadores, Botones de acción │
├─────────────────────────────────────────────────────────────────┤
│  CAPA DE LÓGICA (Motor DAX & Modelo Relacional)                │
│  └── Medidas calculadas, Inteligencia de tiempo, Filtros       │
├─────────────────────────────────────────────────────────────────┤
│  CAPA DE TRANSFORMACIÓN (Power Query / Lenguaje M)             │
│  └── Limpieza, tipado estricto, fusión de tablas, parámetros   │
├─────────────────────────────────────────────────────────────────┤
│  CAPA DE ORIGEN (Archivo Excel .xlsx)                          │
│  └── Hojas: Empleados, Departamentos, RegistroFormacion         │
└─────────────────────────────────────────────────────────────────┘
Flujo de ejecución: Origen → ETL → Modelo → Visualización
```

### Flujo de Datos Detallado
- **Capa de Datos**: Importación directa de tres hojas planas desde un archivo Excel externo.
- **Capa de Lógica**: Aplicación de transformaciones tipográficas (`Table.TransformColumnTypes`), establecimiento de relaciones estrella y cálculo de KPIs mediante expresiones DAX.
- **Capa de Presentación**: Renderizado interactivo en 8 páginas con segmentadores cruzados, mapas geoespaciales y gráficos de correlación.

---

## 🧩 Componentes Principales

### 1. Plantilla Principal (`GestionPeople_Informe.19062026.pbit`)
- **Responsabilidad**: Archivo contenedor que almacena el modelo de datos, consultas M, medidas DAX y diseño de informes. Actúa como plantilla reutilizable.
- **Funciones Clave**: Inicialización dinámica del parámetro `Path_Excel`, carga diferida de hojas, aplicación de temas corporativos y configuración de navegación entre páginas.

### 2. Consultas Power Query (ETL)
- **`Empleados_final`**: Importa y limpia la tabla maestra de personal. Aplica conversión de tipos (Texto, Fecha, Decimal, Entero) y elimina registros duplicados por `IDEmpleado`.
- **`Departamentos_final`**: Carga la estructura organizativa. Filtra nulos en `DirectorDpto` y asegura integridad referencial con `IDDepartamento`.
- **`Registro formacion_final`**: Procesa el histórico de cursos. Calcula duración efectiva, filtra modalidades inválidas y prepara campos para agregación temporal.

### 3. Modelo de Datos y Relaciones
- **Tablas**: `Empleados_final` (hecho central), `Departamentos_final` (dimensión), `Registro formacion_final` (dimensión/histo).
- **Relaciones**: 
  - `Departamentos_final[IDDepartamento]` → `Empleados_final[IDDepartamento]` (1:N)
  - `Empleados_final[IDEmpleado]` → `Registro formacion_final[IDEmpleado]` (1:N)
- **Tablas Auxiliares**: `LocalDateTable` y `DateTableTemplate` para inteligencia de tiempo (YTD, YoY, últimos 12 meses).

### 4. Medidas DAX Exportadas
- **`Brecha Salarial Genero %`**: Calcula la diferencia porcentual del salario promedio entre géneros.
- **`% Indefinidos`**: Proporción de contratos indefinidos sobre el total de la plantilla.
- **`Evaluacion Numerica`**: Mapeo condicional de categorías cualitativas a escala numérica (1-4) para agregación matemática.
- **`Horas Ultimo Año`**: Suma acumulada de horas de formación filtradas por ventana temporal relativa (`TODAY()`).
- **`Color Fondo Tabla`**: Medida de formato condicional que asigna `#92D050` cuando `HorasFormacion > 40` y `Evaluacion = "Excelente"`.

---

## 🔌 APIs y Endpoints (Consultas de Entrada)

*Nota: Al tratarse de una aplicación de escritorio local (Power BI Desktop), no se exponen endpoints HTTP REST. Se documentan las consultas de entrada bajo la estructura solicitada para mantener la trazabilidad del flujo de datos.*

| Método | Ruta | Descripción | Parámetros |
| :--- | :--- | :--- | :--- |
| `IMPORTAR` | `Path_Excel` | Carga inicial de hojas desde archivo local | `HojaOrigen` (String), `TiposColumna` (Record), `FiltroNulos` (Boolean) |
| `TRANSFORMAR` | `Query_M` | Aplicación de pipeline ETL en memoria | `TablaFuente`, `Operaciones` (Lista de funciones M) |
| `CALCULAR` | `Measure_DAX` | Evaluación de KPIs en contexto de filtro | `TablaContexto`, `FiltrosCruzados`, `GranularidadTemporal` |

---

## ⚙️ Variables de Entorno

| Variable | Valor de ejemplo | Obligatoria | Descripción |
| :--- | :--- | :--- | :--- |
| `Path_Excel` | `C:\Datos\GestionPeople_Dataset_PowerBI.xlsx` | Sí | Ruta absoluta al archivo de origen. Debe ser accesible por el usuario o Gateway. |
| `FechaRef` | `HOY()` | No | Fecha base para cálculos temporales dinámicos (ej. `Horas Ultimo Año`). |
| `IdiomaInforme` | `es-ES` | No | Configuración regional para formatos de moneda, fechas y nombres de medidas. |
| `ModoDebug` | `FALSE` | No | Habilita mensajes de error detallados en Power Query durante desarrollo. |

---

## 🚀 Guía de Despliegue Paso a Paso

### 1. Prerrequisitos
- **Software**: Power BI Desktop (versión estable recomendada ≥ 2024).
- **Datos**: Archivo Excel `GestionPeople_Dataset_PowerBI.xlsx` con estructura intacta (17 columnas en Empleados, 5 en Departamentos, 9 en RegistroFormacion).
- **Permisos**: Lectura/Escritura en la ruta definida por `Path_Excel`.

### 2. Instalación y Configuración Local
1. Descargar `GestionPeople_Informe.19062026.pbit` desde el repositorio.
2. Abrir el archivo con doble clic o arrastrándolo a Power BI Desktop.
3. En el cuadro de diálogo **Configuración de parámetros**, ingresar la ruta completa al archivo Excel en `Path_Excel`.
4. Hacer clic en **Cargar**. Power Query ejecutará las transformaciones M y validará los tipos de datos.
5. Verificar en el panel **Modelo** que las relaciones 1:N estén activas y sin cruces de filtro no deseados.

### 3. Publicación y Distribución
1. Hacer clic en **Publicar** → Seleccionar Workspace de destino en Power BI Service.
2. Configurar **Gateway de datos** (On-premises o Cloud) si el archivo Excel reside en una red local o servidor compartido.
3. Programar actualización automática: `Configuración del conjunto de datos` → `Programar actualización` (recomendado: diario o semanal).
4. Compartir el informe mediante enlaces directos o integración en Teams/SharePoint.

---

## ⚠️ Limitaciones Conocidas y Posibles Mejoras Futuras

### Limitaciones Actuales
- **Dependencia de Ruta Estática**: El parámetro `Path_Excel` requiere acceso directo al disco local. Cambios de directorio o migración a entornos multiusuario romperán el ETL sin intervención manual.
- **Ausencia de Seguridad Granular**: No se implementa Row Level Security (RLS). Todos los usuarios con acceso ven los salarios y evaluaciones completas.
- **Rendimiento en Escala**: Las medidas DAX evalúan contexto completo sin optimización por variables (`VAR`), lo que puede degradar el rendimiento con >50k registros.
- **Manejo de Errores**: Falta validación explícita de esquemas; una columna adicional o renombrada en el Excel generará fallos silenciosos o abortará la carga.

### Hoja de Ruta y Mejoras Recomendadas
1. **Migración a Fuente Centralizada**: Reemplazar Excel por SQL Server o Azure Synapse. Utilizar Power BI Gateway para sincronización segura y soportar concurrencia.
2. **Implementación de RLS Dinámico**: Crear roles basados en `Departamento` o `Cargo` para restringir visibilidad salarial según jerarquía organizativa.
3. **Optimización DAX**: Reescribir medidas críticas usando `VAR` para caching intermedio, y aplicar `TREATAS` o `USERELATIONSHIP` para evitar ambigüedades en tablas de fecha.
4. **Automatización de Validación**: Incorporar pasos de `try...catch` en Power Query para registrar errores de tipo o estructura en una tabla de log interna antes de cargar el modelo.
5. **Internacionalización**: Externalizar cadenas de UI y formatos numéricos a archivos JSON/CSV para facilitar despliegues multilingüe sin recompilar el informe.