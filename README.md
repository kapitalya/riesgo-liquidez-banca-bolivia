# Análisis de sentimiento de noticias financieras (español, LATAM)

> Script de línea de comandos y notebook que asignan una polaridad de sentimiento a titulares financieros en español, y agregan un índice. Para analistas y estudiantes de NLP aplicado a finanzas.

**Estado (2026-11-17):** PARCIAL — `monitor_sentimiento.py` es un CLI de sentimiento (VADER más un lexicón financiero propio); no hay noticias en el repo (el CSV es de entrada manual); el notebook no tiene outputs guardados; el README anterior describía LDA, TF-IDF + regresión logística y correlación con BTC/ILF, que no están en el script; LDA y regresión logística sí aparecen en el notebook, sin resultados verificables; falta `requirements.txt`, `.gitignore` y licencia.
**Repo:** kapitalya/riesgo-liquidez-banca-bolivia · **Rama por defecto:** main · **Alias/nombres anteriores:** el nombre del repo no coincide con su contenido: no es un análisis de riesgo de liquidez bancaria. No se renombra.

> Aviso: análisis educativo, no es consejo financiero. El repo no calcula riesgo de liquidez bancaria.

## Qué hace

- `monitor_sentimiento.py` (raíz): lee un CSV de noticias, limpia el texto, tokeniza en español, calcula un score con VADER más un lexicón financiero propio (`calcular_sentimiento`), detecta menciones de entidades (`detectar_entidades`), calcula un índice agregado (`calcular_indice_sentimiento`) e imprime un reporte (`generar_reporte`). Puede guardar el resultado en CSV con `--output`.
- `nlp_noticias_financieras.ipynb`: exploración más amplia. Además de VADER y TextBlob, incluye TF-IDF y CountVectorizer, LDA (`LatentDirichletAllocation`), regresión logística y Naive Bayes, y una sección "Correlación Sentimiento vs Precio BTC" que descarga BTC-USD con `yfinance`. Sin outputs guardados, así que sus resultados no son verificables.

## Stack

Python. Librerías importadas por el script: `nltk` (stopwords y tokenización), `vaderSentiment`, `scikit-learn` (importa `TfidfVectorizer`, que no se llama en el script), `pandas`, `numpy`. Librerías importadas por el notebook: además `textblob`, `wordcloud`, `yfinance`, `seaborn`, `matplotlib`, y `scikit-learn` (LDA, regresión logística, Naive Bayes). Jupyter Notebook.

## Estructura

```
riesgo-liquidez-banca-bolivia/
├── monitor_sentimiento.py          # CLI de sentimiento (en la raíz, no en src/)
├── nlp_noticias_financieras.ipynb  # Notebook de exploración (sin outputs)
└── README.md
```

`datos/` no existe en el repo. El CSV de noticias lo prepara el usuario; el notebook tiene comentada la lectura de `datos/noticias_financieras.csv`.

## Instalación y uso

```bash
git clone https://github.com/kapitalya/riesgo-liquidez-banca-bolivia
cd riesgo-liquidez-banca-bolivia

python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install pandas numpy nltk vaderSentiment scikit-learn textblob wordcloud yfinance seaborn matplotlib jupyter   # sin requirements.txt
mkdir -p datos
```

El script descarga por sí mismo los recursos de NLTK (`punkt`, `stopwords`, `punkt_tab`) al iniciar. Sin verificar: que TextBlob y WordCloud requieran pasos adicionales (son solo del notebook).

Formato de entrada (el CSV lo prepara el usuario; no hay muestra en el repo). Columnas que el script usa por defecto: `titulo` (texto) y `fecha`. El formato del README anterior, `fecha,titulo,fuente,categoria,pais`, es un ejemplo; sin verificar si el script usa las columnas `fuente`, `categoria` o `pais`.

```
fecha,titulo,fuente,categoria,pais
2024-01-05,Titular de ejemplo,Fuente de ejemplo,macroeconomia,BO
```

Ejecución (desde la raíz del repo):

```bash
# Columnas por defecto: titulo (texto) y fecha
python monitor_sentimiento.py --csv datos/noticias.csv

# Columnas personalizadas
python monitor_sentimiento.py --csv datos/noticias.csv --col-texto headline --col-fecha date

# Guardar resultados en CSV
python monitor_sentimiento.py --csv datos/noticias.csv --output datos/con_sentimiento.csv
```

## Variables de entorno (solo nombres)

Ninguna. No hay `.env.example`.

## Pruebas y CI

- Sin pruebas automatizadas ni workflows de CI en el repo.

### Resultados

- El bloque "Output del Monitor" del README anterior (89 noticias, 34 positivas, 18 neutras, 37 negativas, score promedio -0.0821) es un ejemplo sin datos en el repo que lo respalden. Se retira. Resultado no verificable en el repo.
- Los resultados de LDA, regresión logística y correlación con BTC del notebook no tienen outputs guardados. No verificables en el repo.

## Despliegue

No aplica: script y notebook de ejecución local.

## Fuentes de noticias

- El script no descarga noticias: la entrada es un CSV manual.
- El README anterior listaba feeds RSS (El Deber, La Razón, Bloomberg Línea, El Financiero MX, Portafolio CO, Gestión PE) como próximos pasos. Ni el script ni el notebook los consumen. Sin verificar que las URL sigan vigentes.
- Precios de BTC-USD: descargados con `yfinance` en el notebook. No se encontró código que use ILF en la correlación; la correlación con ILF del README anterior no está verificada.

## Estado y relaciones

- Repositorio independiente. El README anterior enlazaba repos de una cuenta personal fuera de la organización (`Serius69/*`); no auditados en este corte.
- Relación con el riesgo de liquidez bancaria: ninguna en el código. Este repo no mide liquidez.

## Precios y planes

No aplica (repositorio de análisis, sin venta). Precios y planes: fuente única `Vault/EMPRESA/portafolio/PRECIOS-APROBADOS-20261108.md`.

## Seguridad, privacidad y derechos

- Sin secretos en el repo.
- Noticias: el texto de los medios tiene derechos de sus autores. El repo no incluye noticias; cualquier CSV que se cargue debe respetar los términos del medio.
- Precios: Yahoo Finance (vía `yfinance`), sujeto a sus términos; no verificados en este corte.
- Licencia: el repo no tiene archivo LICENSE; no hay derechos de reutilización declarados.

## Convenciones de trabajo

Cambios por rama `codex/*` y PR contra `main` (no hay rama `develop`). CI: no hay workflows configurados.
