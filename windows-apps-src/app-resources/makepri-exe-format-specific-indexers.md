---
description: En este tema se describen los indizadores específicos de formato utilizados por la herramienta MakePri.exe para generar su índice de recursos.
title: Indexadores específicos del formato de MakePri.exe
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 3794d369ae9d47cfc7aad1b24ca2768b04024581
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031698"
---
# <a name="makepriexe-format-specific-indexers"></a>Indexadores específicos del formato de MakePri.exe

En este tema se describen los indizadores específicos de formato utilizados por la herramienta [MakePri.exe](compile-resources-manually-with-makepri.md) para generar su índice de recursos.

> [!NOTE]
> MakePri.exe se instala al activar la opción **Windows SDK para aplicaciones administradas para UWP** al instalar el kit de desarrollo de software de Windows. Se instala en la ruta de acceso `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (así como en las carpetas denominadas para las otras arquitecturas). Por ejemplo, `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

Normalmente, MakePri.exe se usa con `new` los `versioned` comandos, o `resourcepack` . Consulte [MakePri.exe opciones de la línea de comandos](makepri-exe-command-options.md). En esos casos, indiza los archivos de código fuente para generar un índice de recursos. MakePri.exe usa varios indexadores individuales para leer diferentes archivos de recursos de origen o contenedores para los recursos. El indexador más sencillo es el indizador de carpetas, que indiza el contenido de una carpeta, como `.jpg` o `.png` imágenes.

Los indizadores específicos de formato se identifican mediante `<indexer-config>` la especificación de elementos dentro `<index>` de un elemento del [ archivo de configuración deMakePri.exe](makepri-exe-configuration.md). El `type` atributo identifica el indizador específico del formato que se utiliza.

Los contenedores de recursos que se encuentran durante la indexación suelen obtener su contenido indizado en lugar de agregarse al propio índice. Por ejemplo, los `.resjson` archivos que encuentra el indexador de carpetas se pueden indexar más mediante un `.resjson` indexador, en cuyo caso el `.resjson` archivo no aparece en el índice. **Nota:** un `<indexer-config>` elemento para el indizador asociado a ese contenedor debe estar incluido en el archivo de configuración para que se produzca.

Normalmente, los calificadores que se encuentran en una entidad contenedora, como &mdash; una carpeta o un `.resw` archivo, &mdash; se aplican a todos los recursos que contiene, como los archivos dentro de la carpeta o las cadenas del `.resw` archivo.

## <a name="folder"></a>Carpeta

El indizador de carpetas se identifica mediante un `type` atributo de carpeta. Indiza el contenido de una carpeta y determina los calificadores de recursos de la carpeta y los nombres de archivo. Su elemento de configuración se ajusta al esquema siguiente.

```xml
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
    <xs:simpleType name="ExclusionTypeList">
        <xs:restriction base="xs:string">
            <xs:enumeration value="path"/>
            <xs:enumeration value="extension"/>
            <xs:enumeration value="name"/>
            <xs:enumeration value="tree"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:complexType name="FolderExclusionType">
        <xs:attribute name="type" type="ExclusionTypeList" use="required"/>
        <xs:attribute name="value" type="xs:string" use="required"/>
        <xs:attribute name="doNotTraverse" type="xs:boolean" use="required"/>
        <xs:attribute name="doNotIndex" type="xs:boolean" use="required"/>
    </xs:complexType>
    <xs:simpleType name="IndexerConfigFolderType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((f|F)(o|O)(l|L)(d|D)(e|E)(r|R))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:sequence>
                <xs:element name="exclude" type="FolderExclusionType" minOccurs="0" maxOccurs="unbounded"/>
            </xs:sequence>
            <xs:attribute name="type" type="IndexerConfigFolderType" use="required"/>
            <xs:attribute name="foldernameAsQualifier" type="xs:boolean" use="required"/>
            <xs:attribute name="filenameAsQualifier" type="xs:boolean" use="required"/>
            <xs:attribute name="qualifierDelimiter" type="xs:string" use="required"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

El `qualifierDelimiter` atributo especifica el carácter después del cual se especifican los calificadores en un nombre de archivo, omitiendo la extensión. El valor predeterminado es ".".

## <a name="pri"></a>PRI

El indexador PRI se identifica mediante un `type` atributo de PRI. Indiza el contenido de un archivo PRI. Normalmente, se usa al indizar el recurso contenido en otro ensamblado, DLL, SDK o biblioteca de clases en el PRI de la aplicación.

Todos los nombres de recursos, calificadores y valores contenidos en el archivo PRI se mantienen directamente en el nuevo archivo PRI. El mapa de recursos de nivel superior, sin embargo, no se mantiene en la PRI final. Se combinan los mapas de recursos.

```xml
<xs:schema id="prifile" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigPriType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((p|P)(r|R)(i|I))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigPriType" use="required"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

