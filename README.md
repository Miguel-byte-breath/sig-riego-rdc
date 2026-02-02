# S.I.G. Riego Pro v1.1 (API Connect)

Sistema de Información Geográfica para la Gestión Integral de Recursos Hídricos.  
Herramienta avanzada de ingeniería agronómica aplicada para el cálculo automático del balance hídrico mensual y la planificación semanal de riego, incorporando estrategias de Riego Deficitario Controlado (RDC) y redistribución térmica diaria, mediante conexión directa y resiliente con AEMET OpenData.

---

## Evolución del Sistema (v1.1)

La versión 1.1 amplía la lógica clásica de balance hídrico incorporando:

- Ajuste estratégico de riego mediante RDC (% sobre $NH_n$).
- Programación semanal basada en semanas ISO reales.
- Redistribución térmica diaria del riego dentro de cada mes.
- Exportación profesional completa (mensual y semanal).

El sistema mantiene su objetivo principal: robustez climática, coherencia agronómica y aplicabilidad operativa en campo.

---

## Automatización y Resiliencia vía API AEMET

### Validación automática de estaciones meteorológicas

A partir de las coordenadas de la parcela (latitud / longitud), el sistema ejecuta:

- Búsqueda por proximidad:  
  Identificación de estaciones cercanas mediante cálculo de distancia euclidiana en coordenadas decimales.

- Validación técnica de datos:  
  Verificación directa con la API de AEMET.  
  Si la estación más próxima no dispone de datos válidos, se activa un bucle de resiliencia que evalúa automáticamente las siguientes estaciones más cercanas hasta encontrar una fuente fiable.

---

## Motor de Estabilidad Climática (Media Trienal)

Para evitar sesgos derivados de años anómalos, el sistema implementa un enfoque de climatología media representativa.

### Período de análisis

- Se utilizan los últimos 36 meses completos (3 años naturales).
- El cálculo se basa en un mes climático medio, evitando depender de un solo año.

### Gestión de datos incompletos

- Los meses sin datos no bloquean el proceso.
- Las medias se calculan únicamente con los registros válidos disponibles.
- Se filtran valores erróneos o negativos.

Este enfoque garantiza continuidad de cálculo y estabilidad del diseño de riego.

---

## Funcionalidades Core

### 1. Balance Hídrico Agronómico Mensual (Sección 2)

Cálculo técnico completo de las necesidades hídricas del cultivo:

- Evapotranspiración de referencia ($ET_0$), obtenida de AEMET y promediada mediante climatología trienal.
- Coeficiente de cultivo ($K_c$), definido por cultivo y etapa fenológica.
- Evapotranspiración del cultivo ($ET_c$), calculada como:  
  $ET_c = ET_0 \cdot K_c$
- Precipitación efectiva ($P_e$), estimada mediante metodología USDA (SCS).
- Necesidades hídricas netas ($NH_n$), expresadas en $m^3/ha$.

---

### 2. Estrategia de Riego Deficitario Controlado (RDC)

El sistema permite definir una estrategia de ajuste sobre las necesidades netas:

- Introducción de porcentajes mensuales sobre $NH_n$.
- Cálculo automático del volumen RDC mensual en $m^3/ha$.
- Limitación automática:  
  $0 \le RDC \le 100\% \cdot NH_n$
- Control visual de:
  - Volumen total planificado RDC.
  - Recursos disponibles y posibles excedentes o déficits.

La aplicación de cambios se realiza explícitamente mediante el botón "Actualizar".
### Nota técnica sobre coeficientes de cultivo y dotaciones reales

- $K_c$ representa la **demanda potencial** del cultivo, adaptada a **riego localizado** (con evaporación del suelo reducida, i.e. `$K_e$` bajo).
- **RDC** aplica la **restricción real de dotación** (gestión del recurso), ajustando el volumen respecto a las necesidades netas calculadas

---

### 3. Programación Semanal de Riego Neto (Sección 3)

Conversión del plan mensual RDC en una planificación semanal operativa:

- Uso de semanas ISO reales (lunes–domingo).
- Las semanas parciales del ciclo solo contabilizan los días incluidos entre las fechas de inicio y fin del cultivo.
- Cálculo del riego neto semanal a partir de la suma de los valores diarios.
- Representación gráfica mediante curva semanal.

---

## Redistribución Térmica Diaria del RDC (Opcional)

Funcionalidad avanzada orientada a mejorar la coherencia fisiológica del riego dentro de cada mes.

### Principio de funcionamiento

- El RDC mensual se reparte diariamente, no por semanas fijas.
- Se aplica un gradiente diario lineal:
  - Ascendente si la temperatura media mensual aumenta respecto al mes anterior.
  - Descendente si la temperatura media mensual disminuye.
- Intensidad del ajuste: ±10 % respecto al promedio diario.
- El volumen mensual total se conserva exactamente mediante normalización matemática.

### Beneficios agronómicos

- Evita repartos semanales artificialmente planos.
- Introduce sensibilidad térmica sin aumentar la complejidad operativa.
- Mejora la representatividad fisiológica del riego aplicado.

La redistribución térmica se activa o desactiva mediante un checkbox explícito y se aplica junto con el botón "Actualizar".

---

## Visualización y Exportación

- Gráficos dinámicos mediante Chart.js:
  - Comparativa climática y de necesidades ($P_e$, $NH_n$, asignación).
  - Curva de planificación semanal.
- Exportación profesional a Excel (.xlsx):
  - Balance mensual completo.
  - Programación semanal detallada (semana, fechas y volumen).

---

## Stack Tecnológico

- Datos climáticos: AEMET OpenData (REST API).
- Frontend: HTML5 y JavaScript ES6+.
- Visualización: Chart.js, chartjs-plugin-datalabels.
- Exportación: SheetJS (xlsx).
- Estilo: CSS3 con interfaz técnica premium.

---

## Configuración del Desarrollador

Para activar el sistema es necesario configurar una API Key válida de AEMET:

```javascript
const API_KEY = "TU_AEMET_API_KEY";
