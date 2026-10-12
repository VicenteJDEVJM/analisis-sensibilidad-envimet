# Análisis de sensibilidad paramétrica en simulaciones microclimáticas

Pipeline en Python que ingiere, limpia y analiza salidas de simulación microclimática
ENVI-met, y cuantifica cuánto cambian los indicadores de confort térmico cuando se mueve
un parámetro que casi nadie declara.

Desarrollado como tesis de **Magíster en Ciencias de la Ingeniería** en la Universidad
Diego Portales. Grado conferido en septiembre de 2026 con nota **6,4 sobre 7,0**,
Distinción Máxima.

---

## El hallazgo

Al comparar escenarios de simulación surgió un problema de método: una **convención de
cálculo que la literatura rara vez declara explícitamente** desplazaba los indicadores de
confort térmico **hasta 2,77 °C**.

Eso no es ruido. Es suficiente para mover un resultado de una categoría de confort a otra
y, en algunos casos, para invertir el sentido de la conclusión. Si dos estudios eligen
convenciones distintas y ninguno lo declara, sus resultados no son comparables aunque
ambos estén bien ejecutados.

El pipeline existe para hacer ese efecto visible y medible, no para esconderlo detrás de
un promedio.

---

## Qué hace el código

**Más de 1.500 líneas**, siete versiones iteradas a lo largo de dos años.

| Etapa | Qué resuelve |
|---|---|
| **Ingesta** | Lee salidas NetCDF de ENVI-met con `xarray`, de varios escenarios y pasos temporales |
| **Limpieza** | Extrae el nivel peatonal (k=2, 1,5 m), maneja celdas de edificio como `NaN` explícitos y alinea rejillas entre escenarios |
| **Análisis** | 16 variables sobre 5 escenarios, con verificación de supuestos y pruebas post hoc, no solo diferencias de medias |
| **Agregación** | Media o mediana sobre el eje temporal, con la opción de restringir a horas diurnas (08:00 a 19:00 locales) |
| **Visualización** | Mapas espaciales absolutos y de diferencia, categorías de confort PET, y líneas de viento (`streamplot` con componentes U y V del mismo nivel peatonal) |

**Stack:** Python, `numpy`, `pandas`, `xarray`, `matplotlib`, `scipy`.

---

## Decisiones de ingeniería que vale la pena mirar

**Las versiones no se sobrescriben.** Cada cambio de criterio vive en una copia nueva y
el encabezado del archivo dice qué cambió, por qué y quién lo pidió. Cuando el profesor
guía pidió agregar líneas de viento sobre todos los mapas, eso quedó aislado en una
versión y la anterior siguió intacta, porque otros scripts ya la importaban. El historial
de decisiones está en el código, no en la memoria de nadie.

**Un error de orientación documentado, no borrado.** Una versión dejó anotada la sospecha
de que la rosa de los vientos apuntaba al lado equivocado. La verificación visual lo
confirmó semanas después y la corrección quedó registrada con fecha. El rastro completo
sigue ahí.

**Los edificios dejaron de ser blancos.** Las celdas sin dato se pintaban transparentes y
se confundían con el centro de la escala azul-blanco-rojo. Pasaron a un gris neutro vía
`cmap.set_bad()`. Un detalle de visualización que cambiaba cómo se leía el mapa.

---

## Sobre los datos

Este repositorio contiene el código de análisis. **Las salidas de simulación no se
publican**: son datos de investigación del proyecto y pesan varios gigabytes. El código
está escrito para leer la estructura NetCDF estándar de ENVI-met.

---

## Autor

**Vicente Del Escoval Von Johnn-Marteville**
Ingeniero Civil Industrial y Magíster en Ciencias de la Ingeniería, Universidad Diego
Portales. Ambos con Distinción Máxima.

Análisis de datos para la toma de decisiones. Python, SQL, Power BI, Excel avanzado.
