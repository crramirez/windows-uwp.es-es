---
description: En este tema se describe el esquema del archivo de configuración XML de MakePri.exe.
title: Archivo de configuración de MakePri.exe
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 5427927322f61f44cf3a8b53112f5f7811520290
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031728"
---
# <a name="makepriexe-configuration-file"></a>Archivo de configuración de MakePri.exe

En este tema se describe el esquema del archivo de configuración XML de [MakePri.exe](compile-resources-manually-with-makepri.md) ; también se conoce como archivo de configuración de PRI. La herramienta MakePri.exe tiene un [comando createconfig](makepri-exe-command-options.md#createconfig-command) que puede usar para crear un nuevo archivo de configuración de PRI inicializado.

> [!NOTE]
> MakePri.exe se instala al activar la opción **Windows SDK para aplicaciones administradas para UWP** al instalar el kit de desarrollo de software de Windows. Se instala en la ruta de acceso `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (así como en las carpetas denominadas para las otras arquitecturas). Por ejemplo, `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

El archivo de configuración de PRI controla qué recursos se indexan y cómo. El XML de configuración debe ajustarse al esquema siguiente.

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

- El `default` elemento especifica el contexto (lenguaje, escala, contraste, etc.) que se debe usar para resolver los recursos cuando el contexto de tiempo de ejecución no coincide con ningún candidato de recurso. Dado que este contexto se especifica en tiempo de compilación y no cambia, los recursos se resuelven en este contexto a medida que se crean los calificadores. La puntuación coincidente se almacena en tiempo de compilación. Cada calificador debe tener un valor especificado. Consulte [ResourceContext](resource-management-system.md#resourcecontext) para obtener información detallada sobre cómo se eligen los recursos.
- El `index` elemento define las fases de indización discretas que se realizan en los recursos. Cada fase de indización determina los [indizadores específicos de formato](makepri-exe-format-specific-indexers.md) que se van a usar y los recursos que se van a indexar.
- El `qualifiers` elemento establece los calificadores iniciales para el primer archivo o carpeta que heredan otros recursos. Cada elemento calificador debe tener un nombre y un valor válidos (vea [adaptar los recursos para el idioma, la escala, el contraste alto y otros calificadores](tailor-resources-lang-scale-contrast.md)).
- El `root` atributo es la raíz de la ruta de acceso del archivo físico para el paso de índice. Puede ser relativa o absoluta. Si es relativo, se anexa a la raíz del proyecto que se proporciona en la línea de comandos. Si es absoluto, se usa directamente como la raíz del paso de índice. Las barras diagonales o posteriores son aceptables. Las barras diagonales finales se recortan. La raíz de la fase de índice determina la carpeta en la que todos los recursos se consideran relativos.
- El `startIndexAt` atributo es el archivo inicial de inicialización o la carpeta que se usa en la indexación. Es relativo a la raíz del paso de índice. Un valor vacío asume la carpeta raíz del paso de índice.

## <a name="default-pri-config-file"></a>Archivo de configuración de PRI predeterminado

MakePri.exe genera este nuevo archivo de configuración de PRI inicializado cuando se emite el [comando createconfig](makepri-exe-command-options.md#createconfig-command) .

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

## <a name="packaging-element"></a>Elemento Packaging

El `packaging` elemento define la información de división de PRI. El esquema para el `packaging` elemento se define para la configuración automática (compatibilidad con a `autoResourcePackage` lo largo de una dimensión específica) y la configuración manual.

En este ejemplo se muestra cómo usar a `autoResourcePackage` lo largo de una dimensión específica.

```xml
    <packaging>
        <autoResourcePackage qualifier="Language"/>
        <autoResourcePackage qualifier="Scale"/>
        <autoResourcePackage qualifier="DXFeatureLevel"/>
    </packaging>
```

En este ejemplo se muestra cómo usar manual `resourcePackage` .

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

MakePri.exe no bloquea explícitamente la generación de archivos PRI de recursos a lo largo de una dimensión específica. Las restricciones a lo largo de un determinado conjunto de dimensiones se definen e implementan externamente por MakeAppx.exe u otras herramientas de la canalización.

MakePri.exe analiza el `packaging` elemento después de todos los `index` nodos para rellenar todos los calificadores predeterminados. MakePri.exe recopila información analizada en estas estructuras de datos.

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

Este atributo establece una marca en el archivo PRI que causa

- Combinación de implementación para identificar que este archivo PRI puede fusionar mediante combinación.
- GetFullyQualifiedReference para devolver un error en caso de que se establezca esta marca y se haya inicializado el administrador de recursos con un archivo.

El valor predeterminado de este atributo es `true`. MakePri.exe solo establece la marca en PRI si tiene como destino Windows 10.

Se recomienda que omita `isDeploymentMergeable` (o lo establezca explícitamente en `true` ) para la creación de paquetes de recursos si tiene como destino Windows 10.

MakePri.exe agrega el valor de `isDeploymentMergeable` al archivo de volcado si `makepri dump` se ejecuta con la `/dt detailed` opción.

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

El valor predeterminado de este atributo es 1. Si proporciona un valor explícito y también utiliza la `/VersionMajor(vma)` opción de línea de comandos en desuso para la herramienta de MakePri.exe, el valor del archivo de configuración tiene prioridad.

Aquí tiene un ejemplo.

```xml
<resources majorVersion="2">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

## <a name="resourcestargetosversion-attribute"></a>Atributo resources@targetOsVersion

Indica la versión del sistema operativo de destino. En la tabla siguiente se muestran los valores que se admiten; el valor predeterminado es 6.3.0.

| Valor | Significado |
| ----- | ------- |
| 10.0.0 | Windows 10 |
| 6.3.0 (valor predeterminado) | Windows 8.1 |
| 6.2.1 | Windows 8 |

Aquí tiene un ejemplo.

```xml
<resources targetOsVersion="10.0.0">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

**Nota:** Windows es compatible con versiones anteriores con respecto a los archivos PRI; pero no siempre es compatible con versiones posteriores.

MakePri.exe agrega el valor de `targetOsVersion` al archivo de volcado si `makepri dump` se ejecuta con la `/dt detailed` opción.

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

A continuación se muestran algunas condiciones de error de ejemplo y el mensaje de error correspondiente.

| Condición | severity | Message |
| --------- | -------- | ------- |
| Se especifica un targetOsVersion que no sea uno de los valores admitidos. | Error | Configuración no válida: se especificó targetOsVersion no válido. |
| Se especifica un targetOsVersion de "6.2.1" y un `packaging` elemento está presente. | Error | Configuración no válida: el nodo ' Packaging ' no es compatible con este targetOsVersion. |
| Se encontró más de un modo en la configuración. Por ejemplo, manual y AutoResourcePackage especificados. | Error | Configuración no válida: el nodo ' empaquetado ' no puede tener más de un modo de operación. |
| Aparece un calificador predeterminado en paquete de recursos. | Error | Configuración no válida: <Qualifiername> = <QualifierValue> es un calificador predeterminado y sus candidatos no se pueden agregar a un paquete de recursos. |
| El calificador AutoResourcePackage contiene varios calificadores. Por ejemplo, language_scale. | Error | Configuración no válida: no se admite AutoResourcePackage con varios calificadores. |
| ResourcePackage QualifierSet contiene varios calificadores. Por ejemplo, Language-en-us_scale-100 | Error | Configuración no válida: no se admite QualifierSet con varios calificadores. |
| Se encontró un nombre de ResourcePack duplicado. | Error | Configuración no válida: nombre del paquete de recursos duplicado <rpname> . |
| El mismo conjunto de calificadores definido en dos paquetes de recursos. | Error | Configuración no válida: se encontraron varias instancias de QualifierSet " <qualifier tags> ". |
| No se encontraron candidatos para el QualifierSet indicado para el nodo ' ResourcePackage '. | Advertencia | Configuración no válida: no se encontraron candidatos para <Resource Package Name> . |
| No se encontraron candidatos para el calificador que aparecen en el nodo ' AutoResourcePackage '. | Advertencia | Configuración no válida: no se encontraron candidatos para el calificador <qualifier name> . No se generó el paquete de recursos. |
| No se encuentra ninguno de los modos. Es decir, se encontró el nodo ' empaquetado ' vacío. | Advertencia | Configuración no válida: no se especificó ningún modo de empaquetado. |

## <a name="related-topics"></a>Temas relacionados

* [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md)
* [MakePri.exe comando createconfig opciones de línea de comandos &mdash;](makepri-exe-command-options.md#createconfig-command)
* [Adaptar los recursos al idioma, escala, alto contraste y otros calificadores](tailor-resources-lang-scale-contrast.md)
* [Sistema de administración de recursos &mdash; ResourceContext](resource-management-system.md#resourcecontext)
