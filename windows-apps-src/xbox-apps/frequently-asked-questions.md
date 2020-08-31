---
title: Preguntas más frecuentes
description: Si las cosas no funcionan como se esperaba, consulte esta página de preguntas más frecuentes sobre UWP en Xbox.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 265fe827-bd4a-48d4-b362-8793b9b25705
ms.localizationpriority: medium
ms.openlocfilehash: 7da3b6a6507f2652eb2357ab80897d9ba35d7484
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170739"
---
# <a name="frequently-asked-questions"></a>Preguntas más frecuentes

¿Las cosas no funcionan de la forma esperada? Consulta esta página de preguntas más frecuentes. Asimismo, consulta el tema [Known issues (Problemas conocidos)](known-issues.md) y el foro [Developing Universal Windows apps (Desarrollo de aplicaciones universales de Windows)](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

### <a name="why-arent-my-games-and-apps-working"></a>¿Por qué no funcionan mis juegos y aplicaciones?

Si los juegos y las aplicaciones no funcionan, o si no tiene acceso a la tienda o a los servicios Live, es probable que se esté ejecutando en modo de desarrollador. Para averiguar el modo en el que se encuentra actualmente, presione el botón **Inicio** del controlador. Si esto le lleva a dev Home en lugar de la experiencia de venta al por menor, está en modo de desarrollador. Si quieres jugar a juegos, puedes abrir la Página principal para desarrolladores y volver al modo comercial utilizando el botón **Leave developer mode**.

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>¿Por qué no puedo conectarme a mi Xbox One con Visual Studio?

Para empezar, asegúrate de que estés ejecutando el modo de desarrollador y no el modo comercial. No puedes conectarte a Xbox One cuando está en modo comercial. Para averiguar el modo en el que se encuentra actualmente, presione el botón **Inicio** del controlador. Si ve contenido Gold/Live en lugar de dev Home, está en modo comercial y necesita ejecutar la aplicación de activación de modo de desarrollo para cambiar al modo de desarrollador.

> [!NOTE]
> Para implementar una aplicación, es necesario contar con un usuario que haya iniciado sesión.

Para obtener más información, consulta [Solucionar errores de implementación](#fixing-deployment-failures) más adelante en esta página.

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>¿Cómo puedo alternar entre el modo comercial y el modo de desarrollador?

Sigue las instrucciones de [Xbox One Developer Mode Activation (Activación del modo de desarrollador de Xbox)](devkit-activation.md) para obtener más información sobre estos estados.

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>¿Cómo sé si estoy en modo comercial o desarrollador?

Sigue las instrucciones de [Xbox One Developer Mode Activation (Activación del modo de desarrollador de Xbox)](devkit-activation.md) para obtener más información sobre estos estados. 

Para averiguar el modo en el que se encuentra actualmente, presione el botón **Inicio** del controlador. 
- Si esto le lleva a dev Home, está en modo de desarrollador.
- Si ve contenido Gold o en directo, está en modo de venta directa.

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>¿Mis juegos y aplicaciones aún funcionarán si activo el modo de desarrollador?

Sí, puedes alternar entre el modo de desarrollador y el modo comercial, en el que puedes jugar a juegos. Para obtener más información, consulta la página [Activación del modo de desarrollador de Xbox One](devkit-activation.md). 

### <a name="can-i-develop-and-publish-x86-apps-for-xbox"></a>¿Puedo desarrollar y publicar aplicaciones x86 para Xbox?
Xbox ya no admite el desarrollo de aplicaciones x86 o envíos de aplicaciones x86 a la tienda. 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>¿Perderé mis juegos y aplicaciones o los cambios guardados?

Si decides abandonar el programa para desarrolladores, no perderás tus aplicaciones y juegos instalados. Además, siempre que esté en línea al reproducirlos, los juegos guardados se guardan en el perfil de nube de la cuenta activa, por lo que no los perderá.

### <a name="how-do-i-leave-the-developer-program"></a>¿Cómo puedo abandonar el programa para desarrolladores?

Consulta el tema [Desactivación del modo de desarrollador de Xbox One](devkit-deactivation.md) para obtener más información acerca de cómo abandonar el programa para desarrolladores.

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>He vendido mi consola Xbox One y la he dejado en el modo de desarrollador. ¿Cómo puedo desactivar el modo de desarrollador?

Si ya no tiene acceso a la consola Xbox One, puede desactivarla en el centro de Partners de Windows. Para obtener más información, consulte la sección **desactivación de la consola mediante el centro de Partners** del tema [desactivación del modo de desarrollador de Xbox One](devkit-deactivation.md#deactivate-your-console-using-partner-center) . 

### <a name="i-left-the-developer-program-using-partner-center-but-im-in-still-developer-mode-what-do-i-do"></a>Dejo el programa para desarrolladores con el centro de Partners, pero estoy en modo de desarrollador. ¿Qué puedo hacer?

Inicia la Página principal para desarrolladores y selecciona el botón **Leave developer mode**. La consola se reiniciará en modo comercial. 

### <a name="can-i-publish-my-app"></a>¿Puedo publicar mi aplicación?

Puede [publicar aplicaciones](../publish/index.md) a través del centro de partners si tiene una [cuenta de desarrollador](https://developer.microsoft.com/store/register). Las aplicaciones para UWP creadas y probadas en una consola Xbox One de venta directa pasarán por el mismo proceso de ingesta, revisión y publicación que Windows realiza hoy en día, con revisiones adicionales para cumplir con los estándares de Xbox One de hoy en día.

### <a name="can-i-publish-my-game"></a>¿Puedo publicar mi juego?

Puedes usar UWP y Xbox One en el modo de desarrollador para crear y probar los juegos de Xbox One. Para publicar juegos para UWP, debes registrarte con [ID@XBOX](https://www.xbox.com/Developers/id) o formar parte del [programa de creadores de Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator). Para obtener más información, vea [información general del programa para desarrolladores](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html).

### <a name="will-the-standard-game-engines-work"></a>¿Los motores de juego estándar funcionarán?

Echa un vistazo a la página [Problemas conocidos](known-issues.md) de esta versión.

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>¿Qué funcionalidades y recursos del sistema hay disponibles para los juegos para UWP en Xbox One? 

Para obtener más información, consulta [Recursos del sistema de aplicaciones para UWP y juegos en Xbox One](system-resource-allocation.md).

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>¿Si creo un juego para UWP de DirectX 12, se ejecutará en mi Xbox One en el modo de desarrollador?

Para obtener más información, consulta [Recursos del sistema de aplicaciones para UWP y juegos en Xbox One](system-resource-allocation.md).

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>¿Toda la superficie de la API de UWP estará disponible en Xbox?

Echa un vistazo a la página [Problemas conocidos](known-issues.md) de esta versión.

### <a name="fixing-deployment-failures"></a>Solucionar errores de implementación

Si no puede implementar la aplicación desde Visual Studio, estos pasos pueden ayudarle a solucionar el problema. Si sigues teniendo problemas, pide ayuda en el foro.

> [!NOTE]
> Para implementar una aplicación, es necesario contar con un usuario que haya iniciado sesión. Si recibes un mensaje de error 0x87e10008, asegúrate de que haya un usuario que haya iniciado sesión y vuelve a intentarlo.

Si Visual Studio no puede conectarse a tu Xbox One:

1. Asegúrate de que te encuentras en el modo de desarrollador (se describe anteriormente en esta página).
2. Asegúrate de que hayas configurado correctamente el equipo de desarrollo. ¿Has seguido *todas* las instrucciones de [Getting started with UWP app development on Xbox One (Introducción al desarrollo de aplicaciones para UWP en Xbox One)](getting-started.md)? 

3. Si todavía no lo ha hecho, lea el tema [configuración del entorno de desarrollo](development-environment-setup.md) y el tema [Introducción a las herramientas de Xbox One](introduction-to-xbox-tools.md) .

4. Asegúrate de que puedes "hacer ping" a la dirección IP de la consola desde el equipo de desarrollo.
  > [!NOTE]
  > Para obtener el mejor rendimiento de la implementación, te recomendamos que uses una conexión con cable a la consola.

5. Asegúrese de que está usando universal (protocolo sin cifrar) en la lista desplegable autenticación de la pestaña **depurar** . Para obtener más información, vea [configuración del entorno de desarrollo](development-environment-setup.md).


### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>Si estoy compilando una aplicación con HTML/JavaScript, ¿Cómo habilito la navegación del controlador de juegos?

TVHelpers es un conjunto de muestras y bibliotecas de JavaScript y XAML/C# para ayudarte a crear fantásticas experiencias de Xbox One y televisión en JavaScript y C#. TVJS es una biblioteca que te ayuda a compilar aplicaciones para UWP de alta gama para Xbox One. TVJS incluye compatibilidad para la navegación de controlador automática, reproducción de elementos multimedia enriquecidos, búsqueda y más. Puedes usar TVJS con tu aplicación web hospedada tan fácilmente como con una aplicación para UWP web empaquetada con acceso total a API de Windows Runtime.

Para obtener más información, vea el proyecto [TVHelpers](https://github.com/Microsoft/TVHelpers) y el [wiki](https://github.com/Microsoft/TVHelpers/wiki)del proyecto.

## <a name="see-also"></a>Vea también
- [Problemas conocidos de UWP en Xbox One](known-issues.md)
- [UWP en Xbox One](index.md)
- [UWP en Xbox One](index.md)
