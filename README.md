# 🏆 Modelo Final - Experimento 9500

## Resultado

| Métrica | Valor |
|---------|-------|
| **Mejor ganancia Kaggle** | 4.349 |
| **Corte óptimo** | 950 |
| **AUC validación** | ~0.937 |

---

## Cómo reproducir

### 1. Requisitos
- Google Colab con Runtime R
- Cuenta de Kaggle configurada
- Google Drive montado

### 2. Ejecutar en Colab

```r
# 1. Montar Drive (en Python primero)
from google.colab import drive
drive.mount('/content/.drive')

# 2. Cambiar Runtime a R

# 3. Ejecutar script
source("/content/buckets/b1/modelo_final/script_final.R")
```

### 3. Estructura de carpetas esperada

```
/content/buckets/b1/
├── datasets/
│   └── gerencial_competencia_2025.csv.gz
├── kaggle/
│   └── kaggle.json
└── exp/
    └── WF9500/
        ├── prediccion.txt
        ├── BO_log.txt
        └── kaggle/
            └── KA9500_*.csv
```

---

## Metodología

### 1. Catastrophe Analysis
- Mes afectado: **202006** (pandemia COVID-19)
- 12 variables con valores anómalos → asignadas como NA

### 2. Feature Engineering

#### Variables de negocio (basadas en Diccionario de Datos):
- **Rentabilidad**: margen_neto, rentabilidad_sin_comisiones, ratio_comisiones_rent
- **Tarjetas**: visa_ratio_uso_limite, master_ratio_uso_limite, ticket_promedio_tarjeta
- **Canales**: ratio_digital_telefono, intensidad_digital
- **Cliente**: valor_por_antiguedad, vip_rentable, joven_digital
- **Salud financiera**: ratio_ahorro_deuda, capacidad_pago, liquidez_total

#### Variables temporales:
- Lags: orden 1 y 2
- Deltas: cambio respecto a lag 1 y 2
- TREND: trend2 (tendencia), accel (aceleración)

### 3. Training Strategy
- **Training**: 14 meses (202005 - 202106)
- **Validación**: 202107
- **Predicción**: 202109

### 4. Optimización
- **Método**: Bayesian Optimization (mlrMBO)
- **Iteraciones**: 50
- **Hiperparámetros optimizados**: num_leaves, min_data_in_leaf, feature_fraction, bagging_fraction, bagging_freq

### 5. Ensemble
- **Semillerío**: 5 modelos con diferentes semillas
- **Agregación**: Promedio de probabilidades

---

## Hiperparámetros óptimos

```json
{
  "num_leaves": 231,
  "min_data_in_leaf": 19,
  "feature_fraction": 0.83072,
  "bagging_fraction": 0.8602651,
  "bagging_freq": 4,
  "num_iterations": 1009,
}
```
---

## Submits seleccionados para evaluación final

1. **KA9500_950** (4.349) - Mejor resultado

