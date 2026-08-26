# Pobreza e informalidad laboral en el Perú

Proyecto del curso **Data Mining** (Universidad del Pacífico, docente Soledad Espezúa) —
Entrega 1 / Hito Formativo.

## Descripción del proyecto

Este proyecto analiza la relación entre la informalidad laboral y la pobreza monetaria a
nivel distrital en el Perú, integrando microdatos de la Encuesta Nacional de Hogares (ENAHO)
2024 con el Mapa de Pobreza Provincial y Distrital del INEI. El objetivo del hito 1 es
justificar el planteamiento del problema, documentar las fuentes de datos, definir la unidad
de análisis y la clave de integración, y presentar una primera integración y exploración
visual de los datos.

**Problema aplicado:** ¿en qué distritos coexisten niveles altos de informalidad laboral y de
pobreza monetaria, y qué patrones distinguen a esos distritos de aquellos con mejores
indicadores?

**Usuario del análisis:** Ministerio de Trabajo y Promoción del Empleo (MTPE) y gobiernos
locales/regionales interesados en priorizar zonas de intervención.

## Estructura del repositorio

```
├── README.md                     — este archivo
├── notebook/
│   └── Entrega1_Notebook.ipynb   — código fuente documentado (Google Colab / Jupyter)
├── docs/
│   └── Entrega1_Hito_Formativo.docx  — documento de la entrega (problema, fuentes, diagnóstico, plan de integración)
└── data/                         — carpeta donde se colocan los archivos descargados (no se sube a GitHub, ver más abajo)
```

## Fuentes de datos

### 1. ENAHO 2024 — Módulo 500 (Empleo e Ingresos)

- **Institución:** Instituto Nacional de Estadística e Informática (INEI)
- **Enlace:** http://iinei.inei.gob.pe/microdatos/
- **Diccionario de variables:** https://proyectos.inei.gob.pe/iinei/srienaho/Descarga/DocumentosMetodologicos/2024-55/20_Diccionario_2024.pdf
- **Contenido:** microdatos de personas de 14 años a más — condición de ocupación, categoría
  ocupacional, informalidad laboral (`CODINFOR`), horas trabajadas, ingresos por trabajo.
- **Cómo llegar al archivo** (la descarga no es un enlace único y directo):
  1. Entrar al portal de Microdatos del INEI.
  2. Seleccionar **"Consulta por Encuestas"**.
  3. Buscar **"Encuesta Nacional de Hogares - ENAHO"**, periodo **Anual**, año **2024**.
  4. Elegir el módulo **"500 - Empleo e Ingresos"**.
  5. Seleccionar el formato de descarga (CSV, SPSS/SAV, STATA o DBF) y aceptar los términos de uso.
  6. Descargar por separado el diccionario de variables desde la sección de documentación
     metodológica de la misma encuesta.

### 2. Mapa de Pobreza Provincial y Distrital (INEI)

- **Institución:** Instituto Nacional de Estadística e Informática (INEI)
- **Enlace:** https://www.gob.pe/institucion/inei/colecciones/6608-mapa-de-pobreza
- **Publicación de referencia:** https://www.inei.gob.pe/media/MenuRecursivo/publicaciones_digitales/Est/Lib1718/Libro.pdf (edición 2018, la más reciente con desagregación distrital)
- **Contenido:** población proyectada, intervalo de confianza y tasa de incidencia de la
  pobreza monetaria total, por distrito.
- **Cómo llegar al archivo:**
  1. Entrar a la colección "Mapa de Pobreza" en gob.pe/INEI (enlace de arriba).
  2. Seleccionar la edición vigente del Mapa de Pobreza Provincial y Distrital.
  3. Descargar la **"Base de datos"** (Excel / Anexo Estadístico) publicada junto al informe.
  4. El archivo trae filas de cabecera y notas metodológicas antes de la tabla de datos —
     se eliminan al cargarlo (ver notebook, sección 1).

> Nota de vigencia: la última edición distrital publicada por el INEI es de 2018; no existe
> aún una actualización distrital para 2024. Este proyecto combina informalidad laboral 2024
> con el nivel de pobreza distrital más reciente disponible — esta diferencia de años queda
> documentada como limitación en el documento de la entrega.

Los archivos descargados **no se suben al repositorio** (son demasiado pesados y de
distribución restringida por el INEI); cada quien los descarga siguiendo los pasos de arriba
y los coloca en `data/` con los nombres que espera el notebook (ver siguiente sección).

## Cómo ejecutar el código

1. Clonar el repositorio:
   ```
   git clone <URL-de-este-repositorio>
   cd <nombre-del-repositorio>
   ```
2. Descargar ambas fuentes de datos siguiendo las instrucciones de la sección anterior y
   colocarlas dentro de `data/`.
3. Abrir `notebook/Entrega1_Notebook.ipynb` en Google Colab o Jupyter.
4. En la primera celda de configuración, ajustar `RUTA_ENAHO` y `RUTA_POBREZA` con la ruta
   real de los archivos descargados.
5. Ejecutar las celdas en orden. El notebook cubre: carga de datos, inspección inicial,
   validación de claves, revisión de duplicados, integración de fuentes, visualizaciones
   iniciales y conclusiones preliminares.

### Dependencias

```
pandas
numpy
matplotlib
openpyxl   # para leer el Excel del Mapa de Pobreza
```

Instalación rápida:
```
pip install pandas numpy matplotlib openpyxl
```

## Integrante

Daniela — Universidad del Pacífico
