# Pobreza monetaria e informalidad laboral en el Perú

Integración de la **ENAHO 2024 — Módulo 500 (Empleo e Ingresos)** con el **Mapa de Pobreza
Monetaria Distrital 2018**, ambas fuentes del INEI, para construir una matriz analítica a nivel
distrital y segmentar los distritos según la estructura de su empleo.

**Curso:** Data Mining · Universidad del Pacífico · **Integrantes:** Thiago Ormeño y Daniela Eléspuru
**Estado:** Hito 3 — revisión metodológica e implementación inicial (clustering de distritos)

---

## Problema y usuario

Los programas de formalización laboral y de reducción de pobreza se focalizan territorialmente
usando, casi siempre, una sola fuente. El proyecto cruza la dimensión laboral (ENAHO) con la
dimensión de pobreza (Mapa de Pobreza) para describir qué tipo de informalidad tiene cada distrito,
y no solo cuánta.

El usuario del análisis es el **Ministerio de Trabajo y Promoción del Empleo (MTPE)**, en particular
las direcciones regionales de trabajo y las áreas de formalización laboral.

**Pregunta de análisis.** ¿Qué perfiles laborales distintos existen entre los distritos del Perú y
se asocian con distintos niveles de pobreza monetaria?

## Unidad de análisis

**Un distrito del Perú**, identificado por su `UBIGEO` (texto de 6 dígitos = departamento +
provincia + distrito).

| Fuente | Fila original | Registros | Tratamiento |
|---|---|---|---|
| ENAHO 2024 (Mód. 500) | una persona | 85 992 | Se filtra a población ocupada y se agrega por `UBIGEO` con promedios ponderados por `FAC500A` |
| Mapa de Pobreza 2018 | un distrito | 1 873 | Se usa directamente |

La base integrada resultante tiene **1 270 distritos** (inner join sobre `UBIGEO`).

## Estructura del repositorio

```
.
├── README.md
├── requirements.txt
├── DB/                                         # fuentes originales (no versionadas, ver abajo)
│   ├── 2024Empleo e Ingresos.csv
│   └── Anexo Estadístico.xlsx
├── Notebook_Hito2_Matriz_Analitica.ipynb       # Hito 2: limpieza, integración y matriz analítica
├── Notebook_Hito3_Metodologia.ipynb            # ★ CUADERNO VIGENTE (Hito 3): clustering
├── Notebook_Pobreza_Informalidad.ipynb         # Hito 1 — superado, se conserva como registro
├── Visualizaciones_Pobreza_Informalidad.ipynb  # Hito 1 — superado, se conserva como registro
├── Entrega_PCA_NMF/                            # actividad previa: PCA y NMF (cuaderno + notas)
├── Entrega_KMeans/                             # actividad previa: K-means (cuaderno + notas)
├── Entrega_DBSCAN_Jerarquico/                  # actividad previa: DBSCAN y jerárquico (cuaderno + notas)
├── Entregas/                                   # documentos y presentaciones entregados
└── output/                                     # todo lo que generan los cuadernos
    ├── matriz_analitica_distrital.csv          # matriz analítica del Hito 2 (1,270 × 47)
    ├── diccionario_matriz_analitica.csv
    ├── resultados_hito3.csv                    # matriz + etiquetas de clustering del Hito 3
    ├── tablas_h3/                              # t01…t11: tablas que alimentan el informe
    └── figuras/                                # 01–16 (Hitos 2 y anteriores) y h3_01–h3_06 (Hito 3)
```

## Cómo ejecutar

```bash
git clone https://github.com/delespurum/DataMining.git
cd DataMining
pip install -r requirements.txt
jupyter lab
```

Los cuadernos se ejecutan **de arriba abajo y en este orden**:

1. `Notebook_Hito2_Matriz_Analitica.ipynb` (≈ 20–30 s; requiere `DB/`): genera
   `output/matriz_analitica_distrital.csv`.
2. `Notebook_Hito3_Metodologia.ipynb` (≈ 15 s; solo requiere la matriz del paso 1): genera
   `output/resultados_hito3.csv`, `output/tablas_h3/` y `output/figuras/h3_*.png`.

Todos los resultados aleatorios usan la semilla `SEMILLA = 42`. En Google Colab, subir la carpeta
`DB/` (y `output/` si se omite el paso 1) y ajustar únicamente las constantes de ruta de la celda de
configuración.

## Obtener los datos

Los microdatos no se versionan (el CSV de la ENAHO pesa ~260 MB). Hay que descargarlos y dejarlos en
`DB/` con estos nombres exactos:

**1. ENAHO 2024 — Módulo 500 (Empleo e Ingresos)** → `DB/2024Empleo e Ingresos.csv`
<http://webinei.inei.gob.pe/anda_inei/index.php/catalog/816/get_microdata>
Seleccionar el año 2024, el módulo *Empleo e Ingresos (500)* y descargar el archivo en formato CSV.
Codificación `latin-1`, 1 425 columnas, una fila por persona.

**Documentación de referencia — Diccionario de Datos de la ENAHO 2024** (INEI, mayo 2025)
Define nombre, etiqueta, formato y rango de valores de las 1,425 variables del archivo
`ENAHO01A-2024-500`. Se descarga junto con los microdatos desde el mismo catálogo ANDA. Todas las
variables usadas están verificadas contra él.

