---
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: Obtén información sobre cómo habilitar las pruebas y compras desde la aplicación en aplicaciones para UWP orientadas a versiones anteriores a Windows 10, versión 1607.
title: Pruebas y compras desde la aplicación con el espacio de nombres Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
keywords: uwp, compras desde la aplicación, in-app purchases, IAP, complementos, add-ons, pruebas, trials, Windows.ApplicationModel.Store
ms.localizationpriority: medium
ms.openlocfilehash: 72f5875721d17bda79842989c1ac22475a06e938
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/07/2018
ms.locfileid: "8780879"
---
# <a name="in-app-purchases-and-trials-using-the-windowsapplicationmodelstore-namespace"></a>Pruebas y compras desde la aplicación con el espacio de nombres Windows.ApplicationModel.Store

Puedes usar los miembros del espacio de nombres [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) para agregar las funcionalidades de pruebas y compras desde la aplicación a tu aplicación para la Plataforma universal de Windows (UWP) con el fin de rentabilizar la aplicación y agregar nuevas funcionalidades. Estas API también proporcionan acceso a la información de licencia de la aplicación.

En los artículos de esta sección se ofrecen instrucciones detalladas y ejemplos de código para usar los miembros del espacio de nombres **Windows.ApplicationModel.Store** para varios escenarios comunes. Para obtener una introducción a los conceptos básicos relacionados con las compras desde la aplicación en las aplicaciones para UWP, consulta [Pruebas y compras desde la aplicación](in-app-purchases-and-trials.md). Para obtener una muestra completa que demuestre cómo implementar pruebas y compras desde la aplicación con el espacio de nombres **Windows.ApplicationModel.Store**, consulta la [muestra de la Store](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store).

