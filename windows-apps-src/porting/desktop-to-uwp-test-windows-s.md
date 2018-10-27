---
author: normesta
Description: Test your app for Windows 10 in S mode.
Search.Product: eADQiWindows 10XVcnh
title: Probar la aplicación de Windows en Windows 10 S
ms.author: normesta
ms.date: 05/11/2017
ms.topic: article
keywords: windows 10 S, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3307506c5cf62d04cd19fbc302ad14bfcedd0045
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/27/2018
ms.locfileid: "5693329"
---
# <a name="test-your-windows-app-for-windows-10-in-s-mode"></a>Consulta Probar la aplicación de Windows 10 en modo S

Puedes probar la aplicación de Windows para garantizar que funciona correctamente en dispositivos que ejecuten Windows 10 en modo S. De hecho, si vas a publicar la aplicación en la Microsoft Store, debes hacerlo porque es un requisito de la Store. Para probar la aplicación, puedes aplicar una directiva de integridad de código de Device Guard en un dispositivo que ejecute Windows 10 Pro.

> [!NOTE]
> El dispositivo en el que aplica la directiva de integridad de código de Device Guard debe ejecutar Windows 10 Creators Edition (10.0; Build 15063) o posterior.

La directiva de integridad de código de Device Guard aplica las reglas que las aplicaciones deben cumplir para poder ejecutarse en Windows 10 S.

> [!IMPORTANT]
>Te recomendamos que apliques estas directivas en una máquina virtual, pero si quieres aplicarlas en el equipo local, asegúrate de revisar nuestras directrices de procedimientos recomendados en la sección "A continuación, instala la directiva y reinicia el sistema" que encontrarás en este artículo antes de aplicar una directiva.

<a id="choose-policy" />

## <a name="first-download-the-policies-and-then-choose-one"></a>En primer lugar, descarga las directivas y elige una

Descarga las directivas de integridad de código de Device Guard [aquí](https://go.microsoft.com/fwlink/?linkid=849018).

A continuación, elige la que más te convenga. Aquí te mostramos resumen de cada directiva.

|Directiva |Cumplimiento |Certificado de firma |Nombre del archivo |
|--|--|--|--|
|Directiva del modo de auditoría |Registra los problemas / no se bloquea |Tienda |SiPolicy_Audit.p7b |
|Directiva del modo de producción |Sí |Tienda |SiPolicy_Enforced.p7b |
|Directiva del modo de producto con aplicaciones autofirmadas |Sí |Certificado de prueba de AppX  |SiPolicy_DevModeEx_Enforced.p7b |

Te recomendamos que empieces con la directiva del modo de auditoría. Puedes revisar los registros de eventos de integridad de código y usar esa información para realizar ajustes en la aplicación. A continuación, aplica la directiva del modo de producción cuando quieras realizar la prueba final.

Aquí tienes un poco más de información acerca de cada directiva.

### <a name="audit-mode-policy"></a>Directiva del modo de auditoría
Con este modo, la aplicación se ejecuta incluso si realiza tareas que no se admiten en Windows 10 S. Windows registra los archivos ejecutables que se habrían bloqueado en los registros de eventos de integridad de código.

Para encontrar estos registros, abre el **Visor de eventos** y, a continuación, navega hasta esta ubicación: Registros de aplicaciones y servicios->Microsoft->Windows->CodeIntegrity->Operativo.

![code-integrity-event-logs](images/desktop-to-uwp/code-integrity-logs.png)

Este modo es seguro y no impedirá que el sistema se inicie.

#### <a name="optional-find-specific-failure-points-in-the-call-stack"></a>(Opcional) Encontrar puntos de error específicos en la pila de llamadas
Para buscar puntos específicos en la pila de llamadas dónde se producen los problemas de bloqueo, tienes que agrega esta clave del Registro y luego [configurar un entorno de depuración del modo kernel](https://docs.microsoft.com/windows-hardware/drivers/debugger/getting-started-with-windbg--kernel-mode-#span-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanset-up-a-kernel-mode-debugging).

|Clave|Nombre|Tipo|Valor|
|--|---|--|--|
|HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\CI| DebugFlags |REG_DWORD | 1 |


![reg-setting](images/desktop-to-uwp/ci-debug-setting.png)

### <a name="production-mode-policy"></a>Directiva del modo de producción
Esta directiva aplica las reglas de integridad de código que coinciden con Windows 10, para que puedas simular la ejecución en Windows 10 S. Esta es la directiva más estricta y es excelente para probar la producción final. En este modo, la aplicación está sujeta a las mismas restricciones que tendría si estuviera en el dispositivo del usuario. Para usar este modo, la aplicación debe estar firmada por Microsoft Store.

### <a name="production-mode-policy-with-self-signed-apps"></a>Directiva del modo de producción con aplicaciones autofirmadas
Este modo es similar a la directiva del modo de producción, pero también permite ejecutar contenido firmado con el certificado de prueba que se incluye en el archivo zip. Instala el archivo PFX que se incluye en la carpeta **AppxTestRootAgency** de este archivo zip. A continuación, firma la aplicación con él. De este modo, puedes iterar rápidamente sin tener la firma de la Tienda.

Como el nombre del publicador del certificado debe coincidir con el nombre del publicador de la aplicación, tendrás que cambiar temporalmente el valor del atributo **Editor** del elemento **Identidad** a "CN=Appx Test Root Agency Ex". Puedes cambiar este atributo de nuevo a su valor original después de completar las pruebas.

## <a name="next-install-the-policy-and-restart-your-system"></a>A continuación, instala la directiva y reinicia el sistema

Te recomendamos que apliques estas directivas a una máquina virtual, ya que estas directivas podrían provocar errores de arranque. Eso es porque estas directivas bloquean la ejecución del código que no haya firmado Microsoft Store (controladores incluidos).

Si quieres aplicar estas directivas en el equipo local, es mejor comenzar con la directiva del modo de auditoría. Con esta directiva, puedes revisar los registros de eventos de integridad de código para garantizar que nada importante se bloquee en la directiva aplicada.

Cuando estés listo para aplicar una directiva, busca el archivo .P7B de la directiva que hayas elegido, denomínalo **SIPolicy.P7B** y, a continuación, guárdalo en esta ubicación del sistema: **C:\Windows\System32\CodeIntegrity\\**.

A continuación, reinicia el sistema.

>[!NOTE]
>Para quitar una directiva del sistema, elimina el archivo .P7B y, a continuación, reinicia el sistema.

## <a name="next-steps"></a>Pasos siguientes

**Encuentra respuestas a tus preguntas**

¿Tienes alguna pregunta? Pregúntanos en Stack Overflow. Nuestro equipo supervisa estas [etiquetas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). También puedes preguntarnos [aquí](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Enviar comentarios o realizar sugerencias acerca de las características**

Consulta [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Revisar un artículo de blog detallado que publicó nuestro equipo de consultas de aplicaciones**

Consulta [Portar y probar sus aplicaciones de escritorio clásicas en Windows 10 S con el Puente de dispositivo de escritorio](https://blogs.msdn.microsoft.com/appconsult/2017/06/15/porting-and-testing-your-classic-desktop-applications-on-windows-10-s-with-the-desktop-bridge/).

**Obtener información acerca de las herramientas que facilitan las pruebas para Windows en modo S**

Consulta [Desempaquetar, modificar, volver a empaquetar, firmar un APPX](https://blogs.msdn.microsoft.com/appconsult/2017/08/07/unpack-modify-repack-sign-appx/).
