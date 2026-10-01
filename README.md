# 📊 Análisis ConnectaTel: Comportamiento de Clientes de Telecomunicaciones

## 📋 Descripción del Proyecto

Este proyecto analiza el comportamiento de clientes de **ConnectaTel**, una empresa de telecomunicaciones en Latinoamérica, utilizando datos registrados hasta 2024.

**Objetivo principal:** Identificar patrones de consumo, segmentar clientes por su uso de servicios, detectar comportamientos atípicos y proporcionar recomendaciones para optimizar planes y estrategias de retención.

### Preguntas de Negocio Respondidas
- ¿Qué segmentos de clientes muestran mayor o menor uso de llamadas y mensajes?
- ¿Qué usuarios presentan valores atípicos que indiquen fraude o errores de registro?
- ¿Cómo varía el uso según la edad y el tipo de plan contratado?
- ¿Qué patrones pueden ayudar a diseñar mejores planes y mejorar la satisfacción del cliente?

---

## 📁 Contenido del Repositorio

```
├── S7_Version-Estudiante-Project-ConnectaTel.ipynb    # Notebook principal con análisis completo
├── README.md                                           # Este archivo
├── data/                                               (opcional)
│   ├── plans.csv                                       # Información de planes
│   ├── users_latam.csv                                 # Información de clientes
│   └── usage.csv                                       # Detalle de uso de servicios
└── outputs/                                            (opcional)
    ├── gráficos/                                       # Visualizaciones generadas
    └── análisis_resumidos/                             # Resúmenes de análisis
```

### Datasets Utilizados

| Dataset | Descripción | Registros | Columnas |
|---------|-------------|-----------|----------|
| **plans.csv** | Planes disponibles con precio, minutos incluidos, GB | 2 | 8 |
| **users_latam.csv** | Información de clientes (edad, ciudad, fecha registro, plan, churn) | 4,000 | 8 |
| **usage.csv** | Detalle de uso (llamadas y mensajes) | 40,000 | 6 |

---

## 🔍 Etapas del Análisis

### 1️⃣ **Exploración de Datos**
- Carga de 3 datasets desde `/datasets/`
- Revisión de estructura (filas, columnas, tipos de datos)
- Detección inicial de problemas de calidad

### 2️⃣ **Limpieza de Datos**
- **Valores nulos:** 
  - `city` (11% faltante) → Imputado como 'Unknown'
  - `duration` y `length` → Conservados (Missing at Random)
  - `date` en usage (0.125% faltante) → Conservados
  
- **Valores sentinela:**
  - `age`: 55 valores (-999) → Reemplazados con mediana
  - `city`: Sentinels (?) → Imputados como 'Unknown'
  
- **Registros inválidos:**
  - 40 registros del año 2026 (usuarios) → Eliminados
  - Análisis enfocado **exclusivamente en 2024**

### 3️⃣ **Construcción de Tabla Unificada**
- Combinación de `users` + agregación de `usage` por usuario
- Tabla final `user_profile` con 1,330 registros únicos de 2024
- Columnas clave: edad, ciudad, plan, uso (llamadas, mensajes, minutos)

### 4️⃣ **Análisis Estadístico y Segmentación**
- Resumen estadístico de variables numéricas (media, mediana, desviación estándar, valores atípicos)
- Detección de outliers con método IQR
- Segmentación de clientes por tipo de uso:
  - **Bajo uso:** < 5 llamadas Y < 5 mensajes
  - **Uso medio:** < 10 llamadas Y < 10 mensajes
  - **Alto uso:** Resto

### 5️⃣ **Visualizaciones**
- Histogramas de distribución de edad por plan
- Boxplots de variables numéricas para detectar outliers
- Countplots de distribución de planes por grupo de edad
- Gráficos de distribución de uso por segmentación

---

## 🚀 Cómo Reproducir el Análisis

### Requisitos
```python
pandas
seaborn
matplotlib
numpy
```

Instala con:
```bash
pip install pandas seaborn matplotlib numpy
```

### Pasos para Ejecutar

1. **Abre el notebook:**
   ```bash
   jupyter notebook S7_Version-Estudiante-Project-ConnectaTel.ipynb
   ```

2. **Ejecuta las celdas en orden:**
   - **Paso 1:** Carga de datos (importa las 3 bases de datos)
   - **Paso 2:** Exploración de calidad de datos
   - **Paso 3:** Limpieza y transformación de datos
   - **Paso 4:** Análisis estadístico y segmentación
   - **Paso 5:** Visualizaciones e interpretación

3. **Los datasets se cargan automáticamente** desde `/datasets/` usando:
   ```python
   plans = pd.read_csv('/datasets/plans.csv')
   users = pd.read_csv('/datasets/users_latam.csv')
   usage = pd.read_csv('/datasets/usage.csv')
   ```

### Notas Importantes
- El notebook contiene toda la lógica de transformación de datos
- No requiere archivos externos adicionales (excepto los CSVs de `/datasets/`)
- Todos los gráficos se generan dentro del notebook
- El análisis está comentado para facilitar la comprensión

---

## 📊 Principales Hallazgos

### Calidad de Datos (2024)
- **Registros limpios:** 1,330 usuarios únicos
- **Outliers detectados:**
  - `cant_mensajes`: 19 outliers (1.43%)
  - `cant_llamadas`: 12 outliers (0.90%)
  - `cant_minutos_llamada`: 34 outliers (2.56%)
  - **Decisión:** Mantener (clientes valiosos, no errores)

### Segmentación de Clientes
Los clientes se distribuyen en 3 categorías según su uso de servicios, permitiendo diseñar estrategias diferenciadas.

### Variabilidad por Edad y Plan
Se observan patrones distintos en el consumo según el grupo de edad y el tipo de plan contratado.

---

## 🎯 Recomendaciones Futuras

1. Análisis temporal de uso (patrones mensuales/semanales)
2. Optimización de planes según segmentación identificada
3. Análisis de impacto de cambios en planes

---

## 👤 Autor

**Rodrigo Gutiérrez** - Análista de Datos en Formación

---

## 📝 Notas Metodológicas

- **Período de análisis:** Año 2024 (único período con datos confiables)
- **Método IQR para outliers:** Q3 ± 1.5 × IQR
- **Imputación de valores faltantes:** Por contexto (moda, mediana, categoría 'Unknown')
- **Combinación de datos:** LEFT JOIN entre usuarios y agregación de uso

---

## ⚖️ Licencia

Proyecto educativo - Tripleten

---

**Última actualización:** Octubre 2026
