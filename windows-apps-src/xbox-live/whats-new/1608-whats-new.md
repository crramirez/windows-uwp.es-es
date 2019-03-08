---
title: Novedades para el SDK de Xbox Live - agosto de 2016
description: Novedades para el SDK de Xbox Live - agosto de 2016
ms.assetid: fa52e7bd-2c2c-4c25-94ab-761036a7ca79
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2a498fea1ed0974935a273c9ee72ba2c95d15959
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627910"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2016"></a>Novedades para el SDK de Xbox Live - agosto de 2016

Consulte la [What's New - junio de 2016](1606-whats-new.md) artículo para lo que se agregó en la versión de junio de 2016.

## <a name="os-and-tool-support"></a>Sistema operativo y la herramienta de soporte técnico
El SDK de Xbox Live es compatible con Windows 10 RTM [versión 10.0.10240] y Visual Studio 2015 RTM [versión 14.0.23107.0].

## <a name="documentation"></a>Documentación
- Si está escribiendo una aplicación de UWP y va a implementar la capacidad para invitar a usuarios a un juego, hay instrucciones en el ```.appxmanifest``` cambios necesarios en [configurar su AppXManifest para varios jugadores](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md).  Esto se explicó anteriormente en el [foros](https://forums.xboxlive.com) y [portar el código en vivo de xbox de época a uwp](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) artículo
- El [Introducción al administrador de redes sociales](../social-platform/intro-to-social-manager.md) artículo se ha actualizado para reflejar los cambios recientes de API y proporcionar más información sobre los códigos de retorno para algunas de las funciones.

## <a name="unity-samples"></a>Ejemplos de Unity
Se agregaron algunos nuevos ejemplos para desarrolladores de Unity escribiendo una aplicación de UWP.
- Ahora hay una versión de Unity del ejemplo Social, puede encontrarlo en el directorio de ejemplos, Social o Unity.
- También hay un ejemplo donde se describe cómo usar el almacenamiento conectado.  Consulte el ejemplo GameSave/Samples/Unity.
Hay una versión de Unity de AchievementsLeaderboard en AchievementsLeaderboard/Samples/Unity

## <a name="social-manager"></a>Administrador de redes sociales
Además de las actualizaciones de documentación que se mencionó anteriormente, hay algunas API nuevas que se han agregado.  Estos métodos se describen a continuación, y puede ver con más detalle en social_manager.h

- Se ha agregado nueva API que permite la actualización de grupos de redes sociales sin nueva creación:

```cpp
    _XSAPIIMP xbox_live_result<void> update_social_user_group(
        _In_ const std::shared_ptr<xbox_social_user_group>& group,
        _In_ const std::vector<string_t>& users
        );
```
- Se completó la actualización del grupo de redes sociales se indica mediante el ```social_user_group_updated``` eventos


## <a name="multiplayer"></a>Varios jugadores
Exploración de sesión mejorada está disponible y puede usar nuevas API de varios jugadores a usarla.

Mediante las nuevas API, puede filtrar por etiquetas, cadenas y otros datos enriquecidos para permitir que los usuarios a encontrar más fácilmente una sesión que desea reproducir.

Publicaremos documentación más completa en los próximos meses, pero brevemente ahora puede asociar un identificador de"búsqueda" con una sesión MPSD mediante ```set_search_handle``` y, a continuación, los usuarios pueden buscar sesiones mediante un mecanismo de filtrado eficaz llamando el título ```get_search_handles```

A continuación, se enumeran las nuevas API.  Intente enviarlas y si tiene algún problema, publique un subproceso de soporte técnico en el [foros](https://forums.xboxlive.com) o póngase en contacto con su madre.  Tenemos algunos ejemplos de cómo usar estas API pronto.

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> set_search_handle(
    _In_ multiplayer_search_handle_request searchHandleRequest
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<std::vector<multiplayer_search_handle_details>>> get_search_handles(
    _In_ const string_t& serviceConfigurationId,
    _In_ const string_t& sessionTemplateName,
    _In_ const string_t& orderBy,
    _In_ bool orderAscending,
    _In_ const string_t& searchQuery
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> clear_search_handle(_In_ const string_t& handleId);
```

### <a name="xbox-integrated-multiplayer"></a>Varios jugadores integrada de Xbox

Hemos incluido la documentación para la Xbox integrado participan varios jugadores (XIM) API.  La propia API estará disponible en una versión posterior del SDK de Xbox Live, pero la documentación y el encabezado se están realizando disponibles en versión preliminar.

XIM es una interfaz independiente para agregar fácilmente varios jugadores comunicación de redes y chat en tiempo real para su juego a través de la eficacia de los servicios de Xbox Live.

Esta versión preliminar de la documentación de la API se comparte aquí para fomentar la consulta y comentarios del cliente. Hablamos acerca de esta API anteriormente en Xfest 2016, y puede ver archivados [materiales de presentación en el sitio para desarrolladores de socio administrado](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2016.aspx) desde la charla "llave en mano participan varios jugadores a redes y Chat". Tenga en cuenta que esta documentación de vista previa es sólo para la API de C++. Equivalentes de WinRT para C# y otros lenguajes se publicará más adelante en el año.

Si está interesado en las capacidades del XIM, tiene comentarios u otras preguntas sobre este proyecto, no dude en publicar en el [foro para desarrolladores de Xbox](https://forums.xboxlive.com/) o diríjase a través de su administrador de cuentas de desarrollador.

Puede ver esta documentación nueva en xbox_integrated_multiplayer.chm en el directorio de Docs de Xbox Live SDK.  El archivo de inclusión está disponible como una vista previa en \include\xim\XboxIntegratedMultiplayer.h.  
