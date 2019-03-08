---
title: 'Novedades de Windows Docs en enero de 2019: desarrollar aplicaciones para UWP'
description: Instrucciones para desarrolladores, vídeos y las nuevas características se agregaron a la documentación para desarrolladores de Windows 10 de enero de 2019
keywords: Novedades, update, características, instrucciones para desarrolladores, Windows 10, enero
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb80c28866b8f8207f203b70cb504dcd034098d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636580"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>Novedades en la documentación para desarrolladores de Windows de enero de 2019

La documentación del desarrollador de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La introducción a las características, instrucciones para desarrolladores y vídeos siguientes se realizaron disponibles en el mes de enero.

[Instala las herramientas y el SDK](https://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="windows-development-on-microsoft-learn"></a>Desarrollo de Windows en Microsoft Learn

Microsoft Learn proporciona nuevos conocimientos prácticos y las oportunidades de aprendizaje para desarrolladores de Microsoft. Si está interesado en aprender a desarrollar aplicaciones de Windows, consulte [nuestra nueva ruta de aprendizaje](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) para una introducción exhaustiva a la plataforma, las herramientas y cómo escribir sus primera algunas aplicaciones.

![Imagen de la ruta de aprendizaje de desarrollo de Windows](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct3D 12

[Representación de Direct3D 12 pasadas](/windows/desktop/direct3d12/direct3d-12-render-passes) puede mejorar el rendimiento de su representador si se basa en la representación de aplazado basados en mosaicos (TBDR), entre otras técnicas. La técnica le ayuda a su representador para mejorar la eficacia de la GPU habilitando la aplicación para identificar mejor representación ordenación datos y los requisitos de las dependencias de recursos y, lo que reduce el tráfico hacia y desde la memoria fuera del chip memoria.

### <a name="msix-modification-packages"></a>Paquetes de modificación MSIX

Compatibilidad mejorada de Windows 10 versión 1809 para [paquetes de modificación MSIX](https://docs.microsoft.com/windows/msix/modification-package-1809-update). Paquetes de modificación pueden incluir complementos basados en el registro y la personalización asociada y permiten una aplicación implementada a través de MSIX para usar un registro virtual y ejecutarse según lo previsto.

![Creación del paquete MSIX modificación](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>Código abierto de WinUI, Windows Forms y WPF

Los marcos de trabajo WPF, Windows Forms y WinUI UX ahora están disponibles para las contribuciones de código abierto en GitHub. Para obtener más información y vínculos, vea el [creación de blogs de aplicaciones de Windows](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).

### <a name="progressive-web-apps-for-xbox"></a>Aplicaciones Web progresiva para Xbox

Con [progresiva de Web Apps para Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations), puede extender una aplicación web y que esté disponible como una aplicación de Xbox One a través de Microsoft Store mientras todavía sigue utilizando los marcos de trabajo existentes, CDN y el servidor back-end. En su mayor parte, puede empaquetar su PWA para Xbox One en la misma manera que lo haría para Windows, sin embargo, hay varias diferencias claves en esta guía le guiará a través.

### <a name="windows-machine-learning"></a>Aprendizaje automático de Windows

Se ha reestructurado [la página de aterrizaje de WinML APIs](https://docs.microsoft.com/windows/ai/api-reference)y se agrega la nueva documentación para el operador de WinML personalizado y las API nativas.

[Entrenar un modelo con PyTorch](https://docs.microsoft.com/windows/ai/train-model-pytorch) se proporcionan instrucciones sobre cómo entrenar un modelo con el marco de trabajo de PyTorch localmente o en la nube. A continuación, puede descargar este modelo como un archivo ONNX y usarlo en sus aplicaciones de WinML.

![Gráfico de WinML](images/winml-graphic.png)

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="choose-your-platform"></a>Elija la plataforma

¿Está interesado en crear una nueva aplicación de escritorio? Visite nuestro renovada [elija su plataforma](https://docs.microsoft.com/windows/desktop/choose-your-technology) página para obtener una descripción detallada y las comparaciones de las plataformas de Windows Forms, WPF y UWP y obtener más información sobre la API de Win32.

### <a name="faqs-on-win32-webview"></a>Preguntas más frecuentes sobre WebView de Win32

Nuestro [preguntas más frecuentes](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) proporciona respuestas a preguntas habituales al usar la vista de Web de Microsoft Edge en aplicaciones de escritorio, así como vínculos a ejemplos y recursos adicionales.

### <a name="japanese-era-change"></a>Cambio era japonés

[Preparar la aplicación para que el cambio era japonés](../design/globalizing/japanese-era-change.md) se muestra cómo asegurarse de colocan las aplicación está lista para la era japonés cambiar conjunto para aprovechar de Windows en el 1 de mayo de 2019. [Esta página también está disponible en japonés](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change).

## <a name="videos"></a>Vídeos

### <a name="progressive-web-apps"></a>Aplicaciones web progresivas

Aplicaciones Web progresiva son sitios web que funcionan como aplicaciones nativas en distintos exploradores y una amplia variedad de dispositivos Windows 10. [Vea el vídeo](https://youtu.be/ugAewC3308Y) para obtener más información y, a continuación, [desproteger los documentos](https://aka.ms/Windows-PWA) para empezar a trabajar.

### <a name="vs-code-series"></a>Serie de código de VS

Visite nuestro [nueva serie de vídeos sobre Visual Studio Code](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) para obtener información sobre VSCode es, cómo utilizarlo y cómo se creó.

### <a name="one-dev-question"></a>Una pregunta de desarrollo

En la serie de vídeos de una pregunta de desarrollo, los desarrolladores de Microsoft ha cubren una serie de preguntas acerca del desarrollo de Windows, referencia cultural del equipo y el historial. Aquí es que hemos respondido a las preguntas más recientes.

Raymond Chen:

* [¿Por qué tener archivos de programa y los archivos de programa (x86)?](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [¿Por qué es COM tan complicado?](https://youtu.be/-gkXAV-StVA )
* [¿Cuál fue la primera entrevista como para Microsoft?](https://youtu.be/qRb6otsHG5c)
