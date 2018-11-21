---
author: Mtoepke
title: Preguntas más frecuentes
description: Preguntas más frecuentes sobre UWP en Xbox.
ms.author: mstahl
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 265fe827-bd4a-48d4-b362-8793b9b25705
ms.localizationpriority: medium
ms.openlocfilehash: a5ac814dfb86259791de2632587a1cfd0d456a37
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "7427520"
---
# <a name="frequently-asked-questions"></a>Preguntas más frecuentes

¿Las cosas no funcionan de la forma esperada? Consulta esta página de preguntas más frecuentes. Asimismo, consulta el tema [Known issues (Problemas conocidos)](known-issues.md) y el foro [Developing Universal Windows apps (Desarrollo de aplicaciones universales de Windows)](https://go.microsoft.com/fwlink/?linkid=839446). 

### <a name="why-arent-my-games-and-apps-working"></a>¿Por qué mis juegos y aplicaciones no funcionan?

Si tus juegos y aplicaciones no funcionan, o si no tienes acceso a la tienda o a los servicios Live, probablemente estás ejecutando el modo de desarrollador. Para saber en qué modo te encuentras en este momento, presiona el botón **Inicio** botón en el mando. Si te lleva a Home Dev en lugar de a la página Home comercial, estás en modo de desarrollador. Si quieres jugar a juegos, puedes abrir la Página principal para desarrolladores y volver al modo comercial utilizando el botón **Leave developer mode**.

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>¿Por qué no puedo conectarme a mi XboxOne con VisualStudio?

Para empezar, asegúrate de que estés ejecutando el modo de desarrollador y no el modo comercial. No puedes conectarte a Xbox One cuando está en modo comercial. Para saber en qué modo te encuentras en este momento, presiona el botón **Inicio** botón en el mando. Si ves contenido de Gold o Live en lugar de Dev Home, estás en modo comercial y necesitas ejecutar la aplicación Dev Mode Activation para cambiar al modo de desarrollador.

> [!NOTE]
> Para implementar una aplicación, es necesario contar con un usuario que haya iniciado sesión.

Para obtener más información, consulta [Solucionar errores de implementación](#fixing-deployment-failures) más adelante en esta página.

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>¿Cómo puedo alternar entre el modo comercial y el modo de desarrollador?

Sigue las instrucciones de [Xbox One Developer Mode Activation (Activación del modo de desarrollador de Xbox)](devkit-activation.md) para obtener más información sobre estos estados.

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>¿Cómo sé si estoy en modo comercial o desarrollador?

Sigue las instrucciones de [Xbox One Developer Mode Activation (Activación del modo de desarrollador de Xbox)](devkit-activation.md) para obtener más información sobre estos estados. 

Para saber en qué modo te encuentras en este momento, presiona el botón **Inicio** botón en el mando. 
- Si te lleva a Dev Home, estás en modo de desarrollador.
- Si ves contenido de Gold o Live, estás en modo comercial.

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>¿Mis juegos y aplicaciones aún funcionarán si activo el modo de desarrollador?

Sí, puedes alternar entre el modo de desarrollador y el modo comercial, en el que puedes jugar a juegos. Para obtener más información, consulta la página [Activación del modo de desarrollador de Xbox One](devkit-activation.md). 

### <a name="can-i-develop-and-publish-x86-apps-for-xbox"></a>¿Puedo desarrollar y publicar aplicaciones x86 para Xbox?
Xbox ya no admite el desarrollo o envío de aplicaciones x86 a la Store. 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>¿Perderé mis juegos y aplicaciones o los cambios guardados?

Si decides abandonar el programa para desarrolladores, no perderás tus aplicaciones y juegos instalados. Además, si estabas conectado cuando jugabas, las partidas guardadas estarán almacenadas en tu perfil en la nube de la cuenta de Live, por lo que no las perderás.

### <a name="how-do-i-leave-the-developer-program"></a>¿Cómo puedo abandonar el programa para desarrolladores?

Consulta el tema [Desactivación del modo de desarrollador de Xbox One](devkit-deactivation.md) para obtener más información acerca de cómo abandonar el programa para desarrolladores.

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>He vendido mi consola Xbox One y la he dejado en el modo de desarrollador. ¿Cómo puedo desactivar el modo de desarrollador?

Si ya no tienes acceso a tu Xbox One, puedes desactivar Windows del centro de partners. Para obtener más información, consulta la sección de **desactivar la consola con el centro de partners** en el tema de la [Desactivación del modo de desarrollador One de Xbox](devkit-deactivation.md#deactivate-your-console-using-partner-center) . 

### <a name="i-left-the-developer-program-using-partner-center-but-im-in-still-developer-mode-what-do-i-do"></a>Abandoné el programa para desarrolladores en el centro de partners, pero estoy en modo de desarrollador aún. ¿Qué puedo hacer?

Inicia la Página principal para desarrolladores y selecciona el botón **Leave developer mode**. La consola se reiniciará en modo comercial. 

### <a name="can-i-publish-my-app"></a>¿Puedo publicar mi aplicación?

Si tienes una [cuenta de desarrollador](https://developer.microsoft.com/store/register), puede [publicar aplicaciones](../publish/index.md) a través del centro de partners. Las aplicaciones para UWP creadas y probadas en una consola XboxOne comercial pasarán por el mismo proceso de ingesta, revisión y publicación que Windows realiza hoy en día, con revisiones adicionales para cumplir los estándares actuales de XboxOne.

### <a name="can-i-publish-my-game"></a>¿Puedo publicar mi juego?

Puedes usar UWP y Xbox One en el modo de desarrollador para crear y probar los juegos de Xbox One. Para publicar juegos para UWP, te debes registrar en [ID@XBOX](http://www.xbox.com/Developers/id) o formar parte del [Programa de creadores de Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator). Para más información, consulta [Introducción al programa para desarrolladores](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html).

### <a name="will-the-standard-game-engines-work"></a>¿Los motores de juego estándar funcionarán?

Echa un vistazo a la página [Problemas conocidos](known-issues.md) de esta versión.

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>¿Qué funcionalidades y recursos del sistema hay disponibles para los juegos para UWP en Xbox One? 

Para obtener más información, consulta [Recursos del sistema de aplicaciones para UWP y juegos en Xbox One](system-resource-allocation.md).

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>¿Si creo un juego para UWP de DirectX 12, se ejecutará en mi Xbox One en el modo de desarrollador?

Para obtener más información, consulta [Recursos del sistema de aplicaciones para UWP y juegos en Xbox One](system-resource-allocation.md).

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>¿Toda la superficie de la API de UWP estará disponible en Xbox?

Echa un vistazo a la página [Problemas conocidos](known-issues.md) de esta versión.

### <a name="fixing-deployment-failures"></a>Solucionar errores de implementación

Si no puedes implementar la aplicación desde VisualStudio, estos pasos pueden ayudarte a solucionar el problema. Si sigues teniendo problemas, pide ayuda en el foro.

> [!NOTE]
> Para implementar una aplicación, es necesario contar con un usuario que haya iniciado sesión. Si recibes un mensaje de error 0x87e10008, asegúrate de que haya un usuario que haya iniciado sesión y vuelve a intentarlo.

Si Visual Studio no puede conectarse a tu Xbox One:

1. Asegúrate de que te encuentras en el modo de desarrollador (se describe anteriormente en esta página).
2. Asegúrate de que hayas configurado correctamente el equipo de desarrollo. ¿Has seguido *todas* las instrucciones de [Getting started with UWP app development on Xbox One (Introducción al desarrollo de aplicaciones para UWP en Xbox One)](getting-started.md)? 

3. Si aún no lo has hecho, lee el tema [Configuración del entorno de desarrollo](development-environment-setup.md) y el tema [Introducción a las herramientas de XboxOne](introduction-to-xbox-tools.md).

4. Asegúrate de que puedes "hacer ping" a la dirección IP de la consola desde el equipo de desarrollo.
  > [!NOTE]
  > Para obtener el mejor rendimiento de la implementación, te recomendamos que uses una conexión con cable a la consola.

5. Asegúrate de usar la opción Universal (protocolo sin cifrar) de la lista desplegable Autenticación de la pestaña **Depurar**. Para más detalles, consulta [Configuración del entorno de desarrollo](development-environment-setup.md).


### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>Si voy a crear una aplicación con HTML o JavaScript, ¿cómo puedo habilitar la navegación con el controlador para juegos?

TVHelpers es un conjunto de muestras y bibliotecas de JavaScript y XAML/C# para ayudarte a crear fantásticas experiencias de Xbox One y televisión en JavaScript y C#. TVJS es una biblioteca que te ayuda a compilar aplicaciones para UWP de alta gama para Xbox One. TVJS incluye compatibilidad para la navegación de controlador automática, reproducción de elementos multimedia enriquecidos, búsqueda y más. Puedes usar TVJS con tu aplicación web hospedada tan fácilmente como con una aplicación para UWP web empaquetada con acceso total a API de Windows Runtime.

Para obtener más información, consulta el proyecto [TVHelpers](https://github.com/Microsoft/TVHelpers) y la [wiki](https://github.com/Microsoft/TVHelpers/wiki) del proyecto.

## <a name="see-also"></a>Consulta también
- [Problemas conocidos de UWP en Xbox One](known-issues.md)
- [UWP en Xbox One](index.md)
- [UWP en Xbox One](index.md)
