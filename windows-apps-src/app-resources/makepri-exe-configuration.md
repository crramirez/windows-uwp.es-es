---
author: stevewhims
Description: This topic describes the schema of the MakePri.exe XML configuration file.
title: Archivo de configuración de MakePri.exe
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: 512880b7a7ea955a45697762cbbdb7f74ac70102
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2018
ms.locfileid: "2816346"
---
# <a name="makepriexe-configuration-file"></a>Archivo de configuración de MakePri.exe

En este tema se describe el esquema del archivo de configuración XML de [MakePri.exe](compile-resources-manually-with-makepri.md), también conocido como archivo de configuración PRI. La herramienta MakePri.exe tiene un [comando createconfig](makepri-exe-command-options.md#createconfig-command) que puedes usar para crear un nuevo archivo de configuración PRI inicializado.

> [!NOTE]
> MakePri.exe se instala al comprobar la opción de **SDK de Windows para las aplicaciones administradas de UWP** al instalar el Kit de desarrollo de Software de Windows. Se instala en la ruta de acceso `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (así como en carpetas con el nombre de las otras arquitecturas). Por ejemplo, `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

El archivo de configuración PRI controla qué recursos se indizan y cómo lo hacen. El XML de configuración debe ajustarse al siguiente esquema.

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="resources">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="packaging" maxOccurs="1" minOccurs="0">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="autoResourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:attribute name="qualifier" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
              <xs:element name="resourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name="qualifierSet" maxOccurs="unbounded" minOccurs="0">
                      <xs:complexType>
                        <xs:attribute name="definition" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                  <xs:attribute name="name" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element maxOccurs="unbounded" name="index">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="qualifiers" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="default" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="indexer-config" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip"/>
                  </xs:sequence>
                  <xs:attribute name="type" type="xs:string" use="required" />
                  <xs:anyAttribute processContents="skip"/>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
            <xs:attribute name="root" type="xs:string" use="required" />
            <xs:attribute name="startIndexAt" type="xs:string" use="required" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
      <xs:attribute name="isDeploymentMergeable" type="xs:boolean" use="optional" />
      <xs:attribute name="majorVersion" type="xs:positiveInteger" use="optional" />
      <xs:attribute name="targetOsVersion" type="xs:string" use="optional" />
    </xs:complexType>
  </xs:element>
</xs:schema>
```

- El elemento `default` especifica el contexto (idioma, escala, contraste, etc.) que se deberá usar para resolver recursos cuando el contexto de tiempo de ejecución no coincida con ningún candidato de recurso. Como este contexto se especifica en el momento de la compilación y no cambia, los recursos se resuelven en este contexto cuando se crean los calificadores. La puntuación coincidente se almacena en el momento de la compilación. Cada calificador debe tener algún valor especificado. Consulta [ResourceContext](resource-management-system.md#resourcecontext) para obtener más información sobre cómo se eligen los recursos.
- El elemento `index` define los pasos de indización discretos que se llevan a cabo en los activos. Cada paso de indización determina los [indizadores específicos del formato](makepri-exe-format-specific-indexers.md) que se deben usar, y qué recursos se indizarán.
- El elemento `qualifiers` establece los calificadores iniciales del primer archivo o carpeta que heredan otros recursos. Cada elemento calificador debe tener un nombre y un valor válidos (consulta [Adaptar los recursos para idioma, escala, contraste alto y otros calificadores](tailor-resources-lang-scale-contrast.md)).
- El atributo `root` es la raíz de la ruta de acceso del archivo físico del paso de índice. Puede ser relativo o absoluto. Si es relativo, se adjunta a la raíz del proyecto que indiques en la línea de comandos. Si es absoluto, se usa directamente como raíz de paso de índice. Se pueden usar barras diagonales y barras diagonales inversas. Las barras finales se recortarán. La raíz del paso del índice determina la carpeta en la que todos los recursos se consideran relativos.
- El atributo `startIndexAt` es el archivo semilla inicial o la carpeta usada durante la indización. Es relativo a la raíz de paso de índice. Un valor vacío supondría la carpeta raíz de paso de índice.

## <a name="default-pri-config-file"></a>Archivo de configuración PRI predeterminado

MakePri.exe genera este nuevo archivo de configuración PRI inicializado cuando se emite el [comando createconfig](makepri-exe-command-options.md#createconfig-command).

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<resources targetOsVersion="10.0.0" majorVersion="1">
  <packaging>
    <autoResourcePackage qualifier="Language"/>
    <autoResourcePackage qualifier="Scale"/>
    <autoResourcePackage qualifier="DXFeatureLevel"/>
  </packaging>
  <index root="\" startIndexAt="\">
    <default>
      <qualifier name="Language" value="en-US"/>
      <qualifier name="Contrast" value="standard"/>
      <qualifier name="Scale" value="100"/>
      <qualifier name="HomeRegion" value="001"/>
      <qualifier name="TargetSize" value="256"/>
      <qualifier name="LayoutDirection" value="LTR"/>
      <qualifier name="Theme" value="dark"/>
      <qualifier name="AlternateForm" value=""/>
      <qualifier name="DXFeatureLevel" value="DX9"/>
      <qualifier name="Configuration" value=""/>
      <qualifier name="DeviceFamily" value="Universal"/>
      <qualifier name="Custom" value=""/>
    </default>
    <indexer-config type="folder" foldernameAsQualifier="true" filenameAsQualifier="true" qualifierDelimiter="."/>
    <indexer-config type="resw" convertDotsToSlashes="true" initialPath=""/>
    <indexer-config type="resjson" initialPath=""/>
    <indexer-config type="PRI"/>
  </index>
  <!--<index startIndexAt="Start Index Here" root="Root Here">-->
  <!--        <indexer-config type="resfiles" qualifierDelimiter="."/>-->
  <!--        <indexer-config type="priinfo" emitStrings="true" emitPaths="true" emitEmbeddedData="true"/>-->
  <!--</index>-->
</resources>
```

## <a name="packaging-element"></a>Elemento de empaquetado

El elemento `packaging` define la información de división de PRI. El esquema para el elemento `packaging` se define tanto para la configuración automática (compatible con `autoResourcePackage` a lo largo de una dimensión específica) como para la configuración manual.

Este ejemplo muestra cómo usar `autoResourcePackage` a lo largo de una dimensión específica.

```xml
    <packaging>
        <autoResourcePackage qualifier="Language"/>
        <autoResourcePackage qualifier="Scale"/>
        <autoResourcePackage qualifier="DXFeatureLevel"/>
    </packaging>
```

Este ejemplo muestra cómo utilizar el `resourcePackage` manual.

```xml
  <packaging>
    <resourcePackage name="Germany">
      <qualifierSet definition="lang-de-de"/>
      <qualifierSet definition="lang-es-es"/>
    </resourcePackage>  
    <resourcePackage name="France">
      <qualifierSet definition="lang-fr-fr"/>
    </resourcePackage>  
    <resourcePackage name="HighRes1">
      <qualifierSet definition="scale-200"/>
    </resourcePackage>
    <resourcePackage name="HighRes2">
      <qualifierSet definition="scale-400"/>
    </resourcePackage>
  </packaging>
```

MakePri.exe no bloquea explícitamente la generación de archivos PRI de recursos a lo largo de cualquier dimensión específica. MakeAppx.exe u otras herramientas de la canalización definen e implementan restricciones externamente a lo largo de un determinado conjunto de dimensiones.

MakePri.exe analiza el elemento `packaging` después de que todos los nodos `index` rellenen todos los calificadores predeterminados. MakePri.exe recopila la información analizada en estas estructuras de datos.

```csharp
enum ResourcePackageMode
{
    None,
    AutoPackQualifier,
    ManualPack
}

ResourcePackageMode eResourcePackageMode;
list<string> RPQualifierList; // To store AutoResourcePackage Qualifiers
map<string, list<string>> RPNameToQSIMap; // To store ResourcePackage name to QualifierSet list mapping.
```

## <a name="resourcesisdeploymentmergeable-attribute"></a>Atributo resources@isDeploymentMergeable

Este atributo establece una marca en el archivo PRI origen

- La combinación de implementación identifica que este archivo PRI se puede combinar.
- GetFullyQualifiedReference devuelve un error si esta marca está establecida y el administrador de recursos se ha inicializado con un archivo.

El valor predeterminado de este atributo es `true`. MakePri.exe solo establece la marca en PRI si el destino es Windows10.

Te recomendamos que omitas `isDeploymentMergeable` (o lo establezcas explícitamente en `true`) para crear paquetes de recursos si el destino es Windows 10.

MakePri.exe agrega el valor de `isDeploymentMergeable` en el archivo de volcado si `makepri dump`se ejecuta con la opción `/dt detailed`.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <IsDeploymentMergeable>true</IsDeploymentMergeable>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="resourcesmajorversion-attribute"></a>Atributo resources@majorVersion

El valor predeterminado de este atributo es 1. Si proporcionas un valor explícito y utilizas también la opción de línea de comandos en desuso `/VersionMajor(vma)` para la herramienta MakePri.exe, en ese caso tiene prioridad el archivo de configuración.

Aquí tienes un ejemplo.

```xml
<resources majorVersion="2">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

## <a name="resourcestargetosversion-attribute"></a>Atributo resources@targetOsVersion

Indica la versión del sistema operativo de destino. La siguiente tabla muestra los valores que se admiten; el valor predeterminado es 6.3.0.

| Valor | Significado |
| ----- | ------- |
| 10.0.0 | Windows 10 |
| 6.3.0 (predeterminado) | Windows 8.1 |
| 6.2.1 | Windows8 |

Aquí tienes un ejemplo.

```xml
<resources targetOsVersion="10.0.0">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

**Nota** Windows es compatible con versiones anteriores con respecto a los archivos PRI; pero no siempre es compatible con versiones posteriores.

MakePri.exe agrega el valor de `targetOsVersion` en el archivo de volcado si `makepri dump` se ejecuta con la opción `/dt detailed`.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <TargetOS version="10.0.0"/>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="validation-error-messages"></a>Mensajes de error de validación

Aquí te mostramos algunas condiciones de error de ejemplo y el mensaje de error correspondiente.

| Condición | Gravedad | Mensaje |
| --------- | -------- | ------- |
| Se especifica una targetOsVersion distinta de todos los valores admitidos. | Error | Configuración no válida: especificada targetOsVersion no válida. |
| Se especifica una targetOsVersion de "6.2.1" y está presente un elemento `packaging`. | Error | Configuración no válida: el nodo 'packaging' no se admite con esta targetOsVersion. |
| Se encontró más de un modo en la configuración. Por ejemplo, se especificó Manual y AutoResourcePackage. | Error | Configuración no válida: el nodo 'packaging' no puede tener más de un modo de funcionamiento. |
| Se enumera un calificador predeterminado en el paquete de recursos. | Error | Configuración no válida: <Qualifiername>=<QualifierValue> es un calificador predeterminado y sus candidatos no se pueden agregar a un paquete de recursos. |
| El calificador AutoResourcePackage contiene varios calificadores. Por ejemplo, language_scale. | Error | Configuración no válida: no se admite AutoResourcePackage con varios calificadores. |
| El QualifierSet de ResourcePackage contiene varios calificadores. Por ejemplo, language-en-us_scale-100 | Error | Configuración no válida: no se admite QualifierSet con varios calificadores. |
| Se encontró un nombre de paquete de recursos duplicado. | Error | Configuración no válida: nombre de paquete de recursos duplicado <rpname>. |
| El mismo conjunto de calificadores se ha definido en dos paquetes de recursos. | Error | Configuración no válida: se encontraron varias instancias del QualifierSet "<qualifier tags>". |
| No se encuentran candidatos para el QualifierSet enumerado para el nodo 'ResourcePackage'. | Advertencia | Configuración no válida: no se han encontrado candidatos para <Resource Package Name>. |
| No se encuentran candidatos para el calificador enumerado en el nodo ‘AutoResourcePackage’. | Advertencia | Configuración no válida: no se han encontrado candidatos para el calificador <qualifier name>. Paquete de recursos no generado. |
| No se ha encontrado ninguno de los modos. Es decir, se ha encontrado el nodo 'packaging' vacío. | Advertencia | Configuración no válida: no se especificó un modo de empaquetado. |

## <a name="related-topics"></a>Temas relacionados

* [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md)
* [Opciones de línea de comandos de MakePri.exe: comando createconfig](makepri-exe-command-options.md#createconfig-command)
* [Adaptar los recursos al idioma, la escala, el contraste alto y otros calificadores](tailor-resources-lang-scale-contrast.md)
* [Sistema de administración de recursos: ResourceContext](resource-management-system.md#resourcecontext)