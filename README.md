# 🏦 Bank Marketing — Predicción de Suscripción a Plazo Fijo

Trabajo Final de **Introducción a la Ciencia de Datos (ICD 2026)** — Licenciatura en Ciencia de Datos, Universidad Católica Argentina (sede Rosario).

Entrenamos y comparamos dos modelos de Machine Learning supervisado para predecir si un cliente de un banco portugués suscribirá un depósito a plazo fijo a partir de los datos de campañas telefónicas previas (clasificación binaria).

## 👥 Autores

- Agustín Casal
- Teo Martínez
- Francisco Medrano
- Martina Saldías

## 📊 El dataset

`Bank_Marketing_Dataset_TF.csv` — 11.162 clientes, 16 variables predictoras + 1 variable objetivo (`deposit`).

| | |
|---|---|
| Registros | 11.162 |
| Variables | 16 features + target |
| Suscribió (`yes`) | 5.289 (47,4 %) |
| No suscribió (`no`) | 5.873 (52,6 %) |
| Origen | Bank Marketing Dataset (Kaggle), provisto por la cátedra |

Las clases están **balanceadas** (~47 / 53), por lo que el modelo está obligado a aprender diferencias reales entre ambos grupos en lugar de predecir siempre la clase mayoritaria.

## 🗂️ Estructura del repositorio

```
.
├── README.md
├── notebook/
│   └── Trabajo_Final_ICD_Bank_Marketing.ipynb   # Notebook de Colab (análisis completo)
├── data/
│   └── Bank_Marketing_Dataset_TF.csv            # Dataset provisto por la cátedra
├── informe/
│   ├── Informe_Bank_Marketing.pdf               # Informe en lenguaje coloquial
│   └── informe_bank_marketing.html              # Versión HTML del informe
└── .gitignore
```

## ⚙️ Metodología

1. **Carga e inspección** — lectura del CSV, tipos de datos, detección de nulos y estadística descriptiva.
2. **Análisis exploratorio (EDA)** — distribuciones, box plots, violin plots, tasas de suscripción por categoría y mapa de calor de correlaciones.
3. **Limpieza y preparación** — imputación de nulos con la **mediana**, codificación de variables categóricas con **Label Encoding** y escalado de variables numéricas con **StandardScaler**.
4. **División** — `train_test_split` 80 / 20 **estratificado** (`random_state=42`) para mantener la proporción de clases.
5. **Modelado** — Regresión Logística y Random Forest.
6. **Evaluación y comparación** — matriz de confusión, curva ROC y métricas sobre el conjunto de prueba.

## 🤖 Modelos

| Modelo | Configuración |
|---|---|
| Regresión Logística | `max_iter=1000`, `C=1.0` |
| Random Forest | `n_estimators=200`, `max_features='sqrt'`, `random_state=42` |

## 📈 Resultados (conjunto de prueba, 20 %)

| Métrica | Regresión Logística | Random Forest |
|---|:---:|:---:|
| Accuracy | 0,782 | **0,842** |
| AUC-ROC | 0,861 | **0,911** |
| Precision | 0,777 | **0,819** |
| Recall | 0,759 | **0,857** |
| F1-Score | 0,768 | **0,837** |

**🏆 Random Forest** supera a la Regresión Logística en todas las métricas. La diferencia es más marcada en AUC-ROC (0,911 vs. 0,861) y en Recall (0,857 vs. 0,759), lo que en términos de negocio significa **menos clientes potenciales perdidos**.

## 🔍 Hallazgos clave

- **Duración de la llamada**: el predictor más fuerte (correlación +0,45 con `deposit`). Quienes suscribieron hablaron ~538 s en promedio vs. ~224 s de quienes no. ⚠️ Solo se conoce *después* de la llamada, por lo que no es accionable para decidir a quién contactar.
- **Historial previo (`poutcome`)**: un cliente que suscribió en una campaña anterior tiene **91,3 %** de probabilidad de volver a hacerlo.
- **Ocupación**: estudiantes (74,7 %) y jubilados (66,3 %) son los más receptivos; los obreros (36,4 %), los menos.
- **Edad**: patrón en forma de "U" — jóvenes (18-25) y mayores de 65 suscriben más que el grupo de mediana edad.

## 🚧 Limitaciones

- `duration` es muy predictiva pero no es accionable antes de la llamada; un modelo más realista debería evaluarse sin ella.
- Los datos provienen de un banco portugués en un período específico; el contexto económico y cultural puede no generalizar.
- El modelo aprende patrones del pasado: ante cambios de estrategia o mercado conviene reentrenarlo.

## ▶️ Cómo reproducir

### En Google Colab (recomendado)
1. Abrir el notebook de la carpeta `notebook/` en [Google Colab](https://colab.research.google.com).
2. Subir `data/Bank_Marketing_Dataset_TF.csv` al entorno (o montar Google Drive).
3. Ejecutar las celdas en orden (`Entorno de ejecución → Ejecutar todo`).

### En local
```bash
pip install numpy pandas matplotlib seaborn scikit-learn
jupyter notebook notebook/Trabajo_Final_ICD_Bank_Marketing.ipynb
```

> **Nota:** asegurate de que la ruta de lectura del CSV en el notebook coincida con la ubicación real del archivo (`data/Bank_Marketing_Dataset_TF.csv`).

## 🛠️ Tecnologías

Python · pandas · NumPy · scikit-learn · Matplotlib · seaborn · Google Colab