## <a name="priinfo"></a>PriInfo

El indizador PriInfo se identifica mediante un `type` atributo de PriInfo. Indiza el contenido de un archivo de volcado de memoria detallado. Para generar un archivo de volcado de memoria detallado, ejecute `makepri dump` con la `/dt detailed` opción. El elemento de configuración para el indizador se ajusta al esquema siguiente.

```xml
<xs:schema id="priinfo" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
  <xs:simpleType name="IndexerConfigPriInfoType">
    <xs:restriction base="xs:string">
      <xs:pattern value="((p|P)(r|R)(i|I)(i|I)(n|N)(f|F)(o|O))"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:element name="indexer-config">
    <xs:complexType>
      <xs:attribute name="type" type="IndexerConfigPriInfoType" use="required"/>
      <xs:attribute name="emitStrings" type="xs:boolean" use="optional"/>
      <xs:attribute name="emitPaths" type="xs:boolean" use="optional"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

Este elemento de configuración permite atributos opcionales para configurar el comportamiento del indexador PriInfo. El valor predeterminado de `emitStrings` y `emitPaths` es `true` . Si `emitStrings` es `true` , los candidatos de recurso con el `type` atributo establecido en "cadena" se incluyen en el índice, de lo contrario se excluyen. Si ' emitPaths ' es `true` , los candidatos de recurso con el `type` atributo establecido en "path" se incluyen en el índice, de lo contrario se excluyen.

Este es un ejemplo de configuración que incluye tipos de recursos de cadena, pero omite los tipos de recursos de ruta de acceso.

```xml
<indexer-config type="priinfo" emitStrings="true" emitPaths="false" />
```

Para indizar, un archivo de volcado de memoria debe terminar con la extensión `.pri.xml` y debe ajustarse al esquema siguiente.

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" >
  <xs:simpleType name="candidateType">
    <xs:restriction base="xs:string">
      <xs:pattern value="Path|String"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="scopeType">
    <xs:sequence>
      <xs:element name="ResourceMapSubtree" type="scopeType" minOccurs="0" maxOccurs="unbounded"/>
      <xs:element name="NamedResource" minOccurs="0" maxOccurs="unbounded">
        <xs:complexType>
          <xs:sequence>
            <xs:element name="Decision" minOccurs="0" maxOccurs="unbounded">
              <xs:complexType>
                <xs:sequence>
                  <xs:any processContents="skip" minOccurs="0" maxOccurs="unbounded"/>
                </xs:sequence>
                <xs:anyAttribute processContents="skip" />
              </xs:complexType>
            </xs:element>
            <xs:element name="Candidate" minOccurs="0" maxOccurs="unbounded">
              <xs:complexType>
                <xs:sequence>
                  <xs:element name="QualifierSet" minOccurs="0" maxOccurs="unbounded">
                    <xs:complexType>
                      <xs:sequence>
                        <xs:element name="Qualifier" minOccurs="0" maxOccurs="unbounded">
                          <xs:complexType>
                            <xs:attribute name="name" type="xs:string" use="required" />
                            <xs:attribute name="value" type="xs:string" use="required" />
                            <xs:attribute name="priority" type="xs:integer" use="required" />
                            <xs:attribute name="scoreAsDefault" type="xs:decimal" use="required" />
                            <xs:attribute name="index" type="xs:integer" use="required" />
                          </xs:complexType>
                        </xs:element>
                      </xs:sequence>
                      <xs:anyAttribute processContents="skip" />
                    </xs:complexType>
                  </xs:element>
                  <xs:element name="Value" type="xs:string"  minOccurs="0" maxOccurs="unbounded"/>
                </xs:sequence>
                <xs:attribute name="type" type="candidateType" use="required" />
              </xs:complexType>
            </xs:element>
          </xs:sequence>
          <xs:attribute name="name" use="required" type="xs:string" />
          <xs:anyAttribute processContents="skip" />
        </xs:complexType>
      </xs:element>
    </xs:sequence>
    <xs:attribute name="name" use="required" type="xs:string" />
    <xs:anyAttribute processContents="skip" />
  </xs:complexType>
  <xs:element name="PriInfo">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="PriHeader" >
          <xs:complexType>
            <xs:sequence>
              <xs:any minOccurs ="0" maxOccurs="unbounded" processContents="skip" />
            </xs:sequence>
            <xs:anyAttribute processContents="skip" />
          </xs:complexType>
        </xs:element>
        <xs:element name="QualifierInfo">
          <xs:complexType>
            <xs:sequence>
              <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip" />
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element name="ResourceMap">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="VersionInfo">
                <xs:complexType>
                  <xs:anyAttribute processContents="skip" />
                </xs:complexType>
              </xs:element>
              <xs:element minOccurs="0" maxOccurs="unbounded" name="ResourceMapSubtree" type="scopeType" />
            </xs:sequence>
            <xs:attribute name="name" type="xs:string" use="required" />
            <xs:anyAttribute processContents="skip" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

MakePri.exe admite los tipos de volcado ' Basic ', ' Detailed ', ' Schema ' y ' Summary '. Para configurar MakePri.exe para emitir el tipo de volcado de memoria que puede leer el indexador PriInfo, incluya "detailed" de/DumpType al usar el `dump` comando.

MakePri.exe omite varios elementos del `.pri.xml` archivo. Estos elementos se calculan durante la indización o se especifican en el archivo de configuración MakePri.exe. Los nombres de los recursos, calificadores y valores contenidos en el archivo de volcado de memoria se mantienen directamente en el nuevo archivo PRI. El mapa de recursos de nivel superior, sin embargo, no se mantiene en la PRI final. Los mapas de recursos se combinan como parte de la indización.

Este es un ejemplo de un recurso de tipo cadena válido de un archivo de volcado de memoria.

```xml
<NamedResource name="SampleString " index="96" uri="ms-resource://SampleApp/resources/SampleString ">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
  </Decision>
  <Candidate type="String">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>A Sample String Value</Value>
  </Candidate>
