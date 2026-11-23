# Pronóstico mensual de tarifas spot de camión refrigerado por lane

[![Abrir el proyecto final en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jmartinezvogel/reefer-spot-rate-forecasting/blob/main/ML_ProyectoFinal_Equipo3.ipynb)

El botón abre el notebook del proyecto final en Google Colab; basta con **Entorno de ejecución → Ejecutar todas** (Run All) y el notebook descarga solo los datos del repositorio.

Proyecto Integrador de **Inteligencia Artificial y Aprendizaje Automático (TC4058.2)**, Maestría en Inteligencia Artificial Aplicada, Tecnológico de Monterrey.

**Equipo 1**
| Integrante | Matrícula |
|---|---|
| Jaime Martínez Vogel | A01283238 |
| Héctor de Luna Aguilar | A01610988 |

## Pregunta predictiva

¿Cuál será la tarifa spot *(USD por carga)* del mes siguiente para cada lane (origen y destino) de camión refrigerado?
- El caso es el de un distribuidor de frutas y verduras frescas que necesita estimar el costo de flete para su presupuesto y para decidir cuándo conviene firmar un contrato.

## Fuentes de datos

| Fuente | Publicador | Acceso | Periodo |
|---|---|---|---|
| Refrigerated Truck Rates and Availability | USDA, Agricultural Marketing Service (AgTransport) | API pública, sin registro: https://agtransport.usda.gov/d/acar-e3r8 | Semanal, 2000 a 2026 |
| Weekly U.S. No 2 Diesel Retail Prices | U.S. Energy Information Administration (EIA) | Excel público: https://www.eia.gov/dnav/pet/hist_xls/EMD_EPD2D_PTE_NUS_DPGw.xls | Semanal, 1994 a 2026 |

Ambas son datos abiertos del gobierno de EE. UU. y no contienen datos personales.

## Estructura

```
├── ML_EDA_Equipo1.ipynb     Entregable 3: carga, EDA, calidad y preparación
├── ML_Modelo_Equipo1.ipynb  Entregable 4: entrenamiento, validación y evaluación del modelo
├── ML_ProyectoFinal_Equipo3.ipynb  Proyecto final: notebook de la demostración
├── requirements.txt       Bibliotecas con versiones fijas
├── .python-version        Versión de Python (3.11)
├── data/
│   ├── raw/               Copias originales descargadas (no se modifican)
│   └── processed/         Datos limpios (E3), predicciones de prueba y proyección (E4)
└── docs/                  Presentaciones de los entregables y del proyecto final
```

## Cómo ejecutarlo

Se necesita **Python 3.11 o superior**.

### macOS / Linux
```bash
git clone https://github.com/jmartinezvogel/reefer-spot-rate-forecasting.git
cd reefer-spot-rate-forecasting
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

### Windows (PowerShell)
```powershell
git clone https://github.com/jmartinezvogel/reefer-spot-rate-forecasting.git
cd reefer-spot-rate-forecasting
py -3.11 -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Después se abre el notebook en VS Code o Jupyter, se selecciona el kernel de ".venv" y se ejecuta con **Run All**. El orden es:

1. "ML_EDA_Equipo1.ipynb" descarga los datos y genera "data/processed/lanes_mensual.csv" (todas las lanes) y "lanes_modelo.csv".
2. "ML_Modelo_Equipo1.ipynb" verifica la versión de la copia de USDA, lee "lanes_mensual.csv", la copia semanal de USDA (para la última semana de cada mes) y el diésel en "data/raw", elige las lanes en cada corte, entrena el modelo y lo evalúa. Como los archivos ya vienen en el repositorio, este notebook se puede correr directo, sin internet.

### Google Colab
| Notebook | |
|---|---|
| Entregable 3: EDA y preparación | [![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jmartinezvogel/reefer-spot-rate-forecasting/blob/main/ML_EDA_Equipo1.ipynb) |
| Entregable 4: modelo y evaluación | [![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jmartinezvogel/reefer-spot-rate-forecasting/blob/main/ML_Modelo_Equipo1.ipynb) |
| Proyecto final: demostración | [![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jmartinezvogel/reefer-spot-rate-forecasting/blob/main/ML_ProyectoFinal_Equipo3.ipynb) |

Con el botón **Abrir en Colab** corre en la nube sin instalar nada. El E3 descarga los datos desde las APIs y el E4 clona el repositorio para tener los archivos de "data/".

## Reproducibilidad

- El notebook funciona en cualquier computadora sin cambiar nada.
- Los datos originales ya vienen en "data/raw", así que el notebook corre completo **aunque no haya internet** o la API de USDA no esté disponible. Si se borran esos archivos, el notebook los vuelve a descargar.
- USDA actualiza el dataset cada semana. Los resultados de los entregables 3 y 4 usan la copia local de **207,024 registros, con semanas del 4 de enero de 2000 al 27 de octubre de 2026** (SHA-256 "006237410b57...6bb9"). Los registros adicionales que muestra el servidor después de esa fecha son semanas publicadas más tarde; por ejemplo, los 207,109 que reportaba al correr el E3 incluyen 85 registros de la semana del 3 de noviembre de 2026, un mes incompleto que no entra al análisis. El notebook del E4 se detiene si la copia local no es esta versión.
- Para trabajar con una versión más reciente basta con borrar "data/raw/usda_refrigerated_truck_rates_raw.csv.gz" y volver a ejecutar el E3, aunque entonces los resultados ya no coincidirán con los reportados.
- Las transformaciones que aprenden parámetros (imputación, escalamiento, codificación) se ajustan *solo con el conjunto de entrenamiento* para evitar fuga de información.

## Resultados del Entregable 4

| Pronóstico en prueba (ene-2025 a oct-2026) | MAE (USD por carga) | RMSE | WAPE |
|---|---|---|---|
| Gradient Boosting que parte de la última semana del mes anterior | 242 | 428 | 4.5% |
| Baseline ingenuo (repetir el mes anterior) | 299 | 504 | 5.6% |
| Baseline estacional (mismo mes del año anterior) | 632 | 1,054 | 11.7% |
| Referencia: última semana del mes anterior | 245 | 435 | 4.5% |

El modelo reduce el error 19% contra repetir el mes anterior (intervalo de 95% de 8.1% a 29.3%) y 22% en la validación cruzada (TimeSeriesSplit por meses, 2020 a 2024). Las lanes se eligen en cada corte histórico usando solo la información disponible hasta ese momento y la prueba se usó una sola vez. La mayor parte de la mejora viene de usar la última semana publicada del mes que cerró, que ya se conoce al pronosticar; una regla que solo usa esa semana logra casi lo mismo.

## Proyecto final

"ML_ProyectoFinal_Equipo3.ipynb" es la versión resumida del Entregable 4 para la presentación ejecutiva y la demostración. Parte de los datos originales de "data/raw", no necesita internet, corre en menos de un minuto y reproduce las mismas cifras. La presentación está en "docs/ML_PresentacionFinal_Equipo3.pptx".

Incluye la retroalimentación del Entregable 4:

- **Independencia de la prueba.** La prueba se midió en tres versiones del proyecto (MAE de 338, 295 y 242). Dentro de cada versión ninguna decisión se tomó con la prueba: el algoritmo, la hoja mínima y el factor se eligieron solo con la validación cruzada, que confirmó la mejora por su cuenta (gana los 5 años, con 22% menos error). Aun así, el 19% se reporta como una estimación algo optimista.
- **¿Vale la pena el algoritmo?** Contra la regla de la última semana el modelo mejora solo 1.0% del MAE (IC 95% de 0.2% a 1.9%) y gana 11 de 22 meses de prueba. La recomendación es usar la regla como base y el modelo como retador durante tres meses en paralelo.

## Referencias

- U.S. Department of Agriculture, Agricultural Marketing Service. (2026). *Refrigerated Truck Rates and Availability* [Conjunto de datos]. AgTransport. https://agtransport.usda.gov/d/acar-e3r8
- U.S. Energy Information Administration. (2026). *Weekly U.S. No 2 Diesel Retail Prices* [Conjunto de datos]. https://www.eia.gov/dnav/pet/hist/LeafHandler.ashx?n=PET&s=EMD_EPD2D_PTE_NUS_DPG&f=W
