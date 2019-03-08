---
ms.assetid: D06AA3F5-CED6-446E-94E8-713D98B13CAA
title: Crear un selector de dispositivos
description: La creación de un selector de dispositivos permite limitar los dispositivos en los que se realizan búsquedas al enumerar los dispositivos.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 01a4bfc2ec4c1d442058dbb6009065541f93cc7f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57652500"
---
# <a name="build-a-device-selector"></a>Crear un selector de dispositivos



**API importantes**

- [**Windows.Devices.Enumeration**](https://docs.microsoft.com/en-us/uwp/api/Windows.Devices.Enumeration)

La creación de un selector de dispositivos permite limitar los dispositivos en los que se realizan búsquedas al enumerar los dispositivos. Esto te permitirá obtener solo los resultados relevantes y también mejorará el rendimiento del sistema. En la mayoría de los escenarios, obtienes un selector de dispositivos de una pila de dispositivos. Por ejemplo, puedes usar [**GetDeviceSelector**](https://msdn.microsoft.com/library/windows/apps/Dn264015) para los dispositivos detectados a través de USB. Estos selectores de dispositivos devuelven una cadena de sintaxis de consulta avanzada (AQS). Si no estás familiarizado con el formato AQS, puedes leer más información en [Uso de la sintaxis de consulta avanzada mediante programación](https://msdn.microsoft.com/library/windows/desktop/Bb266512).

## <a name="building-the-filter-string"></a>Crear la cadena de filtro

Existen algunos casos en los que necesitarás enumerar dispositivos y no habrá ningún selector de dispositivos disponible para tu escenario. Un selector de dispositivos es una cadena de filtro AQS que contiene la siguiente información. Antes de crear una cadena de filtro, debes conocer algunas partes clave de información sobre los dispositivos que quieres enumerar.

-   El [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de los dispositivos en los que estás interesado. Para obtener más información sobre cómo **DeviceInformationKind** repercute en la enumeración de dispositivos, consulta [Enumerar dispositivos](enumerate-devices.md).
-   Cómo crear una cadena de filtro AQS, que se explica en este tema.
-   Las propiedades en las que estás interesado. Las propiedades disponibles dependerán del [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991). Consulta [Propiedades de información de dispositivo](device-information-properties.md) para obtener más información.
-   Los protocolos que vas a consultar. Solo son necesarios si buscas dispositivos a través de una red inalámbrica o cableada. Para obtener más información sobre cómo hacerlo, consulta el tema [Enumerar dispositivos a través de una red](enumerate-devices-over-a-network.md).

Al usar la API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459), a menudo combinas el selector de dispositivos con el tipo de dispositivo que te interesa. La lista disponible de tipos de dispositivos se define mediante la enumeración [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991). Esta combinación de factores te ayuda a filtrar los dispositivos que te interesan de los que están disponibles. Si no se especifica **DeviceInformationKind** o el método que se va a usar no proporciona ningún parámetro **DeviceInformationKind**, el tipo predeterminado será **DeviceInterface**.

La API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) usa la sintaxis AQS canónica, pero no se admiten todos los operadores. Para obtener una lista de propiedades que están disponibles cuando se construye la cadena de filtro, consulta [Propiedades de información de dispositivo](device-information-properties.md).

**Precaución**  propiedades personalizadas que se definen mediante la `{GUID} PID` no se puede utilizar el formato al construir la cadena de filtro AQS. Esto se debe a que el tipo de propiedad se deriva del nombre de propiedad conocido.

 

En la siguiente tabla se muestran los operadores AQS y los tipos de parámetros que admiten.

| Operador                       | Tipos de parámetros admitidos                                                             |
|--------------------------------|-----------------------------------------------------------------------------|
| **COP\_IGUAL**                 | Cadena, booleano, GUID, UInt16, UInt32                                       |
| **COP\_NOTEQUAL**              | Cadena, booleano, GUID, UInt16, UInt32                                       |
| **COP\_LESSTHAN**              | UInt16, UInt32                                                              |
| **COP\_GREATERTHAN**           | UInt16, UInt32                                                              |
| **COP\_LESSTHANOREQUAL**       | UInt16, UInt32                                                              |
| **COP\_GREATERTHANOREQUAL**    | UInt16, UInt32                                                              |
| **COP\_VALOR\_CONTAINS**       | Cadena, matriz de cadena, matriz booleana, matriz GUID, matriz UInt16, UInt32 matriz |
| **COP\_VALOR\_NOTCONTAINS**    | Cadena, matriz de cadena, matriz booleana, matriz GUID, matriz UInt16, UInt32 matriz |
| **COP\_VALOR\_STARTSWITH**     | Cadena                                                                      |
| **COP\_VALOR\_ENDSWITH**       | Cadena                                                                      |
| **COP\_DOSWILDCARDS**          | No se admite                                                               |
| **COP\_WORD\_IGUAL**           | No se admite                                                               |
| **COP\_WORD\_STARTSWITH**      | No se admite                                                               |
| **COP\_APLICACIÓN\_ESPECÍFICO** | No se admite                                                               |


> **Sugerencia**  puede especificar **NULL** para **COP\_igual** o **COP\_NOTEQUAL**. Esto se traduce en una propiedad sin ningún valor o en que el valor no existe. En AQS, especifique **NULL** mediante el uso de corchetes vacíos \[ \].

> **Importante**  cuando se usa el **COP\_valor\_CONTAINS** y **COP\_valor\_NOTCONTAINS** operadores, se comportan de manera diferente con cadenas y matrices de cadenas. En el caso de las cadenas, el sistema realizará una búsqueda sin distinguir entre mayúsculas y minúsculas para ver si el dispositivo contiene la cadena indicada como subcadena. En el caso de las matrices de cadenas, no se buscan las subcadenas. Con la matriz de cadenas, la matriz se busca para ver si contiene toda la cadena especificada. No es posible buscar una matriz de cadena para ver si los elementos de la matriz contienen una subcadena.

Si no se puedes crear una única cadena de filtro AQS que defina el ámbito de los resultados de forma adecuada, puedes filtrar los resultados después de recibirlos. Sin embargo, si decides hacerlo, te recomendamos que limites los resultados de la cadena de filtro AQS inicial tanto como puedas cuando la proporciones a la API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459). Esto ayudará a mejorar el rendimiento de la aplicación.

