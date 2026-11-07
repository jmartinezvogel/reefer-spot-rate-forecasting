# Pronóstico mensual de tarifas spot de camión refrigerado por lane

[![Abrir en Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jmartinezvogel/reefer-spot-rate-forecasting/blob/main/ML_EDA_Equipo1.ipynb)

Proyecto Integrador de **Inteligencia Artificial y Aprendizaje Automático (TC4058.2)**, Maestría en Inteligencia Artificial Aplicada, Tecnológico de Monterrey.

**Equipo 1**
| Integrante | Matrícula |
|---|---|
| Jaime Martínez Vogel | A01283238 |
| Héctor de Luna Aguilar | A01610988 |

## Pregunta predictiva

¿Cuál será la tarifa spot (USD por carga) del mes siguiente para cada lane (origen y destino) de camión refrigerado? El caso es el de un distribuidor de frutas y verduras frescas que necesita estimar el costo de flete para su presupuesto y para decidir cuándo conviene amarrar un contrato.

## Fuentes de datos

| Fuente | Publicador | Acceso | Periodo |
|---|---|---|---|
| Refrigerated Truck Rates and Availability | USDA, Agricultural Marketing Service (AgTransport) | API pública, sin registro: https://agtransport.usda.gov/d/acar-e3r8 | Semanal, 2000 a 2026 |
| Weekly U.S. No 2 Diesel Retail Prices | U.S. Energy Information Administration (EIA) | Excel público: https://www.eia.gov/dnav/pet/hist_xls/EMD_EPD2D_PTE_NUS_DPGw.xls | Semanal, 1994 a 2026 |

Ambas son datos abiertos del gobierno de EE. UU. y no contienen datos personales.

## Estructura

```
├── ML_EDA_Equipo1.ipynb   Entregable 3: carga, EDA, calidad y preparación
├── requirements.txt       Bibliotecas con versiones fijas
├── .python-version        Versión de Python (3.11)
├── data/
│   ├── raw/               Copias originales descargadas (no se modifican)
│   └── processed/         Datos limpios generados por el notebook
└── docs/                  Presentación y notebook exportado a HTML
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

Después se abre "ML_EDA_Equipo1.ipynb" en VS Code o Jupyter, se selecciona el kernel de ".venv" y se ejecuta con **Run All**.

### Google Colab
Con el botón **Abrir en Colab** de arriba el notebook corre en la nube sin instalar nada. En ese caso los datos se descargan directamente desde las APIs.

## Reproducibilidad

- El notebook usa rutas relativas, por lo que funciona en cualquier computadora sin cambiar nada.
- Los datos originales ya vienen en "data/raw", así que el notebook corre completo **aunque no haya internet** o la API de USDA no esté disponible. Si se borran esos archivos, el notebook los vuelve a descargar.
- USDA actualiza el dataset cada semana. Para trabajar con la versión más reciente basta con borrar "data/raw/usda_refrigerated_truck_rates_raw.csv.gz" y volver a ejecutar.
- Las transformaciones que aprenden parámetros (imputación, escalamiento, codificación) se ajustan solo con el conjunto de entrenamiento para evitar fuga de información.

## Referencias

- U.S. Department of Agriculture, Agricultural Marketing Service. (2026). *Refrigerated Truck Rates and Availability* [Conjunto de datos]. AgTransport. https://agtransport.usda.gov/d/acar-e3r8
- U.S. Energy Information Administration. (2026). *Weekly U.S. No 2 Diesel Retail Prices* [Conjunto de datos]. https://www.eia.gov/dnav/pet/hist/LeafHandler.ashx?n=PET&s=EMD_EPD2D_PTE_NUS_DPG&f=W
