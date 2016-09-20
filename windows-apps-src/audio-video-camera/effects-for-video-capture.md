---
author: drewbatgit
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: "Este tema describe los efectos diseñados para usarse en escenarios de captura de vídeo. Esto incluye el efecto de estabilización de vídeo."
title: "Efectos para captura de vídeos"
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 3af5ed7146f2420c2a6d3035c26290cbeaff8375

---

# Efectos para captura de vídeos

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tema describe los efectos diseñados para su uso en escenarios de captura de vídeo. Esto incluye el efecto de estabilización de vídeo.

**Nota**  
Este artículo se basa en los conceptos y el código analizados en [Capturar fotografías y vídeos con MediaCapture](capture-photos-and-video-with-mediacapture.md), donde se describen los pasos para implementar capturas básicas de fotografías y vídeos. Se recomienda que te familiarices con el patrón de captura de multimedia básico de ese artículo antes de pasar a escenarios más avanzados de captura. El código que encontrarás en este artículo se ha agregado suponiendo que la aplicación ya tiene una instancia de MediaCapture inicializada correctamente.

## Efecto de estabilización de vídeo

El efecto de estabilización de vídeo manipula los marcos de una secuencia de vídeo para minimizar la sacudida causada por sujetar el dispositivo de captura con la mano. Dado que esta técnica hace que los píxeles de desplazamiento se muevan a derecha, izquierda, arriba o abajo, y como el efecto no puede saber cuál es el contenido que queda justo fuera del marco de vídeo, el vídeo estabilizado se recorta ligeramente en relación al vídeo original. Se proporciona una función de utilidad que te permite ajustar la configuración para administrar de forma óptima el corte realizado por el efecto de codificación de vídeo.

En los dispositivos que lo admiten, la estabilización de imagen óptica (OIS) estabiliza el vídeo manipulando mecánicamente el dispositivo de captura y, por lo tanto, no es necesario recortar los bordes de los fotogramas de vídeos. Para más información, consulta [capturar controles del dispositivo de captura de vídeo](capture-device-controls-for-video-capture.md).

### Configurar tu aplicación para usar la estabilización de vídeo

Además de los espacios de nombres necesarios para la captura básica de elementos multimedia, si usas el efecto de estabilización de vídeo necesitarás el siguiente espacio de nombres.

[!code-cs[VideoStabilizationEffectUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoStabilizationEffectUsing)]

Declara una variable de miembro para almacenar el objeto [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760). Como parte de la implementación del efecto, tendrás que modificar las propiedades de codificación que uses para codificar el vídeo capturado. Para ello, declara dos variables para almacenar una copia de seguridad tanto de la entrada inicial como de las propiedades de codificación de salida, para que así puedas restaurarlas más adelante cuando se deshabilite el efecto. Por último, declara una variable de miembro de tipo [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026), ya que se obtendrá acceso a este objeto desde varias ubicaciones dentro del código.

[!code-cs[DeclareVideoStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareVideoStabilizationEffect)]

En la implementación básica de capturas de vídeo que se describe en el artículo [Capturar fotos y vídeos con MediaCapture](capture-photos-and-video-with-mediacapture.md), el objeto del perfil de codificación de medios se asigna a una variable local porque no se usa en otro lugar del código. Para este escenario, debes asignar el objeto a una variable de miembro de modo que puedas acceder a él más adelante.

[!code-cs[EncodingProfileMember](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEncodingProfileMember)]

### Inicializar el efecto de estabilización de vídeo

