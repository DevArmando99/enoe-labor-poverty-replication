# Réplica y validación de la pobreza laboral con ENOE

[English version](README.md)

Reconstrucción reproducible y validación estadística del indicador de pobreza laboral en México utilizando microdatos ENOE para la serie trimestral disponible entre 2006T1 y 2026T1.

## Resumen del proyecto

Este repositorio contiene el caso de estudio aplicado construido sobre la biblioteca de procesamiento `enoe-utilities`. El objetivo no es únicamente reproducir la serie oficial de pobreza laboral, sino documentar un flujo auditable desde los insumos ENOE hasta el indicador trimestral final y evaluar qué tan estrechamente coinciden los valores reconstruidos con la serie oficial.

El estudio cubre 80 trimestres disponibles entre 2006T1 y 2026T1. La serie reconstruida reproduce casi perfectamente la dinámica temporal del indicador oficial, aunque la validación también identifica un pequeño sesgo negativo sistemático y una discrepancia mayor en los periodos recientes analizados.

## Principales resultados

- Se reconstruyeron y compararon 80 observaciones trimestrales con la serie oficial.
- La correlación y el R² son aproximadamente 1, lo que muestra una correspondencia prácticamente perfecta en la dinámica temporal.
- Diferencia media (Calculada − Oficial): aproximadamente **−0.0166 puntos porcentuales**.
- Error absoluto medio (MAE): aproximadamente **0.0166 puntos porcentuales**.
- Discrepancia máxima absoluta: aproximadamente **0.0869 puntos porcentuales**.
- La prueba TOST establece equivalencia estadística para la serie completa bajo un margen práctico de **±0.05 puntos porcentuales**.
- La equivalencia también se establece hasta 2024T1.
- Desde 2024T2 en adelante no puede establecerse equivalencia bajo el mismo margen.
- Los resultados OLS y HAC/Newey-West respaldan una asociación más fuerte entre el incremento de las discrepancias y el componente temporal reciente que con el nivel de pobreza laboral por sí mismo.

El resultado para los periodos recientes describe el comportamiento observado dentro de la muestra analizada y no implica que la discrepancia necesariamente continuará aumentando en trimestres futuros.

## Flujo de trabajo

```text
Microdatos ENOE en ZIP
        ↓
Conversión y normalización de esquema
        ↓
Validación de columnas requeridas y llaves
        ↓
Unión SDEM + COE2 a nivel persona
        ↓
Dataset analítico de ingreso laboral
        ↓
Ingreso laboral del hogar e ingreso per cápita
        ↓
Línea de pobreza extrema por ingresos rural / urbana
        ↓
Estimación ponderada de pobreza laboral
        ↓
Validación estadística Oficial vs. Reconstruido
        ↓
Parquet analítico histórico para análisis posteriores
```

## Validación estadística

La estrategia de validación va deliberadamente más allá de la correlación:

1. Comparación temporal entre la serie oficial y la reconstruida.
2. Correlación y ajuste lineal.
3. Análisis de diferencias trimestrales y magnitud del error.
4. Prueba t de una muestra para evaluar sesgo medio sistemático.
5. Análisis de concordancia Bland–Altman.
6. Segmentación exploratoria por periodo temporal y nivel de pobreza laboral.
7. Regresión OLS e inferencia robusta HAC/Newey-West.
8. Prueba TOST de equivalencia para la serie completa y dos segmentos temporales.

Para TOST se utiliza un margen práctico de equivalencia de **±0.05 puntos porcentuales**, interpretado como media unidad de la resolución de una cifra decimal con la que suele comunicarse públicamente el indicador. Este criterio se utiliza únicamente para esta auditoría y **no representa un umbral oficial de equivalencia definido por INEGI o CONEVAL**.

## Estructura del repositorio

```text
.
├── README.md
├── README_ES.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── Replica_Indice_Pobreza_Laboral.ipynb
├── results/
│   └── serie_pobreza_laboral_2006T1_2026T1.csv
├── docs/
│   └── figures/
└── reproducibility/
    ├── README.md
    └── README_ES.md
```

Los archivos pesados de datos crudos e intermedios se mantienen deliberadamente fuera de GitHub. La documentación de reproducibilidad describe la estructura esperada del paquete completo y los insumos necesarios para reconstruir el análisis.

## Relación con `enoe-utilities`

Este repositorio corresponde al caso de estudio aplicado y a la capa de validación estadística. Las funciones reutilizables de procesamiento se mantienen de forma independiente en el repositorio `enoe-utilities`.

- `enoe-utilities`: utilidades reutilizables para procesamiento ENOE y pobreza laboral.
- `enoe-labor-poverty-replication`: reconstrucción aplicada, validación y resultados documentados.

## Paquete de reproducibilidad

El paquete completo de reproducibilidad se distribuye por separado debido al tamaño de los microdatos ENOE y de los archivos intermedios. Incluye insumos originales, archivos Parquet convertidos, tablas unidas, datasets analíticos trimestrales, Parquet histórico, indicadores, metadatos, paradata y salidas de validación.

Antes de redistribuir los archivos fuente originales de ENOE, deben verificarse las condiciones vigentes de uso y redistribución establecidas por INEGI. Si no resulta apropiado redistribuirlos, conviene mantener un manifiesto con los nombres exactos de los archivos oficiales y sus ubicaciones de descarga.

Consulta [`reproducibility/README_ES.md`](reproducibility/README_ES.md) para más detalles.

## Entorno

El análisis está desarrollado en Python y utiliza pandas, NumPy, PyArrow, SciPy, statsmodels, Matplotlib, Seaborn, openpyxl y Jupyter.

Las dependencias principales pueden instalarse con:

```bash
pip install -r requirements.txt
```

La biblioteca `enoe-utilities` también debe estar disponible en el entorno para ejecutar nuevamente el pipeline completo.

## Alcance

Este repositorio cierra la primera etapa de un proyecto más amplio. El dataset histórico analítico validado queda preparado para análisis exploratorios y estadísticos posteriores sobre pobreza laboral y sus determinantes.

## Aviso

Este es un proyecto independiente de reproducibilidad y portafolio. No constituye un producto oficial de INEGI o CONEVAL, y los criterios prácticos de equivalencia utilizados en la validación estadística no deben interpretarse como tolerancias institucionales.
