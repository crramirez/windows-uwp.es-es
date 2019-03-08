---
title: Solución de problemas de Xbox Live con Fiddler
description: Obtenga información sobre cómo usar Fiddler para solucionar problemas con llamadas de servicio de Xbox Live.
ms.assetid: 7d76e444-027b-4659-80d5-5b2bf56d199e
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox uno, fiddler, llamadas de servicio y solución de problemas
ms.localizationpriority: medium
ms.openlocfilehash: 84c6717a4f9f5aff9fd3ff1f68c870fdd9174865
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606720"
---
# <a name="troubleshooting-xbox-live-using-fiddler"></a>Solución de problemas de Xbox Live con Fiddler

Fiddler es un proxy que registra el tráfico de todos los HTTP y HTTPS entre el dispositivo y el Internet de depuración web. Se usará para iniciar sesión e inspeccionar el tráfico hacia y desde los servicios de Xbox Live y el usuario de confianza servicios web de terceros, para comprender y depurar llamadas a servicios web.

## <a name="for-windows-uwp-pc-apps"></a>Para las aplicaciones UWP PC de Windows

1. Asegúrese de que el usuario actual está en el grupo de administradores en el equipo
1. Descargar Fiddler desde [https://www.telerik.com/fiddler](https://www.telerik.com/fiddler)
1. Asegúrese de que selecciona la versión "Compilado para .NET 4"
1. Una vez instalado, vaya a Herramientas -> Opciones de Fiddler y habilitar Capture HTTPS CONNECTs y HTTPS de descifrar el tráfico.  Todas las comunicaciones entre el tiempo de ejecución y los servicios de Xbox LIVE se cifran con SSL.  Sin esta opción no verá nada útil.  Acepte todos los cuadros de diálogo que aparece Fiddler (deben ser 5 cuadros de diálogo incluidos UAC)
1. Vaya a "WinConfig", "All exento" y "Guardar cambios".  En caso contrario, Fiddler no funcionará con las aplicaciones de Store.
1. Ahora debería ver las solicitudes y respuestas entre la aplicación, en tiempo de ejecución y Xbox LIVE si ejecuta la aplicación.

Para depurar llamadas de nivel de sistema operativo de UWP que normalmente no es necesario, pero puede ser útil al depurar los eventos de inicio de sesión y en el juego, deberá configurar Fiddler para capturar el tráfico de WinHTTP.
Esto puede hacerse con:
```cpp
    netsh winhttp set proxy 127.0.0.1:8888 "<-loopback>"
```
donde el puerto 8888 coincide con el puerto que ha configurado en herramientas Fiddlers | Opciones | Pestaña conexiones cuyo valor predeterminado es 8888.
Para deshacer esta operación, haga lo:
```cpp
    netsh winhttp reset proxy
```

## <a name="for-xbox-one-uwp-based-projects"></a>Los proyectos para UWP de Xbox uno basados en

Siga estos pasos [https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler)

## <a name="for-xbox-one-xdk-based-projects"></a>Para los proyectos en función de XDK una Xbox

En una operación normal, una consola que se comunica a través de un proxy corre el riesgo de que dicho proxy modifique sus comunicaciones, lo que podría permitir a los jugadores hacer trampas. Por lo tanto, las consolas se diseñan para no permitir la comunicación a través de un proxy. El uso de Fiddler con el kit de desarrollo de Xbox One requiere que realices algunos pasos de configuración especial en el kit de desarrollo, para poder usar el proxy de Fiddler.

Fiddler es un software gratuito y puede descargarse desde el [sitio web de Fiddler](https://www.telerik.com/fiddler/).

Fiddler puede afectar al estado de red notificado por la consola. Si una conexión ascendente se deshabilita desde el equipo que ejecuta Fiddler, es posible que la consola no detecte esta desconexión hasta que la autenticación de la consola haya expirado. Si usas Fiddler, asegúrate de desconectar la conexión entre la consola y el equipo que ejecuta Fiddler, en lugar de usar Fiddler para simular una desconexión. Mejor aún, use la carga de red herramientas tan simulan desconexión con fines de prueba.

Instalación y habilitación de Fiddler en el equipo de desarrollo

Siga estos pasos para instalar y habilitar Fiddler supervisar el tráfico desde el kit de desarrollo.

1. Instala Fiddler en el equipo de desarrollo; para ello, sigue las instrucciones que se proporcionan en el [sitio web de Fiddler](https://www.telerik.com/fiddler/).
1. Inicie Fiddler y seleccione las opciones de Fiddler en el menú Herramientas.
1. Seleccione la pestaña conexiones y asegúrese de que los equipos remotos de permitir para conectar la casilla de verificación está activada.
1. Haga clic en Aceptar para confirmar el cambio en la configuración. Verás un cuadro de diálogo que indicará que Fiddler debe reiniciarse para que el cambio surta efecto y que es posible que debas configurar el firewall manualmente. Haga clic en Aceptar en este cuadro de diálogo, pero aún no reiniciar Fiddler.
1. Configura la regla de firewall necesaria para permitir a los equipos remotos conectarse. Iniciar el applet del Panel de Control del Firewall de Windows. Haga clic en Configuración avanzada y, a continuación, la regla de entrada. Busque la regla llamada "FiddlerProxy" y desplácese a la derecha, comprobando que cada una de las siguientes opciones aparece para esa regla.

| Valor          | Valor preferido                |
|------------------|--------------------------------|
| Nombre             | FiddlerProxy                   |
| Grupo            | (no establecido un valor para el grupo) |
| Perfil          | Todos                            |
| Habilitado          | Sí                            |
| Acción           | Permitir                          |
| Invalidar         | No                             |
| Programa          | ruta de acceso a fiddler.exe            |
| LocalAddress     | Cualquiera                            |
| RemoteAddress    | Cualquiera                            |
| Protocolo         | TCP                            |
| LocalPort        | Cualquiera                            |
| RemotePort       | Cualquiera                            |
| AllowedUsers     | Cualquiera                            |
| AllowedComputers | Cualquiera                            |


1. Configure Fiddler para capturar y descifrar el tráfico HTTPS.
    1. Para habilitar el mejor rendimiento, establezca Fiddler para usar el modo de transmisión por secuencias, haga clic en el botón de Stream en la barra de botones.
    1. En Fiddler, seleccione Herramientas, opciones de Fiddler y, luego, HTTPS.
    1. Active la casilla del tráfico HTTPS de descifrar. Si un cuadro de mensaje le preguntará si desea configurar Windows para el certificado de CA de confianza, haga clic en no.
    1. Haga clic en Exportar certificado raíz al botón en el escritorio.
1. Salga de Fiddler y vuelva a iniciarlo.

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>Para configurar un kit de desarrollo para que use Fiddler como su proxy de Internet
Se ha simplificado la configuración de Fiddler en el kit de desarrollo desde el método usado en versiones anteriores.

1. Copie el certificado raíz de Fiddler que exportó en el escritorio, el Kit de desarrollo como``` xs:\Microsoft\Cert\FiddlerRoot.cer```.  Puede usar el comando siguiente:  ```xbcp [local Fiddler Root directory]\FiddlerRoot.cer xs:\Microsoft\Cert\FiddlerRoot.cer```
1. Cree un archivo de texto denominado ```ProxyAddress.txt```, con la dirección IP o nombre de host del equipo de desarrollo ejecutando Fiddler y el número de puerto que está escuchando Fiddler como el único contenido en el archivo. Dar formato al nombre o dirección IP y puerto como sigue: "HOST: PUERTO". (De forma predeterminada, Fiddler usa el puerto 8888). Por ejemplo, "10.124.220.250:8888" o "my_dev_pc.contoso.com:8888". Copie este archivo en el kit de desarrollo como xs:\Microsoft\Fiddler\ProxyAddress.txt.  Puede usar el comando siguiente:  ```xbcp [local Proxy Address file directory]\ProxyAddress.txt xs:\Microsoft\Fiddler\ProxyAddress.txt```
1. Reinicie el kit de desarrollo escribiendo ```xbreboot``` en el símbolo del sistema

### <a name="to-stop-using-fiddler"></a>Dejar de usar Fiddler

Para detener utilizando Fiddler como un proxy a Internet y, por tanto, seguimiento de todo tráfico de red del kit de desarrollo en Fiddler, simplemente cambie el nombre o eliminar el archivo FiddlerRoot.cer en el kit de desarrollo y, a continuación, reinicie el kit de desarrollo.

### <a name="how-it-works"></a>Cómo funciona

El kit de desarrollo se configura para utilizar el servidor proxy especificado en ProxyAddress.txt hace que la presencia de un archivo xs:\Microsoft\Cert\FiddlerRoot.cer y un archivo xs:\Microsoft\Fiddler\ProxyAddress.txt en tiempo de arranque. Si falta cualquiera de los archivos, a continuación, el kit de desarrollo no se configurará para que funcione a través de un proxy de Fiddler.

Cada equipo con Fiddler instalado usa un certificado raíz de Fiddler diferente. Si tiene más de un equipo que se puede usar para proporcionar a un proxy de Fiddler para el kit de desarrollo, puede copiar certificado raíz de cada equipo PC Fiddler en el directorio xs:\Microsoft\Cert, siempre y cuando uno de ellos se denomina FiddlerRoot.cer. Todos los certificados en el directorio de certificado se comprueban cuando se está autenticando un proxy de Fiddler. La instancia de Fiddler de dirección sea cual sea su PC se ProxyAddress.txt se usará como un proxy, siempre y cuando su certificado está presente en el directorio de certificado.
