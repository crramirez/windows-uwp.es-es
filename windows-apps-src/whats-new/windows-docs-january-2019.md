---
title: 'Novedades en los documentos de Windows en enero de 2019: desarrollar aplicaciones para UWP'
description: Se agregaron nuevas características, vídeos y directrices para los desarrolladores a la documentación de desarrolladores de Windows 10 de enero de 2019
keywords: Novedades, actualización, características, directrices para los desarrolladores, Windows 10, enero
ms.date: 1/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cc5323ba12fa72fb5350e62f74206ea72fe96497
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "9046135"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>Novedades en los documentos de Windows de enero de 2019

La documentación del desarrollador de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente información general de características, directrices para los desarrolladores y vídeos que se han puesto a disposición en el mes de enero.

[Instala las herramientas y el SDK](https://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para aprender a usar el [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="windows-development-on-microsoft-learn"></a>Desarrollo de Windows en Microsoft Learn

Microsoft Learn proporciona una nueva experiencia práctica y oportunidades de aprendizaje a los desarrolladores de Microsoft. Si estás interesado en saber cómo desarrollar aplicaciones de Windows, echa un vistazo a [nuestra nueva ruta de acceso de aprendizaje](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) para obtener una introducción completa para la plataforma, las herramientas y cómo escribir tus primera algunas aplicaciones.

![Imagen de la ruta de acceso de aprendizaje de desarrollo de Windows](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct3D 12

[Pasadas de representación de Direct3D 12](/windows/desktop/direct3d12/direct3d-12-render-passes) puede mejorar el rendimiento del representador, si se basa en la representación de aplazado basada en el icono (TBDR), entre otras técnicas. La técnica de Ayuda del representador para mejorar la eficacia de la GPU, habilitar la aplicación para identificar mejor representación para ordenar los datos y los requisitos de las dependencias de recursos y lo que reduce el tráfico de memoria de memoria desactivado chip.

### <a name="msix-modification-packages"></a>Paquetes de modificación de MSIX

Windows 10 versión 1809 mejorado la compatibilidad con [los paquetes de modificación de MSIX](https://docs.microsoft.com/windows/msix/modification-package-1809-update). Paquetes de modificación pueden incluir complementos basada en registro y personalización asociado y te permitirá una aplicación implementada a través de MSIX para usar un registro virtual y ejecutar según lo esperado.

![Creación del paquete de modificación de MSIX](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>Código abierto de WPF, Windows Forms y WinUI

Los marcos de WPF, Windows Forms y WinUI UX ahora están disponibles para las contribuciones de código abierto en GitHub. Para obtener más información y vínculos, consulta la [creación de blog de aplicaciones de Windows](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).

### <a name="progressive-web-apps-for-xbox"></a>Aplicaciones Web progresivas para Xbox

Con [Las aplicaciones Web progresivas para Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations), puedes extender una aplicación web y hacer que esté disponible como una aplicación de Xbox One a través de Microsoft Store mientras aún continúa usando los marcos existentes, back-end de servidor y la red CDN. La mayor parte, se puede empaquetar tu PWA para Xbox One de la misma manera que lo harías para Windows, sin embargo, hay varias diferencias clave en esta guía te guiará por.

### <a name="windows-machine-learning"></a>Aprendizaje automático de Windows

Hemos reestructurar [la página de inicio para las API WinML](https://docs.microsoft.com/windows/ai/api-reference)y agrega la nueva documentación para operador personalizado WinML y las API nativas.

[Formar un modelo con PyTorch](https://docs.microsoft.com/windows/ai/train-model-pytorch) , se proporcionan instrucciones sobre cómo formar un modelo con el marco de PyTorch, ya sea localmente o en la nube. A continuación, puede descargar este modelo como un archivo ONNX y usarla en tus aplicaciones WinML.

![Gráfico de WinML](images/winml-graphic.png)

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="choose-your-platform"></a>Elige tu plataforma

¿Interesado en crear una nueva aplicación de escritorio? Echa un vistazo a nuestra página [Elegir la plataforma de](https://docs.microsoft.com/windows/desktop/choose-your-technology) actualización de la herramienta para obtener descripciones detalladas y las comparaciones de las plataformas UWP, WPF y Windows Forms y obtener más información sobre la API de Win32.

### <a name="faqs-on-win32-webview"></a>Preguntas más frecuentes sobre Win32 WebView

Nuestra [preguntas más frecuentes](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) proporciona respuestas a preguntas habituales al usar el WebView de Microsoft Edge en aplicaciones de escritorio, así como vínculos a ejemplos y recursos adicionales.

### <a name="japanese-era-change"></a>Cambio de japonesa

[Preparar la aplicación de la era japonesa cambiar](../design/globalizing/japanese-era-change.md) , se muestra cómo asegurarse de colocar las ventanas de aplicación está lista para la era japonesa cambiar conjunto para realizar el 1 de mayo de 2019. [Esta página está disponible también en japonés](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change).

## <a name="videos"></a>Vídeos

### <a name="progressive-web-apps"></a>Aplicaciones web progresivas

Aplicaciones Web progresivas son sitios web que funcionan como las aplicaciones nativas en diferentes exploradores y una amplia variedad de dispositivos Windows 10. [Ve el vídeo](https://youtu.be/ugAewC3308Y) para obtener más información y, a continuación, [echa un vistazo a los documentos](https://aka.ms/Windows-PWA) para empezar a trabajar.

### <a name="vs-code-series"></a>Serie de código de VS

Echa un vistazo a nuestra [nueva serie de vídeos en código de Visual Studio](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) para obtener información sobre las novedades de VSCode, cómo usarla, y cómo se haya creado.

### <a name="one-dev-question"></a>Una pregunta de desarrollo

En la serie de vídeos de una pregunta de desarrollo, los desarrolladores de Microsoft siempre cubren una serie de preguntas frecuentes sobre el desarrollo de Windows, referencia cultural de equipo e historial. Aquí es que hemos respondido a las preguntas más recientes.

Raymond Chen:

* [¿Por qué tener archivos de programa y los archivos de programa (x86)?](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [¿Por qué COM tan complicado?](https://youtu.be/-gkXAV-StVA )
* [¿Cuál fue la primera entrevista como de Microsoft?](https://youtu.be/qRb6otsHG5c)
