---
Description: A continuación se indican algunas soluciones para varios problemas de desarrollo comunes relacionados con la mediación de anuncios.
title: Solucionar problemas de mediación de anuncios
ms.assetid: 8728DE4F-E050-4217-93D3-588DD3280A3A
---

# Solucionar problemas de mediación de anuncios


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

A continuación se indican algunas soluciones para varios problemas de desarrollo comunes relacionados con la mediación de anuncios.

**No se puede agregar un objeto AdMediatorControl a la superficie de diseño**  
Cuando se arrastra el control **AdMediatorControl** al diseñador por primera vez en un proyecto de Plataforma universal de Windows (UWP), Windows 8.1 o Windows Phone 8.1 con C# o Visual Basic con XAML, Visual Studio agrega la referencia de ensamblado de mediador de anuncios necesaria al proyecto, pero el control aún no se agrega al diseñador. Para agregarlo, haz clic en Aceptar en el mensaje que se muestra en Visual Studio, espera varios segundos a que se actualice el diseñador y luego arrastra de nuevo el control al diseñador.

Si aún no puedes agregar correctamente el control al diseñador, asegúrate de que el proyecto se dirija a la arquitectura de procesador aplicable para la aplicación (por ejemplo, **x 86**) en lugar de **Cualquier CPU**. El control no se puede agregar al diseñador si el proyecto se dirige a **Cualquier CPU** para la plataforma de compilación.

**El control AdMediatorControl muestra el error “&lt;*ancho*&gt; x &lt;*alto*&gt; no se admite" en tiempo de ejecución cuando ofrece anuncios de Microsoft**  
Microsoft Advertising solo admite [tamaños de anuncio determinados recomendados por Interactive Advertising Bureau (IAB)](add-and-use-the-ad-mediator-control.md#supported-ad-sizes-for-microsoft-advertising). En algunos casos, incluso si en el diseñador o en el código XAML estableces el alto y ancho del control de mediador de anuncios en uno de estos tamaños de anuncio admitidos, es posible que los problemas de escalado y redondeo impidan que el marco de mediación de anuncios ofrezca un anuncio. Para evitar este problema, asigna los parámetros opcionales **Ancho** y **Alto** de Microsoft Advertising del código a uno de los tamaños de anuncio admitidos.

En el siguiente ejemplo de código se muestra cómo asignar los parámetros opcionales **Ancho** y **Alto** para Microsoft Advertising en 728 x 90.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 728;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 90;
```

**Mediación de anuncios no incluye la ubicación (latitud/longitud) de la red de anuncios**  
Si habilitas la funcionalidad de ubicación en la aplicación, el control de Mediador de anuncios capturará automáticamente las coordenadas de latitud y longitud y las proporcionará a las redes de anuncios que las admitan.

**El control de anuncios Smaato no se alinea correctamente**  
Prueba a usar los parámetros opcionales para establecer los valores en los controles SDK:

```
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato][“Margin”] = new Thickness(0, -20, 0, 0);
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato][“Width”] = 50d;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato][“Height”] = 320d;
```

**El control de anuncios AdDuplex no se muestra con el tamaño correcto (se muestra como 250 x 250)**  
La mediación de anuncios no establece ningún valor para el tamaño, por lo que debes cambiarlo con el parámetro opcional Tamaño. Por ejemplo:

```
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.AdDuplex][“Size”] = “160×600″;
```

**Recibes el error "Algo tapa el control de anuncios"**  
AdDuplex siempre mostrará un error si el anuncio queda de alguna forma tapado en la aplicación. [Lee la solución](http://blog.adduplex.com/2014/01/solving-something-is-covering-ad.mdl) a este error.

**Recibes el error "Se ha producido un conflicto entre dos archivos"**  
Se ha hecho referencia a los ensamblados de Microsoft Advertising en otro lugar de la aplicación. La mediación de anuncios está diseñada para funcionar exclusivamente en la aplicación y no funcionará si se usan otras referencias a los ensamblados de Microsoft Advertising. Quita manualmente las referencias de Microsoft Advertising y reinstala el SDK de Microsoft Store Engagement and Monetization para solucionar el error.

## Temas relacionados

* [Seleccionar y administrar redes de anuncios](select-and-manage-your-ad-networks.md)
* [Agregar y usar el control de mediación de anuncios](add-and-use-the-ad-mediator-control.md)
* [Probar la implementación de mediación de anuncios](test-your-ad-mediation-implementation.md)
* [Enviar la aplicación y configurar la mediación de anuncios](submit-your-app-and-configure-ad-mediation.md)
 

 


<!--HONumber=Mar16_HO5-->


