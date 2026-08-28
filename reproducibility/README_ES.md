# Paquete de reproducibilidad

[English version](README.md)

Este directorio documenta el paquete externo de reproducibilidad del proyecto de réplica de pobreza laboral con ENOE. Los archivos pesados de datos crudos e intermedios se mantienen deliberadamente fuera de GitHub.

## Contenido esperado del paquete

```text
Réplica de Pobreza Laboral ENOE — Paquete de Reproducibilidad/
├── Replica_Indice_Pobreza_Laboral.ipynb
├── data/
│   ├── raw/
│   │   ├── zip/
│   │   ├── lpei/
│   │   └── lp/
│   └── parquet/
│       ├── converted/
│       ├── merged/
│       ├── analytical/
│       └── historical/
└── outputs/
    ├── indicators/
    ├── paradata/
    └── validation/
```

## Función de cada conjunto de datos

- `data/raw/zip/`: archivos ZIP trimestrales oficiales de ENOE.
- `data/raw/lpei/`: insumo de líneas de pobreza utilizado por el pipeline de cálculo.
- `data/raw/lp/`: serie oficial de pobreza laboral utilizada para la comparación y validación.
- `data/parquet/converted/`: archivos SDEM y COE2 normalizados en Parquet por trimestre.
- `data/parquet/merged/`: archivos unidos SDEM + COE2 a nivel persona.
- `data/parquet/analytical/`: datasets analíticos trimestrales de pobreza laboral y sus metadatos de calidad.
- `data/parquet/historical/`: dataset analítico persona-trimestre concatenado, manifiesto y metadatos.
- `outputs/indicators/`: serie trimestral reconstruida de pobreza laboral y sus metadatos.
- `outputs/paradata/`: paradata YAML generada por trimestre y para los productos históricos.
- `outputs/validation/`: tablas o diagnósticos de validación guardados de forma opcional.

## Orden recomendado para reproducir el proyecto

1. Crear un entorno de Python e instalar `requirements.txt`.
2. Instalar o dejar disponible la biblioteca `enoe-utilities` utilizada por este proyecto.
3. Restaurar el paquete externo de reproducibilidad con la estructura mostrada arriba.
4. Abrir `notebooks/Replica_Indice_Pobreza_Laboral.ipynb` desde este repositorio de GitHub.
5. Configurar las rutas del proyecto si el notebook no se ejecuta desde el directorio de trabajo esperado.
6. Ejecutar el notebook secuencialmente para convertir los archivos fuente, validar columnas requeridas, unir SDEM y COE2, construir los archivos analíticos, calcular el indicador, generar el Parquet histórico y reproducir la validación estadística.

El notebook publicado en GitHub debe conservar sus salidas ejecutadas para que los resultados puedan revisarse sin necesidad de descargar el paquete completo de datos.

## Redistribución de datos fuente

Antes de redistribuir públicamente los archivos ZIP originales de los microdatos ENOE, deben verificarse las condiciones vigentes de INEGI aplicables a dichos archivos. Si no resulta apropiado redistribuirlos directamente, se recomienda sustituirlos en el paquete público por un manifiesto que registre, como mínimo:

- trimestre;
- nombre exacto del archivo oficial;
- institución fuente;
- página o URL oficial de descarga;
- fecha de descarga o consulta;
- checksum opcional, por ejemplo SHA-256.

Esto conserva la reproducibilidad sin dar a entender que los microdatos fuente son producidos o redistribuidos por este proyecto.

## Recomendación de versionado

Para una versión estable del portafolio conviene registrar:

- commit o release del presente repositorio;
- commit o release de `enoe-utilities`;
- versión de Python;
- versiones de dependencias (`pip freeze` o equivalente);
- manifiesto exacto de archivos fuente;
- fecha de descarga de la serie oficial utilizada en la comparación.

## Alcance

El paquete externo permite reproducir la primera etapa del proyecto: reconstrucción histórica y validación estadística Oficial vs. Calculado. Los análisis exploratorios o modelos posteriores pueden partir directamente del Parquet analítico histórico generado.