</NamedResource>
```

Este es un ejemplo de un recurso de tipo de ruta de acceso válido con dos candidatos de un archivo de volcado.

```xml
<NamedResource name="Sample.png" index="77" uri="ms-resource://SampleApp/Files/Images/Sample.png">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="0.7" index="2"/>
    </QualifierSet>
  </Decision>
  <Candidate type="Path">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-180.png</Value>
  </Candidate>
  <Candidate type="Path">
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-140.png</Value>
  </Candidate>
</NamedResource>
```

## <a name="resfiles"></a>ResFiles

El indizador ResFiles se identifica mediante un `type` atributo de ResFiles. Indiza el contenido de un `.resfiles` archivo. Su elemento de configuración se ajusta al esquema siguiente.

```xml
<xs:schema id="resx" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigResFilesType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((r|R)(e|E)(s|S)(f|F)(i|I)(l|L)(e|E)(s|S))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigResFilesType" use="required"/>
            <xs:attribute name="qualifierDelimiter" type="xs:string" use="required"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

Un `.resfiles` archivo es un archivo de texto que contiene una lista plana de rutas de acceso de archivo. Un `.resfiles` archivo puede contener comentarios "//". Aquí tiene un ejemplo.

<blockquote>
<pre>
Strings\component1\fr\elements.resjson
Images\logo.scale-100.png
Images\logo.scale-140.png
Images\logo.scale-180.png
</pre>
</blockquote>

## <a name="resjson"></a>ResJSON

El indizador ResJSON se identifica mediante un `type` atributo de ResJSON. Indiza el contenido de un `.resjson` archivo, que es un archivo de recursos de cadena. Su elemento de configuración se ajusta al esquema siguiente.

