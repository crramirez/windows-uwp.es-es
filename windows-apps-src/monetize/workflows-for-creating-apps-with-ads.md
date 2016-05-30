---
author: mcleanbyron
ms.assetid: fcebd659-438b-4d03-bc73-6b662ed6f1f3
description: Obtén información sobre el proceso completo para desarrollar y publicar una aplicación con anuncios.
title: Flujos de trabajo para crear aplicaciones con anuncios

---

# Flujos de trabajo para crear aplicaciones con anuncios


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Para mostrar anuncios en tus aplicaciones, estas deben poder recibir anuncios de una red de anuncios. Microsoft proporciona un servicio web que permite a los desarrolladores de aplicaciones de Windows recibir anuncios. Cuando los usuarios hacen clic en el anuncio en la aplicación, puedes (como *publicador* del anuncio) obtener dinero del creador de los anuncios (el *anunciante*). El dinero que obtienes de los anunciantes se te paga a través de tu cuenta.

Los siguientes pasos principales describen el proceso general de desarrollo y publicación de una aplicación con anuncios.

1.  Fase de desarrollo:

    * Configura tu cuenta del Centro de desarrollo de Windows.
    * Desarrolla la aplicación con los valores del modo de prueba.

2.  Listo para publicar:

    * Configura la aplicación para que pueda recibir anuncios dinámicos.
    * Enviar la aplicación al Centro de desarrollo de Windows y revisa los datos de rendimiento.

Para obtener más información acerca de cada paso, lee la sección correspondiente a continuación.

## Configurar la cuenta del Centro de desarrollo de Windows

Debes tener una cuenta del Centro de desarrollo de Windows para publicar la aplicación y recibir anuncios. Este requisito se aplica independientemente de si usas la mediación de anuncios. La administración de la aplicación relacionada con la publicidad también se lleva a cabo en el Centro de desarrollo de Windows. Si usabas Microsoft pubCenter para administrar la publicidad de las aplicaciones, ahora se ha reemplazado por características del Centro de desarrollo de Windows.

Para configurar tu cuenta del Centro de desarrollo de Windows, visita la [página principal](https://dev.windows.com/windows-apps). Existe información adicional disponible en las [páginas de ayuda](https://dev.windows.com/develop) del Centro de desarrollo de Windows.

## Desarrollar la aplicación con los valores del modo de prueba

Sigue las instrucciones de los siguientes tutoriales para agregar un objeto [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) o [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) para mostrar anuncios en tu aplicación:

-   [Anuncios intersticiales](interstitial-ads.md)
-   [AdControl en XAML y .NET](adcontrol-in-xaml-and--net.md)
-   [AdControl en HTML 5 y JavaScript](adcontrol-in-html-5-and-javascript.md)
-   [AdControl en Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md)

Si usas un objeto **AdControl** o **InterstitialAd** para mostrar anuncios en la aplicación, debes especificar un identificador de aplicación y un identificador de unidad de anuncios en el código para vincular la aplicación a tu cuenta del Centro de desarrollo de Windows y para proporcionar anuncios. Mientras desarrolles la aplicación, usa los valores de identificador de aplicación y de identificador de unidad de anuncios para ver cómo la aplicación representa los anuncios durante las pruebas. Esto te permite ver cómo la aplicación recibe y representa anuncios durante las pruebas. Para obtener más información, consulta [Valores del modo de prueba](test-mode-values.md).

Para obtener proyectos de muestra completos que muestren cómo agregar anuncios de banner e intersticiales en vídeo a aplicaciones de JavaScript y HTML y aplicaciones XAML con C# y C++, consulta las [muestras de publicidad en GitHub](http://aka.ms/githubads).

## Configurar la aplicación para que pueda recibir anuncios dinámicos

Cuando termines de probar la aplicación y estés listo para enviarla al Centro de desarrollo de Windows, debes actualizar el código de la aplicación para usar los valores de identificador de aplicación y de identificador de unidad de anuncios desde el [panel del Centro de desarrollo de Windows](https://msdn.microsoft.com/library/windows/apps/mt170658.aspx). Si intentas usar valores de prueba en la aplicación dinámica, la aplicación no recibirá anuncios dinámicos. Para obtener más información, consulta [Configurar unidades de anuncios en la aplicación](set-up-ad-units-in-your-app.md).

## Enviar la aplicación

Después de completar el desarrollo de la aplicación, puedes publicarla en la Tienda Windows mediante el panel del Centro de desarrollo de Windows. Además de cumplir con los requisitos para todas las aplicaciones de la Tienda Windows, las aplicaciones que muestren anuncios deben cumplir varios requisitos adicionales. Para obtener más información, consulta [Enviar una aplicación con anuncios a la Tienda Windows](submit-an-app-with-ads-to-the-windows-store.md).

Cuando tu aplicación se publique y esté disponible en la Tienda Windows, puedes revisar tus [informes de rendimiento de publicidad](../publish/advertising-performance-report.md) en el panel del Centro de desarrollo.

 

 


<!--HONumber=May16_HO2-->


