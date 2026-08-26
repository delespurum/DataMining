# Pobreza e informalidad laboral en el Perú (2024)

Proyecto de integración de datos que cruza la ENAHO 2024 (Módulo 500 — Empleo e Ingresos) con el
Mapa de Pobreza Monetaria Distrital del INEI, para identificar patrones de informalidad laboral y
pobreza a nivel distrital que no son evidentes al mirar cada fuente por separado.

**Problema aplicado:** el MTPE dispone de amplios microdatos de la ENAHO, pero no cuenta con un
sistema exploratorio que combine simultáneamente informalidad, subocupación, ingresos y pobreza para
priorizar territorios de intervención.

## Estructura del repositorio

```
├── notebooks/
│   ├── Notebook_Pobreza_Informalidad.ipynb        # Carga, limpieza e integración de las 2 fuentes
│   └── Visualizaciones_Pobreza_Informalidad.ipynb # Visualizaciones y hallazgos sobre la base integrada
├── data/
│   └── pobreza_informalidad_departamental_2024.csv # Salida de la integración (420 distritos)
└── README.md
```

## Fuentes de datos

| Fuente | Institución | Año | Enlace |
|---|---|---|---|
| ENAHO 2024 — Módulo 500 (Empleo e Ingresos) | INEI | 2024 | http://webinei.inei.gob.pe/anda_inei/index.php/catalog/816/get_microdata |
| Mapa de Pobreza Monetaria Distrital | INEI | 2018 | https://www.gob.pe/institucion/inei/informes-publicaciones/3204872-mapa-de-pobreza-provincial-y-distrital-2018 |

La ENAHO no se descarga como archivo único: hay que entrar a `proyectos.inei.gob.pe/microdatos` →
Encuesta Nacional de Hogares - ENAHO → año 2024 → Módulo 500 (Empleo e Ingresos) → descargar en
formato .csv o .sav. El Mapa de Pobreza se descarga junto a su Anexo Estadístico (.xlsx) desde la
página de la publicación.

**Unidad de análisis:** distrito, identificado por `UBIGEO` (6 dígitos). **Llave de integración:**
`UBIGEO`.

## Cómo ejecutar el código

1. Clona el repositorio:
   ```bash
   git clone <URL-DE-ESTE-REPOSITORIO>
   cd <nombre-del-repo>
   ```
2. Instala las dependencias:
   ```bash
   pip install pandas numpy plotly nbformat jupyter
   ```
3. **Notebook de integración** (`notebooks/Notebook_Pobreza_Informalidad.ipynb`): requiere descargar
   la ENAHO 2024 completa desde INEI (ver sección de fuentes) y ajustar la variable `RUTA_ENAHO` en la
   celda de configuración con la ubicación del archivo descargado. Al ejecutarlo genera
   `data/pobreza_informalidad_departamental_2024.csv`.
4. **Notebook de visualizaciones** (`notebooks/Visualizaciones_Pobreza_Informalidad.ipynb`): no
   requiere descargas adicionales, se ejecuta directamente sobre el CSV ya incluido en `data/`.
   ```bash
   jupyter notebook notebooks/Visualizaciones_Pobreza_Informalidad.ipynb
   ```

## Principales hallazgos

- La correlación entre informalidad laboral y pobreza monetaria a nivel distrital es prácticamente
  nula (r ≈ −0.07): son dos problemas que se solapan solo parcialmente en el territorio.
- Esa independencia no es un artefacto del cruce: el ingreso laboral sí desciende de forma consistente
  con la pobreza (r ≈ −0.41), lo que valida la integración.
- Un cuadrante crítico de ~84 distritos (con muestra suficiente) combina alta informalidad y alta
  pobreza simultáneamente, concentrado en la sierra norte y sur del país.

## Limitaciones conocidas

- Desfase temporal: la ENAHO es de 2024 y el Mapa de Pobreza Distrital es de 2018 (el INEI no ha
  publicado una actualización distrital más reciente).
- La ENAHO está diseñada para representatividad nacional/departamental, no distrital; la mediana es de
  14 personas encuestadas por distrito, por lo que las estimaciones a ese nivel son exploratorias.
- `UBICACION_POBREZA_MONETARIA_TOTAL` no es una tasa sino un puesto en el ranking nacional de pobreza;
  la tasa usada en los gráficos se reconstruyó a partir del intervalo de confianza publicado.