```xml
<xs:schema id="resjson" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigResJsonType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((r|R)(e|E)(s|S)(j|J)(s|S)(o|O)(n|N))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigResJsonType" use="required"/>
            <xs:attribute name="initialPath" type="xs:string" use="optional"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

Un `.resjson` archivo contiene texto JSON (consulte [el tipo de medio JSON o de aplicación para notación de objetos JavaScript (JSON)](https://www.ietf.org/rfc/rfc4627.txt)). El archivo debe contener un único objeto JSON con propiedades jerárquicas. Cada propiedad debe ser otro objeto JSON o un valor de cadena.

Las propiedades JSON con nombres que comienzan por un carácter de subrayado ("_") no se compilan en el archivo PRI final, pero se mantienen en el archivo de registro.

El archivo también puede contener comentarios "//" que se omiten durante el análisis.

El `initialPath` atributo coloca todos los recursos en esta ruta de acceso inicial anteponiéndole el nombre del recurso. Normalmente se usaría al indizar los recursos de la biblioteca de clases. El valor predeterminado es en blanco.

## <a name="resw"></a>ResW

El indexador ResW se identifica mediante un `type` atributo de ResW. Indiza el contenido de un `.resw` archivo, que es un archivo de recursos de cadena. Su elemento de configuración se ajusta al esquema siguiente.

```xml
<xs:schema id="resw" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
    <xs:simpleType name="IndexerConfigResxType">
        <xs:restriction base="xs:string">
            <xs:pattern value="((r|R)(e|E)(s|S)(w|W))"/>
        </xs:restriction>
    </xs:simpleType>
    <xs:element name="indexer-config">
        <xs:complexType>
            <xs:attribute name="type" type="IndexerConfigResxType" use="required"/>
            <xs:attribute name="convertDotsToSlashes" type="xs:boolean" use="required"/>
            <xs:attribute name="initialPath" type="xs:string" use="optional"/>
        </xs:complexType>
    </xs:element>
</xs:schema>
```

Un `.resw` archivo es un archivo XML que se ajusta al esquema siguiente.

```xml
  <xsd:schema id="root" xmlns="" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">
    <xsd:import namespace="http://www.w3.org/XML/1998/namespace" />
    <xsd:element name="root" msdata:IsDataSet="true">
      <xsd:complexType>
        <xsd:choice maxOccurs="unbounded">
          <xsd:element name="metadata">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" />
              </xsd:sequence>
              <xsd:attribute name="name" use="required" type="xsd:string" />
              <xsd:attribute name="type" type="xsd:string" />
              <xsd:attribute name="mimetype" type="xsd:string" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="assembly">
            <xsd:complexType>
              <xsd:attribute name="alias" type="xsd:string" />
              <xsd:attribute name="name" type="xsd:string" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="data">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
                <xsd:element name="comment" type="xsd:string" minOccurs="0" msdata:Ordinal="2" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" msdata:Ordinal="1" />
              <xsd:attribute name="type" type="xsd:string" msdata:Ordinal="3" />
              <xsd:attribute name="mimetype" type="xsd:string" msdata:Ordinal="4" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="resheader">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" />
            </xsd:complexType>
          </xsd:element>
        </xsd:choice>
      </xsd:complexType>
    </xsd:element>
  </xsd:schema>
```

El `convertDotsToSlashes` atributo convierte todos los caracteres de punto (".") que se encuentran en los nombres de recursos (atributos de nombre de elemento de datos) en una barra diagonal "/", excepto cuando los caracteres de punto están entre "[" y "]".

El `initialPath` atributo coloca todos los recursos en esta ruta de acceso inicial anteponiéndole el nombre del recurso. Normalmente se usa al indizar recursos de la biblioteca de clases. El valor predeterminado es en blanco.

## <a name="related-topics"></a>Temas relacionados

* [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md)
* [Opciones de línea de comandos de MakePri.exe](makepri-exe-command-options.md)
* [Archivo de configuración de MakePri.exe](makepri-exe-configuration.md)
* [El tipo de medio de aplicación/JSON para notación de objetos JavaScript (JSON)](https://www.ietf.org/rfc/rfc4627.txt)
