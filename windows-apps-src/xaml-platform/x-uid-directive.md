---
author: jwmsft
description: Proporciona un identificador único para los elementos de marcado. En XAML de la Plataforma universal de Windows (UWP), este identificador único se usa en los procesos y herramientas de localización de XAML, por ejemplo, en el uso de recursos de un archivo de recursos .resw.
title: Directiva xUid
ms.assetid: 9FD6B62E-D345-44C6-B739-17ED1A187D69
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4bec4bd5d35fc2bb3013b37c1386520a769ddeb6
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2018
ms.locfileid: "6273995"
---
# <a name="xuid-directive"></a>Directiva x:Uid


Proporciona un identificador único para los elementos de marcado. En XAML de la plataforma universal de Windows (UWP), este identificador único se usa en los procesos y herramientas de localización de XAML, por ejemplo, en el uso de recursos de un archivo de recursos .resw.

## <a name="xaml-attribute-usage"></a>Uso del atributo XAML

``` syntax
<object x:Uid="stringID".../>
```

## <a name="xaml-values"></a>Valores de XAML

| Término | Descripción |
|------|-------------|
| stringID | Una cadena que identifica exclusivamente un elemento XAML en una aplicación y se convierte en parte de la ruta de acceso del recurso en un archivo de recursos. Consulta Observaciones.| 

## <a name="remarks"></a>Observaciones

Usa **x:Uid** para identificar un elemento de objeto en tu código XAML. Este elemento de objeto suele ser una instancia de una clase de control u otro elemento mostrado en una interfaz de usuario. La relación entre la cadena que usas en **x:Uid** y las cadenas que usas en un archivo de recursos es que las cadenas del archivo de recursos son **x:Uid** seguida de un punto (.) y, a continuación, seguida del nombre de una propiedad específica del elemento que se está localizando. Observa este ejemplo:

``` syntax
<Button x:Uid="GoButton" Content="Go"/>
```

Para especificar contenido para reemplazar el texto para mostrar **Go**, debes especificar un nuevo recurso procedente de un archivo de recursos. El archivo de recursos debería contener una entrada para el recurso denominada "GoButton.Content". [**Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) es, en este caso, una propiedad específica heredada por la clase [**Button**](/uwp/api/windows.ui.xaml.controls.button). También podrías proporcionar valores localizados para otras propiedades de este botón; por ejemplo, podrías proporcionar un valor basado en recursos para "GoButton.FlowDirection". Para obtener más información acerca de cómo usar **x:Uid** y archivos de recursos juntos, consulta [Localizar las cadenas de la interfaz de usuario y el manifiesto de paquete de la aplicación](../app-resources/localize-strings-ui-manifest.md).

Desde un punto de vista práctico, la validez de las cadenas que pueden usarse para un valor de **x:Uid** se determina según las cadenas que son legales como identificador en un archivo de recursos y una ruta de acceso de recurso.

**x:Uid** se diferencia de **x:Name** por el escenario de localización de XAML establecido; gracias a esto, los identificadores que se usan para la localización no tienen dependencias en las implicaciones del modelo de programación de **x:Name**. Además, **x:Name** está sometido al concepto de ámbito de nombres XAML, mientras que la exclusividad de **x:Uid** se controla mediante el sistema de índice de recursos del paquete (PRI). Para obtener más información, consulta [Sistema de administración de recursos](../app-resources/resource-management-system.md).

El lenguaje XAML de UWP tiene algunas reglas para la exclusividad de **x:Uid** diferentes de las tecnologías anteriores que usaban XAML. En XAML de UWP, es legal que exista el mismo valor de ID **x:Uid** como directiva en varios elementos XAML. Sin embargo, cada uno de estos elementos debe compartir la misma lógica de resolución para resolver los recursos de un archivo de recursos. Por otra parte, todos los archivos XAML de un proyecto comparten un único ámbito de recursos con fines de resolución de **x:Uid**; no existe el concepto de ámbitos **x:Uid** que se alinean con archivos XAML individuales.

En algunos casos usarás una ruta de acceso de recurso en lugar de la funcionalidad integrada del sistema de índice de recursos del paquete (PRI). Cualquier cadena que se usa como un valor de **x: Uid** define una ruta de acceso de recurso que empieza por ms-resource:///Resources/ e incluye la cadena **x: Uid**. La ruta de acceso se completa con los nombres de las propiedades que especifiques en un archivo de recursos o que intervengan de otro modo en la selección de destinos.

No incluyas **x:Uid** en elementos de propiedad, ya que no está permitido en XAML de Windows Runtime.

