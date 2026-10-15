# Power BI - Módulo 6 | Unidad 2
## Extracción, limpieza y transformación de datos

### Descripción del proyecto

En este trabajo práctico utilicé Power BI Desktop y Power Query para preparar un archivo de ventas exportado desde un sistema de gestión.

El objetivo fue revisar la calidad de los datos, corregir los tipos de datos, tratar valores faltantes y eliminar registros duplicados. También separé la información de clientes y ventas para organizar mejor la estructura y facilitar futuros análisis.

## Archivos de la entrega

### Documentación principal

**[Ver documentación completa en PDF](Pipeline_ETL_Mermel_Dylan.pdf)**

El documento incluye las explicaciones del procedimiento, las decisiones tomadas durante la limpieza y las capturas de pantalla de Power Query y del modelo de datos.

### Proyecto de Power BI

**[Descargar proyecto de Power BI (.pbix)](https://drive.google.com/file/d/1oU7qFZRlORuooFUsM5qdzEx2nLRyB4I9/view?usp=drive_link)**

El archivo está disponible en Google Drive y permite revisar las consultas, los pasos aplicados y las relaciones entre las tablas en Power BI Desktop.

## 1. Importación de datos

Comencé importando el archivo `Ventas_export_legacy.xlsx` mediante el conector de Excel de Power BI.

Una vez cargado, ingresé al Editor de Power Query para revisar las columnas y detectar posibles problemas antes de comenzar con las transformaciones.

El archivo original contenía **949 registros y 20 columnas**.

## 2. Renombrar columnas

El archivo tenía nombres técnicos como `COD_CLI`, `F_VTA` y `TOT_VTA`, que no resultaban demasiado descriptivos.

Para mejorar la lectura de la información, cambié los encabezados por nombres en español utilizando el formato `snake_case`.

Algunos ejemplos:

| Nombre original | Nombre nuevo |
|---|---|
| COD_OP | id_operacion |
| COD_CLI | id_cliente |
| NOM_CLI | nombre_cliente |
| F_VTA | fecha_venta |
| COD_PROD | id_producto |
| CANT | cantidad |
| PU_VTA | precio_unitario |
| DTO_PCT | descuento_pct |
| TOT_VTA | total_venta |

De esta manera, las columnas son más fáciles de identificar al realizar consultas o construir reportes.

## 3. Corrección de tipos de datos

Revisé los tipos de datos asignados por Power Query y realicé los ajustes necesarios.

- **Fecha:** `fecha_venta` y `fecha_alta_cliente`.
- **Número entero:** `cantidad`.
- **Número decimal:** precios, descuentos e importes.
- **Texto:** nombres, correos, teléfonos, códigos identificadores y categorías.

En el caso de los identificadores, decidí conservarlos como texto porque representan códigos y no cantidades sobre las que necesite realizar operaciones matemáticas.

Esta revisión es importante porque utilizar un tipo de dato incorrecto puede generar problemas al aplicar filtros, realizar cálculos o analizar fechas.

## 4. Tratamiento de valores nulos

Durante la revisión encontré valores faltantes en distintas columnas.

En `descuento_pct`, reemplacé los valores nulos por cero, considerando para este ejercicio que un descuento no informado equivale a una venta sin descuento.

En `total_venta`, decidí no reemplazar directamente los valores vacíos por cero porque eso podía afectar los resultados de facturación.

Para recuperar los importes faltantes creé una columna llamada `total_venta_calculado`, utilizando la siguiente fórmula:

`cantidad * precio_unitario * (1 - descuento_pct)`

La columna conserva el importe original cuando está disponible y realiza el cálculo cuando falta.

Después identifiqué cuatro registros sin información de operación, cliente ni importe, por lo que decidí excluirlos.

En cambio, conservé los valores faltantes de correo electrónico y teléfono, ya que no impedían identificar las operaciones comerciales.

## 5. Eliminación de duplicados

Revisé los registros que compartían un mismo identificador de operación.

Primero comprobé que tener el mismo ID no significa necesariamente que una venta esté duplicada, porque una operación podría incluir más de un producto.

Por ese motivo, eliminé las filas completamente duplicadas considerando todas las columnas, en lugar de utilizar solamente `id_operacion`.

Este criterio permitió evitar que se contabilizaran registros repetidos sin descartar operaciones diferentes por compartir un identificador.

## 6. Estandarización de datos

También encontré diferencias en la escritura de los canales de venta, por ejemplo `ONLINE`, `online` y `Sucursal`.

Unifiqué los valores para que quedaran representados de manera consistente como:

- Online
- Sucursal
- Telefónico

Esto permite agrupar correctamente las operaciones por canal comercial y evitar categorías repetidas por diferencias de escritura.

## 7. Normalización de la estructura

Una vez terminada la limpieza, separé la información en dos consultas utilizando referencias a la consulta principal.

### Tabla Clientes

La tabla contiene **119 clientes únicos**, identificados mediante el campo `id_cliente`.

Conservé la información de identificación, contacto, ubicación, segmento comercial, estado del cliente y fecha de alta.

### Tabla Ventas

La tabla contiene **900 registros de ventas**, con la información de las operaciones comerciales.

Conservé los identificadores de operación, cliente y producto, además de las fechas, cantidades, precios, descuentos, importes y canales de venta.

### Relación entre tablas

Relacioné ambas tablas mediante `id_cliente`, utilizando una relación de uno a muchos (1:*).

Esto permite que un mismo cliente tenga varias operaciones de venta sin necesidad de repetir toda su información personal en la tabla Clientes.

## 8. Validación final

Al finalizar las transformaciones, revisé que los importes calculados no presentaran valores vacíos ni errores y que los identificadores principales fueran válidos.

También comprobé que los canales de venta estuvieran estandarizados, que la tabla Clientes tuviera un registro por identificador y que la relación entre Clientes y Ventas quedara activa.

## 9. Conclusión

Este trabajo me ayudó a entender mejor la importancia de preparar los datos antes de comenzar a realizar visualizaciones.

Una de las cosas que aprendí es que no siempre conviene eliminar un registro solamente porque tiene información faltante. Primero es necesario revisar qué dato falta, si es posible recuperarlo y cómo podría afectar los resultados del análisis.

También me resultó útil trabajar con la separación de clientes y ventas, porque permite mantener una estructura más ordenada y evitar información repetida innecesariamente.

El resultado fue un conjunto de datos preparado para continuar trabajando en Power BI, con las transformaciones registradas y un modelo que facilita futuros análisis comerciales.

## Herramientas utilizadas

- Microsoft Power BI Desktop
- Power Query
- Microsoft Excel
- Google Drive
- GitHub

---

**Alumno:** Dylan Mermel  
**Curso:** Data Analytics - Coderhouse  
**Módulo:** 6 - Unidad 2  
**Tema:** Extracción y transformación de datos con Power BI
