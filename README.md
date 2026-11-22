<img width="569" height="104" alt="image" src="https://github.com/user-attachments/assets/4d35042d-bd92-4564-94c1-112384a62865" /># Análisis y predicción del rendimiento académico mediante ciencia de datos

Proyecto académico individual de **Ingeniería de Sistemas** realizado en **Google Colab** con Python, Pandas, NumPy, Matplotlib, Seaborn y Scikit-learn.

## Resumen

Se exploran las calificaciones y características de 395 estudiantes del curso de Matemáticas del conjunto **Student Performance** de la UCI. El objetivo es comprender el comportamiento de la calificación final (`G3`, escala de 0 a 20) y comparar dos enfoques predictivos:

- **Modelo A:** regresión lineal múltiple sin notas parciales (`G1`, `G2`), utilizando 12 variables contextuales y académicas.
- **Modelo B:** las mismas 12 variables más `G1` y `G2`.

**Pregunta:** ¿Cuánto mejora la predicción de G3 cuando se dispone de las notas parciales?

## Fuente y atribución

- Dataset: [Student Performance — UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/320/student-performance)
- Archivo analizado: `student-mat.csv` (curso de Matemáticas; 395 filas y 33 columnas).
- Referencia: Cortez, P. (2008). *Student Performance*. UCI Machine Learning Repository. https://doi.org/10.24432/C5TG7T
- Licencia del dataset: **CC BY 4.0**. Reconocer la fuente y conservar la atribución al redistribuir los datos.

## Proceso metodológico

1. **Carga y limpieza:** verificación de dimensiones, tipos de datos, faltantes y duplicados. Se encontraron **0 nulos**, **0 duplicados** y se conservaron los **395 registros**. El método IQR identificó **13 valores atípicos en G2**; se conservaron por ser posibles calificaciones válidas.
2. **Análisis exploratorio:** medidas descriptivas, histogramas, boxplots y matriz de correlación.
3. **Regresión lineal simple didáctica:** `G3 ≈ 0.534 × studytime + 9.3283`, con `R² = 0.0096`. `studytime` es una categoría ordinal (1–4), no una medida continua de horas.
4. **Modelado supervisado:** división 80/20 de entrenamiento/prueba (316/79 estudiantes; `random_state=42`), codificación de variables categóricas y regresión lineal múltiple con pipelines.
5. **Validación:** contraste contra un predictor de la media en prueba y validación cruzada de cinco particiones **solo en el conjunto de entrenamiento**.

## Hallazgos de análisis exploratorio

| Indicador | G1 | G2 | G3 |
|---|---:|---:|---:|
| Media | 10.9089 | 10.7139 | 10.4152 |
| Mediana | 11 | 11 | 11 |
| Desviación estándar | 3.3192 | 3.7615 | 4.5814 |
| Moda | 10 | 9 | 10 |

Correlaciones con `G3`: `G2 = 0.905`, `G1 = 0.801`, `failures = -0.360`, `studytime = 0.098`. Estas asociaciones **no son causales**.

## Resultados de modelos

### Prueba independiente (79 estudiantes)

| Métrica | Modelo A | Modelo B |
|---|---:|---:|
| MAE | 3.7323 | **1.4812** |
| RMSE | 4.5132 | **2.2363** |
| R² | 0.0066 | **0.7561** |

El predictor de referencia (media del conjunto de entrenamiento) registró **MAE = 3.6459** y **R² = -0.0097** en prueba. En esa división, el Modelo A no superó a la referencia en MAE, aunque obtuvo un R² ligeramente superior.

### Validación cruzada (5 particiones sobre los 316 registros de entrenamiento)

| Métrica | Modelo A | Modelo B |
|---|---:|---:|
| MAE promedio | 3.2845 | **1.2656** |
| R² promedio | 0.0468 | **0.8260** |

Estas métricas pertenecen a validaciones distintas y no sustituyen la evaluación sobre el conjunto de prueba.

### Limitaciones

- El Modelo B necesita **G1 y G2**, de modo que no representa una predicción al principio del curso.
- Su **error absoluto máximo** en prueba fue **8.5685** puntos.
- Hubo **2 predicciones fuera de la escala 0–20**; no se modificaron para calcular las métricas publicadas.
- El conjunto es pequeño, histórico y procede de un contexto educativo específico: es necesario validar externamente antes de cualquier uso real.
- Las características no deben usarse para tomar decisiones automatizadas sobre estudiantes.

## Estructura sugerida del repositorio

```text
proyecto-ciencia-de-datos/
├── datos/
│   ├── student-mat.csv
│   └── student_mat_limpio.csv
├── notebooks/
│   └── analisis_rendimiento_academico.ipynb
├── resultados/
│   ├── resultados_modelos.csv
│   └── validacion_cruzada.csv
├── imagenes/
│   ├── comparacion_modelos.png
│   └── errores_modelo_B.png
├── presentacion/
│   └── presentacion_rendimiento_academico.pptx
├── video/
│   └── enlace_video.txt
├── requirements.txt
└── README.md
```

La estructura es una **propuesta**: incorpora los archivos generados en Colab y exporta el notebook antes de publicar. No publiques archivos con datos personales propios ni secretos.

## Cómo reproducir

1. Descargar el dataset original de UCI y ubicar `student-mat.csv` en `datos/`.
2. Abrir `notebooks/analisis_rendimiento_academico.ipynb` con Google Colab.
3. Instalar las dependencias, si fuera necesario: `pip install -r requirements.txt`.
4. Ejecutar las celdas desde el inicio. El notebook implementa la selección y evaluación de los modelos con `random_state=42`.
5. Consultar las tablas exportadas en `resultados/` y los gráficos de `imagenes/`.

## Posible aplicación en Ingeniería de Sistemas

Un panel de seguimiento académico podría mostrar las calificaciones y estimaciones para apoyar el acompañamiento docente, siempre con protección de datos, supervisión humana y validación para la población en que se aplique.

## Presentación y video

- Diapositivas: `presentacion/presentacion_rendimiento_academico.pptx`.
- Guion: `guion_video_maximo_5_minutos.md` (material de apoyo).
- Enlace a video: agregarlo en `video/enlace_video.txt` después de grabar.

## Documentación adicional

- [Scikit-learn: validación cruzada](https://scikit-learn.org/stable/modules/cross_validation.html)
- [Scikit-learn: métricas de regresión](https://scikit-learn.org/stable/modules/model_evaluation.html#regression-metrics)
- [GitHub: crear un repositorio](https://docs.github.com/en/get-started/start-your-journey/creating-a-repository-for-your-project-on-github)

## Autoría

**Estudiante:** Simon Rodriguez
**Programa:** Ingeniería de Sistemas  
**Fecha:** Noviembre de 2026
