---
Description: This topic describes the format-specific indexers used by the MakePri.exe tool to generate its index of resources.
title: Indizadores específicos de formato de MakePri.exe
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, recursos, imagen, activo, MRT, calificador
ms.localizationpriority: medium
ms.openlocfilehash: e6938807a589337489f07f5865e02a580a72dae2
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/03/2018
ms.locfileid: "8476460"
---
# <a name="makepriexe-format-specific-indexers"></a>Indizadores específicos de formato de MakePri.exe

Este tema describe los indizadores específicos de formato usados por la herramienta [MakePri.exe](compile-resources-manually-with-makepri.md) para generar su índice de recursos.

> [!NOTE]
> MakePri.exe se instala al comprobar la opción de **SDK de Windows administra las aplicaciones para UWP** al instalar el Kit de desarrollo de Software de Windows. Se instala en la ruta de acceso `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (también en carpetas con el nombre para el resto de arquitecturas). Por ejemplo, `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

MakePri.exe se suele usar con los comandos `new`, `versioned` o `resourcepack`. Consulta [Opciones de línea de comandos de MakePri.exe](makepri-exe-command-options.md). En esos casos indexa los archivos de origen para generar un índice de recursos. MakePri.exe usa diversos indizadores individuales para leer distintos archivos de recursos de origen o contenedores de recursos. El indexador más sencillo es el indizador de carpeta, que indiza el contenido de una carpeta, como imágenes `.jpg` o `.png`.

Pueden identificarse los indexadores específicos del formato especificando elementos `<indexer-config>` de un elemento `<index>` del [archivo de configuración MakePri.exe](makepri-exe-configuration.md). El atributo `type` identifica el indexador específico del formato que se usa.

Los contenedores de recursos detectados durante la indexación suelen obtener su contenido indizado en vez de agregarse al índice. Por ejemplo, los archivos `.resjson` que encuentra el indexador de carpeta se pueden indizar más con un indizador `.resjson`, en cuyo caso el propio archivo `.resjson` no aparece en el índice. **Nota** Un elemento `<indexer-config>` del indexador asociado a ese contenedor debe incluirse en el archivo de configuración para que esto ocurra.

Por lo general, los calificadores encontrados en una entidad contenedora, como una carpeta o un archivo `.resw` se aplican a todos los recursos que contengan, como los archivos de la carpeta o las cadenas del archivo `.resw`.

## <a name="folder"></a>Carpeta

El indizador de carpeta se identifica por un atributo `type` de CARPETA. Indiza el contenido de una carpeta y determina los calificadores de recursos de la carpeta y los nombres de archivo. Su elemento de configuración se ajusta al siguiente esquema.

```xml
<xs:schema attributeFormDefault=\"unqualified\" elementFormDefault=\"qualified\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">\
    <xs:simpleType name=\"ExclusionTypeList\">\
        <xs:restriction base=\"xs:string\">\
            <xs:enumeration value=\"path\"/>\
            <xs:enumeration value=\"extension\"/>\
            <xs:enumeration value=\"name\"/>\
            <xs:enumeration value=\"tree\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:complexType name=\"FolderExclusionType\">\
        <xs:attribute name=\"type\" type=\"ExclusionTypeList\" use=\"required\"/>\
        <xs:attribute name=\"value\" type=\"xs:string\" use=\"required\"/>\
        <xs:attribute name=\"doNotTraverse\" type=\"xs:boolean\" use=\"required\"/>\
        <xs:attribute name=\"doNotIndex\" type=\"xs:boolean\" use=\"required\"/>\
    </xs:complexType>\
    <xs:simpleType name=\"IndexerConfigFolderType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((f|F)(o|O)(l|L)(d|D)(e|E)(r|R))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:sequence>\
                <xs:element name=\"exclude\" type=\"FolderExclusionType\" minOccurs=\"0\" maxOccurs=\"unbounded\"/>\
            </xs:sequence>\
            <xs:attribute name=\"type\" type=\"IndexerConfigFolderType\" use=\"required\"/>\
            <xs:attribute name=\"foldernameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"filenameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>
```

El atributo `qualifierDelimiter` especifica el carácter tras el cual se especifican los calificadores en un nombre de archivo, omitiendo la extensión. El valor predeterminado es ".".

## <a name="pri"></a>PRI