Después de inicializar el objeto **MediaCapture**, puedes crear una nueva instancia del objeto [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762). Llama a [**MediaCapture.AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035) para agregar el efecto a la canalización de vídeo y recuperar una instancia de clase [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760). Especifica [**MediaStreamType.VideoRecord**](https://msdn.microsoft.com/library/windows/apps/br226640) para indicar que el efecto debe aplicarse a la secuencia de grabación del vídeo.

Registra un controlador de eventos para el evento [**EnabledChanged**](https://msdn.microsoft.com/library/windows/apps/dn948982) y llama al método auxiliar **SetUpVideoStabilizationRecommendationAsync**; describiremos ambos más adelante en este artículo. Por último, establece la propiedad [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926775) del efecto en "true" para habilitar el efecto.

[!code-cs[CreateVideoStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateVideoStabilizationEffect)]

### Usa las propiedades de codificación recomendadas

Tal como se explicó anteriormente en este artículo, la técnica que el efecto de estabilización del vídeo usa, hace que el vídeo estabilizado se recorte ligeramente en relación al vídeo de origen. Define la siguiente función auxiliar en el código para ajustar las propiedades de codificación de vídeo para controlar esta limitación del efecto de forma óptima. Este paso no es necesario para poder usar el efecto de estabilización de vídeo, pero si lo no realizas, el vídeo resultante se escalará un poco verticalmente y, por lo tanto, la fidelidad visual será un poco más baja.

Llama al método [**GetRecommendedStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948983) que se encuentra en la instancia del efecto de estabilización de vídeo y pasa el objeto [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) (el cual informará al efecto sobre las propiedades de codificación de secuencia) y la clase **MediaEncodingProfile** (la cual permite al efecto conocer las propiedades de codificación de salida actuales). Este método devuelve un objeto [**VideoStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn926727) que contiene nuevas propiedades recomendadas de codificación de la secuencia de entrada y salida.

Una propiedad de codificación de entrada recomendada es, en caso de ser compatible con el dispositivo, una resolución más alta que la configuración inicial que proporcionaste; gracias a ella, habrá una pérdida mínima en la resolución después de aplicar la opción de recorte del efecto.

Llama a [**VideoDeviceController.SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895) para establecer las nuevas propiedades de codificación. Antes de establecer las propiedades nuevas, usa la variable de miembro para almacenar las propiedades iniciales de codificación y que así puedas cambiar la configuración cuando se desactive el efecto.

Si el efecto de estabilización de vídeo debe recortar el vídeo de salida, la codificación de propiedades de salida recomendada será del tamaño del vídeo recortado. Esto significa que la resolución de salida coincidirá con el tamaño del vídeo recortado. Si no usas las propiedades de salida recomendadas, el vídeo se escalará verticalmente para hacerlo coincidir con el tamaño de salida inicial, lo que dará como resultado una pérdida de fidelidad visual.

Establece la propiedad [**Video**](https://msdn.microsoft.com/library/windows/apps/hh701124) del objeto **MediaEncodingProfile**. Antes de establecer las propiedades nuevas, usa la variable de miembro para almacenar las propiedades iniciales de codificación y que así puedas cambiar la configuración cuando se desactive el efecto.

[!code-cs[SetUpVideoStabilizationRecommendationAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetUpVideoStabilizationRecommendationAsync)]

### Controlar el efecto de estabilización de vídeo que se va a deshabilitar

El sistema puede deshabilitar automáticamente el efecto de estabilización de vídeo si el rendimiento de píxeles es demasiado alto para que el efecto pueda controlarlo, o si detecta que el efecto se está ejecutando lentamente. Si esto ocurre, se genera el evento EnabledChanged. La instancia **VideoStabilizationEffect** del parámetro *sender* indica el nuevo estado del efecto: habilitado o deshabilitado. La clase [**VideoStabilizationEffectEnabledChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948979) tiene un valor [**VideoStabilizationEffectEnabledChangedReason**](https://msdn.microsoft.com/library/windows/apps/dn948981) que indica por qué se habilitó o deshabilitó el efecto. Ten en cuenta que este evento también se genera, si mediante programación, habilitas o deshabilitas el efecto; en tal caso, el motivo será **Programmatic**.

Por lo general, deberías usar este evento para ajustar la interfaz de usuario de la aplicación para indicar el estado actual de la estabilización de vídeo.

[!code-cs[VideoStabilizationEnabledChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoStabilizationEnabledChanged)]

### Limpiar el efecto de estabilización de vídeo

Para limpiar el efecto de estabilización de vídeo, llama al método [**ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592) para borrar todos los efectos de la canalización de vídeo. Si las variables de miembro que contienen las propiedades de codificación iniciales no son nulas, úsalas para restaurar las propiedades de codificación. Por último, quita el controlador de eventos **EnabledChanged** y establece el efecto en "null".

[!code-cs[CleanUpVisualStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVisualStabilizationEffect)]

## Temas relacionados

* [Capturar fotografías y vídeos con MediaCapture](capture-photos-and-video-with-mediacapture.md)
 

 







<!--HONumber=Jun16_HO4-->


