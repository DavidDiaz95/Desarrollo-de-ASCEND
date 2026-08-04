# ASCEND — Desarrollo

Repositorio de desarrollo del proyecto **ASCEND**: fitness y nutrición
gamificados, adaptados a ti — no a un promedio. Contiene los notebooks,
datos, modelos y documentación de ciencia de datos que sustentan la app en
producción.

- **App en producción:** https://ascend-mejora-t.streamlit.app
- **Repo de producción (código de la app Streamlit):**
  https://github.com/DavidDiaz95/ASCEND
- **Autor:** David Díaz Sánchez
- **Contexto:** Trabajo final — Diplomado en Ciencia de Datos, FES Acatlán
  (UNAM), Generación 33

> Este repo se mantiene **separado** del repo de producción a propósito: aquí
> vive el desarrollo pesado (notebooks, datos crudos, experimentos) sin
> ensuciar el despliegue liviano que corre en Streamlit Community Cloud.

---

## Contenido

- [Qué hace ASCEND](#qué-hace-ascend)
- [Estructura del repositorio](#estructura-del-repositorio)
- [Cómo levantar el entorno](#cómo-levantar-el-entorno)
- [Notebooks](#notebooks)
- [Datos](#datos)
- [Modelos](#modelos)
- [Documento final](#documento-final)

---

## Qué hace ASCEND

ASCEND combina tres piezas de ciencia de datos:

1. **Perfilador físico** — un modelo no supervisado (GMM) sobre datos de
   laboratorio del programa de aptitud física de Corea del Sur (KSPO)
   descubre perfiles de condición física; un modelo supervisado (regresión
   logística) aprende a aproximar esos perfiles usando solo variables que
   cualquier persona puede auto-medirse en casa.
2. **Motor de recomendación de rutinas** — sobre un catálogo de 1,324
   ejercicios calificados en dificultad continua por un LLM, con un nivel
   objetivo que se ajusta dinámicamente según el feedback del usuario.
3. **Motor de nutrición** — visión por computadora sobre fotos de
   refrigerador + la API de Spoonacular, alineado al mismo objetivo del
   entrenamiento.

## Estructura del repositorio

```
ASCEND-desarrollo/
├── README.md                        ← este archivo
├── requirements-dev.txt             ← dependencias del entorno de desarrollo
├── .gitignore
│
├── notebooks/
│   ├── 1.1. Data_Wrangler_1.ipynb              # limpieza inicial KSPO
│   ├── 1.2. Data_Wrangler_2.ipynb              # imputación e ingeniería de variables
│   ├── 1.3. EDA_Perfilador.ipynb               # análisis exploratorio del perfilador físico
│   ├── 1.4. Clustering_gmm_no_supervisado.ipynb # segmentación GMM por sexo
│   ├── 1.5. clasificador_restringido.ipynb     # comparación de modelos supervisados
│   ├── 1.6. Clasificador Restringido version lite .ipynb  # versión ligera para producción
│   ├── 2.1. EDA_Recomendador de Alimentos y Comidas.ipynb # EDA del motor de nutrición
│   ├── 2.2 Integracion_Recomendador_Nutricion.ipynb        # integración Spoonacular + visión
│   ├── 3.1 exercises_dificultad_completo.ipynb  # scoring de dificultad vía LLM
│   ├── 3.2 EDA_Catalogo_Ejercicios.ipynb        # EDA del catálogo de 1,324 ejercicios
│   ├── 3.3 Recomendador_Similitud_Coseno.ipynb  # v1 del motor de rutinas (descartada)
│   └── 3.4 Recomendador_ASCEND_Regla_Negocio.ipynb # v2 del motor de rutinas (producción)
│
├── Data/
│   ├── raw/                         # CSV crudos de KSPO (ver instrucciones abajo)
│   │   └── README_KSPO_INFO.md           # cómo descargar el dataset
│   └── processed/                   # datos procesados / checkpoints
│
├── Models/                          # modelos entrenados (.joblib)
│
└── documento/
    └── ASCEND_documento_final.pdf   # documento final del diplomado
```

## Cómo levantar el entorno

```bash
git clone https://github.com/DavidDiaz95/Desarrollo-de-ASCEND.git
cd Desarrollo-de-ASCEND

python3 -m venv ds-venv
source ds-venv/bin/activate

pip install --upgrade pip
pip install -r requirements-dev.txt

# Registrar el entorno como kernel de Jupyter/VS Code
python -m ipykernel install --user --name=ds-venv --display-name "Python (ds-venv)"
```

Luego abre cualquier notebook y selecciona el kernel **"Python (ds-venv)"**.

## Notebooks

Los notebooks están numerados por bloque temático y deben ejecutarse en
orden dentro de cada bloque:

| Bloque | Notebooks | Qué cubre |
|---|---|---|
| **1. Perfilador físico** | 1.1 – 1.6 | Limpieza de KSPO, EDA, clustering GMM no supervisado, clasificador restringido (con datos auto-medibles) y su versión ligera de producción |
| **2. Nutrición** | 2.1 – 2.2 | EDA del recomendador de alimentos, integración de visión por computadora + Spoonacular |
| **3. Rutinas** | 3.1 – 3.4 | Scoring de dificultad de ejercicios vía LLM, EDA del catálogo, comparación entre el motor v1 (similitud coseno, descartado con evidencia) y el motor v2 (regla de negocio, en producción) |

## Datos

Los CSV crudos de KSPO **no están incluidos por defecto** para efectos de
organización. Instrucciones completas de descarga en:

➡️ [`Data/raw/README_KSPO_INFO.md`](Data/raw/README_KSPO_INFO.md)

En resumen: se descargan desde el
[Big Data Culture Portal](https://www.bigdata-culture.kr/bigdata/user/data_market/detail.do?id=ace0aea7-5eee-48b9-b616-637365d665c1)
(requiere cuenta con verificación por teléfono coreano), en lotes de hasta
10 archivos a la vez, y se colocan en `Data/raw/`.

El catálogo de ejercicios combina dos fuentes públicas:
- [hasaneyldrm/exercises-dataset](https://github.com/hasaneyldrm/exercises-dataset)
  (1,324 ejercicios, sin dificultad etiquetada)
- [yuhonas/free-exercise-db](https://github.com/yuhonas/free-exercise-db)
  (873 ejercicios, usado para prestar dificultad por coincidencia de nombre)

## Modelos

Los modelos entrenados (`.joblib`) para el clasificador restringido y sus
reconstructores de variables de laboratorio viven en `Models/`. La versión
de producción (`1.6`) usa una regresión logística aligerada para caber
dentro de los límites de memoria de Streamlit Community Cloud.

## Documento final

El documento completo del proyecto (metodología, EDA, modelación,
resultados y conclusiones) está en
[`documento/ASCEND_documento_final.pdf`](documento/ASCEND_documento_final.pdf).

---

**Variables de entorno:** los notebooks que usan OpenAI o Spoonacular
esperan un archivo `.env` en la raíz (no incluido en el repo, ver
`.gitignore`) con las llaves correspondientes:

```
OPENAI_API_KEY=...
SPOONACULAR_API_KEY=...
```
