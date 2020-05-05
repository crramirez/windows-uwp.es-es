---
Description: En esta sección se muestra cómo crear, empaquetar y consumir los recursos de cadena, imagen y archivo de la aplicación.
title: Recursos de aplicación y el sistema de administración de recursos
label: Intro
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: windows 10, uwp, resource, image, asset, MRT, qualifier
ms.localizationpriority: medium
ms.openlocfilehash: 5b45f33e9849a46e22250640b88a85ea16143231
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2020
ms.locfileid: "77089331"
---
# <a name="app-resources-and-the-resource-management-system"></a>Recursos de aplicación y el sistema de administración de recursos


En esta sección se muestra cómo crear, empaquetar y consumir los recursos de cadena, imagen y archivo de la aplicación. Por ejemplo, puedes empaquetar un archivo junto con su juego esporádico que contiene una definición de los niveles del juego, y cargar el archivo en tiempo de ejecución. También se muestra que mantener los recursos independientes de la aplicación facilita la localización y personalización de la aplicación con diferentes configuraciones regionales, pantallas de dispositivo, configuración de accesibilidad y otros contextos de usuario y máquina. Los recursos, como cadenas e imágenes deben existir normalmente en diversas variantes de idioma, escala y contraste. Para los recursos de este tipo, cuentas con la ayuda del [Sistema de administración de recursos](resource-management-system.md).

Existen dos tipos de recursos de aplicación.
- Un recurso de archivo es un recurso almacenado como un archivo en el disco. Un recurso de archivo puede contener una imagen de mapa de bits, datos XAML, XML, HTML o de cualquier otro tipo.
- Un recurso integrado es un recurso que se integra dentro de algún archivo de recursos que lo contiene. El ejemplo más común es un recurso de cadena integrado dentro de un archivo de recursos (.resw o .resjson).

Para más información sobre la propuesta de valor de localizar la aplicación, consulta [Globalización y localización](../design/globalizing/globalizing-portal.md).

| Artículo | Descripción |
|---------|-------------|
| [Sistema de administración de recursos](resource-management-system.md) | En tiempo de compilación, el sistema de administración de recursos crea un índice de todas las diferentes variantes de los recursos que se empaquetan con la aplicación. En tiempo de ejecución, el sistema detecta la configuración de usuario y máquina que está en vigor y carga los recursos que mejor coinciden con esos valores. |
| [Cómo el sistema de administración de recursos compara y elige recursos](how-rms-matches-and-chooses-resources.md) | Cuando se solicita un recurso, podría haber varios candidatos que coincidan hasta cierto punto con el contexto de recursos actual. El sistema de administración de recursos analizará todos los candidatos y determinará el mejor candidato para devolver. En este tema se describe con detalle ese proceso y se proporcionan ejemplos. |
| [Cómo el sistema de administración de recursos compara etiquetas de idioma](how-rms-matches-lang-tags.md) | En el tema anterior ([Cómo compara y elige recursos el sistema de administración de recursos](how-rms-matches-and-chooses-resources.md)) se examina la coincidencia de calificadores en general. Este tema se centra con mayor detalle en la coincidencia de etiquetas de idioma. |
| [Adaptar los recursos al idioma, escala, alto contraste y otros calificadores](tailor-resources-lang-scale-contrast.md) | En este tema se explica el concepto general de calificadores de recursos, cómo usarlos y la finalidad de cada uno de los nombres de calificador. |
| [Localizar cadenas en la interfaz de usuario y el manifiesto de paquete de la aplicación](localize-strings-ui-manifest.md) | Si quieres que tu aplicación admita diferentes idiomas de pantalla y tenga literales de cadena en el código, o bien marcado XAML o manifiesto de paquete de la aplicación, mueve esas cadenas a un archivo de recursos (.resw). A continuación, puedes hacer una copia traducida de ese archivo de recursos para cada idioma que admita la aplicación. |
| [Cargar imágenes y recursos adaptados a escala, tema, contraste alto y otros](images-tailored-for-scale-theme-contrast.md) | La aplicación puede cargar archivos de recursos de imagen que contengan imágenes adaptadas al factor de escala de visualización, tema, contraste alto y otros contextos en tiempo de ejecución. |
| [Esquemas de URI](uri-schemes.md) | Existen varios esquemas de URI (identificador uniforme de recursos) que puedes usar para hacer referencia a archivos que provienen del paquete de la aplicación, las carpetas de datos de la aplicación o la nube. También puedes usar un esquema de URI para hacer referencia a cadenas cargadas desde archivos de recursos (.resw) de la aplicación. |
| [Especificar los recursos predeterminados que la aplicación usa](specify-default-resources-installed.md) | Si la aplicación no tiene recursos que coincidan con la configuración concreta de un dispositivo de cliente, se usan los recursos predeterminados de la aplicación. En este tema se explica cómo especificar cuáles son esos recursos predeterminados. |
| [Crear recursos en el paquete de la aplicación, en lugar de en un paquete de recursos](build-resources-into-app-package.md) | Algunos tipos de aplicaciones (diccionarios multilingües, herramientas de traducción, etc.) necesitan reemplazar el comportamiento predeterminado de un lote de aplicaciones y crear recursos en el paquete de la aplicación en lugar de tenerlos en distintos paquetes de recursos. En este tema se explica cómo hacerlo. |
| [API de indexación de recursos de paquetes (PRI) y sistemas de compilación personalizados](pri-apis-custom-build-systems.md) | Con las [API de indexación de recursos de paquetes (PRI)](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference), puedes desarrollar un sistema de compilación personalizado para los recursos de la aplicación para UWP. El sistema de compilación podrá crear y volcar (como XML) archivos de índice de recursos del paquete (PRI), así como controlar sus versiones, en cualquier nivel de complejidad que necesite la aplicación para UWP. |
| [Compilar recursos manualmente con MakePri.exe](compile-resources-manually-with-makepri.md) | MakePri.exe es una herramienta de línea de comandos que se puede usar para crear y volcar archivos PRI. Viene integrada como parte de MSBuild en Microsoft Visual Studio, pero podría ser útil para crear paquetes manualmente o con un sistema de compilación personalizado. |
| [Usar el sistema de administración de recursos de Windows 10 en una aplicación o juego heredados](using-mrt-for-converted-desktop-apps-and-games.md) | Al empaquetar la aplicación o juego en .NET o Win32 como un paquete .msix o .appx, puedes aprovechar el sistema de administración de recursos para cargar recursos de la aplicación adaptados al contexto en tiempo de ejecución. En este tema se describen en profundidad las técnicas. |

Consulta también [Compatibilidad de ventanas y notificaciones del sistema para el idioma, la escala y el contraste alto](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).
