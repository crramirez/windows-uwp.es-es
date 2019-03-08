---
title: Cómo usar Fiddler con Xbox One al desarrollar para la UWP
description: Describe cómo usar la herramienta Fiddler de software gratuito para ver el tráfico de red en un kit de desarrollo para UWP de Xbox One.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 9c133c77-fe9d-4b81-b4b3-462936333aa3
ms.localizationpriority: medium
ms.openlocfilehash: fae6caf73cb8a5b569193a17e65e5d8b4f582ff2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57652230"
---
# <a name="how-to-use-fiddler-with-xbox-one-when-developing-for-uwp"></a>Cómo usar Fiddler con Xbox One al desarrollar para la UWP

Fiddler es un proxy de depuración web que registra todo el tráfico HTTP y HTTPS entre el kit de desarrollo de Xbox One e Internet. Se usa para registrar e inspeccionar el tráfico a los servicios de Xbox y los servicios web de terceros de confianza, así como desde estos, para comprender y depurar llamadas al servicio web. 

En una operación normal, una consola que se comunica a través de un proxy corre el riesgo de que dicho proxy modifique sus comunicaciones, lo que podría permitir a los jugadores hacer trampas. Por lo tanto, las consolas se diseñan para evitar la comunicación a través de un proxy. El uso de Fiddler con el kit de desarrollo de Xbox One requiere que realices algunos pasos de configuración especial en el kit de desarrollo, para poder usar el proxy de Fiddler. 

Fiddler es un software gratuito y puede descargarse desde el [sitio web de Fiddler](https://www.fiddler2.com/fiddler2/). 

Fiddler puede afectar al estado de red notificado por la consola. Si una conexión ascendente se deshabilita desde el equipo que ejecuta Fiddler, es posible que la consola no detecte esta desconexión hasta que la autenticación de la consola haya expirado. Si usas Fiddler, asegúrate de desconectar la conexión entre la consola y el equipo que ejecuta Fiddler, en lugar de usar Fiddler para simular una desconexión.

### <a name="to-install-and-enable-fiddler-on-your-development-pc"></a>Para instalar y habilitar Fiddler en el equipo de desarrollo
Sigue estos pasos para instalar y habilitar Fiddler para supervisar el tráfico desde el kit de desarrollo:

1. Instala Fiddler en el equipo de desarrollo; para ello, sigue las instrucciones que se proporcionan en el [sitio web de Fiddler](https://www.fiddler2.com/fiddler2/). 
2. Inicia Fiddler y selecciona **Opciones de Fiddler** desde el menú **Herramientas**. 
3. Selecciona la pestaña **Connections** (Conexiones) y asegúrate de que **Allow remote computers to connect** (Permitir que se conecten equipos remotos) está seleccionado. 
4. Haz clic en **OK** (Aceptar) para aceptar el cambio en la configuración. Verás un cuadro de diálogo que indicará que Fiddler debe reiniciarse para que el cambio surta efecto y que es posible que debas configurar el firewall manualmente. Haz clic en **OK** (Aceptar) en este cuadro de diálogo, pero *no reinicies Fiddler aún*.
5. Configura la regla de firewall necesaria para permitir a los equipos remotos conectarse. Iniciar el applet del Panel de Control del Firewall de Windows. Haz clic en **Configuración avanzada** y después en **Regla de entrada**. Busca la regla denominada "FiddlerProxy" y desplázate hacia la derecha, para comprobar que todas de las configuraciones de la siguiente tabla aparecen para esa regla.
  
  | Valor           | Valor preferido                |
  | ----              | ----                           |
  | Nombre              | FiddlerProxy                   |
  | Grupo             | *Ningún valor* |
  | Perfil           | Todos                            |
  | Habilitado           | Sí                            |
  | Acción            | Permitir                          |
  | Invalidar          | No                             |
  | Programa           | *ruta de acceso a fiddler.exe*          |
  | LocalAddress      | Cualquiera                            |
  | RemoteAddress     | Cualquiera                            |
  | Protocolo          | TCP                            |
  | LocalPort         | Cualquiera                            |
  | RemotePort        | Cualquiera                            |
  | AllowedUsers      | Cualquiera                            |
  | AllowedComputers  | Cualquiera                            |


6. Configura Fiddler para capturar y descifrar el tráfico HTTPS mediante lo siguiente:
  1. Para lograr un rendimiento óptimo, establece Fiddler para que use el modo de transmisión haciendo clic en botón **Emisión** de la barra de botones.
  2. En el menú **Herramientas** de Fiddler, selecciona **Opciones de Fiddler** y, a continuación, haz clic en **HTTPS**.
  3. Selecciona la casilla **Descifrar tráfico HTTPS**. Si un cuadro de diálogo pregunta si deseas configurar Windows para que confíe en el certificado de la entidad de certificación, haz clic en **n**.
  4. Haz clic en **Export Root Certificate to Desktop** (Exportar certificado raíz al escritorio).
7. Sal y reinicia Fiddler.

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>Para configurar un kit de desarrollo para que use Fiddler como su proxy de Internet

1. Dirígete a la herramienta **Red** en la interfaz de usuario de Xbox Device Portal.
2. Busca el certificado raíz de Fiddler que has exportado al escritorio. 
3. Escribe la dirección IP o el nombre de host del equipo de desarrollo que ejecuta Fiddler.
4. Escribe el número de puerto en el que está escuchando Fiddler (de forma predeterminada, Fiddler usa el puerto 8888). 
5. Haga clic en **Habilitar**. Esto reiniciará el kit de desarrollo.

### <a name="to-stop-using-fiddler"></a>Dejar de usar Fiddler
Para dejar de usar Fiddler como proxy de Internet (y hacer que Fiddler deje de seguir todo el tráfico de red del kit de desarrollo), haz lo siguiente:

1. Dirígete a la herramienta **Red** en la interfaz de usuario de Xbox Device Portal.
2. Haga clic en **Deshabilitar**.

> [!NOTE]
> Cada equipo con Fiddler instalado usa un certificado raíz de Fiddler diferente. Si tienes más de un equipo que pueda usarse para proporcionar un proxy de Fiddler para el kit de desarrollo, tendrás seleccionar el nuevo certificado raíz al cambiar entre ellos. Si usas un único equipo, tendrás que seleccionar el certificado raíz solo la primera vez que habilites Fiddler. Debes especificar la dirección IP y el puerto.

## <a name="see-also"></a>Consulte también
- [Referencia de API de configuración de Fiddler](wdp-fiddler-api.md)
- [Preguntas más frecuentes](frequently-asked-questions.md)
- [UWP en Xbox One](index.md)



