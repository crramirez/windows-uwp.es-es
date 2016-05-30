---
author: QuinnRadich Description: Windows 10 versión 1511 y las actualizaciones de las herramientas de desarrollo siguen proporcionando acceso a las características más recientes y a experiencias con tecnología de la Plataforma universal de Windows.
title: Novedades para desarrolladores de Windows 10, versión 1511: noviembre de 2015
---

# Novedades para desarrolladores de Windows 10, versión 1511: noviembre de 2015

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

La versión 1511 de Windows 10 y las actualizaciones de herramientas de desarrollo siguen proporcionando herramientas, características y experiencias con la tecnología de la Plataforma universal de Windows. [Instala las herramientas y el SDK](https://dev.windows.com/downloads) en la versión 1511 de Windows 10 y estarás listo para [crear una nueva aplicación universal de Windows](https://msdn.microsoft.com/library/windows/apps/bg124288) o para explorar cómo puedes usar tu [código de aplicación existente en Windows](https://msdn.microsoft.com/library/windows/apps/mt238321).

## Experiencia del usuario

Las nuevas clases <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx">Windows.UI.StartScreen.JumpList</a> y <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.aspx">Windows.UI.StartScreen.JumpListItem</a> proporcionan a las aplicaciones la posibilidad de seleccionar mediante programación el tipo de lista de accesos directos administrada por el sistema que quieren usar y de agregar puntos de entrada de tareas personalizadas y grupos personalizados a sus listas de accesos directos.

## Entrada
                                        
* <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.input.keyboarddeliveryinterceptor.aspx">Interceptor de entrega de teclado</a>
                                        
    Permite que una aplicación invalide el procesamiento del sistema de una entrada de teclado sin procesar, incluidas teclas de método abreviado, teclas de acceso (o teclas de acceso rápido), teclas de aceleración y teclas de aplicación, pero sin incluir combinaciones de teclas de secuencia de aviso de seguridad (SAS).

    El sistema sigue procesando las combinaciones de teclas de secuencia de aviso de seguridad (SAS), incluidas Ctrl+Alt+Supr y Windows+L.
                                        
* El encadenamiento entre procesos de entrada de puntero para las <a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.core.corewindow.aspx">aplicaciones para UWP</a> y <a href="https://msdn.microsoft.com/library/windows/desktop/hh454903(v=vs.85).aspx">aplicaciones de Windows clásicas</a>.
                                        
    Nuevos eventos de puntero que permiten el encadenamiento entre procesos de entrada.    
                                        
* <a href="https://msdn.microsoft.com/library/windows/desktop/mt622165(v=vs.85).aspx">Moderador de lápiz para aplicaciones de escritorio clásicas</a>
                                        
    Las API de moderador de lápiz permiten a las aplicaciones de Microsoft Win32 administrar la entrada, procesamiento y representación de entrada de lápiz (estándar o modificada) a través de un objeto <a href="https://msdn.microsoft.com/library/windows/desktop/windows.ui.input.inking.inkpresenter.aspx">InkPresenter</a> insertado en el árbol visual <a href="https://msdn.microsoft.com/library/windows/desktop/hh437371(v=vs.85).aspx">DirectComposition</a> de la aplicación.    
                                    
## Redes
                                                                        
Para los usuarios de WebSockets: <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">MessageWebSocket.OutputStream.FlushAsync</a> y <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">StreamWebSocket.OutputStream.FlushAsync</a> se han implementado completamente y deben esperar a que las llamadas de WriteAsync emitidas anteriormente se completen. Ten en cuenta que esto puede causar que el código existente inicie una excepción si el WebSocket está en un estado no válido cuando se llama a <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.streams.datawriter.flushasync.aspx">FlushAsync</a>.    

Una nueva propiedad <a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx">CookieUsageBehavior</a> se agregó a la <a href="https://msdn.microsoft.com/library/windows/apps/windows.web.http.filters.httpbaseprotocolfilter.aspx">clase Windows.Web.Http.Filters.HttpBaseProtocolFilter</a> existente. Esto permite a los desarrolladores controlar cómo administra el sistema las cookies.    
                                    
## ORTC
                                    
Ahora, Microsoft Edge implementa <a href="https://msdn.microsoft.com/library/mt433097(v=vs.85).aspx">ORTC (Comunicaciones en tiempo real mediante objetos)</a>, lo que permite realizar llamadas de audio o vídeo en tiempo real directamente entre exploradores, dispositivos móviles y servidores a través de API de Javascript nativas. Los desarrolladores ya pueden crear aplicaciones avanzadas de comunicación en tiempo real de audio y vídeo en el explorador Microsoft Edge con la API de ORTC, que admite videollamadas de grupo, simulación de radio, codificación de vídeo escalable (SVC), etc.    

Para obtener una demostración de una llamada de audio o vídeo 1.1 a través de la API de ORTC entre exploradores Microsoft Edge, visita <a href="/microsoft-edge/testdrive/demos/ortcdemo/">sitios y demostraciones de versiones de prueba</a>. Para obtener una información general y un ejemplo de código, visita la <a href="https://msdn.microsoft.com/library/mt588497(v=vs.85).aspx">entrada de la Guía para desarrolladores de ORTC</a>.
                                        
## Herramientas de desarrollo F12 de Microsoft Edge
                                                                        
Microsoft Edge incorpora fantásticas mejoras para las Herramientas de desarrollo F12, incluidas algunas de las características más solicitadas de <a href="https://wpdev.uservoice.com/forums/257854-microsoft-edge-developer">UserVoice</a>. Explora las nuevas características de las herramientas Explorador DOM, Consola, Depurador, Red, Rendimiento, Memoria, Emulación y de la nueva herramienta Experimentos, que te permite probar las nuevas y eficaces características antes de que estén acabadas. Las nuevas herramientas están integradas en TypeScript y siempre se están ejecutando, por lo que no es necesario cargarlas de nuevo. Además, la documentación de Herramientas de desarrollo F12 forma parte ahora del <a href="http://dev.modern.ie/">sitio Microsoft Edge Dev</a> y está totalmente disponible en <a href="https://github.com/MicrosoftEdge/MicrosoftEdge-Documentation">GitHub</a>. A partir de ahora, los documentos no solo variarán en función de tus comentarios, sino que te invitamos a contribuir a nuestra documentación y a ayudarnos a darle forma. Para obtener una breve vídeo de introducción a las Herramientas de desarrollo F12, visita <a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Microsoft-Edge-F12-tools">Un minuto para el desarrollador de Channel9</a>.    
                                    
## Windows Hello
                                    
Windows Hello proporciona a la aplicación la capacidad de habilitar el reconocimiento facial o de huellas digitales para iniciar sesión en un dispositivo o sistema Windows.

Las API de proveedores permiten a los IHV y OEM exponer cámaras a color, de infrarrojos y en profundidad (y los metadatos relacionados) para la visión de los equipos en UWP, y para diseñar una cámara que participe en la autenticación facial de Windows Hello. El espacio de nombres <a href="http://go.microsoft.com/fwlink/?LinkId=691697">Windows.Devices.Perception</a> contiene API de cliente que permiten que la aplicación para UWP tenga acceso a los datos de infrarrojos, profundidad y color de las cámaras de visión del equipo.
                                    
## Nueva API de juegos

Usa la nueva clase Windows.Gaming.UI.GameBar para recibir notificaciones cuando la barra de juegos se muestra o descarta.    
                            
                                    
## API para Bluetooth
                                    
Se han agregado y actualizado varias API para ampliar la compatibilidad con Bluetooth LE, la enumeración de dispositivos y otras características de Bluetooth. Consulta el espacio de nombres <a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.aspx">Windows.Devices.Bluetooth</a>.    
                                   
## API de tarjetas inteligentes ## 

Se agregaron varias API de SmartCardCryptogram al espacio de nombres <a href="https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.aspx">Windows.Devices.SmartCards</a> para admitir los protocolos de pago seguro por criptogramas. Las aplicaciones de pago con emulación de tarjeta basada en host para admitir tocar para pagar pueden usar estas API para obtener rendimiento y seguridad adicionales. Las aplicaciones pueden crear una clave y proteger las claves de transacción de uso limitado con el TPM. Las aplicaciones también pueden aprovechar el marco de NGC (credenciales de próxima generación) para proteger las claves con el PIN del usuario. Estas API delegan la generación de criptogramas al sistema para mejorar el rendimiento. Esto también impide que otras aplicaciones obtengan acceso a las claves y criptogramas.    
                                    
## API de almacenamiento actualizado ## 
    
<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.aspx">Clase Windows.Storage.DownloadsFolder</a><br />
La aplicación puede ahora <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfileforuserasync.aspx">crear un archivo</a> o <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.downloadsfolder.createfolderforuserasync.aspx">crea una carpeta</a> dentro de la carpeta Descargas para un determinado <a href="https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx">Usuario</a>.
                                            
<a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.aspx">Clase Windows.Storage.StorageLibrary</a><br />
La aplicación puede ahora <a href="https://msdn.microsoft.com/library/windows/apps/windows.storage.storagelibrary.getlibraryforuserasync.aspx">obtener una determinada Biblioteca</a> para un determinado <a href="https://msdn.microsoft.com/library/windows/apps/windows.system.user.aspx">Usuario</a>.
                                    
## Kit para la certificación de aplicaciones en Windows ## 
                                    
El Kit para la certificación de aplicaciones en Windows se actualizó con pruebas mejoradas. Para obtener una lista completa de las actualizaciones, visita la página <a href="/develop/app-certification-kit">Kit para la certificación de aplicaciones en Windows</a>.    
                                    
## Descargas de diseños ## 

Echa un vistazo a nuestras nuevas plantillas de diseño de aplicaciones para UWP para Adobe Photoshop. También hemos actualizado las plantillas de Microsoft PowerPoint y Adobe Illustrator y creado una versión PDF de nuestras directrices. <a href="/design/assets">Visita la página Descargas de diseños</a>.    




<!--HONumber=May16_HO2-->


