---
ms.assetid: 54ECD653-7FC2-4A95-AC5A-972C4FB5A54B
description: Antes de enviar tu aplicación, es conveniente probar la implementación de mediación de anuncios.
title: Probar la implementación de mediación de anuncios
---

# Probar la implementación de mediación de anuncios


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Antes de enviar tu aplicación, es conveniente probar la implementación de mediación de anuncios.

## Probar con valores de configuración de prueba de la red de anuncios


Si ejecutas tu aplicación sin especificar una configuración de red de anuncios iniciando **Servicios conectados** para tu proyecto en Visual Studio, la mediación de anuncios usará automáticamente los valores de configuración de prueba al ejecutar la aplicación en tu equipo de desarrollo (para aplicaciones para la Plataforma universal de Windows (UWP) y XAML de Windows 8.1) o en el emulador o dispositivo (para aplicaciones de Windows Phone). Esto te permite probar rápidamente la aplicación y asegurarte de que esté correctamente codificada antes de que escribas los parámetros necesarios de la red de anuncios.

Las redes de anuncios girarán en orden secuencial, mostrándose una después de la otra durante la misma cantidad de tiempo. Procura esperar lo suficiente para ejecutar a través de algunos ciclos de modo que puedas ver todas las redes de anuncios y reducir la posibilidad de problemas de conectividad temporales que pudieran producirse.

Se mostrarán anuncios de prueba para las redes de anuncios que los admiten. Ten en cuenta que los anuncios de prueba a veces pueden parecer errores. Asegúrate de revisar los eventos para determinar si se han producido errores.

**Nota**  Al probar una aplicación Silverlight de Windows Phone, AdMob de Google siempre devolverá un error de **Solicitud no válida** porque no usa metadatos de prueba. Para comprobar la implementación de AdMob de Google, debes especificar los parámetros necesarios, tal como se describe en la siguiente sección.

 

Si usas el código de control de eventos que se muestra en el tema sobre cómo [Agregar y usar el control de mediación de anuncios](add-and-use-the-ad-mediator-control.md), todos los errores se mostrarán en el resultado de la consola.

## Probar con tus valores de configuración de la red de anuncios


Después de probar la aplicación con datos de configuración de prueba, querrás probarla con los valores de la red de anuncios que piensas usar para la versión que publicarás en la Tienda Windows.

Primero, abre la ventana **Agregar servicio conectado** (Visual Studio 2015) o **Administrador de servicios** (Visual Studio 2013) y configura cada red de anuncios tal y como se describe en [Agregar y usar el control de mediación de anuncios](add-and-use-the-ad-mediator-control.md). Escribe los parámetros necesarios para cada red de anuncios.

Ya estás listo para probar la aplicación. Asegúrese de ejecutar la aplicación durante el tiempo suficiente para comprobar que cada red de anuncios puede mostrar un anuncio correctamente. Comprueba las excepciones y corrige los errores de codificación antes de enviar tu aplicación.

Cuando envías el paquete de la aplicación al panel del Centro de desarrollo de Windows, los valores de configuración que escribes en Visual Studio se rellenan automáticamente en la página **Rentabilizar con anuncios** del panel. Puedes modificar estos valores para configurar el comportamiento de la red de anuncios de tu aplicación en la Tienda Windows. Para obtener más información, consulta [Enviar la aplicación y configurar la mediación de anuncios](submit-your-app-and-configure-ad-mediation.md).

## Temas relacionados

* [Seleccionar y administrar redes de anuncios](select-and-manage-your-ad-networks.md)
* [Agregar y usar el control de mediación de anuncios](add-and-use-the-ad-mediator-control.md)
* [Enviar la aplicación y configurar la mediación de anuncios](submit-your-app-and-configure-ad-mediation.md)
* [Solucionar problemas de mediación de anuncios](troubleshoot-ad-mediation.md)
 

 





<!--HONumber=Mar16_HO1-->


