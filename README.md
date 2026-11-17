# Análisis de sentimiento de noticias financieras (español, LATAM)

> Script de línea de comandos y notebook que asignan una polaridad de sentimiento a titulares financieros en español y calculan un índice diario o agregado. Para analistas y estudiantes de NLP aplicado a finanzas.

**Estado (2026-11-17):** PARCIAL — `monitor_sentimiento.py` es un CLI de sentimiento (VADER + lexicón financiero propio); no hay datos de noticias en el repo (`datos/noticias.csv` no incluido); el notebook no tiene outputs guardados; el README anterior describía LDA, TF-IDF + regresión logística y correlación con BTC/ILF, que no están en el script; falta `requirements.txt`, `.gitignore` y licencia.
**Repo:** kapitalya/riesgo-liquidez-banca-bolivia · **Rama por defecto:** main · **Alias/nombres anteriores:** nombre del repo no coincide con el contenido. Alias propuesto: `nlp-noticias-financieras-latam` (nombre que usa el README anterior). Sin renombrar en este PR; decisión del dueño.

> Aviso: análisis educativo, no es consejo financiero ni una medida de riesgo de liquidez bancaria. El repo no calcula riesgo de liquidez.

## Qué hace

- `monitor_sentimiento.py`: lee un CSV de noticias, limpia el texto, calcula un score de sentimiento con VADER más un lexicón financiero propio (función `calcular_sentimiento`), detecta menciones de entidades (`detectar_entidades`), calcula un índice agregado (`calcular_indice_sentimiento`) e imprime un reporte (`generar_reporte`). Puede guardar el resultado en CSV.
- `nlp_noticias_financieras.ipynb`: exploración del análisis en español. Sin outputs guardados. El README anterior describía LDA y regresión logística; no hay esos modelos en el script, y no es verificable si están en el notebook.

## Stack

Python, `vaderSentiment`, `nltk` (stopwords y tokenización), `scikit-learn` (importa `TfidfVectorizer`; su uso en el script no se verificó), `pandas`, `numpy`, Jupyter Notebook.

## Estructura

```
riesgo-liquidez-banca-bolivia/
├── monitor_sentimiento.py          # CLI de sentimiento (en la raíz, no en src/)
├── nlp_noticias_financieras.ipynb  # Notebook de exploración (sin outputs)
└── README.md
```

`datos/` no existe en el repo: el usuario debe crear el CSV de noticias.

## Instalación y uso

```bash
git clone https://github.com/kapitalya/riesgo-liquidez-banca-bolivia
cd riesgo-liquidez-banca-bolivia

python -m venv venv && source venv/bin/activate   # Windows: venv\Scripts\activate
pip install pandas numpy nltk vaderSentiment scikit-learn jupyter   # sin requirements.txt
python -c "import nltk; nltk.download('stopwords'); nltk.download('punkt')"
mkdir -p datos
```

Formato de entrada (el CSV lo prepara el usuario; no hay muestra en el repo):

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

Sin verificar: que los recursos `nltk.download` requeridos coincidan con los que el script usa en tiempo de ejecución.

## Variables de entorno (solo nombres)

Ninguna. No hay `.env.example`.

## Pruebas y CI

Sin pruebas automatizadas ni workflows de CI en el repo.

## Despliegue

No aplica: script y notebook de ejecución local.

## Resultados

El bloque "Output del Monitor" del README anterior (89 noticias, 34 positivas, 18 neutras, 37 negativas, score promedio -0.0821) es un ejemplo sin datos en el repo que lo respalden. Resultados no verificables en el repo; se retira.

## Fuentes de noticias (previstas, no implementadas)

El README anterior listaba feeds RSS de medios bolivianos y latinoamericanos (El Deber, La Razón, Bloomberg Línea, El Financiero MX, Portafolio CO, Gestión PE). El script no los consume: la ingesta es un CSV manual. Sin verificar que las URL RSS sigan vigentes.

## Estado y relaciones

- Repositorio independiente. El README anterior enlazaba repos de una cuenta personal fuera de la organización (`Serius69/*`); no auditados en este corte.
- Relación con el riesgo de liquidez bancaria: ninguna en el código. Si se quiere medir riesgo de liquidez, es un proyecto distinto.

## Precios y planes

No aplica (repositorio de análisis, sin venta). Precios y planes: fuente única `Vault/EMPRESA/portafolio/PRECIOS-APROBADOS-20261108.md`.

## Seguridad, privacidad y derechos

- Sin secretos en el repo.
- Noticias: el texto de los medios tiene derechos de sus autores. El repo no incluye noticias; cualquier CSV que se cargue debe respetar los términos del medio.
- Licencia: el repo no tiene archivo LICENSE; no hay derechos de reutilización declarados.

## Convenciones de trabajo

Cambios por rama `codex/*` y PR contra `main` (no hay rama `develop`). CI: no hay workflows configurados.
