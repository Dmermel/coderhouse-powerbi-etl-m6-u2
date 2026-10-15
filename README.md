# Power BI - Módulo 6 | Unidad 2
## Extracción, limpieza y transformación de datos

### Descripción del proyecto

En este trabajo práctico utilicé Power BI Desktop y Power Query para preparar un archivo de ventas exportado desde un sistema de gestión.

El objetivo fue revisar la calidad de los datos, corregir problemas en las columnas, tratar valores faltantes y eliminar registros duplicados. También separé la información de clientes y ventas para dejar una estructura más organizada y facilitar futuros análisis.

## Archivos de la entrega

**Documentación principal:**

[Ver documentación completa en PDF](Pipeline_ETL_Mermel_Dylan_prolijo.pdf)

El PDF contiene las explicaciones del trabajo realizado, las decisiones tomadas durante la limpieza y las capturas de los pasos aplicados en Power Query.

**Proyecto de Power BI:**

[Descargar proyecto de Power BI (.pbix)](https://drive.google.com/file/d/1oU7qFZRlORuooFUsM5qdzEx2nLRyB4I9/view?usp=drive_link)

Desde este archivo se pueden revisar las consultas, las transformaciones y el modelo de datos en Power BI Desktop.

## 1. Importación de datos

Comencé importando el archivo `Ventas_export_legacy.xlsx` mediante el conector de Excel de Power BI.

Una vez cargado, ingresé al Editor de Power Query para revisar las columnas y detectar posibles problemas antes de trabajar con los datos.

El archivo original contenía 949 registros y 20 columnas.

## 2. Renombrar columnas

El archivo tenía nombres técnicos como `COD_CLI`, `F_VTA` y `TOT_VTA`, que no eran demasiado descriptivos.

Para mejorar la lectura y el mantenimiento de la información, cambié los encabezados por nombres en español utilizando el formato `snake_case`.

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

De esta forma, las columnas resultan más fáciles de identificar al momento de realizar consultas y análisis.

## 3. Corrección de tipos de datos

Revisé los tipos de datos asignados por Power Query y realicé los ajustes necesarios.

- **Fecha:** para `fecha_venta` y `fecha_alta_cliente`.
- **Número entero:** para `cantidad`.
- **Número decimal:** para precios, descuentos e importes.
- **Texto:** para nombres, correos, teléfonos, códigos identificadores y categorías.

En el caso de los identificadores, utilicé texto porque representan códigos y no cantidades con las que necesite realizar cálculos matemáticos.

Esta revisión es importante porque un tipo de dato incorrecto puede generar problemas al realizar operaciones, filtros o análisis temporales.

## 4. Tratamiento de valores nulos

Durante la revisión encontré valores faltantes en distintas columnas.

En `descuento_pct`, reemplacé los valores nulos por cero, considerando para este ejercicio que un descuento no informado equivale a una venta sin descuento aplicado.

En `total_venta`, decidí no reemplazar directamente los valores vacíos por cero, ya que eso podía afectar los resultados de facturación.

Para recuperar los importes faltantes creé la columna `total_venta_calculado`, utilizando la siguiente lógica:

`cantidad * precio_unitario * (1 - descuento_pct)`

La fórmula conserva el importe original cuando existe y realiza el cálculo cuando está vacío.

Después identifiqué cuatro registros que no contenían información de operación, cliente ni importe, por lo que decidí excluirlos.

Los valores faltantes de correo electrónico y teléfono se conservaron porque no impedían identificar las ventas ni realizar el análisis comercial.

## 5. Eliminación de duplicados

Revisé los registros que compartían un mismo identificador de operación.

Primero comprobé que tener el mismo ID no significa necesariamente que una venta esté duplicada, ya que una operación podría contener distintos productos.

Por ese motivo, eliminé las filas completamente duplicadas considerando todas las columnas y no solamente `id_operacion`.

Esto permitió evitar que se contabilizaran operaciones repetidas sin eliminar registros diferentes por compartir un identificador.

## 6. Estandarización de datos

También encontré diferencias en la escritura de los canales de venta, por ejemplo `ONLINE`, `online` y `Sucursal`.

Unifiqué los valores para que quedaran representados de manera consistente como:

- Online
- Sucursal
- Telefónico

Esto evita que un mismo canal aparezca dividido en varias categorías durante la creación de reportes.

## 7. Normalización de la estructura

Una vez terminada la limpieza, separé la información en dos consultas utilizando referencias a la consulta principal.

### Tabla Clientes

Contiene 119 clientes identificados de forma única mediante `id_cliente`.

Conservé los datos de identificación, contacto, ubicación, segmento comercial, estado y fecha de alta.

### Tabla Ventas

Contiene 900 registros de operaciones comerciales.

Conservé la información relacionada con el identificador de operación, cliente, producto, fechas, cantidades, precios, descuentos, importes y canal de venta.

### Relación entre tablas

Relacioné ambas tablas mediante el campo `id_cliente`, utilizando una relación de uno a muchos (1:*).

Esto permite que un cliente tenga varias operaciones de venta sin necesidad de repetir todos sus datos personales en cada registro de la tabla Clientes.

## 8. Validación final

Al finalizar, revisé que los importes calculados no presentaran valores vacíos ni errores, que los identificadores principales fueran válidos y que los canales de venta estuvieran estandarizados.

También comprobé que la tabla Clientes tuviera un solo registro por identificador y que la relación con Ventas quedara activa.

## 9. Conclusión

Esta práctica me sirvió para entender mejor la importancia de preparar los datos antes de comenzar a construir visualizaciones.

Una de las cosas que aprendí es que no siempre conviene eliminar una fila solamente porque tiene información faltante. Primero es necesario revisar qué dato falta, si puede recuperarse y cómo podría afectar al análisis.

También me resultó útil trabajar con la separación de clientes y ventas, porque permite mantener la información más ordenada y evitar repeticiones innecesarias.

El resultado fue un conjunto de datos preparado para continuar trabajando en Power BI, con las transformaciones registradas y una estructura que facilita futuras consultas comerciales.

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
