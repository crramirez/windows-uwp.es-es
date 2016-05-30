---
author: mcleanbyron
ms.assetid: 3C03FDD8-FA61-4E7B-BDCA-3C29DFEA20E4
description: Después de instalar el SDK de Microsoft Store Engagement and Monetization, sigue las instrucciones de este tema para usar el control de Ad Mediator en tu aplicación.
title: Agregar y usar el control de Ad Mediator
---

# Agregar y usar el control de Ad Mediator


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Después de [instalar el SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk), sigue las instrucciones de este tema para usar el control de Ad Mediator en tu aplicación. Para obtener una lista de las redes de anuncios y los tipos de proyecto que admite actualmente la mediación de anuncios, consulta [Seleccionar y administrar redes de anuncios](select-and-manage-your-ad-networks.md).

## Agregar el control de Ad Mediator al proyecto


Para agregar una instancia del control de Ad Mediator al proyecto:

1.  Abre el proyecto en Visual Studio.
2.  Si estás agregando mediación de anuncios a una aplicación que ya rentabiliza con cualquiera de las [redes de anuncios](select-and-manage-your-ad-networks.md) compatibles con la dicha mediación, quita la implementación de anuncios existente y todas sus referencias antes de continuar.
3.  En el **Explorador de soluciones**, busca la página de la aplicación donde deseas mostrar anuncios y, después, haz doble clic en la página para abrirla en el diseñador.
4.  Desde el **Cuadro de herramientas**, arrastra un nuevo **AdMediatorControl** al diseñador (asegúrate de arrastrar el control dentro del diseñador, no dentro del código XAML). Coloca el control en la ubicación donde quieres que se muestren los anuncios. Puedes agregar varios controles si quieres mostrar anuncios en más de un área de la aplicación.

    El **AdMediatorControl** se encuentra en las siguientes ubicaciones de **Cuadro de herramientas**:

    -   En un proyecto de la Plataforma universal de Windows (UWP), usa el **AdMediatorControl** de la sección **AdMediator Universal**.
    -   En un proyecto de Windows 8.1 o Windows Phone 8.1 con C# o Visual Basic con XAML, usa el control **AdMediatorControl** de la sección **AdMediator**.
    -   En un proyecto de Windows Phone Silverlight, usa el control **AdMediatorControl** de la sección **Todos los controles de Windows Phone**.

    > **Nota**: Cuando se arrastra el control **AdMediatorControl** al diseñador por primera vez en un proyecto de UWP, Windows 8.1 o Windows Phone 8.1 con C# o Visual Basic con XAML, Visual Studio agrega la referencia de ensamblado de Ad Mediator necesaria al proyecto, pero el control aún no se agrega al diseñador. Para agregarlo, haz clic en Aceptar en el mensaje que se muestra en Visual Studio, espera varios segundos a que se actualice el diseñador y luego arrastra de nuevo el control al diseñador. Si aún no puedes agregar correctamente el control al diseñador, asegúrate de que el proyecto se dirija a la arquitectura de procesador aplicable para la aplicación (por ejemplo, **x 86**) en lugar de **Cualquier CPU**. El control no se puede agregar al diseñador si el proyecto se dirige a **Cualquier CPU** para la plataforma de compilación.

5.  Visual Studio agrega una referencia de ensamblado de Ad Mediator a tu proyecto e inserta el código XAML del control de Ad Mediator en la página actual, incluido un identificador único y un nombre para el control. La referencia de ensamblado y el código XAML varían según la plataforma de destino. Por ejemplo, en el caso de una aplicación para Plataforma universal de Windows (UWP) de Windows 8.1, el nombre del ensamblado es **Microsoft.AdMediator.Universal** y el código XAML generado es similar al del siguiente ejemplo.

    ```xml
    // Code that gets added to the XAML page header
    xmlns:Universal="using:Microsoft.AdMediator.Universal"

    // Code that gets added for the ad mediator control
    <Universal:AdMediatorControl x:Name="AdMediator_3D4884"
      Grid.ColumnSpan="2" HorizontalAlignment="Left" Height="250"
      Id="AdMediator-Id-D1FDFDA7-EABB-474C-940C-ECA7FBCFF143" Margin="121,175,0,0"
      VerticalAlignment="Top" Width="300"/>
    ```

    El elemento **Name** te ayuda a identificar el control específico de la aplicación cuando configuras la mediación de anuncios. Puedes cambiar esta opción a gusto, pero asegúrate de no cambiar ni duplicar el elemento **Id**. Este elemento **Id** debe ser único para cada control dentro de la aplicación.

