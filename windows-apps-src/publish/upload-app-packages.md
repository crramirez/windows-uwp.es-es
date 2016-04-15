---
Description: La página Paquetes es donde se cargan todos los archivos del paquete (xap, AppX, .appxupload o .appxbundle) de la aplicación que vas a enviar. En este paso puedes cargar paquetes para cualquier sistema operativo de destino de la aplicación.
title: Cargar paquetes de aplicación
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
---

# Cargar paquetes de aplicación


La página **Paquetes** es donde se cargan todos los archivos del paquete (xap, AppX, .appxupload o .appxbundle) de la aplicación que vas a enviar. En este paso puedes cargar paquetes para cualquier sistema operativo de destino de la aplicación. Cuando un cliente descargue la aplicación, la Tienda buscará entre todos los paquetes de la aplicación disponibles y proporcionará automáticamente a cada cliente el que mejor se adapte a su dispositivo.

Consulta [Requisitos del paquete de la aplicación](app-package-requirements.md) para obtener más información sobre lo que incluye un paquete y cómo debe estructurarse. También debes obtener información sobre [cómo los números de versión pueden afectar a los paquetes que se entregan a clientes específicos](package-version-numbering.md) y [cómo se distribuyen los paquetes a diferentes sistemas operativos](guidance-for-app-package-management.md).

## Cargar paquetes en el envío


Para cargar paquetes, arrástralos en el campo de carga o haz clic para examinar los archivos. La página **Paquetes** te permite cargar archivos .xap, .appx, .appxupload y .appxbundle.

Si has creado algún [paquete piloto](package-flights.md) para tu aplicación, verás una lista desplegable con la opción para copiar los paquetes de uno de los paquetes piloto. Selecciona el paquete piloto que tiene los paquetes que quieres extraer. A continuación, podrás seleccionar varios o todos los paquetes, para incluirlos en este envío.

> **Importante**  Para Windows 10, siempre debes cargar aquí el archivo .appxupload, no .appx ni .appxbundle. Para más información sobre cómo empaquetar aplicaciones para UWP para la Tienda, consulta [Empaquetar aplicaciones universales de Windows para Windows 10](../packaging/packaging-uwp-apps.md).

Si se detectan problemas con los paquetes durante su validación, deberás quitar el paquete, corregir el problema y, a continuación, intenta cargarlo de nuevo. Para obtener más información, consulta [Resolver errores de carga de paquetes](resolve-package-upload-errors.md).

En otros casos se mostrarán advertencias para informarte de posibles problemas, aunque no se te impedirá que continúes con el envío.

## Detalles del paquete


Una vez que los paquetes se han cargado correctamente, los incluiremos en una lista agrupados por sistema operativo de destino. Se mostrará el nombre, la versión y la arquitectura del paquete. Para obtener más información, como los idiomas admitidos, las capacidades de la aplicación y el tamaño de archivo para cada paquete, haz clic en **Detalles**.

Si estás usando [Mediación de anuncios de Windows](../monetize/use-ad-mediation-to-maximize-revenue.md), también verás un vínculo para configurar la mediación de anuncios para cada paquete.

Si necesitas quitar un paquete de tu envío, haz clic en el vínculo **Quitar** en la parte inferior de la sección **Detalles** de cada paquete.

## Quitar paquetes redundantes


Si se detecta que uno o varios de los paquetes son redundantes, se mostrará una advertencia para sugerirte que quites los paquetes redundantes de este envío. Esto sucede a menudo cuando has cargado paquetes previamente y ahora estás proporcionando paquetes de una versión posterior que admiten el mismo conjunto de clientes. En este caso, ningún cliente recibirá el paquete redundante porque hay un paquete mejor (de versión más reciente) para ellos.

Cuando detectemos paquetes redundantes, proporcionaremos una opción para quitarlos de este envío automáticamente. También puedes quitar paquetes del envío individualmente, si lo prefieres.

## Paquetes con Application Insights de Visual Studio


Te recomendamos que uses [Applications Insights de Visual Studio](http://go.microsoft.com/fwlink/?LinkId=615086) en tus paquetes (o que lo habilites activando la casilla "Mostrar telemetría en el Centro de desarrollo de Windows" al crear el paquete) para que podamos proporcionarte [detalles de telemetría de uso de la aplicación](usage-report.md). Si no configuraste Application Insights en Microsoft Visual Studio, cuando detectemos que un paquete lo incluye, mostraremos un mensaje que confirma que al enviar el paquete, aceptas habilitar la telemetría de uso de la aplicación sobre tu cuenta de desarrollador. Puedes deshabilitar la telemetría de uso de la aplicación en cualquier momento en la **configuración de la cuenta**.

 

 






<!--HONumber=Mar16_HO5-->


