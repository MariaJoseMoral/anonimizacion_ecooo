
# Backlog del proyecto

**Proyecto:** Herramienta de anonimización documental para ECOOO

------------------------------------------------------------------------

# ÉPICA 1. Análisis de la documentación

## HU-1.1 Inventario documental

**Como desarrolladora**, quiero clasificar todos los documentos para
decidir qué estrategia de anonimización necesita cada uno.

### Tareas

-   Identificar todas las tipologías documentales.
-   Contar cuántos documentos hay de cada tipo.
-   Detectar documentos duplicados.
-   Detectar PDFs escaneados.
-   Detectar DOCX y XLSX.

**Estado:** 🟡 En progreso

------------------------------------------------------------------------

## HU-1.2 Inventario de datos personales

Extraer todos los campos que puedan contener datos personales:

-   Nombre y apellidos
-   DNI / NIE
-   CIF
-   Dirección
-   Correo electrónico
-   Teléfono
-   Firma manuscrita
-   Firma digital
-   CUPS
-   Coordenadas
-   Matrícula
-   Cuenta bancaria
-   Referencia catastral (si aparece)
-   Nº de expediente (evaluar si debe conservarse)

**Estado:** Pendiente

------------------------------------------------------------------------

## HU-1.3 Clasificación por estrategia

  Tipo de documento   Estrategia
  ------------------- ---------------
  PDF con texto       PyMuPDF
  PDF escaneado       OCR + PyMuPDF
  DOCX                python-docx
  XLSX                openpyxl

------------------------------------------------------------------------

# ÉPICA 2. Arquitectura

## HU-2.1 Estructura del proyecto

``` text
anonimizador/
├── config/
├── input/
├── output/
├── revision/
├── logs/
├── tests/
├── templates/
├── main.py
├── pdf.py
├── docx.py
├── xlsx.py
├── ocr.py
├── rules.py
└── utils.py
```

## HU-2.2 Configuración

Archivo `config.yml`

``` yaml
anonimizar:
  - nombre
  - dni
  - direccion
  - telefono
  - email
  - cups
```

## HU-2.3 Logging

Registrar: - Documento procesado - Tiempo de ejecución - Errores -
Incidencias

------------------------------------------------------------------------

# ÉPICA 3. Motor de anonimización PDF

## HU-3.1 Extracción de texto

-   PyMuPDF

## HU-3.2 Localización de datos

Localizar coordenadas de campos como: - Nombre - NIF - Dirección -
Email - CUPS - Teléfono

## HU-3.3 Redacción

``` python
page.add_redact_annot(...)
page.apply_redactions()
```

## HU-3.4 Validación

Verificar que: - El dato desaparece visualmente. - El dato desaparece
del texto interno del PDF.

------------------------------------------------------------------------

# ÉPICA 4. DOCX

Anonimizar: - Párrafos - Tablas - Encabezados - Pies de página

------------------------------------------------------------------------

# ÉPICA 5. XLSX

Anonimizar: - Hojas - Tablas - Comentarios - Celdas ocultas

------------------------------------------------------------------------

# ÉPICA 6. OCR

Solo para PDFs escaneados.

``` text
PDF
 ↓
Imagen
 ↓
OCR
 ↓
Coordenadas
 ↓
Anonimización
```

------------------------------------------------------------------------

# ÉPICA 7. Procesamiento masivo

## HU-7.1

Recorrer automáticamente `input/`.

## HU-7.2

Conservar la estructura de carpetas en `output/`.

## HU-7.3

Mostrar barra de progreso.

Ejemplo:

``` text
1789 / 3600 documentos
```

## HU-7.4

Procesamiento en paralelo si el hardware lo permite.

------------------------------------------------------------------------

# ÉPICA 8. Calidad

## HU-8.1

Validar una muestra aleatoria de 100 documentos.

## HU-8.2

Generar informe:

``` text
Procesados: 3598
Correctos: 3594
Errores: 4
```

## HU-8.3

Enviar documentos con incidencias a `revision/`.

------------------------------------------------------------------------

# ÉPICA 9. Entrega

Generar automáticamente:

``` text
logs/
errores.csv
estadisticas.csv
resumen.txt
```

------------------------------------------------------------------------

# MVP

1.  Inventario automático de documentos.
2.  Clasificación de tipologías.
3.  Motor de anonimización para PDFs con texto seleccionable.
4.  Procesamiento por lotes con registro de errores.
5.  Revisión manual de incidencias.
6.  Soporte para DOCX y XLSX.
7.  OCR únicamente para los pocos PDFs escaneados.