El indizador PRI se identifica por un atributo `type` de PRI. Indiza el contenido de un archivo PRI. Normalmente se usa al indizar el recurso contenido en otro ensamblado, DLL, SDK o biblioteca de clases en el PRI de la aplicación.

Todos los valores, calificadores y nombres de recursos contenidos en el archivo PRI se mantienen directamente en el nuevo archivo PRI. Sin embargo, el mapa de recursos de nivel superior no se mantiene en el PRI final. Los mapas de recursos se combinan.

```xml
<xs:schema id=\"prifile\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigPriType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((p|P)(r|R)(i|I))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigPriType\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

## <a name="priinfo"></a>PriInfo

El indizador PriInfo se identifica por un atributo `type` de PRIINFO. Indiza el contenido de un archivo de volcado detallado. Puedes generar un archivo de volcado detallado ejecutando `makepri dump` con la opción `/dt detailed`. El elemento de configuración del indizador se ajusta al siguiente esquema.

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

Este elemento de configuración permite que los atributos opcionales puedan configurar el comportamiento del indizador PriInfo. El valor predeterminado de `emitStrings` y `emitPaths` es `true`. Si `emitStrings`es `true`, en ese caso los candidatos de recursos con el atributo `type` establecido en "String" se incluirán en el índice, y en caso contrario se excluirán. Si 'emitPaths' es `true`, en ese caso los candidatos de recursos con el atributo `type` establecido en "Path" se incluirán en el índice, y en caso contrario se excluirán.

El siguiente es un ejemplo de configuración que incluye tipos de recurso String, pero omite tipos de recurso Path.

```xml
<indexer-config type="priinfo" emitStrings="true" emitPaths="false" />
```

Para indizarse, un archivo de volcado debe terminar con la extensión `.pri.xml` y debe ajustarse al siguiente esquema.

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" >\
  <xs:simpleType name="candidateType">\
    <xs:restriction base="xs:string">\
      <xs:pattern value="Path|String"/>\
    </xs:restriction>\
  </xs:simpleType>\
  <xs:complexType name="scopeType">\
    <xs:sequence>\
      <xs:element name="ResourceMapSubtree" type="scopeType" minOccurs="0" maxOccurs="unbounded"/>\
      <xs:element name="NamedResource" minOccurs="0" maxOccurs="unbounded">\
        <xs:complexType>\
          <xs:sequence>\
            <xs:element name="Decision" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:any processContents="skip" minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:anyAttribute processContents="skip" />\
              </xs:complexType>\
            </xs:element>\
            <xs:element name="Candidate" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:element name="QualifierSet" minOccurs="0" maxOccurs="unbounded">\
                    <xs:complexType>\
                      <xs:sequence>\
                        <xs:element name="Qualifier" minOccurs="0" maxOccurs="unbounded">\
                          <xs:complexType>\
                            <xs:attribute name="name" type="xs:string" use="required" />\
                            <xs:attribute name="value" type="xs:string" use="required" />\
                            <xs:attribute name="priority" type="xs:integer" use="required" />\
                            <xs:attribute name="scoreAsDefault" type="xs:decimal" use="required" />\
                            <xs:attribute name="index" type="xs:integer" use="required" />\
                          </xs:complexType>\
                        </xs:element>\
                      </xs:sequence>\
                      <xs:anyAttribute processContents="skip" />\
                    </xs:complexType>\
                  </xs:element>\
                  <xs:element name="Value" type="xs:string"  minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:attribute name="type" type="candidateType" use="required" />\
              </xs:complexType>\
            </xs:element>\
          </xs:sequence>\
          <xs:attribute name="name" use="required" type="xs:string" />\
          <xs:anyAttribute processContents="skip" />\
        </xs:complexType>\
      </xs:element>\
    </xs:sequence>\
    <xs:attribute name="name" use="required" type="xs:string" />\
    <xs:anyAttribute processContents="skip" />\
  </xs:complexType>\
  <xs:element name="PriInfo">\
    <xs:complexType>\
      <xs:sequence>\
        <xs:element name="PriHeader" >\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs ="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="QualifierInfo">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="ResourceMap">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:element name="VersionInfo">\
                <xs:complexType>\
                  <xs:anyAttribute processContents="skip" />\
                </xs:complexType>\
              </xs:element>\
              <xs:element minOccurs="0" maxOccurs="unbounded" name="ResourceMapSubtree" type="scopeType" />\
            </xs:sequence>\
            <xs:attribute name="name" type="xs:string" use="required" />\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
      </xs:sequence>\
    </xs:complexType>\
  </xs:element>\
</xs:schema>
```

