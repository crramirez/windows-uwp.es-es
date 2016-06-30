---
author: mcleanbyron
Description: "A continuación se indican algunas soluciones para varios problemas de desarrollo comunes relacionados con la mediación de anuncios."
title: "Solucionar problemas de mediación de anuncios"
ms.assetid: 8728DE4F-E050-4217-93D3-588DD3280A3A
translationtype: Human Translation
ms.sourcegitcommit: 10dcf3c2b8ea530b94e9c17ada80aaa98e9418fe
ms.openlocfilehash: f32dc28c9b199c11a1932639f49ab4c29d3e1e8f

---

# Solucionar problemas de mediación de anuncios


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

A continuación se indican algunas soluciones para varios problemas de desarrollo comunes relacionados con la mediación de anuncios.

**No se puede agregar un objeto AdMediatorControl a la superficie de diseño**  
Cuando se arrastra el control **AdMediatorControl** al diseñador por primera vez en un proyecto de Plataforma universal de Windows (UWP), Windows 8.1 o Windows Phone 8.1 con C# o Visual Basic con XAML, Visual Studio agrega la referencia de ensamblado de mediador de anuncios necesaria al proyecto, pero el control aún no se agrega al diseñador. Para agregarlo, haz clic en Aceptar en el mensaje que se muestra en Visual Studio, espera varios segundos a que se actualice el diseñador y luego arrastra de nuevo el control al diseñador.

Si aún no puedes agregar correctamente el control al diseñador, asegúrate de que el proyecto se dirija a la arquitectura de procesador aplicable para la aplicación (por ejemplo, **x 86**) en lugar de **Cualquier CPU**. El control no se puede agregar al diseñador si el proyecto se dirige a **Cualquier CPU** para la plataforma de compilación.

*
              *El objeto AdMediatorControl muestra el error "&lt;*width*
            &gt; x &lt;*height*&gt; no se admite" en tiempo de ejecución al ofrecer anuncios de Microsoft**. Microsoft Advertising solo admite [determinados tamaños de anuncio recomendados por Interactive Advertising Bureau (IAB)](add-and-use-the-ad-mediator-control.md#supported-ad-sizes-for-microsoft-advertising). En algunos casos, incluso si en el diseñador o en el código XAML estableces el alto y ancho del control de Ad Mediator en uno de estos tamaños de anuncio admitidos, es posible que los problemas de escalado y redondeo impidan que el marco de mediación de anuncios ofrezca un anuncio. Para evitar este problema, asigna en el código los parámetros opcionales **Width** y **Height** correspondientes a Microsoft Advertising a uno de los tamaños de anuncio admitidos.

En el siguiente ejemplo de código se muestra cómo asignar los parámetros opcionales **Ancho** y **Alto** para Microsoft Advertising en 728 x 90.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 728;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 90;
```

**Mediación de anuncios no incluye la ubicación (latitud/longitud) de la red de anuncios**  
Si habilitas la funcionalidad de ubicación en la aplicación, el control de Mediador de anuncios capturará automáticamente las coordenadas de latitud y longitud y las proporcionará a las redes de anuncios que las admitan.

**El control de anuncios Smaato no se alinea correctamente**  
Prueba a usar los parámetros opcionales para establecer los valores en los controles SDK:

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Margin"] = new Thickness(0, -20, 0, 0);
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 50d;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 320d;
```

**El control de anuncios AdDuplex no se muestra con el tamaño correcto (se muestra como 250 x 250)**  
La mediación de anuncios no establece ningún valor para el tamaño, por lo que debes cambiarlo con el parámetro opcional **Size**. Por ejemplo:

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.AdDuplex]["Size"] = "160x600";
```

**Recibes el error "Algo tapa el control de anuncios"**  
AdDuplex siempre mostrará un error si el anuncio queda de alguna forma tapado en la aplicación. [Lee la solución](http://blog.adduplex.com/2014/01/solving-something-is-covering-ad.mdl) a este error.

**Recibes el error "Se ha producido un conflicto entre dos archivos"**  
Se ha hecho referencia a los ensamblados de Microsoft Advertising en otro lugar de la aplicación. La mediación de anuncios está diseñada para funcionar exclusivamente en la aplicación y no funcionará si se usan otras referencias a los ensamblados de Microsoft Advertising. Quita manualmente las referencias de Microsoft Advertising y reinstala el SDK de Microsoft Store Engagement and Monetization para solucionar el error.

**Se produce un comportamiento inesperado después de cambiar el valor de RefreshRate en el archivo AdMediator.config.**

Después de configurar las redes de anuncios mediante la ejecución del componente **Ad Mediator** en el cuadro de diálogo **Agregar servicio conectado** de Visual Studio, la información de la configuración predeterminada se guarda en el archivo AdMediator.config en el proyecto. No tienes que modificar este archivo directamente. En su lugar, puedes modificar esta información (incluida la frecuencia de actualización de los anuncios nuevos) al [configurar la mediación de anuncios para tu aplicación](submit-your-app-and-configure-ad-mediation.md) después de cargar el paquete de la aplicación en el panel del Centro de desarrollo de Windows.

Si cambias el valor de **RefreshRate** en el archivo AdMediator.config, ten en cuenta que este valor debe contener un número entero entre 30 y 120 que represente la frecuencia de actualización en segundos. Si lo estableces en un número entero inferior a 30 o superior a 120, el marco de mediación de anuncios usará automáticamente una frecuencia de actualización de 60 segundos.

## Temas relacionados

* [Seleccionar y administrar redes de anuncios](select-and-manage-your-ad-networks.md)
* [Agregar y usar el control de mediación de anuncios](add-and-use-the-ad-mediator-control.md)
* [Probar la implementación de mediación de anuncios](test-your-ad-mediation-implementation.md)
* [Enviar la aplicación y configurar la mediación de anuncios](submit-your-app-and-configure-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


