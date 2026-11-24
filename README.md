# Análisis Comparativo de Mercado Laboral e Ingresos: CABA vs. Partidos del GBA (2016–2026)

**Universidad Tecnológica Nacional (UTN) - Facultad Regional Avellaneda**  
**Tecnicatura Universitaria en Programación**  
*Materia:* Introducción al Análisis de Datos (Comisión 341)  
*Docente:* Luis N. Fernández  
*Ciclo Lectivo:* 2° Cuatrimestre 2026  

---

## Integrantes (Grupo)
* Condarco, Bruno
* Fernández, Agustina
* Hulej, Manuel
* Sparta, Luciano

---

## Descripción del Proyecto
Este repositorio contiene el código fuente y los notebooks correspondientes al **Segundo Parcial / Trabajo Práctico Final** de la materia. 

El objetivo principal es realizar un estudio exhaustivo y comparativo sobre el mercado de trabajo y los ingresos entre dos aglomerados urbanos vecinos de la República Argentina:
* **Ciudad Autónoma de Buenos Aires (CABA)** - Código INDEC `32`.
* **Partidos del Gran Buenos Aires (GBA)** - Código INDEC `33`.

El análisis abarca un decenio completo (**2016–2026**, 40 trimestres continuos) a partir de las bases de microdatos individuales de la **Encuesta Permanente de Hogares (EPH)** provistas por el **INDEC**, incorporando la deflactación de ingresos mediante el **Índice de Precios al Consumidor (IPC)** y el desarrollo de un modelo econométrico para la imputación de no respuesta.

---

## Estructura del Repositorio

```text
tp_analisis_datos/
│
├── notebooks/
│   ├── 01_analisis_univariado.ipynb      
│   ├── 02_analisis_multivariado.ipynb    
│   └── 03_modelo_imputacion.ipynb        
│
├── .gitignore                            
├── requirements.txt                      
└── README.md                            
```

> **Nota sobre los datos e imágenes:** La carpeta `datos/raw/` (que contiene ~880 MB de microdatos del INDEC) y los gráficos generadas se encuentran excluidas del repositorio vía `.gitignore`. Las visualizaciones completas y tablas se encuentran integradas y vinculadas en el informe final entregado en formato PDF.

---

## Metodología y Contenido por Notebook

### 1. `01_analisis_univariado.ipynb` (Punto 1)
* **Auditoría de Calidad:** Detección y cuantificación de la no respuesta deliberada en ingresos (código `-9` en `P21`) sobre población ocupada, comparando las puntas temporales homogéneas T2 2016 vs. T2 2026 ($N = 18.278$).
* **Valores Atípicos:** Aplicación de la regla de Tukey ($Q_3 + 1.5 \times \text{IQR}$) para la detección y caracterización de *outliers* en las distribuciones salariales de CABA y GBA.

### 2. `02_analisis_multivariado.ipynb` (Puntos 2)
* **Consolidación Masiva:** Pipeline de ingestión y filtrado que procesa los 40 trimestres del período 2016–2026 ($N = 326.028$).
* **Tasas Oficiales Ponderadas:** Evolución histórica trimestral de la Tasa de Actividad, Tasa de Empleo y Tasa de Desocupación ponderadas por el factor de expansión poblacional (`PONDERA`).
* **Ingresos Reales e Inflación:** Deflactación del ingreso de la ocupación principal (`P21`) a moneda constante de T2 2026 utilizando la serie del IPC del INDEC, evaluando media, mediana y rango intercuartílico ($Q_1 - Q_3$).
* **Cruces del Empleo:** Caracterización por Calificación Ocupacional (`PP04D_COD`), Rama de Actividad (`PP04B_COD`), Nivel Educativo (`NIVEL_ED`), Brecha de Género (`CH04`) y Grupos de Edad (`CH06`).

### 3. `03_modelo_imputacion.ipynb` (Punto 4)
* **Modelo Estadístico:** Regresión Lineal Múltiple estimada por Mínimos Cuadrados Ordinarios (OLS) sobre la variable dependiente original en pesos **sin transformaciones no lineales**.
* **Rendimiento:** Obtención de un **$R^2 = 0.3422$** (superando la condición de $R^2 > 0.30$), con $R^2 \text{ ajustado} = 0.3389$.
* **Diagnóstico e Imputación:** Análisis de coeficientes *ceteris paribus*, evaluación de residuos e imputación condicional determinística sobre los 1.052 casos de no respuesta observados en T2 2026.

---

## Instalación y Replicabilidad

Para clonar y ejecutar localmente este proyecto:

1. **Clonar el repositorio:**
   ```bash
   git clone https://github.com/TU_USUARIO/TU_REPOSITORIO.git
   cd TU_REPOSITORIO
   ```

2. **Crear y activar un entorno virtual:**
   ```bash
   python -m venv .venv
   .venv\Scripts\activate
   ```

3. **Instalar dependencias:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Descargar los datos:**
   * Descargar las bases de microdatos individuales de la EPH continua desde la página oficial del [INDEC](https://www.indec.gob.ar/indec/web/Institucional-Indec-BasesDeDatos).
   * Descomprimir los archivos de texto `.txt` en la ruta `datos/raw/`.

5. **Ejecución:**
   * Abrir y ejecutar los notebooks en orden numérico (`01`, `02`, `03`) en VS Code.
