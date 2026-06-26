# Dataset de Gestión de Personas para Power BI: Empleados, Departamentos y Formación

## 📋 Resumen
Este documento describe el contenido y la estructura del archivo `GestionPeople_Dataset_PowerBI.xlsx`, un dataset de práctica diseñado para el análisis de datos de Recursos Humanos. Incluye información detallada sobre empleados, departamentos y registros de formación, preparado para su importación y uso en Power BI Desktop.

## 🔑 Puntos clave
- El dataset contiene tres hojas principales: **Empleados**, **Departamentos** y **RegistroFormacion**.
- Proporciona datos completos para un análisis de Recursos Humanos, incluyendo información demográfica, laboral, de satisfacción, formación y estructura departamental.
- Incluye instrucciones explícitas para la importación y establecimiento de relaciones entre tablas en Power BI Desktop.
- Está diseñado como material de práctica para el módulo IFCT153 de Power BI, centrado en el diseño de informes.

## 📝 Detalle

### Contexto del Dataset
El archivo `GestionPeople_Dataset_PowerBI.xlsx` es un dataset ficticio creado para el módulo IFCT153 "Análisis de Datos con Excel: Power Query, Power Pivot y Power BI" impartido por Daniela Laborda en Izquierdo FP Zaragoza. Simula los datos de una consultoría de RRHH, GestiónPeople S.L., con sede en Zaragoza, y está específicamente diseñado para ejercicios del Capítulo 4 sobre el diseño de informes en Power BI.

### Estructura de las Hojas

El libro de Excel se compone de las siguientes hojas:

#### 1. `Empleados`
Contiene 55 registros de la plantilla, con 17 columnas de datos detallados por empleado.

**Columnas principales:**
| Columna | Descripción |
| :---------------- | :------------------------------------------------------------------------------------------------------------------ |
| `IDEmpleado` | Identificador único del empleado. |
| `NombreCompleto` | Nombre y apellidos del empleado. |
| `Genero` | Género del empleado (Femenino/Masculino). |
| `FechaNacimiento` | Fecha de nacimiento del empleado. |
| `FechaContratacion` | Fecha de inicio del contrato del empleado. |
| `IDDepartamento` | Identificador del departamento al que pertenece el empleado. |
| `Cargo` | Puesto de trabajo del empleado. |
| `TipoContrato` | Tipo de contrato del empleado (Prácticas, Temporal, Indefinido). |
| `Salario` | Salario anual del empleado. |
| `AniosExperiencia` | Años de experiencia laboral. |
| `Ciudad`, `Provincia`, `ComunidadAutonoma`, `Pais` | Ubicación geográfica del empleado. |
| `Satisfaccion` | Nivel de satisfacción del empleado (escala del 1 al 10). |
| `HorasFormacion` | Total de horas de formación acumuladas. |
| `Evaluacion` | Evaluación de desempeño del empleado (Bueno, Mejorable, Excelente, Aceptable). |

#### 2. `Departamentos`
Contiene 8 registros de los departamentos de la empresa, con 5 columnas de datos de referencia departamental.

**Columnas principales:**
| Columna | Descripción |
| :------------------ | :------------------------------------------- |
| `IDDepartamento` | Identificador único del departamento. |
| `NombreDepartamento` | Nombre del departamento. |
| `DirectorDpto` | Nombre del director o directora del departamento. |
| `PresupuestoAnual` | Presupuesto anual asignado al departamento. |
| `NumeroObjetivos` | Número de objetivos anuales definidos para el departamento. |

#### 3. `RegistroFormacion`
Contiene 129 registros de formación correspondientes a 52 empleados, detallando los cursos realizados.

**Columnas principales:**
| Columna | Descripción |
| :---------------- | :---------------------------------------------------------- |
| `IDFormacion` | Identificador único del registro de formación. |
| `IDEmpleado` | Identificador del empleado que realizó el curso. |
| `NombreCurso` | Título del curso de formación. |
| `FechaInicio` | Fecha de inicio del curso. |
| `FechaFin` | Fecha de finalización del curso. |
| `Horas` | Duración del curso en horas. |
| `Modalidad` | Modalidad de impartición del curso (Presencial, Online, Mixta). |
| `Superado` | Indica si el curso fue superado (SÍ/NO). |
| `Coste` | Coste del curso. |

### Relaciones entre Tablas
Para un análisis efectivo en Power BI, se deben establecer las siguientes relaciones entre las tablas:
- `Empleados.IDDepartamento` ──────► `Departamentos.IDDepartamento` (Relación de Muchos a Uno)
- `RegistroFormacion.IDEmpleado` ──► `Empleados.IDEmpleado` (Relación de Muchos a Uno)

### Uso con Power BI Desktop
Para importar y preparar este dataset en Power BI Desktop, siga estos pasos:
1.  **Abrir Power BI Desktop.**
2.  Desde el menú `Inicio`, seleccionar `Obtener datos` y luego `Excel`.
3.  **Seleccionar el archivo** `GestionPeople_Dataset_PowerBI.xlsx`.
4.  En la ventana del Navegador, **marcar las tres tablas**: `Empleados`, `Departamentos` y `RegistroFormacion`.
5.  Hacer clic en `Transformar datos` para revisar la estructura y tipos de datos en Power Query antes de cargar.
6.  Una vez cargadas las tablas en el modelo de datos, ir a la `Vista Modelo` para **crear las relaciones** si Power BI no las ha detectado automáticamente.

### Medidas DAX Complementarias
Para un análisis más avanzado, el dataset está diseñado para ser complementado con medidas DAX. Se menciona la existencia de un archivo `Medidas_DAX_GestionPeople.txt` que contiene 12 medidas DAX listas para usar. Se recomienda crear una tabla de medidas vacía en Power BI Desktop, nombrarla `_Medidas`, y pegar las medidas directamente desde el editor DAX.

## ✅ Conclusiones / siguientes pasos
Este dataset es una herramienta excelente para practicar y desarrollar habilidades en Power BI, permitiendo crear informes y paneles de control sobre la gestión de Recursos Humanos. Una vez importado y relacionadas las tablas, se pueden empezar a construir visualizaciones para analizar aspectos como el rendimiento de los empleados, el seguimiento de la formación, la distribución salarial por cargo y departamento, y la satisfacción laboral.