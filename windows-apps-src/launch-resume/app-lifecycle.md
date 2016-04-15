---
title: Ciclo de vida de la aplicación
description: En este tema se describe el ciclo de vida de una aplicación para la Plataforma universal de Windows (UWP), desde el momento en que se activa hasta que se cierra.
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
---

# Ciclo de vida de la aplicación


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**Clase Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)
-   [**Espacio de nombres Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)

En este tema se describe el ciclo de vida de una aplicación para la Plataforma universal de Windows (UWP), desde el momento en que se activa hasta que se cierra. Muchos usuarios usan múltiples dispositivos y aplicaciones mientras trabajan. Hoy en día, los usuarios esperan que las aplicaciones recuerden sus estados al realizar múltiples tareas con sus dispositivos. Por ejemplo, los usuarios esperan que la página se encuentre en la misma posición en que la dejaron y que todos los controles se encuentren en el mismo estado que antes. Al comprender el ciclo de vida de inicio, suspensión y reanudación de la aplicación, podrás ofrecer este tipo de comportamiento sin problema alguno para el usuario.

## Estado de ejecución de la aplicación


Esta ilustración representa las transiciones entre los estados de ejecución de la aplicación. En las siguientes secciones se describen estos estados y eventos. Para más información sobre cada transición de estado y cómo debe reaccionar la aplicación, consulta la referencia relativa a la enumeración [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694).

![Diagrama de estados en el que se muestran las transiciones entre los estados de ejecución de la aplicación](images/state-diagram.png)

## Implementación


Para poder activar una aplicación, primero se debe implementar. La aplicación se implementa cuando un usuario instala la aplicación o cuando se usa Visual Studio para compilar y ejecutar la aplicación durante el desarrollo y las pruebas. Para más información sobre este tema y los escenarios de implementación avanzada, consulta [Implementación y paquetes de la aplicación](https://msdn.microsoft.com/library/windows/apps/hh464929).

## Inicio de la aplicación


Una aplicación se inicia cuando se encuentra en estado **NotRunning** y el usuario pulsa el icono de la aplicación en la pantalla Inicio o en la lista de aplicaciones. Las aplicaciones que se usan con frecuencia también se pueden iniciar previamente para optimizar la capacidad de respuesta (consulta [Controlar el inicio previo de aplicaciones](handle-app-prelaunch.md)). Una aplicación puede estar en el estado **NotRunning** porque no se ha iniciado nunca, porque se estaba ejecutando y se bloqueó, o porque se suspendió pero no se pudo conservar en la memoria y el sistema la finalizó. El inicio de la aplicación es distinto de la activación. La activación es cuando la aplicación se activa mediante un contrato o una extensión como, por ejemplo, el contrato de Buscar.

El método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) se llama cuando se inicia una aplicación, por ejemplo, cuando la aplicación está actualmente en suspensión en la memoria. El parámetro [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) contiene el estado anterior de la aplicación y los argumentos de activación.

Cuando el usuario cambia a la aplicación finalizada, el sistema envía el argumento [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) con el objeto [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) establecido en **Launch** y el objeto [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) establecido en **Terminated** o **ClosedByUser**. La aplicación debe cargar sus datos de aplicación guardados y actualizar el contenido que muestra.

Si el valor del objeto [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) es **NotRunning**, la aplicación debe iniciarse desde cero como si se fuera la primera vez que se iniciara.

