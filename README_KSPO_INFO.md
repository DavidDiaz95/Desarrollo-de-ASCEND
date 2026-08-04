# Datos KSPO — Instrucciones de descarga

Este documento explica cómo obtener el dataset crudo de condición física de
Corea del Sur (KSPO — Korea Sports Promotion Foundation) usado en los
notebooks `1_1__Data_Wrangler_1.ipynb` y `1_2__Data_Wrangler_2.ipynb`.

> Los CSV **no se distribuyen en este repositorio por defecto** (ver
> `.gitignore` en la raíz) para mantenerlo liviano. Sigue estos pasos para
> obtenerlos tú mismo desde la fuente oficial.

## 1. Fuente oficial

Portal: **Big Data Culture Portal** (gobierno de Corea del Sur)

```
https://www.bigdata-culture.kr/bigdata/user/data_market/detail.do?id=ace0aea7-5eee-48b9-b616-637365d665c1
```

Dataset: `국민체력100` (KS_NFA_FTNESS) — programa nacional de medición de
condición física.

## 2. Crear una cuenta

El portal requiere registro con **verificación por número de teléfono
coreano válido** (no acepta números internacionales para el SMS de
verificación).

**Formato de número de celular coreano:**

| Campo | Detalle |
|---|---|
| Prefijo | Siempre `010` (los prefijos antiguos `011`, `016`, `017`, `018`, `019` ya casi no se usan) |
| Dígitos totales | **11** (3 del prefijo + 8 del número) — sí, es impar |
| Formato con guiones | `010-XXXX-XXXX` |
| Ejemplo (no funcional) | `010-1234-5678` |

Si no tienes un número coreano propio, necesitarás conseguir uno temporal
(por ejemplo, a través de un servicio de SMS virtual que soporte numeración
+82, o pedirle el favor a un contacto en Corea) — el portal no permite
completar el registro sin esta verificación.

## 3. Descargar los archivos

1. Inicia sesión con la cuenta ya verificada.
2. Entra al dataset enlazado arriba.
3. Los archivos están divididos en **CSV mensuales** (40 en total, uno por
   mes, cubriendo el rango completo del programa).
4. El portal permite descargar **hasta 10 archivos a la vez** por selección
   — marca 10, descarga, repite hasta completar los 40. No intentes
   seleccionar más de 10: el botón de descarga masiva se desactiva o falla
   silenciosamente si excedes ese límite.

## 4. Dónde colocar los archivos

Una vez descargados, coloca los 40 CSV sin modificar en:

```
Data/raw/
```

Los nombres originales siguen el patrón:
```
KS_NFA_FTNESS_MESURE_ITEM_MESURE_INFO_YYYYMM.csv
```

## 5. Verificación rápida

Después de copiar los archivos, confirma que están completos:

```bash
ls Data/raw/*.csv | wc -l        # debe regresar 40
du -sh Data/raw/                  # ~15 MB en total (peso real verificado)
```

Si `wc -l` regresa menos de 40, revisa qué meses faltaron en la descarga por
lotes de 10 y complétalos.

## 6. Siguiente paso

Con los 40 CSV en `Data/raw/`, corre en orden:

```
notebooks/1.1. Data_Wrangler_1.ipynb
notebooks/1.2. Data_Wrangler_2.ipynb
```

Estos notebooks concatenan los 40 archivos, traducen las columnas
(coreano → inglés) y validan el esquema antes de continuar con el resto del
pipeline.

## Cita

Korea Sports Promotion Foundation (KSPO). (s.f.). *Gungmin Chearyeok 100
(KS_NFA_FTNESS)* [Conjunto de datos]. Big Data Culture Portal.
https://www.bigdata-culture.kr/bigdata/user/data_market/detail.do?id=ace0aea7-5eee-48b9-b616-637365d665c1
