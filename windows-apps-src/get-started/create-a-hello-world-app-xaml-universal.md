---
author: GrantMeStrength
ms.assetid: 03A74239-D4B6-4E41-B2FA-6C04F225B844
title: Aprender a crear una aplicación "Hola mundo" (XAML)
description: Usa el lenguaje de marcado de aplicaciones extensible (XAML) con C# para crear una sencilla aplicación Hola mundo para la Plataforma universal de Windows (UWP) en Windows 10.
ms.author: jken
ms.date: 03/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, primera aplicación, hola mundo
ms.localizationpriority: medium
ms.openlocfilehash: 950b2f3fac44c8350a51fd5c1b7071f05c92d746
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "2789673"
---
# <a name="create-a-hello-world-app-xaml"></a>Crear una aplicación "Hello, world" (XAML)

Este tutorial te enseña a usar XAML y C# para crear una aplicación sencilla "Hello, world" para la Plataforma universal de Windows (UWP) en Windows 10. Con un único proyecto en Microsoft Visual Studio, puedes compilar una aplicación que se ejecute en cualquier dispositivo de Windows 10.

Aquí aprenderás a:

-   Creación de un nuevo proyecto de **Visual Studio 2017** diseñado para **Windows 10** y **UWP**.
-   Escribir XAML para cambiar la interfaz de usuario en la página de inicio.
-   Ejecutar el proyecto en el escritorio local de Visual Studio.
-   Usar un SpeechSynthesizer para que la aplicación hable al presionar un botón.


## <a name="before-you-start"></a>Antes de comenzar...

