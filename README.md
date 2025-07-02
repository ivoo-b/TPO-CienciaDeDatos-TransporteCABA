# TPO: Optimización de Frecuencias del Transporte Público en CABA

**Materia:** Ciencia de Datos | UADE  
**Integrantes:**
*   **[Adan Rodriguez]** - *[1174251]*
*   **[Ivo Biscardi]** - *[1132206]*
*   **[Bratti, Conrado Stefano] - *[1134447]*
*   **[Nicolás Ferreyra] - *[1143773]*

---

## Comprensión del Negocio

### El Problema
La red de transporte público de CABA enfrenta un desequilibrio crónico entre la oferta de servicios y la demanda real de pasajeros. Esto se manifiesta en dos problemas clave:
- **Costos Operativos Elevados:** Unidades circulando con baja ocupación en horarios y zonas de menor demanda.
- **Experiencia de Usuario Deficiente:** Saturación y largas esperas en puntos de alta congestión durante las horas pico.

### Hipótesis
Creemos que es posible **predecir con alta precisión la cantidad de viajes en colectivo** para cualquier franja horaria y comuna de la Ciudad de Buenos Aires. Un modelo de regresión preciso que prediga la cantidad de viajes por hora y por comuna, permitiendo una planificación de rutas más inteligente y eficiente.

## Metodología y Pipeline de Datos

El proyecto sigue el ciclo de vida CRISP-DM, abarcando desde la preparación de los datos hasta el modelado y la evaluación.

### Fuentes de Datos
- **`Viajes 2023.csv`**: Dataset transaccional con más de 7 millones de registros de viajes del sistema SUBE.
- **`Paradas de Colectivo.csv`**: Dataset geoespacial con la ubicación y comuna de más de 6,900 paradas en CABA.

### Preparación y Limpieza de Datos
1.  **Limpieza:** Conversión de tipos de datos, manejo de nulos y estandarización de nombres de columnas.
2.  **Filtrado:** Se seleccionaron únicamente los viajes que incluían al menos una etapa en colectivo.
3.  **Ingeniería de Características :** Se enriqueció el dataset de viajes asignando una `comuna_origen` a cada registro. Para ello, se entrenó un modelo **K-Nearest Neighbors (k=1)** utilizando las ubicaciones de las paradas como referencia.
4.  **Agregación:** Se transformó el dataset de viajes individuales en una tabla de demanda agregada, que resume la `cantidad_viajes` por `comuna_origen` y `hora`.

### Modelado Predictivo
- **Tipo de Problema:** Regresión.
- **Algoritmo Seleccionado:** **`RandomForestRegressor`** Se eligió por su alta capacidad para capturar relaciones no lineales complejas (como los picos de demanda) y su robustez, sin necesidad de escalar los datos.
- **División de Datos:** El conjunto de datos de demanda se dividió en 80% para entrenamiento y 20% para prueba, asegurando una evaluación objetiva del modelo.

### Evaluación

- **Rendimiento Cuantitativo:**
  - **R² = 0.936**: Nuestro modelo explica el **93.6%** de la variabilidad en la demanda de viajes.
  - **RMSE = 6488 Viajes**: En promedio, nuestras predicciones se desvían solo en ±6488 viajes del valor real.
