---
title: Cadenas localizadas
description: Obtenga información sobre cómo localizar cadenas en el centro de partners
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 11/17/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, juegos, uwp, windows 10, Xbox, localizar cadenas, centro de partners
ms.openlocfilehash: 127f566dc5ae57b920d396623b6a84ff5d5eed96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656600"
---
# <a name="configuring-localized-strings-in-partner-center"></a>Configurar cadenas traducidas en el centro de partners

Puede usar esta página para localizar todas las configuraciones de Xbox Live a todos los idiomas que admite el juego. Todas las configuraciones de servicio que ha creado en cualquiera de las páginas de Xbox Live posteriores se agregarán al archivo que desea descargar.

Puede usar [centro de partners](https://partner.microsoft.com/dashboard) para configurar las cadenas localizadas en todos los idiomas asociados con su juego. Agregar configuración haciendo lo siguiente:

1. Navegue hasta la **cadenas traducidas** sección del título, ubicado en **servicios** > **Xbox Live** > **localizado cadenas**.
2. Haga clic en el **descargar** botón que se descargará un archivo localization.xml en el equipo local.

![Captura de pantalla de la página de configuración de las cadenas localizadas en el centro de partners](../../images/dev-center/localized-strings/localized-strings-1.png)

3. Puede agregar las cadenas localizadas duplicando el <Value locale="en-US">Laberintos reproducidos</Value> etiqueta y cambiando el valor de la configuración regional para el idioma que prefiera y el valor de la cadena localizada. Debe tener al menos una etiqueta de valor dentro de la configuración regional de visualización de desarrollador para evitar errores.

![editar las cadenas localizadas](../../images/dev-center/localized-strings/localized-strings.gif)

4. Una vez haya agregado todas las cadenas localizadas, puede cargar el archivo arrastrando o examinar el equipo local.

![Imagen del botón para cargar el archivo localization.xml](../../images/dev-center/localized-strings/localized-strings-2.png)

Tenga en cuenta que es posible que aparezcan los siguientes errores al cargar el archivo localization.xml:

| Error | Razón |
|---------------------------|-------------|
| Validación de XSD con errores: El elemento 'LocalizedString' en el espacio de nombres 'http://config.mgt.xboxlive.com/schema/localization/1' no puede contener texto. Lista de posibles elementos esperados: 'Valor' en el espacio de nombres 'http://config.mgt.xboxlive.com/schema/localization/1' | Esto se produce cuando el documento XML con formato incorrecto |
| La cadena de localización falta una entrada para la configuración regional para mostrar de desarrollador | Esto se produce cuando una cadena traducida falta una entrada cuya configuración regional no coincide con la configuración regional para mostrar de desarrollo |
| Validación de XSD con errores: El atributo 'configuración regional' no es válido: el valor ' 'no es válido según su tipo de datos'http://config.mgt.xboxlive.com/schema/localization/1:NonEmptyString'-error de restricción de patrón. | Esto se produce cuando una cadena traducida falta el valor de configuración regional en el <Value> tag|