-   [¿Qué es una aplicación universal de Windows?](universal-application-platform-guide.md)
-   [Descarga Visual Studio 2017 (y Windows 10)](https://developer.microsoft.com/windows/downloads). Si necesitas un poco de ayuda, aprende a [prepararte](get-set-up.md).
-   También se supone que estás usando el diseño de ventana predeterminado de Visual Studio. Si cambias el diseño predeterminado, puedes restablecerlo en el menú **Ventana** con el comando **Restablecer diseño de la ventana**.

> [!NOTE]
> En este tutorial se usa Visual Studio Community 2017. Si usas otra versión de Visual Studio, es posible que tenga una apariencia un poco diferente.

## <a name="video-summary"></a>Vídeo resumen

<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Writing-Your-First-Windows-10-App/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>



## <a name="step-1-create-a-new-project-in-visual-studio"></a>Paso 1: Creación de un nuevo proyecto en Visual Studio.

1.  Inicia Visual Studio 2017.

2.  En el menú **archivo** , seleccione **Nuevo > proyecto** para abrir el cuadro de diálogo *Nuevo proyecto* .

3.  En la lista de plantillas de la izquierda, elija **Installed > Visual C# > Windows Universal** para ver la lista de plantillas de proyecto UWP.

    (Si no ves ninguna plantilla Universal, es posible que falten los componentes para crear aplicaciones para UWP. Puedes repetir el proceso de instalación y agregar compatibilidad con UWP haciendo clic en **Abrir el instalador de Visual Studio** en el diálogo *Nuevo proyecto*. Vea [Configurar la aplicación](get-set-up.md)).

    ![Cómo repetir el proceso de instalación](images/win10-cs-install.png)

4.  Elige la plantilla **Aplicación vacía (Windows Universal)** y escribe "HelloWorld" como **Nombre**. Selecciona **Aceptar**.

    ![La ventana Nuevo proyecto](images/win10-cs-01.png)

> [!NOTE]
> Si es la primera vez que usas Visual Studio, es posible que se te solicite habilitar el **Modo de desarrollador** en un diálogo de configuración. El modo de desarrollador es una opción de configuración especial que habilita determinadas funciones, como permiso para ejecutar aplicaciones directamente, en lugar de solo desde la Tienda. Para obtener más información, lee [Habilitar el dispositivo para el desarrollo](enable-your-device-for-development.md). Para continuar con esta guía, selecciona **Modo de desarrollador**, haz clic en **Sí** y cierra el diálogo.

 ![Diálogo para activar el modo de desarrollador](images/win10-cs-00.png)

5.  Se visualiza el cuadro de diálogo de versión mínima/de destino. La configuración predeterminada es correcta para este tutorial, por lo tanto, selecciona **Aceptar** para crear el proyecto.

    ![La ventana de Explorador de soluciones](images/win10-cs-02.png)

6.  Cuando se abra el nuevo proyecto, sus archivos se muestran en el panel **Explorador de soluciones** de la derecha. Es posible que debas elegir la pestaña **Explorador de soluciones** en lugar de la pestaña **Propiedades** para ver los archivos.

    ![La ventana de Explorador de soluciones](images/win10-cs-03.png)

Aunque **Aplicación vacía (Universal Window)** es una plantilla mínima, contiene muchos archivos. Estos archivos son esenciales para todas las aplicaciones para UWP que usan C#. Todos los proyectos que crees en Visual Studio contendrán estos archivos.


### <a name="whats-in-the-files"></a>¿Qué hay en los archivos?

Para ver y editar un archivo de tu proyecto, haz doble clic en el archivo en el **Explorador de soluciones**. Expande un archivo XAML como una carpeta para ver su archivo de código asociado. Los archivos XAML se abren en una vista en dos paneles que muestra la superficie de diseño y el editor de XAML.
> [!NOTE]
> ¿Qué es XAML? El lenguaje de marcado de aplicaciones extensible (XAML) es el lenguaje que se usa para definir la interfaz de usuario de la aplicación. Puede especificarse manualmente, o crearse con las herramientas de diseño de Visual Studio. Un archivo .xaml tiene un archivo de código subyacente .xaml.cs que contiene la lógica. Juntos, el XAML y el código subyacente forman una clase completa. Para obtener más información, consulta [Introducción a XAML](https://msdn.microsoft.com/library/windows/apps/Mt185595).

*App.xaml y App.xaml.cs*

-   App.xaml es donde declaras recursos que se usan en la aplicación.
-   App.xaml.cs es el archivo de código subyacente de App.xaml. Como todas las páginas de código subyacente, contiene un constructor que llama al método `InitializeComponent`. Tú no escribes el método `InitializeComponent`. Lo genera Visual Studio, y su principal propósito es inicializar los elementos declarados en el archivo XAML.
-   App.xaml.cs es el punto de entrada para la aplicación.
-   App.xaml.cs también contiene métodos para controlar la activación y la suspensión de la aplicación.

*MainPage.xaml*

-   MainPage.xaml es donde defines la interfaz de usuario para la aplicación. Puedes agregar elementos directamente con el marcado XAML o puedes usar las herramientas de diseño suministradas por Visual Studio.
-   MainPage.xaml.cs es la página de código subyacente de MainPage.xaml. Es el sitio donde agregas los controladores de eventos y la lógica de la aplicación.
-   Estos dos archivos juntos definen una nueva clase denominada `MainPage`, que se hereda de [**Página**](https://msdn.microsoft.com/library/windows/apps/BR227503), en el espacio de nombres `HelloWorld`.

*Package.appxmanifest*
-   Un archivo de manifiesto que describe tu aplicación: su nombre, descripción, icono, página de inicio, etc.
-   Incluye una lista de los archivos que contiene la aplicación.

*Un conjunto de imágenes de logotipo*
-   Assets/Square150x150Logo.scale-200.png representa tu aplicación en el menú Inicio.
-   Assets/StoreLogo.png representa tu aplicación en Microsoft Store.
-   Assets/SplashScreen.scale-200.png es la pantalla de presentación que se muestra cuando se inicia la aplicación.

## <a name="step-2-adding-a-button"></a>Paso 2: Adición de un botón

### <a name="using-the-designer-view"></a>Uso de la vista de diseñador

Vamos a agregar un botón a nuestra página. En este tutorial, trabajas con solo algunos de los archivos enumerados anteriormente: App.xaml, MainPage.xaml y MainPage.xaml.cs.

1.  Haz doble clic en **MainPage.xaml** para abrirlo en la vista de Diseño.

    Verás que hay una vista gráfica en la parte superior de la pantalla y la vista del código XAML debajo. Puedes realizar cambios en cualquiera de los dos, pero por ahora vamos a utilizar la vista gráfica.

    ![La ventana de Explorador de soluciones](images/win10-cs-04.png)

2.  Haz clic en la pestaña vertical **Herramientas** a la izquierda para abrir la lista de controles de interfaz de usuario. (Puedes hacer clic en el icono de anclaje en la barra de título para mantenerlo visible.)

    ![La ventana de Explorador de soluciones](images/win10-cs-05.png)

3.  Expande **Controles de XAML comunes**y arrastra el **Botón** hacia el centro del lienzo de diseño.

    ![La ventana de Explorador de soluciones](images/win10-cs-06.png)

    Si buscas en la ventana de código XAML, verás que el botón se ha agregado allí también:

 ```XAML
<Button x:name="button" Content="Button" HorizontalAlignment="Left" Margin = "152,293,0,0" VerticalAlignment="Top"/>
 ```

4.  Cambia el texto del botón.

    Haz clic en la vista del código XAML y cambia el contenido del "Botón" a "Hello, world!".

```XAML
<Button x:name="button" Content="Hello, world!" HorizontalAlignment="Left" Margin = "152,293,0,0" VerticalAlignment="Top"/>
```

Observa cómo se muestra el botón en las actualizaciones del lienzo de diseño para mostrar el texto nuevo.

![La ventana de Explorador de soluciones](images/win10-cs-07.png)

## <a name="step-3-start-the-app"></a>Paso 3: Inicio de la aplicación


En este punto, has creado una aplicación muy sencilla. Este es un buen momento para compilar, implementar e iniciar tu aplicación para ver su aspecto. Puedes depurar la aplicación en el equipo local, en un simulador, un emulador o en un dispositivo remoto. Este es el menú del dispositivo de destino de Visual Studio.

![Lista desplegable de destinos de dispositivo para depurar la aplicación](images/uap-debug.png)

### <a name="start-the-app-on-a-desktop-device"></a>Iniciar la aplicación en un dispositivo de escritorio

De forma predeterminada, la aplicación se ejecuta en el equipo local. El menú del dispositivo de destino proporciona varias opciones para depurar la aplicación en dispositivos de la familia de dispositivos de escritorio.

-   **Simulador**
-   **Equipo local**
-   **Equipo remoto**

**Para empezar la depuración en el equipo local**

1.  En el menú del dispositivo de destino (![menú Iniciar depuración](images/startdebug-full.png)) de la barra de herramientas **Estándar**, asegúrate de que **Equipo local** esté seleccionado. (Es la selección predeterminada).
2.  Haz clic en el botón **Iniciar depuración** (![botón Iniciar depuración](images/startdebug-sm.png)) en la barra de herramientas.

   O bien

   En el menú **Depurar**, haz clic en **Iniciar depuración**.

   O bien

   Presiona F5.

La aplicación se abre en una ventana y, en primer lugar, aparece una pantalla de presentación predeterminada. Esta pantalla se define mediante una imagen (SplashScreen.png) y un color de fondo (especificado en el archivo de manifiesto de la aplicación).

La pantalla de presentación desaparece y, a continuación, aparece tu aplicación. Tiene esta apariencia.

![Pantalla inicial de la aplicación](images/win10-cs-08.png)

Presiona la tecla Windows para abrir el menú **Inicio** y mostrar todas las aplicaciones. Ten en cuenta que al implementar la aplicación localmente se agrega su icono al menú **Inicio**. Para ejecutar la aplicación de nuevo más adelante (no en modo de depuración), pulsa o haz clic en su icono en el menú **Inicio**.

No hace muchas cosas (todavía), pero te felicitamos, has compilado tu primera aplicación para UWP.

**Para detener la depuración**

   Haz clic en el botón **Detener depuración** (![botón Detener depuración](images/stopdebug.png)) en la barra de herramientas.

   O bien

   En el menú **Depurar**, haz clic en **Detener depuración**.

   O bien

   Cierra la ventana de la aplicación.

## <a name="step-4-event-handlers"></a>Paso 4: Controladores de eventos

Un "controlador de eventos" suena complicado, pero es simplemente otra forma de designar el código que se llama cuando se produce un evento (por ejemplo, cuando el usuario hace clic en tu botón).

1.  Para el funcionamiento de la aplicación, si no lo has hecho ya.

2.  Haz doble clic en el control de botón en el lienzo de diseño para hacer que Visual Studio cree un controlador de eventos para el botón.

  Por supuesto, también puedes crear todo el código manualmente. O puedes hacer clic en el botón para seleccionarlo y buscar en el panel **Propiedades** en la esquina inferior derecha. Si cambias a **Eventos** (el pequeño relámpago) puedes agregar el nombre del controlador de eventos.

3.  Edita el código del controlador de eventos en *MainPage.xaml.cs*, la página de código subyacente. Aquí es cuando se pone interesante. El controlador de eventos predeterminado tiene este aspecto:

```C#
private void Button_Click(object sender, RoutedEventArgs e)
{

}
```

  Vamos a cambiarlo para que tenga esta apariencia:

```C#
private async void Button_Click(object sender, RoutedEventArgs e)
        {
            MediaElement mediaElement = new MediaElement();
            var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
            Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream = await synth.SynthesizeTextToStreamAsync("Hello, World!");
            mediaElement.SetSource(stream, stream.ContentType);
            mediaElement.Play();
        }
```

Asegúrate de incluir también la palabra clave **async**, o se producirá un error cuando se intente ejecutar la aplicación.

### <a name="what-did-we-just-do"></a>¿Qué acabamos de hacer?

Este código usa algunas API de Windows para crear un objeto de síntesis de voz y luego le proporciona algo de texto para que lo lea. (Para obtener más información sobre el uso de SpeechSynthesis, consulta los documentos sobre el [Espacio de nombres SpeechSynthesis](https://msdn.microsoft.com/library/windows/apps/windows.media.speechsynthesis.aspx).)

Cuando ejecutes la aplicación y hagas clic en el botón, tu equipo (o teléfono) literalmente dirá "¡Hello, World!".


## <a name="summary"></a>Resumen

Enhorabuena, has creado tu primera aplicación para Windows10 y la UWP.

Para aprender a usar XAML para diseñar los controles que usará la aplicación, prueba el [tutorial sobre la cuadrícula](../design/layout/grid-tutorial.md), o salta directamente a los [pasos siguientes](learn-more.md).

## <a name="see-also"></a>Consulta también

* [Crear tu primera aplicación](your-first-app.md)
* [Publicación de tu aplicación para UWP](https://developer.microsoft.com/store/publish-apps).
* [Desarrollo de aplicaciones para UWP](https://developer.microsoft.com/windows/apps/develop)
* [Muestras de código para desarrolladores de UWP](https://developer.microsoft.com/windows/samples)
* [¿Qué es una aplicación universal de Windows?](universal-application-platform-guide.md)
* [Registrarse para obtener una cuenta de Windows](sign-up.md)
