---
title: Los espacios aislados de Xbox Live
description: Obtenga información sobre los espacios aislados para el desarrollo de Xbox Live.
ms.assetid: a5acb5bf-dc11-4dff-aa94-6d1f01472d2a
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ee284550a9b508a8d46556bf0353bd75d55014f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599480"
---
# <a name="xbox-live-sandboxes-intro"></a>Introducción de Xbox Live de espacios aislados

En [configuración del servicio de Xbox Live](xbox-live-service-configuration.md), se explicó que debe configurar la información sobre el título en línea, normalmente en [centro de partners](https://partner.microsoft.com/dashboard).  Esta información incluye cosas como los marcadores en el título de la que va a mostrar, logros que pueden desbloquear los jugadores, configuración de contactos, etcetera.

Cuando se realizan cambios en la configuración del servicio, estos deben publicarse desde el centro de partners antes de que los cambios son recogidos por el resto de Xbox Live y se pueden ver por su título.

Publicar en lo que se denomina un espacio aislado de desarrollo.  Estos permiten trabajar en los cambios realizados en el título en un entorno aislado.  Estas ofrecen varias ventajas que se describe en la sección siguiente.

De forma predeterminada, las consolas de una Xbox y equipos con Windows 10 están en el espacio aislado de venta directa.

## <a name="benefits"></a>Ventajas

Los espacios aislados de desarrollo ofrecen algunas ventajas:


1. Puede iterar sobre los cambios en una actualización para el título sin que afecte a la versión disponible actualmente.
2. Algunas herramientas sólo funcionan en un espacio aislado de desarrollo por motivos de seguridad.
3. Algunos desarrolladores del equipo que desee cambios de configuración de servicio "rama" y prueba sin que afecte a la configuración de servicio de desarrollo principal.
4. Otros publicadores no pueden ver lo que está trabajando sin que se va a conceder acceso a su espacio aislado.

También puede **opcionalmente** crear cuentas de prueba.  Puede usarlas si no desea usar su cuenta de Xbox Live normal para probar su título, o si necesitan varias cuentas para probar escenarios como la interacción social (p. ej.: ver estadísticas de un amigo) o con varios jugadores.

Cuentas de prueba pueden solo inicio de sesión en espacios aislados de desarrollo y se explicará en una sección a continuación.

## <a name="finding-out-about-your-sandbox"></a>Averiguar sobre su espacio aislado

La inmensa mayoría de los desarrolladores necesitan solo un recinto de seguridad.  Afortunadamente un espacio aislado se crea automáticamente cuando se crea un título.

1. Puede averiguar sobre su espacio aislado para ello, vaya al centro de partners aquí: ![](images/getting_started/first_xbltitle_dashboard.png)

1. A continuación, haga clic en el título: ![](images/getting_started/first_xbltitle_dashboard_overview.png)

1. Por último, haga clic en Servicios -> Xbox Live en el menú izquierdo ![](images/getting_started/first_xbltitle_leftnav.png)

1. Ahora puede ver su recinto de seguridad aparece como se indica a continuación. ![](images/getting_started/devcenter_sandbox_id.png)

## <a name="how-your-sandbox-impacts-your-workflow"></a>Cómo afecta su espacio aislado a su flujo de trabajo

Normalmente, trabajar con espacios aislados de las maneras siguientes:

1. (Una vez) Cambie su PC o en Xbox One a su espacio aislado de desarrollo.
2. (Tiempos de varios) Cuando realice cambios en la configuración del servicio, se publicará los cambios en su espacio aislado de desarrollo.  Los cambios de configuración de servicio son las cosas, como definir logros, tablas de clasificación de agregar o modificar una plantilla de sesión de varios jugadores.
3. (Algunas veces) Si está trabajando con otros miembros del equipo, se puede dar acceso a su espacio aislado
4. (Una vez) Si necesita probar algo en el sector MINORISTA, o desea tomarse un descanso para reproducir su juego favorito de Xbox, deberá cambiar su espacio aislado a minoristas.

Estos escenarios se describirán más detalladamente a continuación.  El proceso presenta algunas diferencias en PC y consola, por lo que hay secciones independientes para cada uno.

## <a name="switch-your-pcs-development-sandbox"></a>Cambiar el espacio aislado de desarrollo de su PC

Si desea cambiar el espacio aislado de desarrollo de su PC, la manera recomendada de hacerlo es usar Windows Device Portal (WDP).  También puede hacerlo a través de la línea de comandos.  Describimos ambas maneras.

### <a name="windows-device-portal"></a>Windows Device Portal

Si aún no ha habilitado WDP en su equipo, siga estas instrucciones para hacerlo. [Portal del programa de instalación de dispositivos en el escritorio de Windows](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

Una vez que lo ha hecho, abra el Portal de desarrollo de Windows conectándose a ella en el explorador web, como se describe en el artículo anterior.

A continuación, haga clic en "Xbox Live" para ir a la sección adecuada como se muestra a continuación.

![](images/getting_started/wdp_switch_sandbox.png)

A continuación, puede escribir su espacio aislado que se obtuvo a través de los pasos descritos en la *buscar Out Your Sandbox* y haga clic en "Cambiar".

Para volver a la venta directa, puede escribir aquí comercial.

### <a name="powershell-module"></a>Módulo de PowerShell

[Módulo de PowerShell de Xbox Live](https://github.com/Microsoft/xbox-live-powershell-module/blob/master/docs/XboxLivePsModule.md) XboxlivePSModule contiene varias utilidades para facilitar el desarrollo de Xbox Live incluido el cambio de espacios aislados en el equipo o la consola.

* Para consumir desde [Galería de PowerShell](https://www.powershellgallery.com/packages/XboxlivePSModule), abra una ventana de PowerShell:
    1. Descargue e instale el módulo: `Install-Module XboxlivePSModule -Scope CurrentUser`
    2. Empezar a usar mediante la ejecución `Import-Module XboxlivePSModule`
    3. Ejecutar los cmdlets, es decir, conjunto XblSandbox XDKS.1 o Get-XblSandbox

* Para consumirlo desde un archivo zip en [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools), abra una ventana de PowerShell
    1. Ejecute `Import-Module <path to unzipped folder>\XboxLivePsModule\XboxLivePsModule.psd1`
    2. Ejecutar los cmdlets, es decir, conjunto XblSandbox XDKS.1 o Get-XblSandbox

### <a name="command-prompt-script"></a>Secuencia de comandos del símbolo del sistema

Descargue el paquete de herramientas de Live en Xbox en [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools) y descomprímalo.  Encontrará un archivo por lotes de SwitchSandbox.cmd dentro.

Ejecútelo en modo de administrador para cambiar su espacio aislado.  El primer argumento es el espacio aislado.  Por ejemplo si está intentando cambiar al espacio aislado XDKS.1, debe hacer:

```
SwitchSandbox.cmd XDKS.1
```

Para volver a la venta directa, basta con proporcionar como segundo argumento.

```
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Cambie su recinto de seguridad de desarrollo de consola Xbox One

### <a name="using-windows-dev-portal"></a>Usar el Portal de desarrollo de Windows

Puede usar el Portal de desarrollo de Windows para cambiar el espacio aislado en la consola.  Para ello, vaya a "Hogar Dev" en la consola y habilitarla.

Después de que puede escribir la dirección IP en el explorador web en su PC para conectarse a la consola.  Puede, a continuación, haga clic en "Xbox Live" y escriba el espacio aislado en el cuadro de texto.

### <a name="using-xbox-one-manager"></a>Uso de un administrador de Xbox

Xbox One Manager le permite administrar determinados aspectos de la consola desde su PC.  Esto incluye el reinicio, administrar las aplicaciones instaladas y cambiar su espacio aislado.

Haga clic con el botón derecho en la consola que desea cambiar el espacio aislado y vaya a "Configuración …"

A continuación, puede escribir un recinto de seguridad.

### <a name="using-xbox-one-console-ui"></a>Mediante la interfaz de usuario de consola Xbox One

Si desea cambiar su espacio aislado de desarrollo directamente desde la consola, puede ir a "Configuración".  A continuación, vaya a "Configuración de desarrollador" y verá una opción para cambiar su espacio aislado.

## <a name="sandbox-uses"></a>Usa el espacio aislado

### <a name="data-that-is-sandboxed"></a>Datos que es un espacio aislados
Puede usar las características del recinto de seguridad para administrar el acceso entre los desarrolladores del equipo durante el proceso de desarrollo.  Por ejemplo, desea aislar datos entre el equipo de desarrollo y los evaluadores.

Incluyen los datos en un espacio aislado:
- Logros, tablas de clasificación y estadísticas de un usuario.  Logros acumulados para un usuario en un recinto de seguridad no se traducen en espacio aislado de otro.
- Varios jugadores y contactos.  Los usuarios no pueden jugar en red juego con otra persona es un espacio aislado diferentes.
- Configuración del servicio.  Si agrega una mención especial de nuevo a un título en un recinto de seguridad, no es visible en un recinto de seguridad diferente.  Esto se aplica a todos los datos de configuración de servicio.

Los datos en un espacio aislado no están información fundamentalmente social.  Así pues, por ejemplo, si un usuario sigue a otro usuario, que la relación es independiente del espacio aislado.

### <a name="examples"></a>Ejemplos
Algunos ejemplos se proporcionará a continuación para ayudar a ilustrar algunas de las ventajas de por qué podría desear usar varios de los espacios aislados.

> **Nota**: Si está en el programa de creadores de Xbox, solo puede tener un recinto de seguridad.  Si desea crear varios de los espacios aislados, solicítela el ID@Xbox programa.

#### <a name="service-config-isolation"></a>Aislamiento de la configuración de servicio
Como se mencionó anteriormente, la configuración del servicio es específica de espacio aislado.  Por lo que es posible que tenga un *desarrollo* espacio aislado y un *pruebas* espacio aislado.  Cuando se ejecuta una compilación de su título a los evaluadores, desea publicar su [configuración del servicio](xbox-live-service-configuration.md) a la *pruebas* espacio aislado.

Mientras tanto, podría agregar logros o sesión de varios jugadores diferentes tipos de su *desarrollo* recinto de seguridad sin que afecte a la configuración del servicio que están viendo los evaluadores.

#### <a name="multiplayer"></a>Varios jugadores
Tome el ejemplo anterior con un *desarrollo* y *pruebas* espacio aislado.  Quizás la configuración del servicio es el mismo entre los espacios aislados, pero los desarrolladores son crear características para varios jugadores y desean probar contactos entre sí.  Los evaluadores también están probando varios jugadores.

En un caso como éste, los desarrolladores que no desee el servicio Xbox Live contactos para hacerlos coincidir con los evaluadores, porque están depurando problemas por separado.  Sería una buena forma de evitar que esto para que los desarrolladores en el *desarrollo* espacio aislado y evaluadores a estar en otro *pruebas* espacio aislado.  Esto evita que ambos grupos de aislamiento.

## <a name="advanced"></a>Avanzado

Para simplificar el proceso de desarrollo, comience con su espacio aislado de forma predeterminada y agregar nuevos espacios aislados con prudencia.

Una vez que encuentre su aislamiento de datos y de control de acceso necesita aumentar, verá [avanzada Xbox Live recintos](advanced-xbox-live-sandboxes.md) artículo.  