> [!IMPORTANT]
> El espacio de nombres **Windows.ApplicationModel.Store** ya no se está actualizando con las nuevas características. Si el proyecto de la aplicación está destinado a **Windows 10 Anniversary Edition (10.0, compilación 14393)** o una versión posterior de Visual Studio (es decir, está destinado a Windows 10, versión 1607 o posterior), te recomendamos que uses uno el espacio de nombres [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) en su lugar. Para obtener más información, consulta [Pruebas y compras desde la aplicación](https://msdn.microsoft.com/windows/uwp/monetize/in-app-purchases-and-trials). El espacio de nombres **Windows.ApplicationModel.Store** no se admite en aplicaciones de escritorio de Windows que usan el [Puente de escritorio](https://developer.microsoft.com/windows/bridges/desktop) o en las aplicaciones o juegos que usan un espacio aislado de desarrollo en el centro de partners (por ejemplo, este es el caso para cualquier juego que se integra con Xbox Live). Estos productos deben usar el espacio de nombres **Windows.Services.Store** para implementar compras desde la aplicación y periodos de prueba.

## <a name="get-started-with-the-currentapp-and-currentappsimulator-classes"></a>Introducción a las clases CurrentApp y CurrentAppSimulator

El punto de entrada principal al espacio de nombres **Windows.ApplicationModel.Store** es la clase [CurrentApp](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx). Esta clase proporciona métodos y propiedades estáticas que puedes usar para obtener información acerca de la aplicación actual y sus complementos disponibles, obtener información de licencia de la aplicación actual o sus complementos, comprar una aplicación o un complemento para el usuario actual y realizar otras tareas.

La clase [CurrentApp](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.aspx) obtiene los datos de Microsoft Store, por lo que debes tener una cuenta de desarrollador y la aplicación debe publicarse en la Store antes de poder usar esta clase correctamente en la aplicación. Antes de enviar tu aplicación a la Store, puedes probar el código con una versión simulada de esta clase denominada [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx). Tras probar la aplicación y antes de enviarla a Microsoft Store, debes reemplazar las instancias de **CurrentAppSimulator** por **CurrentApp**. La aplicación no conseguirá la certificación si usa **CurrentAppSimulator**.

Cuando se usa **CurrentAppSimulator**, el estado inicial de la licencias y los productos desde la aplicación de tu aplicación se describe en un archivo local en el equipo de desarrollo denominado WindowsStoreProxy.xml. Para obtener más información sobre este archivo, consulta [Uso del archivo WindowsStoreProxy.xml con CurrentAppSimulator](#proxy).

Para obtener más información sobre las tareas comunes que puedes realizar con **CurrentApp** y **CurrentAppSimulator**, consulta los artículos siguientes.

| Tema       | Descripción                 |
|----------------------------|-----------------------------|
| [Excluir o limitar las características de una versión de prueba](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | Si permites que los clientes puedan usar la aplicación gratis durante un período de prueba, puedes animarles a actualizar a la versión completa de la aplicación excluyendo o limitando algunas características durante el período de prueba. |
| [Habilitar compras de productos desde la aplicación](enable-in-app-product-purchases.md)      |  Independientemente de que la aplicación sea gratuita o no, puedes vender contenido, otras aplicaciones o nuevas funcionalidades de la aplicación (como el desbloqueo del nivel siguiente de un juego) desde la misma aplicación. Aquí te mostramos cómo habilitar estos productos en la aplicación.  |
| [Habilitar compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md)      | Ofrece productos consumibles desde la aplicación (artículos que se pueden comprar, usar y volver a comprar) a través de la plataforma de comercio de la Store para proporcionar a tus clientes una experiencia de compra sólida y de confianza. Esto es especialmente útil para cosas como monedas de juego (oro, monedas, etc.) que se pueden comprar y usar para comprar bonificaciones concretas. |
| [Administrar un catálogo extenso de productos desde la aplicación](manage-a-large-catalog-of-in-app-products.md)      |   Si tu aplicación ofrece un catálogo de productos de gran tamaño en la aplicación, también puedes seguir el proceso descrito en este tema para ayudar a administrar dicho catálogo.    |
| [Usar recibos para verificar compras de productos](use-receipts-to-verify-product-purchases.md)      |   Cada transacción de Microsoft Store que tiene como resultado una compra de producto satisfactoria también puede devolver un recibo de transacción que proporcione información sobre el producto enumerado y el coste abonado por el cliente. El acceso a esta información resulta útil en aquellos casos en los que la aplicación debe comprobar que un usuario compró tu aplicación o ha realizado compras de productos desde la aplicación en Microsoft Store. |

<span id="proxy" />

## <a name="using-the-windowsstoreproxyxml-file-with-currentappsimulator"></a>Uso del archivo WindowsStoreProxy.xml con CurrentAppSimulator

Cuando se usa **CurrentAppSimulator**, el estado inicial de la licencias y los productos desde la aplicación de tu aplicación se describe en un archivo local en el equipo de desarrollo denominado WindowsStoreProxy.xml. Los métodos de **CurrentAppSimulator** que modifican el estado de la aplicación, por ejemplo al comprar una licencia o al controlar una compra desde la aplicación, solo actualizan el estado del objeto **CurrentAppSimulator** en la memoria. El contenido de WindowsStoreProxy.xml no cambia. Cuando se vuelve a inicia la aplicación, el estado de licencia se revierte a lo que se describe en WindowsStoreProxy.xml.

Se crea un archivo WindowsStoreProxy.xml de manera predeterminada en la siguiente ubicación: %Perfil_usuario%\AppData\Local\Packages\\&lt;carpeta del paquete de la aplicación&gt;\LocalState\Microsoft\Windows Store\ApiData. Puedes editar este archivo para definir el escenario que quieres simular las propiedades **CurrentAppSimulator**.

Aunque se pueden modificar los valores de este archivo, te recomendamos que crees tu propio archivo WindowsStoreProxy.xml (en una carpeta de datos de tu proyecto de Visual Studio) para que **CurrentAppSimulator** lo use en su lugar. Al simular la transacción, llama a [ReloadSimulatorAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.reloadsimulatorasync) para cargar el archivo. Si no llamas a **ReloadSimulatorAsync** para cargar tu propio archivo WindowsStoreProxy.xml, **CurrentAppSimulator** creará o cargará (pero no sobrescribirá) siempre el archivo WindowsStoreProxy.xml.

> [!NOTE]
> Ten en cuenta que **CurrentAppSimulator** no se inicializa completamente hasta que se completa **ReloadSimulatorAsync**. Y, dado que **ReloadSimulatorAsync** es un método asincrónico, debe tenerse cuidado para evitar la condición de carrera de consultar **CurrentAppSimulator** en un subproceso mientras se inicializa en otro. Una técnica es usar una marca para indicar que la inicialización se ha completado. Debes usar una aplicación que se instale desde Microsoft Store debe usar **CurrentApp** en lugar de **CurrentAppSimulator**, y en ese caso, no se llama a **ReloadSimulatorAsync** y, por tanto, la condición de carrera recién mencionada no se aplica. Por este motivo, diseña el código para que funcione en ambos casos, tanto de forma asincrónica como sincrónica.


<span id="proxy-examples" />

### <a name="examples"></a>Ejemplos

Este ejemplo es un archivo WindowsStoreProxy.xml (con codificación UTF-16) que describe una aplicación con un modo de prueba que expira a las 05:00 (UTC) del 19 de enero de 2015.

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="UTF-16"?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>2B14D306-D8F8-4066-A45B-0FB3464C67F2</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/2B14D306-D8F8-4066-A45B-0FB3464C67F2</LinkUri>
      <CurrentMarket>en-US</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with a trial license</Name>
        <Description>Sample app for demonstrating trial license management</Description>
        <Price>4.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>true</IsTrial>
      <ExpirationDate>2015-01-19T05:00:00.00Z</ExpirationDate>
    </App>
  </LicenseInformation>
  <Simulation SimulationMode="Automatic">
    <DefaultResponse MethodName="LoadListingInformationAsync_GetResult" HResult="E_FAIL"/>
  </Simulation>
</CurrentApp>
```

El siguiente ejemplo es un archivo WindowsStoreProxy.xml (con codificación UTF-16) que describe una aplicación que se ha adquirido, tiene una función que expira a las 05:00 (UTC) del 19 de enero de 2015 y tiene una compra desde la aplicación de un consumible.

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-16" ?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>988b90e4-5d4d-4dea-99d0-e423e414ffbc</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/988b90e4-5d4d-4dea-99d0-e423e414ffbc</LinkUri>
      <CurrentMarket>en-us</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with several in-app products</Name>
        <Description>Sample app for demonstrating an expiring in-app product and a consumable in-app product</Description>
        <Price>5.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
    <Product ProductId="feature1" LicenseDuration="10" ProductType="Durable">
      <MarketData xml:lang="en-us">
        <Name>Expiring Item</Name>
        <Price>1.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
    <Product ProductId="consumable1" LicenseDuration="0" ProductType="Consumable">
      <MarketData xml:lang="en-us">
        <Name>Consumable Item</Name>
        <Price>2.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>false</IsTrial>
    </App>
    <Product ProductId="feature1">
      <IsActive>true</IsActive>
      <ExpirationDate>2015-01-19T00:00:00.00Z</ExpirationDate>
    </Product>
  </LicenseInformation>
  <ConsumableInformation>
    <Product ProductId="consumable1" TransactionId="00000001-0000-0000-0000-000000000000" Status="Active"/>
  </ConsumableInformation>
</CurrentApp>
```


<span id="proxy-schema" />

### <a name="schema"></a>Esquema

Esta sección muestra el archivo XSD que define la estructura del archivo WindowsStoreProxy.xml. Para aplicar este esquema al editor XML de Visual Studio cuando trabajes con el archivo WindowsStoreProxy.xml, haz lo siguiente:

1. Abre el archivo WindowsStoreProxy.xml en Visual Studio.
2. En el menú **XML**, haz clic en **Crear esquema**. Esto creará un archivo WindowsStoreProxy.xsd temporal basado en el contenido del archivo XML.
3. Sustituye el contenido de ese archivo .xsd por el esquema siguiente.
4. Guarda el archivo en una ubicación donde puede aplicarlo a los proyectos de varias aplicaciones.
5. Cambia a tu archivo WindowsStoreProxy.xml en Visual Studio.
6. En el menú **XML**, haz clic en **Esquemas** y busca la fila en la lista del archivo WindowsStoreProxy.xsd. Si la ubicación del archivo es no la que deseas (por ejemplo, si todavía se muestra el archivo temporal), haz clic en **Agregar**. Navega al archivo derecho y haz clic en **Aceptar**. Ahora deberías ver ese archivo en la lista. Asegúrate de que aparece una marca de verificación en la columna **Uso** para ese esquema.

Una vez hayas hecho esto, las modificaciones que realices en WindowsStoreProxy.xml estarán sujetas al esquema. Para obtener más información, consulta [Cómo: Seleccionar los esquemas XML que se van a usar](http://go.microsoft.com/fwlink/p/?LinkId=403014).

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:import namespace="http://www.w3.org/XML/1998/namespace"/>
  <xs:element name="CurrentApp" type="CurrentAppDefinition"></xs:element>
  <xs:complexType name="CurrentAppDefinition">
    <xs:sequence>
      <xs:element name="ListingInformation" type="ListingDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LicenseInformation" type="LicenseDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ConsumableInformation" type="ConsumableDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Simulation" type="SimulationDefinition" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:simpleType name="ResponseCodes">
    <xs:restriction base="xs:string">
      <xs:enumeration value="S_OK">
        <xs:annotation>
          <xs:documentation>0x00000000</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_INVALIDARG">
        <xs:annotation>
          <xs:documentation>0x80070057</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_CANCELLED">
        <xs:annotation>
          <xs:documentation>0x800704C7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_FAIL">
        <xs:annotation>
          <xs:documentation>0x80004005</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_OUTOFMEMORY">
        <xs:annotation>
          <xs:documentation>0x8007000E</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="ERROR_ALREADY_EXISTS">
        <xs:annotation>
          <xs:documentation>0x800700B7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="ConsumableStatus">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Active"/>
      <xs:enumeration value="PurchaseReverted"/>
      <xs:enumeration value="PurchasePending"/>
      <xs:enumeration value="ServerError"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="StoreMethodName">
    <xs:restriction base="xs:string">
      <xs:enumeration value="RequestAppPurchaseAsync_GetResult" id="RPPA"/>
      <xs:enumeration value="RequestProductPurchaseAsync_GetResult" id="RFPA"/>
      <xs:enumeration value="LoadListingInformationAsync_GetResult" id="LLIA"/>
      <xs:enumeration value="ReportConsumableFulfillmentAsync_GetResult" id="RPFA"/>
      <xs:enumeration value="LoadListingInformationByKeywordsAsync_GetResult" id="LLIKA"/>
      <xs:enumeration value="LoadListingInformationByProductIdAsync_GetResult" id="LLIPA"/>
      <xs:enumeration value="GetUnfulfilledConsumablesAsync_GetResult" id="GUC"/>
      <xs:enumeration value="GetAppReceiptAsync_GetResult" id="GARA"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="SimulationMode">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Interactive"/>
      <xs:enumeration value="Automatic"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ListingDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppListingDefinition"/>
      <xs:element name="Product" type="ProductListingDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ConsumableDefinition">
    <xs:sequence>
      <xs:element name="Product" type="ConsumableProductDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppListingDefinition">
    <xs:sequence>
      <xs:element name="AppId" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LinkUri" type="xs:anyURI" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrentMarket" type="xs:language" minOccurs="1" maxOccurs="1"/>
      <xs:element name="AgeRating" type="xs:unsignedInt" minOccurs="1" maxOccurs="1"/>
      <xs:element name="MarketData" type="MarketSpecificAppData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="MarketSpecificAppData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="MarketSpecificProductData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Tag" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Keywords" type="KeywordDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="ImageUri" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="ProductListingDefinition">
    <xs:sequence>
      <xs:element name="MarketData" type="MarketSpecificProductData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="LicenseDuration" type="xs:integer" use="optional"/>
    <xs:attribute name="ProductType" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:simpleType name="guid">
    <xs:restriction base="xs:string">
      <xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ConsumableProductDefinition">
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="TransactionId" type="guid" use="required"/>
    <xs:attribute name="Status" type="ConsumableStatus" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="LicenseDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppLicenseDefinition"/>
      <xs:element name="Product" type="ProductLicenseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="IsTrial" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ProductLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute name="ProductId" type="xs:string" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="SimulationDefinition" >
    <xs:sequence>
      <xs:element name="DefaultResponse" type="DefaultResponseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="SimulationMode" type="SimulationMode" use="optional"/>
  </xs:complexType>
  <xs:complexType name="DefaultResponseDefinition">
    <xs:attribute name="MethodName" type="StoreMethodName" use="required"/>
    <xs:attribute name="HResult" type="ResponseCodes" use="required"/>
  </xs:complexType>
  <xs:complexType name="KeywordDefinition">
    <xs:sequence>
      <xs:element name="Keyword" type="xs:string" minOccurs="0" maxOccurs="10"/>
    </xs:sequence>
  </xs:complexType>
</xs:schema>
```


<span id="proxy-descriptions" />

### <a name="element-and-attribute-descriptions"></a>Descripciones de los elementos y los atributos

Esta sección describe los elementos y atributos del archivo WindowsStoreProxy.xml.

El elemento raíz de este archivo es el elemento **CurrentApp**, que representa la aplicación actual. Este elemento contiene los siguientes elementos secundarios.

|  Elemento  |  Obligatorio  |  Cantidad  |  Descripción   |
|-------------|------------|--------|--------|
|  [ListingInformation](#listinginformation)  |    Sí        |  1  |  Contiene datos de la descripción de la aplicación.            |
|  [LicenseInformation](#licenseinformation)  |     Sí       |   1    |   Describe las licencias disponibles para esta aplicación y sus complementos duraderos.     |
|  [ConsumableInformation](#consumableinformation)  |      No      |   0 o 1   |   Describe los complementos consumibles que están disponibles para esta aplicación.      |
|  [Simulation](#simulation)  |     No       |      0 o 1      |   Describe cómo las llamadas a diversos métodos [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) funcionarán en la aplicación durante las pruebas.    |

<span id="listinginformation" />

#### <a name="listinginformation-element"></a>Elemento ListingInformation

Este elemento contiene datos de la descripción de la aplicación. **ListingInformation** es un elemento secundario obligatorio del elemento **CurrentApp**.

**ListingInformation** contiene los siguientes elementos secundarios.

|  Elemento  |  Obligatorio  |  Cantidad  |  Descripción   |
|-------------|------------|--------|--------|
|  [Aplicación](#app-child-of-listinginformation)  |    Sí   |  1   |    Proporciona datos sobre la aplicación.         |
|  [Product](#product-child-of-listinginformation)  |    No  |  0 o más   |      Describe un complemento de la aplicación.     |     |

<span id="app-child-of-listinginformation"/>

#### <a name="app-element-child-of-listinginformation"></a>Elemento de la aplicación (elemento secundario de ListingInformation)

Este elemento describe la licencia de la aplicación. **App** es un elemento secundario obligatorio del elemento [ListingInformation](#listinginformation).

**App** contiene los siguientes elementos secundarios.

|  Elemento  |  Obligatorio  |  Cantidad  | Descripción   |
|-------------|------------|--------|--------|
|  **AppId**  |    Sí   |  1   |   El GUID que identifica la aplicación en la Store. Puede ser cualquier GUID para pruebas.        |
|  **LinkUri**  |    Sí  |  1   |    El URI de la página de descripción en la Store. Puede ser cualquier identificador URI válido para pruebas.         |
|  **CurrentMarket**  |    Sí  |  1   |    El país o la región del cliente.         |
|  **AgeRating**  |    Sí  |  1   |     Un número entero que representa la clasificación por edades mínima de la aplicación. Este es el mismo valor que se especificaría en el centro de partners al enviar la aplicación. Los valores usados por la Store son: 3, 7, 12 y 16. Para obtener más información sobre estas clasificaciones, consulta [Clasificación por edades](../publish/age-ratings.md).        |
|  [MarketData](#marketdata-child-of-app)  |    Sí  |  1 o más      |    Contiene información sobre la aplicación para un país o una región determinados. Para cada país o región en que se muestra la aplicación, debes incluir un elemento **MarketData**.       |    |

<span id="marketdata-child-of-app"/>

#### <a name="marketdata-element-child-of-app"></a>Elemento MarketData (elemento secundario del elemento App)

Este elemento, proporciona información acerca de la aplicación para un país o una región determinados. Para cada país o región en que se muestra la aplicación, debes incluir un elemento **MarketData**. **MarketData** es un elemento secundario obligatorio del elemento [App](#app-child-of-listinginformation).

**MarketData** contiene los siguientes elementos secundarios.

|  Elemento  |  Obligatorio  |  Cantidad  | Descripción   |
|-------------|------------|--------|--------|
|  **Name**  |    Sí   |  1   |   El nombre de la aplicación en este país o región.        |
|  **Descripción**  |    Sí  |  1   |      La descripción de la aplicación para este país o región.       |
|  **Price**  |    Sí  |  1   |     El precio de la aplicación en este país o región.        |
|  **CurrencySymbol**  |    Sí  |  1   |     El símbolo de la moneda que se usa en este país o región.        |
|  **CurrencyCode**  |    No  |  0 o 1      |      El código de la moneda que se usa en este país o región.         |  |

**MarketData** tiene los atributos siguientes.

|  Atributo  |  Obligatorio  |  Descripción   |
|-------------|------------|----------------|
|  **xml:lang**  |    Sí        |     Especifica el país o región al que se aplica la información de datos del mercado.          |  |

<span id="product-child-of-listinginformation"/>

#### <a name="product-element-child-of-listinginformation"></a>Elemento Product (elemento secundario de ListingInformation)

Este elemento describe un complemento de la aplicación. **Product** es un elemento secundario opcional del elemento [ListingInformation](#listinginformation) y contiene uno o más elementos [MarketData](#marketdata-child-of-product).

**Product** tiene los atributos siguientes.

|  Atributo  |  Obligatorio  |  Descripción   |
|-------------|------------|----------------|
|  **ProductId**  |    Sí        |    Contiene la cadena usada por la aplicación para identificar el complemento.           |
|  **LicenseDuration**  |    No        |    Indica el número de días durante los cuales la licencia será válida tras adquirir el artículo. La fecha de caducidad de la nueva licencia creada por la compra de un producto es la fecha de compra más la duración de la licencia. Este atributo se usa solo si el atributo **ProductType ofrece** atributo es **Durable**; este atributo se omite en el caso de complementos consumibles.           |
|  **ProductType**  |    No        |    Contiene un valor para identificar la persistencia del producto desde la aplicación. Los valores admitidos son **Durable** (el predeterminado) y **Consumible**. En el caso de los tipos duraderos (Durable), un elemento [Product](#product-child-of-licenseinformation) identifica información adicional bajo [LicenseInformation](#licenseinformation); en el caso los tipos consumibles, un elemento [Product](#product-child-of-consumableinformation) muestra información adicional bajo [ConsumableInformation](#consumableinformation).           |  |

<span id="marketdata-child-of-product"/>

#### <a name="marketdata-element-child-of-product"></a>Elemento MarketData (elemento secundario del elemento Product)

Este elemento, proporciona información acerca del complemento para un país o una región determinados. Para cada país o región en que se muestra el complemento, debes incluir un elemento **MarketData**. **MarketData** es un elemento secundario obligatorio del elemento [Product](#product-child-of-listinginformation).

**MarketData** contiene los siguientes elementos secundarios.

|  Elemento  |  Obligatorio  |  Cantidad  | Descripción   |
|-------------|------------|--------|--------|
|  **Name**  |    Sí   |  1   |   El nombre del complemento en este país o región.        |
|  **Price**  |    Sí  |  1   |     El precio del complemento en este país o región.        |
|  **CurrencySymbol**  |    Sí  |  1   |     El símbolo de la moneda que se usa en este país o región.        |
|  **CurrencyCode**  |    No  |  0 o 1      |      El código de la moneda que se usa en este país o región.         |  
|  **Descripción**  |    No  |   0 o 1   |      La descripción del complemento para este país o región.       |
|  **Tag**  |    No  |   0 o 1   |      Los [datos del desarrollador personalizados](../publish/enter-add-on-properties.md#custom-developer-data) (también denominados etiqueta) para el complemento.       |
|  **Palabras clave**  |    No  |   0 o 1   |      Contiene hasta 10 elementos **Keyword** que contienen las [palabras clave](../publish/enter-add-on-properties.md#keywords) para el complemento.       |
|  **ImageUri**  |    No  |   0 o 1   |      El [identificador URI de la imagen](../publish/create-add-on-store-listings.md#icon) en la descripción del complemento.           |  |

**MarketData** tiene los atributos siguientes.

|  Atributo  |  Obligatorio  |  Descripción   |
|-------------|------------|----------------|
|  **xml:lang**  |    Sí        |     Especifica el país o región al que se aplica la información de datos del mercado.          |  |

<span id="licenseinformation"/>

#### <a name="licenseinformation-element"></a>Elemento LicenseInformation

Este elemento describe las licencias disponibles para esta aplicación y sus productos desde la aplicación duraderos. **LicenseInformation** es un elemento secundario obligatorio del elemento **CurrentApp**.

**LicenseInformation** contiene los siguientes elementos secundarios.

|  Elemento  |  Obligatorio  |  Cantidad  | Descripción   |
|-------------|------------|--------|--------|
|  [Aplicación](#app-child-of-licenseinformation)  |    Sí   |  1   |    Describe la licencia de la aplicación.         |
|  [Product](#product-child-of-licenseinformation)  |    No  |  0 o más   |      Describe el estado de la licencia de un complemento duradero en la aplicación.         |   |

La tabla siguiente muestra cómo simular algunas condiciones comunes mediante la combinación de valores que estén en los elementos **App** y **Product**.

|  Condición para simular  |  IsActive  |  IsTrial  | ExpirationDate   |
|-------------|------------|--------|--------|
|  Licencia completa  |    true   |  false  |    Ausente. En realidad, puede estar presente y especificar una fecha futura, pero te sugerimos que omitas el elemento en el archivo XML. Si está presente y especifica una fecha en el pasado, **IsActive** se omitirá y se supondrá que es false.          |
|  En período de prueba  |    true  |  true   |      &lt;una fecha y una hora futuras&gt; Este elemento debe estar presente porque **IsTrial** es true. Puedes visitar un sitio web que muestre la hora universal coordinada (UTC) actual para saber cuánto en el futuro establecer esta opción para obtener el período de prueba restante que desees.         |
|  Período de prueba expirado  |    false  |  true   |      &lt;una fecha y una hora pasadas&gt; Este elemento debe estar presente porque **IsTrial** es true. Puedes visitar un sitio web que muestre la hora universal coordinada (UTC) actual para saber cuándo es "el pasado" en UTC.         |
|  No válido  |    false  | false       |     &lt;cualquier valor o se omite&gt;          |  |

<span id="app-child-of-licenseinformation"/>

#### <a name="app-element-child-of-licenseinformation"></a>Elemento App (elemento secundario de LicenseInformation)

Este elemento describe la licencia de la aplicación. **App** es un elemento secundario obligatorio del elemento [LicenseInformation](#licenseinformation).

**App** contiene los siguientes elementos secundarios.

|  Elemento  |  Obligatorio  |  Cantidad  | Descripción   |
|-------------|------------|--------|--------|
|  **IsActive**  |    Sí   |  1   |    Describe el estado actual de la licencia de esta aplicación. El valor **true** indica que la licencia es válida; **false** indica que la licencia no es válida. Normalmente, este valor es **true** tanto si la aplicación tiene un modo de prueba como si no.  Establece este valor en **false** para probar cómo se comporta la aplicación cuando tiene una licencia no válida.           |
|  **IsTrial**  |    Sí  |  1   |      Describe el estado actual del período de prueba de esta aplicación. El valor **true** indica que la aplicación se está usando durante el período de prueba; **false** indica que la aplicación no está en el período de prueba, bien porque se ha adquirido la aplicación o bien porque ha expirado dicho período de prueba.         |
|  **ExpirationDate**  |    No  |  0 o 1       |     La fecha en la que expira el período de prueba de esta aplicación, en hora universal coordinada (UTC). La fecha debe expresarse como: aaaa-mm-ddThh:mm:ss.ssZ. Por ejemplo, las 05:00 del 19 de enero de 2015 se especificaría como 2015-01-19T05:00:00.00Z. Este elemento es obligatorio si **IsTrial** es **true**. De lo contrario, no es obligatorio.          |  |

<span id="product-child-of-licenseinformation"/>

#### <a name="product-element-child-of-licenseinformation"></a>Elemento Product (elemento secundario de LicenseInformation)

Este elemento describe el estado de la licencia de un complemento duradero en la aplicación. **Product** es un elemento secundario opcional del elemento [LicenseInformation](#licenseinformation).

**Product** contiene los siguientes elementos secundarios.

|  Elemento  |  Obligatorio  |  Cantidad  | Descripción   |
|-------------|------------|--------|--------|
|  **IsActive**  |    Sí   |  1     |    Describe el estado actual de la licencia de este complemento. El valor **true** indica que el complemento se puede usar; por su parte, **false** indica que el complemento no se puede usar o no se ha adquirido.           |
|  **ExpirationDate**  |    No   |  0 o 1     |     La fecha en la que caduca el complemento, en hora universal coordinada (UTC). La fecha debe expresarse como: aaaa-mm-ddThh:mm:ss.ssZ. Por ejemplo, las 05:00 del 19 de enero de 2015 se especificaría como 2015-01-19T05:00:00.00Z. Si este elemento está presente, el complemento tiene una fecha de caducidad. Si no está presente, el complemento no caduca.  |  

**Product** tiene los atributos siguientes.

|  Atributo  |  Obligatorio  |  Descripción   |
|-------------|------------|----------------|
|  **ProductId**  |    Sí        |   Contiene la cadena usada por la aplicación para identificar el complemento.            |
|  **OfferId**  |     No       |   Contiene la cadena que la aplicación usa para identificar la categoría a la que pertenece el complemento. Esto proporciona compatibilidad para catálogos de elementos de gran tamaño, como se describe en [Administrar un catálogo extenso de productos desde la aplicación](manage-a-large-catalog-of-in-app-products.md).           |

<span id="simulation"/>

#### <a name="simulation-element"></a>Elemento Simulation

Este elemento describe cómo las llamadas a diversos métodos [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentappsimulator.aspx) funcionarán en la aplicación durante las pruebas. **Simulation** es un elemento secundario opcional del elemento **CurrentApp** y contiene cero o más elementos [DefaultResponse](#defaultresponse).

**Simulation** tiene los atributos siguientes.

|  Atributo  |  Obligatorio  |  Descripción   |
|-------------|------------|----------------|
|  **SimulationMode**  |    No        |      Los valores pueden ser **Interactive** o **Automatic**. Cuando este atributo se establece en **Automatic**, los métodos devolverán automáticamente los códigos de error HRESULT especificados. Esto puede usarse cuando se ejecutan casos de prueba automatizados.       |

<span id="defaultresponse"/>

#### <a name="defaultresponse-element"></a>Elemento DefaultResponse

Este elemento describe el código de error predeterminado devuelto por un método **CurrentAppSimulator**. **DefaultResponse** es un elemento secundario opcional del elemento [Simulation](#simulation).

**DefaultResponse** tiene los atributos siguientes.

|  Atributo  |  Obligatorio  |  Descripción   |
|-------------|------------|----------------|
|  **MethodName**  |    Sí        |   Asigna este atributo a uno de los valores de enumeración que se muestran para el tipo **StoreMethodName** en el [esquema](#schema). Cada uno de estos valores de enumeración representa un método **CurrentAppSimulator** para el que deseas simular un valor devuelto de código de error en la aplicación durante las pruebas. Por ejemplo, el valor **RequestAppPurchaseAsync_GetResult** indica que deseas simular el valor devuelto de código de error del método [RequestAppPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.requestapppurchaseasync).            |
|  **HResult**  |     Sí       |   Asigna este atributo a uno de los valores de enumeración que se muestran para el tipo **ResponseCodes** en el [esquema](#schema). Cada uno de estos valores de enumeración representa el código de error que deseas recuperar para el método asignado al atributo **MethodName** para este elemento **DefaultResponse**.           |

<span id="consumableinformation"/>

#### <a name="consumableinformation-element"></a>Elemento ConsumableInformation

Este elemento describe los complementos consumibles disponibles para esta aplicación. **ConsumableInformation** es un elemento secundario opcional del elemento **CurrentApp** y puede contener cero o más elementos [Product](#product-child-of-consumableinformation).

<span id="product-child-of-consumableinformation"/>

#### <a name="product-element-child-of-consumableinformation"></a>Elemento Product (elemento secundario de ConsumableInformation)

Este elemento describe un complemento consumible. **Product** es un elemento secundario opcional del elemento [ConsumableInformation](#consumableinformation).

**Product** tiene los atributos siguientes.

|  Atributo  |  Obligatorio  |  Descripción   |
|-------------|------------|----------------|
|  **ProductId**  |    Sí        |   Contiene la cadena usada por la aplicación para identificar el complemento consumible.            |
|  **TransactionId**  |     Sí       |   Contiene un GUID (en forma de cadena) usado por la aplicación para realizar un seguimiento de la transacción de compra de un consumible por el proceso de suministro. Consulta [Habilita compras de productos consumibles desde la aplicación](enable-consumable-in-app-product-purchases.md)            |
|  **Status**  |      Sí      |  Contiene la cadena usada por la aplicación para indicar el estado de suministro de un consumible. Los valores pueden ser **Active**, **PurchaseReverted**, **PurchasePending** o **ServerError**.             |
|  **OfferId**  |     No       |    Contiene la cadena que la aplicación usa para identificar la categoría a la que pertenece el consumible. Esto proporciona compatibilidad para catálogos de elementos de gran tamaño, como se describe en [Administrar un catálogo extenso de productos desde la aplicación](manage-a-large-catalog-of-in-app-products.md).           |
