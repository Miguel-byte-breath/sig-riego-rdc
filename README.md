# 🌾 S.I.G. Riego Pro v1.0 (API Connect)

**Sistema de Información Geográfica para la Gestión Integral de Recursos Hídricos.** Una herramienta avanzada de ingeniería agronómica que automatiza el balance hídrico mensual y semanal mediante la conexión directa y resiliente con los servicios de **AEMET OpenData**.



## 🚀 Innovación: Automatización y Resiliencia vía API

Esta versión 1.0 elimina la dependencia de archivos externos (JSON manuales), integrando un motor de obtención de datos climáticos en tiempo real. 

### 📡 Validación Automática de Estaciones
Al introducir las coordenadas (Latitud/Longitud), el sistema inicia un protocolo de doble verificación:
1.  **Cercanía Geográfica:** Identifica la estación más próxima mediante el cálculo de distancias geoespaciales (Haversine).
2.  **Validación Técnica:** Conecta con la infraestructura de AEMET para confirmar la disponibilidad de datos. Si la estación principal carece de registros, el sistema activa un **bucle de resiliencia** que itera automáticamente entre las 5 estaciones más cercanas hasta validar una fuente fiable.

## 🛰️ Motor de Estabilidad Climática (Media Trienal)

Para garantizar un diseño de riego robusto frente al cambio climático y anomalías meteorológicas puntuales, el software implementa una lógica de **procesamiento histórico profundo**:

### 📅 Período de Análisis: 36 Meses
El sistema solicita mediante el endpoint de la API los datos de los **últimos 3 años naturales completos**. El software no utiliza un solo año de forma aislada para evitar sesgos por años extremadamente secos o húmedos.



### 🛠️ Tratamiento de Datos Ausentes (Data Integrity)
En el sector agrícola, es común que las estaciones sufran fallos técnicos temporales. **Riego Pro v1.0** gestiona estas lagunas de forma inteligente:
* **Contabilización Dinámica:** Si un mes concreto falta en uno de los tres años, el sistema calcula la media aritmética dividiendo únicamente por los registros válidos encontrados (`medias[m].count++`).
* **Filtrado de Nulos:** Se descartan automáticamente valores negativos o erróneos, asegurando que el **"Mes Típico Medio"** sea matemáticamente coherente.
* **Garantía de Cálculo:** El proceso nunca se detiene por falta de un dato mensual; el algoritmo se auto-ajusta para ofrecer la mejor aproximación posible con la serie histórica disponible.



## 🛠️ Funcionalidades Core

### 1. Balance Hídrico Agronómico
* **Evapotranspiración del Cultivo ($ET_c$):** Determinada por la $ET_o$ local y coeficientes $K_c$ específicos por etapa fenológica.
* **Precipitación Efectiva ($P_e$):** Cálculo mediante el método de la **USDA** (SCS), optimizando el aprovechamiento real del agua de lluvia.
* **Necesidades Netas ($NH_n$):** Cálculo preciso del déficit hídrico en $m^3/ha$.



### 2. Programación Semanal Operativa
* Desglose operativo del plan mensual en semanas naturales.
* Gráfico de líneas dinámico para la visualización de la demanda hídrica a lo largo del ciclo.

## 📊 Visualización y Exportación
* **Reportes Dinámicos:** Gráficos comparativos mediante **Chart.js** (Lluvia vs. Necesidades vs. Asignación).
* **Exportación Profesional:** Generación de archivos **.xlsx (Excel)** detallados para planes de riego y auditorías de gestión de recursos.

## 💻 Stack Tecnológico
* **APIs:** AEMET OpenData (REST API).
* **Frontend:** Vanilla JavaScript (ES6+), CSS3 Premium UI.
* **Librerías:** Chart.js, SheetJS, Chartjs-plugin-datalabels.

---

## ⚙️ Configuración del Desarrollador
Para activar el sistema, es necesario integrar una API Key válida en la sección de configuración global del script:

```javascript
const API_KEY = "TU_AEMET_API_KEY";