## <a name="aqs-string-examples"></a>Ejemplos de cadena AQS

En los siguientes ejemplos se muestra cómo se puede usar la sintaxis AQS para limitar los dispositivos que deseas enumerar. Todas estas cadenas de filtro se emparejan con [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) para crear un filtro completo. Si no se especifica ningún tipo, recuerda que el tipo predeterminado es **DeviceInterface**.

Cuando este filtro está emparejado con una enumeración [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de **DeviceInterface**, se enumeran todos los objetos que contienen la clase de interfaz de captura de audio y que están actualmente habilitados. **=** se traduce en **COP\_es igual a**.

``` syntax
System.Devices.InterfaceClassGuid:="{2eef81be-33fa-4800-9670-1cd474972c3f}" AND
System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True
```

Cuando este filtro está emparejado con una enumeración [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de **Device**, se enumeran todos los objetos que tienen al menos un identificador de hardware de GenCdRom. **~~** se traduce en **COP\_valor\_CONTAINS**.

``` syntax
System.Devices.HardwareIds:~~"GenCdRom"
```

Cuando este filtro está emparejado con una enumeración [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de **DeviceContainer**, se enumeran todos los objetos que tienen un nombre de modelo que contiene la subcadena Microsoft. **~~** se traduce en **COP\_valor\_CONTAINS**.

``` syntax
System.Devices.ModelName:~~"Microsoft"
```

Cuando este filtro está emparejado con una enumeración [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de **DeviceInterface**, se enumeran todos los objetos que tienen un nombre que comienza por la subcadena Microsoft. **~&lt;** se traduce en **COP\_STARTSWITH**.

``` syntax
System.ItemNameDisplay:~<"Microsoft"
```

Cuando este filtro está emparejado con una enumeración [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de **Device**, se enumeran todos los objetos que tienen un conjunto de propiedades **System.Devices.IpAddress**. **&lt;&gt;\[\]** se traduce en **COP\_NOTEQUALS** combinado con un **NULL** valor.

``` syntax
System.Devices.IpAddress:<>[]
```

Cuando este filtro está emparejado con una enumeración [**DeviceInformationKind**](https://msdn.microsoft.com/library/windows/apps/Dn948991) de **Device**, se enumeran todos los objetos que no tienen un conjunto de propiedades **System.Devices.IpAddress**. **=\[\]** se traduce en **COP\_es igual a** combinado con un **NULL** valor.

``` syntax
System.Devices.IpAddress:=[]
```

 

 
