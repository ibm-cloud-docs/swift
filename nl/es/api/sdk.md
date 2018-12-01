---

copyright:
  years: 2018
lastupdated: "2018-08-17"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Adición de servicios de fondo a la app con un SDK generado
{: #sdk-cli}

El plugin {{site.data.keyword.IBM}} SDK Generator se puede instalar en la [CLI de {{site.data.keyword.cloud_notm}}](/docs/cli/reference/bluemix_cli/get_started.html).

Con el plug-in de Generador de SDK de {{site.data.keyword.IBM_notm}}, puede integrar fácilmente los servicios de fondo en la app mediante un SDK generado. Cuando se produzca un cambio en una API REST, puede volver a generar el SDK y sustituir el antiguo por una actualización de SDK. También puede integrar la CLI en un conducto de devops y garantizar que el SDK sea siempre coherente con la especificación de la API cada vez que se cree la app.

La definición de la API REST debe ser válida y estar alojada en un punto final de servidor activo o en un archivo local en el sistema.

## Antes de empezar
{: #prereqs}

Asegúrese de cumplir los siguientes requisitos previos:

* Una cuenta de [{{site.data.keyword.cloud_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://bluemix.net){: new_window}.
* Una definición de API válida que se ajusta a la [especificación de Open API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.openapis.org/){: new_window}.

## Instalación del plugin de SDK
{: #installation}

1. [Instale la CLI de {{site.data.keyword.cloud_notm}}](/docs/cli/reference/bluemix_cli/get_started.html).

2. [Instale el plug-in de SDK](/docs/cli/sdk/index.html).
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## Generación del SDK
{: #commands}

Genere un SDK especificando:
```
ibmcloud sdk generate [arguments...] [command options]
```
{: codeblock}

### Argumentos
{: #gen-args}

* **APP_NAME**: El nombre de la app de Cloud Foundry en el espacio actual.
* **OPENAPI_DOC_LOCATION**: Un URL o una vía de acceso de archivos relativa al JSON o Yaml de la definición de la API REST sin formato.
* **GENERATED_SDK_NAME** (opcional): El nombre del SDK generado.

### Opciones
{: #gen-options}

**Platform** (necesario):
  * `--ios`: Generar un SDK de Swift de iOS.
  * `--swift`: Generar un SDK de servidor de Swift.
  * `--js`: Generar un SDK de JavaScript.

**Location** (necesario):

Especifica el tipo para el argumento `OPENAPI_DOC_LOCATION`.

  * `-r`: Un URL remoto.
  * `-f`: Un nombre de archivo.
  * `-a`: App que se ejecuta en {{site.data.keyword.coud_notm}}.
  * `-l`: El URL de localhost.

**Opcional**:
  * `--output "YOUR_RELATIVE_PATH"`: Coloca el SDK generado en el directorio especificado por `YOUR_RELATIVE_PATH` (los SDK existentes se sobrescriben, si los hay).
  * `--unzip`: Extrae el SDK generado (los datos se sobrescriben si hay artefactos SDK existentes).

### Uso
{: #gen-usage}

Para generar un SDK desde una app de Cloud Foundry que se está ejecutando en {{site.data.keyword.cloud_notm}}, puede utilizar el nombre de la app como un parámetro para la CLI. El mandato siguiente utiliza el nombre de la app como el `SDK_Name`.

```
ibmcloud sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

Para generar un SDK desde un URL a un archivo de definición de Open API o un archivo JSON o Yaml local, utilice el siguiente mandato.

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}


## Validación de las definiciones de Open API
{: #validating}

Ejecute el siguiente mandato: `ibmcloud sdk validate [argument]`

### Argumentos
{: #val-args}

* `APP_NAME`: El nombre de la app de Cloud Foundry en el espacio actual
* `OPENAPI_DOC_LOCATION`: Un URL o una vía de acceso de archivos relativa al JSON o Yaml de la definición de la API REST sin formato

### Uso
{: #val-usage}

Para validar una especificación de API de la app Cloud Foundry que se está ejecutando en {{site.data.keyword.cloud_notm}}, puede utilizar el nombre de la app como un parámetro para la CLI.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

Para validar un SDK desde el URL a un documento de especificación de la API o un archivo JSON o Yaml local, utilice el siguiente mandato:
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}
