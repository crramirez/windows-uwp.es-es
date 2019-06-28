---
title: 'Novedades en la documentación de Windows de enero de 2019: desarrollar aplicaciones para UWP'
description: Se han agregado nuevas características, vídeos y directrices para desarrolladores a la documentación para los desarrolladores de Windows 10 de enero de 2019.
keywords: novedades, actualización, características, directrices para desarrolladores, Windows 10, enero.
ms.date: 01/17/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: beb80c28866b8f8207f203b70cb504dcd034098d
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "63800584"
---
# <a name="whats-new-in-the-windows-developer-docs-in-january-2019"></a>Novedades en la documentación para desarrolladores de Windows de enero de 2019.

La documentación para desarrolladores de Windows se actualiza constantemente con información sobre las nuevas características disponibles para los desarrolladores a través de la plataforma de Windows. La siguiente introducción a las características, instrucciones para desarrolladores y vídeos están disponibles desde el mes de enero.

[Instala las herramientas y el SDK](https://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](../get-started/create-uwp-apps.md) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](../porting/index.md).

## <a name="features"></a>Características

### <a name="windows-development-on-microsoft-learn"></a>Desarrollo de Windows en Microsoft Learn

El nuevo sitio de Microsoft Learn ofrece nuevos conocimientos prácticos y las oportunidades de entrenamiento para desarrolladores de Microsoft. Si está interesado en aprender a desarrollar aplicaciones de Windows, consulte [nuestra nueva ruta de aprendizaje](https://docs.microsoft.com/learn/paths/develop-windows10-apps/) para una introducción exhaustiva a la plataforma, las herramientas y cómo escribir sus primeras aplicaciones.

![Imagen de la ruta de aprendizaje de desarrollo de Windows.](images/windows-learn.png)

### <a name="direct-3d-12"></a>Direct 3D 12

[Las transferencias de representación de Direct3D 12](/windows/desktop/direct3d12/direct3d-12-render-passes) pueden mejorar el rendimiento de su representador si se basa en la representación de aplazado basados en mosaicos (TBDR), entre otras técnicas. La técnica ayuda a que el representador mejore la eficacia de la GPU habilitando la aplicación para identificar mejor el orden de los requisitos y las dependencias de datos de representación de recursos y, con ello, reduciendo el tráfico de memoria a y desde la memoria fuera del chip.

### <a name="msix-modification-packages"></a>Paquetes de modificación MSIX

Compatibilidad mejorada de Windows 10 versión 1809 para [paquetes de modificación MSIX](https://docs.microsoft.com/windows/msix/modification-package-1809-update). Paquetes de modificación pueden incluir complementos basados en el registro y la personalización asociada, además permiten una aplicación implementada a través de MSIX para usar un registro virtual y ejecutarse según lo previsto.

![Creación de paquetes de modificación MSIX](images/msix-modification-package.png)

### <a name="open-source-of-wpf-windows-forms-and-winui"></a>Código abierto de WPF, Windows Forms y WinUI

Los marcos de trabajo de WPF, Windows Forms y WinUI UX ahora están disponibles para las contribuciones de código abierto en GitHub. Para obtener más información y vínculos, consulte el [blog compilación de aplicaciones de Windows](https://blogs.windows.com/buildingapps/2018/12/04/announcing-open-source-of-wpf-windows-forms-and-winui-at-microsoft-connect-2018/#OKZjJs1VVTrMMtkL.97).

### <a name="progressive-web-apps-for-xbox"></a>Aplicaciones web progresivas para Xbox

Con [las aplicaciones web progresivas para Xbox One](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/xbox-considerations) puede extender una aplicación web y hacerla disponible como una aplicación de Xbox One a través de Microsoft Store mientras todavía sigue usando los marcos de trabajo existentes, CDN y el back-end del servidor. En su mayor parte, puede empaquetar su PWA para Xbox One de la misma manera que lo haría para Windows, sin embargo, hay varias diferencias clave a través de las cuales le guiará esta guía.

### <a name="windows-machine-learning"></a>Windows Machine Learning

Hemos reestructurado [la página de aterrizaje de WinML APIs](https://docs.microsoft.com/windows/ai/api-reference)y agregado la nueva documentación para el operador de WinML personalizado y las API nativas.

En [Entrenar un modelo con PyTorch](https://docs.microsoft.com/windows/ai/train-model-pytorch) se proporcionan instrucciones sobre cómo entrenar un modelo con el marco de trabajo de PyTorch localmente o en la nube. A continuación, puede descargar este modelo como un archivo ONNX y usarlo en sus aplicaciones de WinML.

![Gráfico de WinML](images/winml-graphic.png)

## <a name="developer-guidance"></a>Guía para desarrolladores

### <a name="choose-your-platform"></a>Elección de la plataforma

¿Está interesado en crear una nueva aplicación de escritorio? Visite nuestra página renovada [Elija su plataforma](https://docs.microsoft.com/windows/desktop/choose-your-technology) para obtener una descripción detallada y las comparaciones de las plataformas de Windows Forms, WPF y UWP, así como para obtener más información sobre la API de Win32.

### <a name="faqs-on-win32-webview"></a>Preguntas frecuentes sobre WebView de Win32

Nuestras [preguntas más frecuentes](https://docs.microsoft.com/windows/communitytoolkit/controls/wpf-winforms/webview#frequently-asked-questions-faqs) proporcionan respuestas a preguntas habituales al usar Microsoft Edge WebView en aplicaciones de escritorio, así como vínculos a ejemplos y recursos adicionales.

### <a name="japanese-era-change"></a>Cambio a la era japonesa

[Prepare su aplicación para el cambio a la era japonesa](../design/globalizing/japanese-era-change.md) le muestra cómo asegurarse de que la aplicación de Windows está lista para el conjunto de cambios de la era japonesa que se realizará el 1 de mayo de 2019. [Esta página también está disponible en japonés](https://docs.microsoft.com/ja-jp/windows/uwp/design/globalizing/japanese-era-change).

## <a name="videos"></a>Vídeos

### <a name="progressive-web-apps"></a>Aplicaciones web progresivas

Las aplicaciones web progresivas son sitios web que funcionan como aplicaciones nativas en distintos exploradores y una amplia variedad de dispositivos Windows 10. [Mire el vídeo](https://youtu.be/ugAewC3308Y) para conocer más y luego [eche un vistazo a los documentos](https://aka.ms/Windows-PWA) para obtener más información.

### <a name="vs-code-series"></a>Serie de código de VS

Visite nuestra [nueva serie de vídeos sobre Visual Studio Code](https://www.youtube.com/playlist?list=PLlrxD0HtieHjQX77y-0sWH9IZBTmv1tTx) para obtener información sobre VSCode, cómo usarlo y cómo se creó.

### <a name="one-dev-question"></a>Una pregunta de desarrollador

En la serie de vídeos de Una pregunta de desarrollador, los desarrolladores de Microsoft cubren una serie de preguntas acerca del desarrollo de Windows, la cultura del equipo y la historia. Aquí es donde hemos respondido a las preguntas más recientes.

Raymond Chen:

* [¿Por qué tener archivos de aplicación y los archivos de programa (x86)?](https://youtu.be/N7o9eJpFYco)

Larry Osterman:

* [¿Por qué es COM tan complicado?](https://youtu.be/-gkXAV-StVA )
* [Cómo fue tu primera entrevista para Microsoft?](https://youtu.be/qRb6otsHG5c)
