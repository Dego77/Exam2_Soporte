# Minería de Datos para Decisiones Comerciales
## Segundo Examen · Proyecto de Aplicación (INF432)

**Universidad Autónoma Gabriel René Moreno (UAGRM)**  
**Facultad de Ingeniería en Ciencias de la Computación y Telecomunicaciones (FICCT)**  
**Materia:** INF432 – Sistemas para el Soporte a la Toma de Decisiones  
**Semestre:** 2-2026  

---

## 1. Ficha del Proyecto

| Campo | Detalle |
|---|---|
| **Grupo** | **RetailAnalytics** |
| **Integrantes** | 1. Banegas Bruno Diego Fernando<br>2. Hernandez Lijeron Roly<br>3. Huanca Silvestre Grover |
| **Escenario / Empresa** | Cadena minorista gourmet multicanal que comercializa vinos, carnes, frutas, pescados, dulces y productos oro mediante tienda física, catálogo y comercio electrónico web. |
| **Dataset Oficial** | **Customer Personality Analysis (Kaggle)**<br>Enlace: https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis<br>Volumen: 2,240 registros originales (2,025 tras limpieza), 29 columnas originales. |

---

## 2. Áreas de Negocio y Preguntas Clave

El proyecto aplica el ciclo metodológico **CRISP-DM** en tres áreas estratégicas:

1. **Área 1 · Campañas de Marketing (A1 - Supervisado):**
   * *Pregunta:* ¿A qué clientes debe dirigir el equipo comercial la próxima campaña para maximizar la tasa de aceptación con el menor número de contactos posibles?
   * *Decisión:* Selección de la lista prioritaria para llamadas/contacto directo.
   * *Técnicas aplicadas:* Reducción supervisada con **LDA** vs **PCA**, clasificación con **kNN** (optimizado con `GridSearchCV`), **Naive Bayes** y **Árbol de Decisión** (optimizado con `GridSearchCV` y balanceo de pesos).
   * *Impacto Financiero:* El modelo de Árbol permite capturar el **68.8% de las aceptaciones** llamando solo al **30% de la base**, convirtiendo una pérdida histórica de $-801$$ en una **ganancia neta de $+149$$**.

2. **Área 2 · Segmentación de Mercado (A2 - No Supervisado):**
   * *Pregunta:* ¿Qué segmentos de clientes existen según su poder adquisitivo y canasta de consumo por categoría de producto para personalizar la oferta comercial?
   * *Decisión:* Estrategia de producto, mix de precios y catálogo.
   * *Técnicas aplicadas:* **PCA** (reducción a 2 componentes con 51.8% de varianza explicada), **$k$-Means** (justificado con codo y silueta $= 0.268$ para $K=3$) y **Clustering Jerárquico** (enlace Ward con dendrograma, 92.6% de coincidencia).
   * *Segmentos Identificados:*
     * Clúster 0: *Adultos amantes del vino*
     * Clúster 1: *Familias de presupuesto ajustado*
     * Clúster 2: *Premium sin hijos de alto gasto*

3. **Área 3 · Patrones de Compra (A3 - No Supervisado):**
   * *Pregunta:* ¿Qué patrones de compra (variables RFM, descuentos y canal preferido) existen y a través de qué canal intervenir comercialmente a cada grupo?
   * *Decisión:* Asignación presupuestaria por canal (tienda física, web o catálogo).
   * *Técnicas aplicadas:* Proyección **PCA**, **$k$-Means** ($K=3$, silueta $= 0.248$) y **DBSCAN** ($k$-distance con $k=8$, $\varepsilon \approx 2.0$, aislando 25 clientes atípicos / ruido `-1`).
   * *Patrones Identificados:*
     * *Leales multicanal de alto valor* (39% de clientes, alto uso de catálogo y fidelidad).
     * *Digitales cazadores de ofertas* (27% de clientes, 46% compras web, alto tráfico y uso de promociones).
     * *Ocasionales de tienda física* (34% de clientes, baja frecuencia, canal presencial exclusivo).

---

## 3. Estructura del Directorio de Código