6.  Ajusta el tamaño y la posición del control según sea necesario. Para más información, consulta [Ajustar el tamaño y la posición](#adjust-size-and-position).

## Configurar las redes de anuncios

Cuando hayas agregado todos los controles que quieras, podrás configurar las redes de anuncios a través de Servicios conectados.

> **Importante**: Si, más adelante, agregas un control AdMediatorControl adicional, también tendrás que configurarlo a través de Servicios conectados. De lo contrario, el nuevo control no podrá usar la mediación de anuncios.

Para configurar las redes de anuncios:

1.  Haz clic con el botón secundario en el nombre del proyecto en el Explorador de soluciones, haz clic en **Agregar** y, a continuación, haz clic en **Servicio conectado…** para iniciar la ventana **Agregar servicio conectado** (Visual Studio 2015) o la ventana **Administrador de servicios** (Visual Studio 2013).
2.  Si usas Visual Studio 2015, haz clic en **Mediador de anuncios** y, a continuación, haz clic en **Configurar** para abrir la ventana del **Mediador de anuncios**. Si usas Visual Studio 2013, simplemente haz clic en **Mediador de anuncios** en el panel izquierdo del **Administrador de servicios**.

    Entonces se agrega al proyecto el archivo AdMediator.config. En este archivo es donde se guardan localmente en el proyecto las opciones de configuración iniciales de red de anuncios.

3.  En la ventana de **Ad Mediator** (Visual Studio 2015) o **Administrador de servicios** (Visual Studio 2013), haz clic en **Seleccionar redes de anuncios**, selecciona las redes de anuncios que quieras usar y haz clic en **Aceptar** en la ventana **Seleccionar redes de anuncios**.

    > **Sugerencia**: Es conveniente agregar todas las redes en las que tienes una cuenta, incluso aunque no tengas previsto usarlas todas inmediatamente en la aplicación. Después de publicar la aplicación, podrás configurar la frecuencia con la que se usa cada red en el Centro de desarrollo (o empezar a usar una red que no habías usado antes), sin tener que realizar cambios en el código y volver a enviar la aplicación.

    Visual Studio captura los ensamblados necesarios para las redes de anuncios seleccionadas y agrega las referencias de ensamblado al proyecto. Una vez completado este proceso, haz clic en **Aceptar** en el cuadro de diálogo **Capturando estado**.

4.  También puedes seleccionar cada red en la ventana **Mediador de anuncios** (Visual Studio 2015) o **Administrador de servicios** (Visual Studio 2013), y hacer clic en **Configurar** para especificar la información de configuración de cada red que se usará mientras pruebas la aplicación. Esta información se guarda en el archivo AdMediator.config del proyecto. Podrás modificar esta información al configurar el comportamiento de la red de anuncios en el panel del Centro de desarrollo de Windows. Para obtener más información, consulta [Enviar la aplicación y configurar la mediación de anuncios](submit-your-app-and-configure-ad-mediation.md).
    > **Nota**: Si no se especifican los datos de configuración en este paso, la mediación de anuncios usará automáticamente los valores de configuración de prueba al ejecutar la aplicación en el equipo de desarrollo (aplicaciones XAML para Windows 8.1 y UWP) o en el emulador o dispositivo (aplicaciones para Windows Phone).

5.  En la ventana **Ad Mediator** (Visual Studio 2015) o **Administrador de servicios** (Visual Studio 2013), confirma que todas las redes de anuncios que hayas seleccionado muestren el estado **Recuperado**. Haz clic en **Aceptar** para enviar los cambios al proyecto.

> **Nota**: Si, más adelante, actualizas a una versión más reciente del SDK de Microsoft Store Engagement and Monetization, tendrás que iniciar de nuevo **Servicios conectados** para asegurarte de que los archivos DLL de las redes de anuncios recuperados automáticamente se actualicen correctamente.

### Declarar las funcionalidades necesarias

Cada red de anuncios puede requerir ciertas funcionalidades de la aplicación. Estas funcionalidades se muestran para cada proveedor en el **Mediador de anuncios** (para Visual Studio de 2015) o en la ventana **Administrador de servicios** (para Visual Studio 2013). Asegúrate de declarar todas las funcionalidades necesarias en el manifiesto de la aplicación para que los anuncios se muestren correctamente.

En la siguiente captura de pantalla se muestran las funcionalidades necesarias de varias redes de anuncios en una aplicación XAML para Windows 8.1 o Windows Phone 8.1.

![administrador de servicios que muestra todas las referencias recuperadas](images/ad-med-8.jpg)

En la siguiente captura de pantalla se muestran las funcionalidades necesarias de varias redes de anuncios en una aplicación Silverlight para Windows Phone 8.1.

![administrador de servicios que muestra todas las referencias recuperadas](images/ad-med-6.jpg)
### Recuperar manualmente los archivos DLL de las redes de anuncios

En algunos casos, puedes ver que algunos archivos DLL no se han capturado. En este caso, deberás agregarlos manualmente. Para obtener vínculos para descargar ensamblados individuales, consulta [Seleccionar y administrar redes de anuncios](select-and-manage-your-ad-networks.md).

> **Nota**: Al agregar archivos DLL manualmente, es posible que recibas un mensaje de error que indique que "No se puede agregar al proyecto una referencia a una versión posterior o a un ensamblado que no es compatible". Para resolver este error, haz clic en el archivo DLL en el explorador y selecciona **Propiedades**. En la sección Seguridad, haz clic en **Desbloquear**.

![botón de desbloqueo para solucionar el mensaje de error](images/ad-med-4.png)
## Ajustar el tamaño y la posición

Puedes configurar el tamaño y la posición del control de Ad Mediator en el diseñador o en el código XAML. Asegúrate de que este tamaño sea suficientemente grande para ajustarse a todos los anuncios que se van mostrar desde las redes de anuncios. Algunas redes de anuncios pueden no servir un anuncio si detectan que el tamaño del lienzo no es lo suficientemente grande como para que el anuncio se muestre en su totalidad. Si alguno de los anuncios es mayor que el tamaño predeterminado, puedes ajustar el tamaño del lienzo para dar cabida a dicho anuncio.

Al arrastrar el control al diseñador, los tamaños de control predeterminados son los siguientes:

-   XAML en UWP y Windows 8.1: 300 de ancho por 250 de alto.
-   XAML en Windows Phone 8.1: 400 de ancho por 67 de alto.
-   Silverlight en Windows Phone 8 y Windows Phone 8.1: 480 de ancho por 80 de alto.

Puedes reemplazar los tamaños predeterminados de anuncios usando los parámetros opcionales de **Ancho** y **Alto** como se muestra a continuación.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Width"] = 400;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.Smaato]["Height"] = 80;
```

También puedes especificar cómo se coloca cada control para dar cabida a una variedad de tamaños y posiciones, según las necesidades de la aplicación. En algunas redes de anuncios, puedes usar parámetros opcionales para realizar ajustes. Por ejemplo, para alinear anuncios de Microsoft Advertising a la parte inferior izquierda:

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["HorizontalAlignment"] = HorizontalAlignment.Left;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["VerticalAlignment"] = VerticalAlignment.Bottom;
```

### Tamaños de anuncios admitidos por Microsoft Advertising

Microsoft Advertising solo admite los anuncios que tengan los siguientes tamaños estándar recomendados por Interactive Advertising Bureau (IAB) para las aplicaciones que se ejecutan en las siguientes plataformas.

-   Windows 10 y Windows 8.1:
    -   160 x 600
    -   300 x 250
    -   300 x 600
    -   728 x 90
-   Windows 10 Mobile, Windows Phone 8.1 y Windows Phone 8:
    -   300 x 50
    -   320 x 50
    -   480 x 80 (Este tamaño solo es compatible con Windows Phone Silverlight)
    -   640 x 100

Tal vez quieras especificar un tamaño de control de Ad Mediator que no coincida con los tamaños de anuncios que admite Microsoft Advertising (por ejemplo, si un tamaño diferente se adapta mejor a la interfaz de usuario de la aplicación o si también está dirigido a otras redes de anuncios que admiten otros tamaños). Para ello, especifica el tamaño exacto del control en el diseñador o en el código XAML y, a continuación, asigna los parámetros opcionales **Ancho** y **Alto** para Microsoft Advertising al tamaño compatible más parecido que quepa dentro de los límites del control. El control se mostrará en el tamaño exacto que especifiques en el diseñador, pero Microsoft Advertising servirá anuncios que coincidan con el tamaño que especifiques mediante los parámetros opcionales **Ancho** y **Alto**.

Por ejemplo, si tienes una aplicación para UWP y quieres que el control de Ad Mediator se muestre a 300 x 300, establece el control a 300 x 300 en el diseñador o en el código XAML. A continuación, asigna los parámetros opcionales para Microsoft Advertising **Ancho** a 300 y **Alto** a 250, como se muestra en el siguiente código.

```CSharp
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Width"] = 300;
myAdMediatorControl.AdSdkOptionalParameters[AdSdkNames.MicrosoftAdvertising]["Height"] = 250;
```

Para comprobar que la configuración de tamaño es compatible con Microsoft Advertising, [prueba la implementación de mediación de anuncios](test-your-ad-mediation-implementation.md) y asegúrate de que se muestran los anuncios de prueba de Microsoft Advertising.

## Pausar, reanudar y deshabilitar la mediación de anuncios

Si quieres pausar la mediación de anuncios un determinado período de tiempo durante el tiempo de ejecución de la aplicación, usa el método AdMediatorControl.Pause(). Ten en cuenta que al hacerlo, el anuncio más reciente se seguirá mostrando hasta que se reanude la mediación con una llamada al método AdMediatorControl.Resume().

Para deshabilitar completamente el mediador de publicidad, usa el método AdMediatorControl.Disable(). Se eliminarán los anuncios que se estén mostrando y se reducirá la superficie de memoria del mediador. Puedes llamar a AdMediatorControl.Resume() para reanudar la mediación, pero ten en cuenta que el tiempo de inicio será más lento de lo normal tras haber deshabilitado la mediación de publicidad.

## Establecer los tiempos de espera

Tienes la opción de especificar el número de segundos (de 2 a 60) que debe esperar la mediación de anuncios después de solicitar un anuncio de esa red de anuncios antes de abandonar la solicitud y realizar una solicitud a otra red en su lugar. De manera predeterminada, el tiempo de espera de todas las redes de anuncios es de 15 segundos.

En el siguiente código se muestra cómo especificar una duración de tiempo de espera para Microsoft Advertising. Puedes modificar las duraciones y las redes según sea necesario.

```CSharp
myAdMediatorControl.AdSdkTimeouts[AdSdkNames.MicrosoftAdvertising] = TimeSpan.FromSeconds(10);
```

> **Nota**: Como alternativa, se puede establecer el valor de tiempo de espera en la página **Rentabilizar con anuncios** del panel del Centro de desarrollo. Si estableces el tiempo de espera en el código y en el panel, el valor que establezcas en el código reemplazará el valor del panel.

## Control de eventos

Si agregas código para registrar eventos y capturar errores de la mediación de publicidad, dispondrás de una ayuda para la solución de problemas. El ejemplo de código siguiente agrega controladores de eventos para eventos concretos de un control.

```CSharp
// add this during initialization of your app

    AdMediator_Bottom.AdSdkError += AdMediator_Bottom_AdError;
    AdMediator_Bottom.AdMediatorFilled += AdMediator_Bottom_AdFilled;
    AdMediator_Bottom.AdMediatorError += AdMediator_Bottom_AdMediatorError;
    AdMediator_Bottom.AdSdkEvent += AdMediator_Bottom_AdSdkEvent;

// and then add these functions

void AdMediator_Bottom_AdSdkEvent(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdSdk event {0} by {1}", e.EventName, e.Name);}

void AdMediator_Bottom_AdMediatorError(object sender, Microsoft.AdMediator.Core.Events.AdMediatorFailedEventArgs e)
{
    Debug.WriteLine("AdMediatorError:" + e.Error + " " + e.ErrorCode );
    // if (e.ErrorCode == AdMediatorErrorCode.NoAdAvailable)
    // AdMediator will not show an ad for this mediation cycle
}

void AdMediator_Bottom_AdFilled(object sender, Microsoft.AdMediator.Core.Events.AdSdkEventArgs e)
{
    Debug.WriteLine("AdFilled:" + e.Name);
}

void AdMediator_Bottom_AdError(object sender, Microsoft.AdMediator.Core.Events.AdFailedEventArgs e)
{
    Debug.WriteLine("AdSdkError by {0} ErrorCode: {1} ErrorDescription: {2} Error: {3}", e.Name, e.ErrorCode, e.ErrorDescription, e.Error);
}
```

## Controlar las excepciones no controladas de las redes de anuncios

> **Nota**: Como parte de nuestras pruebas, hemos identificado varias excepciones no controladas de redes de anuncios concretas que deben controlarse dentro de la aplicación para evitar bloqueos de la aplicación relacionados con estas excepciones. Es muy recomendable que copies y pegues el siguiente ejemplo de código en el archivo App.xaml.cs.

Código que puedes usar con una aplicación para UWP, Windows 8.1 o Windows Phone que use C# y XAML

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += App_UnhandledException;

void App_UnhandledException(object sender, UnhandledExceptionEventArgs e)
   {
      if (e != null)
      {
         Exception exception = e.Exception;
         if (exception is NullReferenceException && exception.ToString().ToUpper().Contains("SOMA"))
         {
            Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
            e.Handled = true;
            return;
         }
      }
// APP SPECIFIC HANDLING HERE

   if (Debugger.IsAttached)
      {
         // An unhandled exception has occurred; break into the debugger
         Debugger.Break();
      }
   }
```

Código que puedes usar con una aplicación para Windows Phone que use Silverlight

```CSharp
// In App.xaml.cs file, register with the UnhandledException event handler.
UnhandledException += Application_UnhandledException;

// Code to execute on unhandled exceptions
private void Application_UnhandledException(object sender, ApplicationUnhandledExceptionEventArgs e)
{
    if (e != null)
   {
       Exception exception = e.ExceptionObject;
       if ((exception is XmlException || exception is NullReferenceException) && exception.ToString().ToUpper().Contains("INNERACTIVE"))
       {
           Debug.WriteLine("Handled Inneractive exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if (exception is NullReferenceException && exception.ToString().ToUpper().Contains("SOMA"))
       {
           Debug.WriteLine("Handled Smaato null reference exception {0}", exception);
           e.Handled = true;
           return;
       }
       else if ((exception is System.IO.IOException || exception is NullReferenceException) && exception.ToString().ToUpper().Contains("GOOGLE"))
      {
          Debug.WriteLine("Handled Google exception {0}", exception);
          e.Handled = true;
          return;
       }
       else if ((exception is NullReferenceException || exception is XamlParseException) && exception.ToString().ToUpper().Contains("MICROSOFT.ADVERTISING"))
       {
           Debug.WriteLine("Handled Microsoft.Advertising exception {0}", exception);
           e.Handled = true;
           return;
       }

   }
// APP SPECIFIC HANDLING HERE

if (Debugger.IsAttached)
   {
       // An unhandled exception has occurred; break into the debugger
       Debugger.Break();
   }
   //e.Handled = true;
}
```

## Temas relacionados

* [Seleccionar y administrar redes de anuncios](select-and-manage-your-ad-networks.md)
* [Probar la implementación de mediación de anuncios](test-your-ad-mediation-implementation.md)
* [Enviar la aplicación y configurar la mediación de anuncios](submit-your-app-and-configure-ad-mediation.md)
* [Solucionar problemas de mediación de anuncios](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=May16_HO2-->


