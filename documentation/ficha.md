# Ficha del artículo

Completa esta ficha al crear el repositorio y mantenla al día. Es el único
lugar con datos propios del artículo; el README es igual en todos los
repositorios.

## Identificación

- **Título:** Tres problemas para el análisis cuantitativo de los femicidios en Ecuador
- **Autoría:** Alejandra Marchán Cascante y Daniel Sánchez Pazmiño
- **Fecha de publicación:** 2023-03-07
- **Artículo publicado:** https://elquantificador.org/post/valores/2023-03-07-tres-problemas-para-el-analisis-cuantitativo-de-los-femicidios-en-ecuador/
- **Categoría:** Valores y comportamiento
- **Edición:** Febrero - Agosto 2023

## Estado

Publicado el 2023-03-07. El sitio enlaza a este análisis como un reporte
investigativo preliminar. Los archivos `EDG_2011.sav` a `EDG_2020.sav` no
están en el repositorio (están en `.gitignore`) y hay que descargarlos del
INEC para ejecutar el análisis completo.

## Pregunta y alcance

El artículo pregunta cómo se reconocen los femicidios en Ecuador y cómo eso
afecta la confiabilidad de las estadísticas oficiales. Compara cuatro fuentes
(Fiscalía General del Estado, Ministerio de Gobierno, defunciones generales del
INEC y la Fundación ALDEA) para el periodo posterior a la tipificación del
delito en el COIP en agosto de 2014 hasta 2022, con cobertura nacional. La
unidad de análisis es el conteo anual de muertes violentas de mujeres y de
femicidios según cada fuente, más las denuncias registradas en la Fiscalía.

## Requisitos

- R: no documentada en el repositorio.
- Paquetes de R: DataCombine, haven, labelled, patchwork, readxl, survey,
  tidyverse. Los scripts los instalan con `if(!require(...)) install.packages(...)`.

## Fuentes de datos

| Fuente | Institución | Periodo | Acceso | Script |
|---|---|---|---|---|
| Defunciones generales, mortalidad violenta por sexo (Registro Civil) | INEC | 2011 a 2021 | Manual; descarga desde https://www.ecuadorencifras.gob.ec/defunciones-generales/. Solo `EDG_2021.sav` está versionado | `code/reg_civil_wrangling.R` |
| Homicidios intencionales, filtrados a femicidio | Ministerio de Gobierno | Registros de 2010 a 2022 en el archivo | Manual; el repositorio no documenta el enlace del archivo. El artículo cita su informe preliminar 2019 | `code/min_gob_wrangling.R` |
| Muertes de mujeres en contexto delictivo (femicidios y otras muertes) | Fiscalía General del Estado, reporte de la Comisión Especial de Estadística | 2014 a 2022 | Manual; copiado a mano desde https://www.fiscalia.gob.ec/analitica-muertes-de-mujeres-en-contexto-delictivo/ | `code/femicidios_analisis.R` |
| Noticias del delito (denuncias) de homicidio, sicariato, violación, asesinato y femicidio | Fiscalía General del Estado, SIAF Analítica FGE | 2014-08-10 a 2022-09-30 | Manual; solicitud al correo de estadística de la Fiscalía, según el artículo | `code/fiscalia_denuncias_wrangling.R` |
| Femicidios (Alianza Feminista para el mapeo de femi(ni)cidios) | Fundación ALDEA | 2014 a 2022 | Manual; copiado a mano desde https://www.fundacionaldea.org/mapas | `code/min_gob_wrangling.R`, `code/femicidios_analisis.R` |

Ninguna fuente tiene descarga automática. Coloca los archivos en `data/` con los
nombres que leen los scripts: `EDG_2011.sav` a `EDG_2021.sav` (con las
excepciones `BDD_EDG_2019.sav` y `BBD_EDG_2017_spss.sav`),
`homicidios_intencionales_mingob.xls`, `muertes_fem_fiscalia.csv`,
`fiscalia_denuncias.xlsx` y `femicidios_aldea.xlsx`. La ALDEA no autorizó el
acceso a su base completa, según el artículo; el repositorio solo contiene la
serie anual.

## Salidas

- `images/graf1-femicidios-vs-otros.png`: femicidios frente a otras muertes violentas de mujeres en el reporte de la Fiscalía.
- `images/graf2-femicidios-fiscalia.png`: serie de femicidios de la Fiscalía.
- `images/graf2-fge-comparacion.png`: denuncias de la Fiscalía frente a las cifras del reporte oficial.
- `images/graf3-femicidios-registro-civil.png`: muertes violentas según el Registro Civil.
- `images/graf3-inec-genero-comp.png`: muertes violentas de mujeres como porcentaje del total, según el INEC.
- `images/graf4-comparacion.png`: comparación entre ALDEA, la Fiscalía y el Registro Civil.
- `images/graf3-comparacion.png`, `images/graf4-femicidios-aldea.png` y `images/graf5-femicidios-compar.png`: están en el repositorio, pero ningún script los genera ni el reporte los usa.
- `report/Femicidios en el Ecuador.Rmd`, con sus versiones `.docx` y `.html`: texto del artículo, que incluye las figuras 1, 2 (`graf2-fge-comparacion`), 3 (`graf3-inec-genero-comp`) y 4 (`graf4-comparacion`).

## Cómo replicar en este repositorio

El README anterior solo describe el repositorio y no da pasos, por lo que el
orden siguiente se deduce de los scripts. No hay `MASTER.R` ni `renv`.

1. Abre `Femicidios.Rproj` (o fija la raíz del repositorio como directorio de
   trabajo); las rutas son relativas a esa raíz.
2. Descarga de INEC los archivos de defunciones generales 2011 a 2020 y
   colócalos en `data/` con los nombres que lee `code/reg_civil_wrangling.R`.
   El de 2021 ya está en el repositorio. Si falta alguno, el script se
   detiene con un mensaje que lista los archivos ausentes.
3. Ejecuta `code/femicidios_analisis.R`. Este script llama, en orden, a
   `code/reg_civil_wrangling.R`, `code/min_gob_wrangling.R` (que vuelve a
   llamar al anterior) y `code/fiscalia_denuncias_wrangling.R`, lee
   `muertes_fem_fiscalia.csv` y `femicidios_aldea.xlsx`, y guarda los gráficos
   en `images/`.
4. Para el texto, compila `report/Femicidios en el Ecuador.Rmd`; usa las
   imágenes de `images/`.

`code/fiscalia_denuncias.R` no es invocado por el análisis.

## README anterior

Texto del README de este repositorio antes de adoptar el README estándar,
conservado sin cambios de contenido para no perder información.

### Tres problemas para el análisis cuantitativo de los femicidios en Ecuador

Repositorio de GitHub donde se incluyen los códigos para la replicación del artículo de El Quantificador: Tres problemas para el análisis cuantitativo de los femicidios en Ecuador.