MakePri.exe admite tipos de volcado 'Básico', 'Detallado', 'Esquema' y 'Resumen'. Para configurar MakePri.exe para que emita el tipo de volcado que puede leer el indizador PriInfo, incluye "/DumpType Detailed" al usar el comando `dump`.

MakePri.exe omite varios elementos del archivo `.pri.xml`. Estos elementos se calculan durante la indización o se especifican en el archivo de configuración MakePri.exe. Los valores, calificadores y nombres de recursos contenidos en el archivo de volcado se mantienen directamente en el nuevo archivo PRI. Sin embargo, el mapa de recursos de nivel superior no se mantiene en el PRI final. Los mapas de recursos se combinan como parte de la indización.

Este es un ejemplo de un recurso de tipo String válido de un archivo de volcado:

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

Este es un ejemplo de un recurso de tipo Path válido con dos candidatos de un archivo de volcado.

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

El indizador ResFiles se identifica por un atributo `type` de RESFILES. Indiza el contenido de un archivo `.resfiles`. Su elemento de configuración se ajusta al siguiente esquema.

```xml
<xs:schema id=\"resx\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResFilesType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(f|F)(i|I)(l|L)(e|E)(s|S))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResFilesType\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

Un archivo `.resfiles` es un archivo de texto que contiene una sencilla lista de rutas de archivos. Un archivo `.resfiles` puede contener comentarios "//". Aquí tienes un ejemplo.

```
Strings\component1\fr\elements.resjson
Images\logo.scale-100.png
Images\logo.scale-140.png
Images\logo.scale-180.png
```

## <a name="resjson"></a>ResJSON

El indizador ResJSON se identifica por un atributo `type` de RESJSON. Indexa el contenido de un archivo `.resjson`, que es un archivo de recursos de cadena. Su elemento de configuración se ajusta al siguiente esquema.

```xml
<xs:schema id=\"resjson\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResJsonType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(j|J)(s|S)(o|O)(n|N))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResJsonType\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

Un archivo `.resjson` contiene el texto JSON (consulta [Tipo de medios aplicación/json para notación de objetos JavaScript (JSON)](http://www.ietf.org/rfc/rfc4627.txt). El archivo debe contener un único objeto JSON con propiedades jerárquicas. Cada propiedad debe ser otro objeto JSON o un valor de cadena.

Las propiedades de JSON con nombres que comiencen por guión bajo ("_") no se compilan en el archivo PRI final, sino que se mantienen en el archivo de registro.

El archivo también puede contener comentarios "//" que se omiten durante el análisis.

El atributo `initialPath` pone todos los recursos en esta ruta de acceso inicial anteponiéndolo al nombre del recurso. Normalmente esto lo usarías al indizar recursos a de bibliotecas de clases. La opción predeterminada es estar en blanco.

## <a name="resw"></a>ResW

El indizador ResW se identifica por un atributo `type` de RESW. Indexa el contenido de un archivo `.resw`, que es un archivo de recursos de cadena. Su elemento de configuración se ajusta al siguiente esquema.

```xml
<xs:schema id=\"resw\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResxType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(w|W))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResxType\" use=\"required\"/>\
            <xs:attribute name=\"convertDotsToSlashes\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

Un archivo `.resw` es un archivo XML que se ajusta al siguiente esquema.

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

El atributo `convertDotsToSlashes` convierte todos los caracteres de punto (".") encontrados en los nombres de recursos (atributos de nombre de elemento de datos) en una barra diagonal "/", excepto cuando los caracteres de punto se encuentran entre "[" y "]".

El atributo `initialPath` pone todos los recursos en esta ruta de acceso inicial anteponiéndolo al nombre del recurso. Normalmente esto lo usarías al indizar recursos de bibliotecas de clases. La opción predeterminada es estar en blanco.

## <a name="related-topics"></a>Temas relacionados

* [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md)
* [Opciones de línea de comandos de MakePri.exe](makepri-exe-command-options.md)
* [Archivo de configuración de MakePri.exe](makepri-exe-configuration.md)
* [Tipo de medios aplicación/json para notación de objetos JavaScript (JSON)](http://www.ietf.org/rfc/rfc4627.txt)