```text
3_Codigo/
├── README.md                          # Documentación completa y guía de ejecución
├── requirements.txt                   # Dependencias con versiones compatibles
├── 00_Datos.ipynb                     # Fase de exploración, limpieza y feature engineering (exporta data/processed/clientes_limpio.csv)
├── 01_Area_Campanas.ipynb             # Área 1 (A1 - Supervisado): Optimización de campañas (kNN, Naive Bayes, Árbol, PCA vs LDA)
├── 02_Area_Segmentacion.ipynb         # Área 2 (A2 - No Supervisado): Segmentación de mercado (k-means K=3, jerárquico Ward, PCA 2D)
├── 03_Area_Patrones.ipynb             # Área 3 (A3 - No Supervisado): Patrones de compra RFM (k-means K=3, DBSCAN y evaluación)
├── 00_Datos_Completo_Respaldo.ipynb   # Cuaderno unificado original completo de respaldo (96 celdas integradas)
└── data/
    ├── raw/
    │   └── marketing_campaign.csv     # Dataset crudo original de Kaggle (sep='\t', 2,240 registros)
    └── processed/
        ├── clientes_limpio.csv        # Dataset procesado y depurado (2,025 clientes, 34 variables)
        └── clientes_segmentados_bi.csv # Dataset consolidado para Power BI (2,025 clientes, 45 variables con segmentos y prioridades)
```

---

## 4. Requisitos del Entorno e Instalación

* **Lenguaje:** Python `>= 3.10` (compatible y verificado en Python 3.10, 3.11, 3.12 y 3.14).
* **Instalación de paquetes:**

```bash
pip install -r requirements.txt
```

Librerías principales requeridas:
* `numpy >= 1.24.0`
* `pandas >= 2.0.0`
* `matplotlib >= 3.7.0`
* `seaborn >= 0.12.0`
* `scikit-learn >= 1.2.0`
* `scipy >= 1.10.0`
* `jupyter >= 1.0.0`

---

## 5. Guía de Ejecución y Reproducibilidad

1. Abrir la terminal en la carpeta `3_Codigo/`.
2. Iniciar el servidor Jupyter:
   ```bash
   jupyter notebook
   ```
3. **Flujo de Ejecución Modular (Recomendado):**
   * **Paso 1:** Ejecutar **`00_Datos.ipynb`** (`Kernel > Restart & Run All`). Este cuaderno limpia los datos crudos y genera `data/processed/clientes_limpio.csv`.
   * **Paso 2:** Ejecutar cualquiera de los tres cuadernos de las áreas de negocio de forma independiente:
     * **`01_Area_Campanas.ipynb`**: Modelado supervisado de respuesta a campañas.
     * **`02_Area_Segmentacion.ipynb`**: Segmentación de mercado según canasta de gasto.
     * **`03_Area_Patrones.ipynb`**: Segmentación de hábitos multicanal RFM y evaluación final.
4. **Flujo Unificado (Respaldo):**
   * También se dispone del cuaderno unificado **`00_Datos_Completo_Respaldo.ipynb`**, el cual contiene el flujo completo de principio a fin en un solo archivo.
5. **Garantía de Reproducibilidad:**
   * Todas las operaciones de modelado y partición de datos utilizan una semilla aleatoria fija (`SEMILLA = 1` / `random_state=1`), garantizando que cualquier ejecución arroje exactamente las mismas métricas, matrices de confusión y gráficos.
   * Todas las lecturas y escrituras de archivos emplean rutas relativas (`data/raw/...` y `data/processed/...`).

---

## 6. Referencias Bibliográficas

1. **Patel, A. (2021).** *Customer Personality Analysis* [Data set]. Kaggle. https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis
2. **Chapman, P. et al. (2000).** *CRISP-DM 1.0: Step-by-step data mining guide*. SPSS Inc.
3. **Pedregosa, F. et al. (2011).** *Scikit-learn: Machine learning in Python*. Journal of Machine Learning Research, 12, 2825–2830.
4. **Ester, M. et al. (1996).** *A density-based algorithm for discovering clusters in large spatial databases with noise*. KDD-96 Proceedings, 226–231.
