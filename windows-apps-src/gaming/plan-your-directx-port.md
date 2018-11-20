---
author: mtoepke
title: Planear la migración de DirectX
description: 'Planea un proyecto para migrar un juego de DirectX 9 a DirectX 11 y a la Plataforma universal de Windows (UWP): actualiza el código de gráficos y coloca el juego en el entorno de Windows Runtime.'
ms.assetid: 3c0c33ca-5d15-ae12-33f8-9b5d8da08155
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, DirectX, port, migración
ms.localizationpriority: medium
ms.openlocfilehash: dea6455b4e9aaef2a4239ef70d0919a4b8841bc5
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7292781"
---
# <a name="plan-your-directx-port"></a>Planear la migración de DirectX



**Resumen**

-   Planea tu migración de DirectX
-   [Cambios importantes en Direct3D 11 respecto a Direct3D 9](understand-direct3d-11-1-concepts.md)
-   [Asignación de características](feature-mapping.md)


Planea un proyecto para migrar un juego de DirectX 9 a DirectX 11 y a la Plataforma universal de Windows (UWP): actualiza el código de gráficos y coloca el juego en el entorno de Windows Runtime.

## <a name="plan-to-port-graphics-code"></a>Planear la migración del código de gráficos


Antes de empezar la migración de tu juego a UWP, es importante asegurarse de que no tenga retenciones de Direct3D 8. Asegúrate de que tu juego no tenga ningún remanente de la canalización de función fija. Para obtener una lista completa de características en desuso, incluida la funcionalidad de canalización fija, consulta [Características desusadas](https://msdn.microsoft.com/library/windows/desktop/cc308047).

Actualizar de Direct3D 9 a Direct3D 11 es más que un cambio de buscar y reemplazar. Necesitas conocer la diferencia entre el dispositivo Direct3D, el contexto de dispositivo y la infraestructura de gráficos, y aprender sobre otros cambios importantes a partir de Direct3D 9. Puedes empezar este proceso leyendo otros temas de esta sección.

Debes reemplazar las bibliotecas auxiliares de D3DX y DXUT con tus propias bibliotecas auxiliares o con herramientas de la comunidad. Consulta la sección [Asignación de características](feature-mapping.md) para obtener más información.

> **Nota**  puede utilizar el [Kit de herramientas de DirectX](http://go.microsoft.com/fwlink/p/?LinkID=248929) o [DirectXTex](http://go.microsoft.com/fwlink/p/?LinkID=248926) para reemplazar algunas funciones que se proporcionaban anteriormente con D3DX y DXUT.

 

Los sombreadores escritos en lenguaje de ensamblado deben actualizarse a HLSL mediante la funcionalidad del modelo de sombreador 4 nivel 9\_1 o 9\_3. En cambio, los sombreadores escritos para la biblioteca de efectos necesitarán actualizarse a una versión más reciente de la sintaxis de HLSL. Consulta la sección [Asignación de características](feature-mapping.md) para obtener más información.

Conoce los distintos [Niveles de característica de Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476876). Los niveles de característica clasifican una amplia gama de hardware de vídeo al definir conjuntos de funciones conocidas. Cada conjunto corresponde, a grandes rasgos, a versiones de Direct3D, de 9.1 a 11.2. Todas los niveles de característica usan la API de DirectX11.

## <a name="plan-to-port-win32-ui-code-to-corewindow"></a>Planear para migrar código de interfaz de usuario de Win32 a CoreWindow


Las aplicaciones para UWP se ejecutan en una ventana creada para un contenedor de la aplicación, denominado [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). El juego controlará la ventana al heredar de la interfaz [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478), lo que requiere menos detalles de implementación que una ventana de escritorio. El bucle principal del juego estará en el método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505).

El ciclo de vida de una aplicación para UWP es muy distinto al de una aplicación de escritorio. Necesitarás guardar el juego con más frecuencia porque, cuando se produce un evento de suspensión, la aplicación solo tiene una cantidad limitada de tiempo para dejar de ejecutar el código y, lo que quieres, es asegurarte de que el jugador pueda retomar el juego en donde lo dejó tan pronto como se reanude la aplicación. Los juegos se deben guardar con la suficiente frecuencia como para mantener una experiencia de juego continua con la reanudación, pero no con demasiada frecuencia para evitar que el juego reduzca el impacto de la velocidad de fotogramas o parpadee. El juego necesitará potencialmente cargar el estado del juego cuando se reanude después de pasar por un estado terminado.

[DirectXMath](https://msdn.microsoft.com/library/windows/desktop/ee415571) se puede usar como reemplazo de D3DXMath y XNAMath, y puede ser útil si necesitas una biblioteca matemática. DirectXMath tiene tipos de datos portátiles rápidos y tipos que se alinean y empaquetan para usar con sombreadores.

Las bibliotecas nativas, como las [API entrelazadas](https://msdn.microsoft.com/library/windows/desktop/dd405529), se han expandido para admitir intrínsecos de ARM. Si tu juego usa API entrelazadas, puedes seguir usándolas en DirectX 11 y UWP.

Nuestras muestras de código y plantillas usan nuevas características de C++ que podrían resultarte desconocidas. Por ejemplo, los métodos asincrónicos se usan con [**expresiones lambda**](https://msdn.microsoft.com/library/windows/apps/dd293608.aspx) para así poder cargar recursos de Direct 3D sin bloquear el subproceso de la interfaz de usuario.

Hay dos conceptos que usarás con frecuencia:

-   Las referencias administradas ([**operador ^**](https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx)) y las [**clases administradas**](https://msdn.microsoft.com/library/windows/apps/6w96b5h7.aspx) (clases ref) son una parte fundamental de Windows Runtime. Necesitarás usar clases ref administradas para una interfaz con componentes de Windows Runtime, por ejemplo [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) (puedes encontrar más información sobre esto en el tutorial).
-   Cuando trabajes con interfaces COM en Direct3D 11, usa el tipo de plantilla [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) para que los punteros COM sean más fáciles de usar.

 

 




