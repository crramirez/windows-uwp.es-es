---
author: QuinnRadich
title: "Novedades de Windows 10, vista previa de la versión 1607"
description: "Tanto la vista previa de la versión 1607 de Windows 10 como las nuevas herramientas para desarrolladores te proporcionan las herramientas, características y experiencias que ofrece la tecnología de la nueva Plataforma universal de Windows."
keywords: "conoce las novedades, novedades, actualizar, actualizaciones, características, nuevo, Windows 10, vista previa de 1607"
ms.author: quradic
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.assetid: 835d5393-427f-4155-a737-d509ea1de99f
ms.openlocfilehash: 7e37846e7696213093e84d7409c6900e0aad5ed5
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="whats-new-in-windows"></a>Novedades de Windows

Tanto la vista previa de la versión 1607 de Windows 10 como las actualizaciones de las herramientas de desarrollador de Windows te siguen proporcionando las herramientas, características y experiencias que permite la tecnología de la Plataforma universal de Windows. [Instala las herramientas y el SDK](http://go.microsoft.com/fwlink/?LinkId=821431) en Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](https://msdn.microsoft.com/library/windows/apps/bg124288) o para aprender a usar el [código de aplicación existente en Windows](https://msdn.microsoft.com/library/windows/apps/mt238321).

## <a name="windows-10-version-1607-preview"></a>Vista previa de la versión 1607 de Windows 10

Característica | Descripción
 :---- | :----
Redes | Si te suscribes al evento [HttpBaseProtocolFilter.ServerCustomValidationRequest](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank), podrás proporcionar tu validación personalizada de certificados SSL/TLS de servidor. También puedes deshabilitar por completo la lectura de las respuestas HTTP de la memoria caché si especificas el valor de enumeración [HttpCacheReadBehavior.NoCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpcachereadbehavior.aspx#_blank) en una solicitud HTTP. Ahora es posible borrar las credenciales de autenticación para habilitar un escenario "cerrar sesión" al llamar al método [HttpBaseProtocolFilter.ClearAuthenticationCache](https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx#_blank).
Extensiones | Microsoft Edge permite ahora usar extensiones. Con las extensiones, los usuarios pueden ampliar las capacidades de Microsoft Edge, lo que proporciona una funcionalidad nicho que es importante para el público de destino. Echa un vistazo a la [documentación de las extensiones](https://developer.microsoft.com/microsoft-edge/platform/documentation/extensions/#_blank) para obtener más información.
API para Bluetooth | Las aplicaciones ahora pueden acceder a servicios RFCOMM en periféricos Bluetooth remotos a través de [Windows.Devices.Bluetooth y Windows.Devices.Bluetooth.Rfcomm](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx#_blank) sin tener que emparejar con el periférico. Existen nuevos métodos que permiten que las aplicaciones busquen y accedan a servicios RFCOMM en dispositivos no emparejados.
API de chat | La nueva clase [ChatSyncManager](https://msdn.microsoft.com/library/windows/apps/mt414181.aspx#_blank) te permite sincronizar mensajes de textos desde y en la nube.
[Asignación del concepto de aplicaciones de Windows para desarrolladores de Android e iOS](https://msdn.microsoft.com/windows/uwp/porting/android-ios-uwp-map#_blank) | Si eres un desarrollador con conocimientos o código de Android o iOS, y quieres hacer el cambio a Windows 10 y la Plataforma universal de Windows (UWP), este recurso tiene todo lo que necesitas para asignar características de la plataforma, y tus conocimientos, entre las tres plataformas.
[Enterprise data protection (EDP)](https://msdn.microsoft.com/windows/uwp/enterprise/wip-hub) | EDP es un conjunto de características en equipos de escritorio, portátiles, tabletas y teléfonos para Administración de dispositivos móviles (MDM). EDP ofrece una mayor control empresarial sobre cómo se controlan sus datos (archivos de empresa y BLOB de datos) en los dispositivos que administra la empresa.
[Windows.ApplicationModel.AppExtensions](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.appextensions.aspx#_blank) | El nuevo espacio de nombres AppExtensions permite que la aplicación de la Tienda Windows hospede contenido proporcionado por otras aplicaciones de la Tienda Windows. Puedes descubrir, enumerar y acceder a contenido de solo lectura de esas aplicaciones.
Windows IoT | Windows 10 IoT Core te permite crear aplicaciones de IoT en la familiaridad de Windows y ahora está disponible en Raspberry Pi 3, el panel de Raspberry Pi más reciente.
API para multimedia | Las nuevas API MediaBreak en el espacio de nombres Windows.Media.Playback permiten programar y administrar las interrupciones multimedia al reproducir contenido multimedia con MediaSource y MediaPlaybackItem. Las nuevas API de AudioGraph en el espacio de nombres Windows.Media.Audio agregan procesamiento de audio espacial que te permite asignar emisores de posición 3D y agentes de escucha a los nodos del gráfico de audio.
API de mapas | La clase [MapControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.maps.mapcontrol.aspx#_blank) se ha mejorado para permitir que los desarrolladores obtengan una región visible que está cerca de la cámara, excluidas las regiones que están alejadas y cerca del horizonte en una vista profundamente inclinada. La clase [MapLocationFinder](https://msdn.microsoft.com/library/windows/apps/windows.services.maps.maplocationfinder.aspx#_blank) se ha extendido, lo que permite a los desarrolladores optimizar el tráfico de red con geocodificación inversa mediante la especificación de una precisión deseada. Los desarrolladores pueden ahora aprovecharse de la descarga de mapas sin conexión con el método [LaunchUriAsync](https://msdn.microsoft.com/library/windows/apps/hh701480.aspx#_blank) y especificando la latitud y longitud. Para obtener más información, consulta [Iniciar la aplicación Mapas de Windows](https://msdn.microsoft.com/windows/uwp/launch-resume/launch-maps-app#_blank).