Cuando se inicia una aplicación, Windows muestra una pantalla de presentación. Para configurar dicha pantalla de presentación, consulta [Agregar una pantalla de presentación](https://msdn.microsoft.com/library/windows/apps/xaml/hh465331).

Mientras se muestra la pantalla de presentación, la aplicación deberá preparar la interfaz de usuario. Las tareas principales de una aplicación son registrar controladores de eventos y configurar las opciones de interfaz de usuario personalizadas necesarias para cargar la página inicial. Estas tareas solo deberían tardar unos pocos segundos. Otras actividades que la aplicación necesite, como pedir datos de la red o recuperar grandes cantidades de datos del disco, se deberán llevar a cabo aparte de la activación. Una aplicación puede usar su propia interfaz de usuario de carga personalizada o una pantalla de presentación ampliada mientras espera a que finalicen estas operaciones cuya ejecución requiere mucho tiempo. Consulta [Mostrar una pantalla de presentación durante más tiempo](create-a-customized-splash-screen.md) y la [muestra de pantalla de presentación](http://go.microsoft.com/fwlink/p/?linkid=234889) para obtener más información. Cuando la aplicación completa la activación, entra en el estado **Running** y desaparece la pantalla de presentación (se desactivan todos sus recursos y objetos).

## Activación de la aplicación


Los usuarios pueden activar una aplicación mediante varias extensiones y contratos como, por ejemplo, el contrato de Buscar. Para obtener una lista con las distintas formas en que se puede activar una aplicación, consulta [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693).

La clase [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) define los métodos que puedes invalidar para controlar los distintos tipos de activación. Varios tipos de activación tienen un método específico que puedes invalidar como, por ejemplo, [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331), [**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336), etc. Para los demás tipos de activación, invalida el método [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330).

El código de activación de la aplicación puede comprobar por qué se activó y si ya se encontraba en estado **Running**.

La aplicación puede restaurar los datos guardados anteriormente durante la activación en caso de que el sistema operativo haya finalizado la aplicación y el usuario la haya vuelto vuelva a iniciar. Windows puede finalizar la aplicación después de que se haya suspendido por diversos motivos. Puede que el usuario la haya cerrado manualmente o haya cerrado la sesión, o bien que el sistema no disponga de suficientes recursos. En caso de que el usuario inicie la aplicación después de que Windows la haya finalizado, la aplicación recibirá la devolución de llamada [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) y se mostrará al usuario la pantalla de presentación hasta que esta se active. Puedes usar este evento para determinar si la aplicación necesita restaurar los datos que guardados cuando se suspendió por última vez, o bien si se deben cargar los datos predeterminados de la aplicación. Dado que la pantalla de presentación se encuentra visible, el código de la aplicación puede tardar algún tiempo en procesar esta tarea sin necesidad de que el usuario experimente ningún retraso aparente; aunque las cuestiones mencionadas anteriormente acerca de las operaciones de larga ejecución también se aplican si se reinicia o se reanuda la aplicación.

El evento [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) incluye una propiedad [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) que indica el estado en el que se encontraba la aplicación antes de su activación. Esta propiedad es uno de los valores de la enumeración [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694):

| Motivo de finalización                                                        | Valor de la propiedad [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) | Medida necesaria          |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------|
| Finalizada por el sistema (por ejemplo, debido a restricciones de recursos).       | **Terminated**                                                                                          | Restaurar los datos del sistema    |
| Cerrado por el usuario o proceso finalizado por el usuario.                             | **ClosedByUser**                                                                                        | Iniciar la aplicación con los datos predeterminados |
| La aplicación finalizó de forma inesperada o no se ejecutó durante la *sesión del usuario actual*. | **NotRunning**                                                                                          | Iniciar la aplicación con los datos predeterminados |

 

**Nota**  La *sesión del usuario actual* se basa en el inicio de sesión de Windows. Siempre y cuando el usuario actual no haya cerrado la sesión explícitamente o apagado el equipo, o bien Windows no se haya reiniciado por otros motivos, la sesión del usuario actual persiste durante los eventos como la autenticación de pantalla de bloqueo y el cambio de usuario, entre otros.

 

[**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) también puede tener los valores **Running** o **Suspended**, aunque en estos casos la aplicación no ha finalizado previamente, por lo que no será necesario restaurar ningún dato, ya que todo está en la memoria.

**Nota**  

Si inicias sesión en el equipo con la cuenta de Administrador, no podrás activar ninguna de las aplicaciones para UWP.

Para más información, consulta [Contratos y extensiones de aplicaciones (aplicaciones de la Tienda Windows)](https://msdn.microsoft.com/library/windows/apps/hh464906).

### **OnActivated** frente a las activaciones específicas

El método [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) permite controlar todos los tipos de activación posibles. Sin embargo, es más habitual usar distintos métodos para controlar los tipos de activación más comunes y usar **OnActivated** solo como método de reserva para los tipos de activación menos comunes. Por ejemplo, el objeto [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) tiene un método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) que se invoca como una devolución de llamada siempre que el objeto [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693) sea **Launch**, y esta es la activación típica de la mayoría de las aplicaciones. Hay más de 6 métodos **On\*** para las activaciones específicas: [**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/hh701797), [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331), [**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701799), [**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701801), [**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336), [**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/hh701806). Las plantillas de inicio de las aplicaciones XAML tienen una implementación para **OnLaunched** y un controlador para [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341).

## Suspensión de aplicaciones


El sistema suspende la aplicación cuando el usuario cambia a otra aplicación, al escritorio o a la pantalla Inicio. La aplicación también puede suspenderse cuando el dispositivo entra en estado de bajo consumo. El sistema reanuda la aplicación cuando el usuario vuelve a cambiar a ella. Cuando el sistema reanuda la aplicación, el contenido de las variables y las estructuras de datos es el mismo que antes de que el sistema la suspendiera. El sistema restaura la aplicación en el punto exacto en el que estaba, para que parezca al usuario que se estaba ejecutando en segundo plano.

Cuando el usuario pasa una aplicación al segundo plano, Windows espera unos segundos para ver si el usuario vuelve inmediatamente a la aplicación. De ser así, la transición se realizará rápidamente. Si el usuario no vuelve durante este intervalo de tiempo, Windows suspende la aplicación.

Si necesitas realizar algún trabajo asincrónico durante la suspensión de la aplicación, deberás aplazar la suspensión hasta que haya finalizado ese trabajo. Puedes usar el método [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) en el objeto [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) (disponible a través de los argumentos de evento) para retrasar la suspensión hasta después de haber llamado al método [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) en el objeto [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) devuelto.

El sistema intentará mantener la aplicación y sus datos en memoria mientras está suspendida. No obstante, si el sistema no tiene los recursos necesarios para mantener la aplicación en memoria, finalizará su ejecución. Las aplicaciones no reciben notificaciones cuando se cierran, por lo que la única oportunidad de guardar los datos de la aplicación es durante la suspensión. Cuando una aplicación determina que se ha activado tras cerrarse, debe cargar los datos de aplicación que se guardaron durante la suspensión para que la aplicación se encuentre en el mismo estado que antes de suspenderse. Cuando el usuario vuelve a una aplicación suspendida que finalizó, la aplicación restaurará los datos de la aplicación en el método [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335). El sistema no notifica a una aplicación cuando finaliza su ejecución, con lo cual la aplicación deberá guardar sus datos de aplicación y liberar los recursos exclusivos y los identificadores de archivos cuando se suspenda, y restaurarlos cuando vuelva a activarse.

Si una aplicación registró un controlador de eventos para el evento [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341), se llamará a este código de inmediato antes de que se suspenda la aplicación. Puedes usar el controlador de eventos para guardar datos de aplicación y de usuario. Te recomendamos que uses las API de datos de aplicación para este fin, ya que así te aseguras de que completen el trabajo antes de que la aplicación entre en estado **Suspended**. Para más información, consulta [Almacenar y recuperar la configuración y otros datos de aplicación](https://msdn.microsoft.com/library/windows/apps/mt299098).

Conviene que liberes los recursos exclusivos y los identificadores de archivos para que otras aplicaciones puedan tener acceso a ellos cuando la aplicación no los esté usando. Algunos ejemplos de recursos exclusivos son las cámaras, los dispositivos de E/S, los dispositivos externos y los recursos de red. Liberar recursos exclusivos e identificadores de archivos sirve para que otras aplicaciones puedan tener acceso a ellos cuando la aplicación no los esté usando. Cuando la aplicación se active después de haberla finalizado, deberá volver a abrir sus recursos exclusivos e identificadores de archivos.

En general, la aplicación debe guardar su estado y liberar sus recursos e identificadores de archivos inmediatamente cuando se controle el evento de suspensión, y el código no debería tardar más de un segundo en completarse. Si una aplicación no regresa del evento de suspensión en unos segundos, Windows da por sentado que la aplicación dejó de responder y la finaliza.

Hay algunos escenarios de aplicación donde la aplicación debe seguir ejecutándose para completar tareas en segundo plano. Por ejemplo, la aplicación puede seguir reproduciendo audio en segundo plano; para más información, consulta [Audio en segundo plano](https://msdn.microsoft.com/library/windows/apps/mt282140)). Además, las operaciones de transferencia en segundo plano continúan incluso cuando la aplicación se haya suspendido o finalizado; para más información, consulta [Cómo descargar un archivo](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/jj152726.aspx#downloading_a_file_using_background_transfer)).

Para obtener instrucciones, consulta [Directrices para suspender y reanudar una aplicación](https://msdn.microsoft.com/library/windows/apps/hh465088).

**Una nota sobre la depuración con Visual Studio: ** Visual Studio impide que Windows suspenda una aplicación que está conectada al depurador. Esto permite que el usuario vea la interfaz de usuario de depuración de Visual Studio mientras se ejecuta la aplicación. Mientras depuras una aplicación, puedes enviarle un evento de suspensión mediante Visual Studio. Asegúrate de que se muestra la barra de herramientas **Ubicación de depuración** y luego haz clic en el botón **Suspender**.

## Visibilidad de la aplicación


Tu aplicación dejará de estar visible cuando el usuario cambie a otra aplicación, aunque permanecerá en estado **Running** hasta que Windows la suspenda. Si el usuario cambia a otra aplicación pero vuelve a la tuya y la activa antes de que se pueda suspender, permanecerá en estado **Running**.

La aplicación no recibe un evento de activación cuando su visibilidad cambia, puesto que sigue en ejecución. Windows cambia entre esta y otras aplicaciones según sea necesario. Si la aplicación tiene que realizar alguna acción cuando un usuario cambia de aplicación y vuelve, puede controlar el evento [**Window.VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458).

No confíes en un orden específico de estos eventos.

## Reanudación de la aplicación


Una aplicación suspendida se reanuda cuando el usuario vuelve a ella o cuando es la aplicación activa cuando el dispositivo sale de un estado de bajo consumo.

Consulta [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) para ver los estados en los que la aplicación puede estar cuando se reanuda. Cuando una aplicación se reanuda desde el estado **Suspended**, entra en el estado **Running** y continúa desde donde estaba en el momento de la suspensión. No se pierde ningún dato de la aplicación almacenado en la memoria. Por ello, con la mayor parte de las aplicaciones no es necesario hacer nada cuando se reanudan. No obstante, la aplicación podría haber estado suspendida durante horas o incluso días. Por lo tanto, si la aplicación tiene contenido o conexiones de red que puedan haber quedado obsoletos, se deberán actualizar al reanudar. Si una aplicación registró un controlador de eventos para el evento [**Application.Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339), se llama a este controlador de eventos cuando la aplicación se reanude desde el estado **Suspended**. Puedes actualizar el contenido y los datos de la aplicación con este controlador de eventos.

Si se activa una aplicación suspendida para que participe en un contrato entre aplicaciones o una extensión, recibirá primero el evento **Resuming** y después el evento **Activated**.

Mientras la aplicación está suspendida, no recibirá ninguno de los eventos de red que registró para recibir. Dichos eventos no se colocarán en la cola; simplemente se perderán. Por ello, la aplicación debe comprobar el estado de red cuando se reanude.

**Nota**  Dado que el evento [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) no se genera a partir del subproceso de interfaz de usuario, es necesario usar un distribuidor si el código del controlador de reanudación se comunica con la interfaz de usuario.

 

Para obtener instrucciones, consulta el tema de [Directrices para suspender y reanudar una aplicación](https://msdn.microsoft.com/library/windows/apps/hh465088).

## Cierre de la aplicación


Por lo general, no es necesario que los usuarios cierren las aplicaciones, sino que pueden dejar que Windows se encargue de ello. No obstante, los usuarios pueden decidir cerrar una aplicación mediante el gesto de cerrar, presionando Alt y F4 en Windows o mediante el conmutador de tareas en Windows Phone.

No hay ningún evento especial que indique que el usuario ha cerrado una aplicación.

Después de que el usuario haya cerrado una aplicación, primero se suspende y luego se finaliza. A continuación, pasa al estado **NotRunning**.

En Windows 8.1 y en versiones posteriores, una vez que el usuario cierra la aplicación, esta se quita de la pantalla y de la lista de cambio, pero no finaliza explícitamente.

Si una aplicación registró un controlador de eventos para el evento **Suspending**, se llamará a este controlador cuando se suspenda la aplicación. Puedes usar este controlador de eventos para guardar datos relevantes de la aplicación y del usuario en un almacenamiento persistente.

**Comportamiento de cierre por parte del usuario: **si la aplicación debe hacer algo distinto cuando la cierra el usuario que cuando la cierra Windows, puedes usar el controlador de eventos de activación para determinar si la finalizó Windows o el usuario. Consulta las descripciones de los estados **ClosedByUser** y **Terminated** en la referencia relativa a la enumeración [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694).

Te recomendamos que las aplicaciones no se cierren automáticamente mediante programación a menos que sea absolutamente necesario. Por ejemplo, si una aplicación detecta una pérdida de memoria, se puede cerrar para preservar la seguridad de los datos personales del usuario. Cuando una aplicación se cierra mediante programación, el sistema considera que se ha bloqueado.

## Bloqueo de la aplicación


La experiencia de bloqueo del sistema está diseñada para que los usuarios puedan volver a lo que estaban haciendo lo antes posible. No se debe proporcionar ningún cuadro de diálogo de advertencia o notificación, ya que retrasaría más al usuario.

Si la aplicación se bloquea, deja de responder o genera una excepción, se enviará un informe del problema a Microsoft según la [configuración de comentarios y diagnósticos](http://go.microsoft.com/fwlink/p/?LinkID=614828) del usuario. Microsoft te proporciona un subconjunto de datos de error en el informe del problema, para que puedas usarlo para mejorar la aplicación. Puedes consultar estos datos en la página Calidad de la aplicación en el panel.

Cuando el usuario activa una aplicación tras un bloqueo, su controlador de eventos de activación recibe un valor [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) de **NotRunning** y debe mostrar simplemente su interfaz de usuario y datos iniciales. Después de un bloqueo, no uses de forma habitual la aplicación que usarías para **Resuming** con **Suspended** porque los datos pueden estar dañados; consulta [Directrices para suspender y reanudar una aplicación (aplicaciones de la Tienda Windows)](https://msdn.microsoft.com/library/windows/apps/hh465088).

## Eliminación de la aplicación


Cuando un usuario elimina la aplicación, esta se quita, junto con todos los datos locales. La eliminación de una aplicación no afecta a los datos del usuario almacenados en ubicaciones comunes como, por ejemplo, las bibliotecas de imágenes o documentos.

## Ciclo de vida de aplicación y plantillas de proyecto de Visual Studio


En las plantillas de proyecto iniciales de Visual Studio se proporciona el código básico relevante para el ciclo de vida de la aplicación. La aplicación básica controla la activación del inicio, ofrece un lugar para restaurar los datos de aplicación y muestra la interfaz de usuario principal, incluso antes de agregar el propio código. Para obtener más información, consulte [Plantillas de proyecto C#, VB y C++ para aplicaciones de la Tienda](https://msdn.microsoft.com/library/windows/apps/hh768232).

## Principales API del ciclo de vida de la aplicación


-   Espacio de nombres [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691)
-   Espacio de nombres [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
-   Espacio de nombres [**Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865)
-   Clase [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) (XAML)
-   Clase [**Windows.UI.Xaml.Window**](https://msdn.microsoft.com/library/windows/apps/br209041) (XAML)

**Nota**  
Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para la Plataforma universal de Windows (UWP). Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Temas relacionados


* [Directrices para suspender y reanudar una aplicación](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [Controlar el inicio previo de aplicaciones](handle-app-prelaunch.md)
* [Controlar la activación de aplicaciones](activate-an-app.md)
* [Controlar la suspensión de la aplicación](suspend-an-app.md)
* [Controlar la reanudación de la aplicación](resume-an-app.md)

 

 





<!--HONumber=Mar16_HO1-->