**2. Mapa de Pobreza Monetaria Provincial y Distrital 2018 — Anexo Estadístico** → `DB/Anexo Estadístico.xlsx`
<https://www.gob.pe/institucion/inei/informes-publicaciones/3204872-mapa-de-pobreza-provincial-y-distrital-2018>
Descargar el Anexo Estadístico (`.xlsx`) que acompaña a la publicación y usar el Anexo N° 1.

> **Nota metodológica.** Hay 6 años de desfase entre fuentes: el INEI no ha publicado una
> actualización distrital del mapa de pobreza posterior a 2018. Se asume que la pobreza estructural
> distrital cambia lentamente. Esta limitación está declarada en los documentos de entrega.

---

## Hito 3 — Revisión metodológica e implementación inicial

**Técnica seleccionada: K-means con K = 3** sobre 11 variables laborales estandarizadas
(`StandardScaler`, mediana imputada en 67 ingresos faltantes). La pobreza (`pobreza_pct`) **no** entra
al algoritmo: se usa solo como criterio externo de validación.

**Qué se comparó** (`Notebook_Hito3_Metodologia.ipynb`):

| Experimento | Alternativas evaluadas |
|---|---|
| A. Representación | 11 variables · PCA-2 · PCA-5 · NMF-3, cada una con K = 2…5 |
| B. Elección de K | K-means con K = 2…6 |
| C. Otras técnicas | Ward (K = 2…6) y enlaces complete/average/single; DBSCAN (2 representaciones × 3 `min_samples` × 6 `eps`) |
| Sensibilidad | Semillas, distritos con n < 10, ingresos imputados, escaladores, PCA/NMF, Ward |

**Criterios de evaluación:** Silhouette, Davies-Bouldin, Calinski-Harabasz, inercia, ARI por
bootstrap (30 submuestras del 80 %), tamaño mínimo de grupo y ε² de Kruskal-Wallis de la pobreza
según el grupo (criterio externo). Todas las particiones se evalúan en el mismo espacio (las 11
variables estandarizadas) para que sean comparables.

**Primeros resultados (preliminares):**

| Grupo | Distritos | Informalidad | Ingreso mensual | Rural | Pobreza 2018 (mediana) |
|---|---|---|---|---|---|
| 1. Urbano asalariado | 343 | 60.3 % | S/ 1,854 | 13.2 % | 15.1 % |
| 2. Mixto de transición | 430 | 84.9 % | S/ 1,324 | 55.1 % | 31.0 % |
| 3. Rural agropecuario | 497 | 97.7 % | S/ 855 | 93.4 % | 44.3 % |

- K-means K = 3 es la configuración más estable (ARI bootstrap 0.953) y la más asociada con la
  pobreza (ε² = 0.398). Ward K = 3 es inestable (0.546) y DBSCAN no encuentra estructura de densidad.
- PCA y NMF no mejoran la partición de las 11 variables.
- **Limitaciones:** la silhouette es baja (0.17): los distritos forman un gradiente continuo más que
  grupos naturales; los criterios internos prefieren K = 2; la partición es sensible al escalamiento;
  hay distritos con muestra pequeña; la pobreza es de 2018 y el empleo de 2024.
- **Pendiente para el Hito 4:** evaluar K = 2 y K = 4, mezclas gaussianas y K-medoids, ponderar por
  tamaño de muestra, contrastar el desfase 2018–2024, mapa distrital, discusión y conclusiones.

## Hito 2 — Preparación de datos y matriz analítica

`Notebook_Hito2_Matriz_Analitica.ipynb`:

1. **Carga e inspección** de ambas fuentes (`usecols` sobre 26 de 1 425 columnas de la ENAHO).
2. **Diagnóstico de calidad** — duplicados, faltantes, valores fuera de rango, consistencia de códigos.
3. **Tratamiento** y construcción de la informalidad con la definición operativa del INEI,
   validada contra la cifra oficial de 2024 (70.0 % nacional frente a ≈ 71 % publicado).
4. **Agregación** de persona a distrito, ponderada por el factor de expansión.
5. **Integración validada** — cardinalidad 1:1 sobre `UBIGEO`, `merge` con indicador y análisis de
   los registros no emparejados.
6. **EDA** sobre la base integrada.
7. **Transformaciones** — derivadas, winsorización, log, discretización, one-hot y z-score.
8. **Matriz analítica** — 1 270 distritos × 47 variables, con su diccionario.
9. **PCA y NMF** exploratorios.

## Corrección importante respecto del Hito 1

El Hito 1 usó `CODINFOR` como condición de informalidad. El Diccionario de Datos oficial de la ENAHO
2024 la define como **«Código de informante»**: qué miembro del hogar respondió la encuesta. Con la
informalidad correctamente construida (categoría ocupacional + registro del negocio en SUNAT o
contabilidad + afiliación a pensiones), la correlación distrital entre informalidad y pobreza pasa de
≈ −0.04 (Hito 1) a **+0.58**.

Al cotejar el resto de variables contra el diccionario aparecieron dos errores menores de etiquetado
heredados del Hito 1, también corregidos: `P510A1` y `P510B` estaban intercambiadas, y `P301A`
incluye el código 12 («Básica especial») fuera de la escala ordinal 1–11.

Los cuadernos del Hito 1 se conservan en el repositorio con una advertencia al inicio, como registro
del avance.
