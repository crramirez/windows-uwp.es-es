---
title: Desarrollo de aplicaciones para Windows como servicio
description: Obtenga información sobre el enfoque de Windows como servicio (WaaS) para la innovación, el desarrollo y la entrega, y sobre el programa de prueba de Windows Insider centrado en la comunidad.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f384ca56-f2b2-4793-b251-f7f5735376bb
ms.localizationpriority: medium
ms.openlocfilehash: d301a2dfcc478f6d90b8aa562bfbd34cf79493c1
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750141"
---
# <a name="application-development-for-windows-as-a-service"></a>Desarrollo de aplicaciones para Windows como servicio

**Se aplica a**
-   Windows 10
-   Windows 10 Mobile
-   Windows 10 IoT Core 

En el entorno actual, donde las expectativas del usuario con frecuencia las establecen las experiencias centradas en los dispositivos, los ciclos completos de un producto se deben medir en meses, no en años. Además, las nuevas versiones deben estar disponibles de forma continua y se deben poder implementar con el mínimo impacto para los usuarios. Microsoft diseñó Windows 10 para satisfacer estos requisitos al implementar un nuevo método de innovación, desarrollo y entrega denominado [Windows como servicio (WaaS)](/windows/deployment/update/waas-overview). La clave para permitir ciclos de producto significativamente más cortos a la vez que se mantienen altos niveles de calidad es un innovador método de pruebas centrado en la comunidad que Microsoft ha implementado para Windows 10. La comunidad, conocida como Windows Insider, incluye millones de usuarios en todo el mundo. Cuando los usuarios de Windows Insider deciden participar en la comunidad, prueban muchas compilaciones a lo largo del ciclo de un producto y proporcionan comentarios a Microsoft a través de una metodología iterativa denominada distribución de paquetes piloto.

Las compilaciones que se distribuyen como paquetes piloto proporcionan al equipo de ingeniería de Windows datos significativos sobre el rendimiento real de las compilaciones. La distribución de paquetes piloto a través de los usuarios de Windows Insider también permite a Microsoft probar compilaciones en entornos de hardware, aplicaciones y redes mucho más diversos que en el pasado, así como identificar problemas de manera mucho más rápida. Como resultado, Microsoft cree que la distribución de paquetes piloto centrada en la comunidad permitirá una entrega de innovación mucho más rápida, así como una calidad mucho más elevada de versiones destinadas al público en general.

## <a name="windows-10-release-types-and-cadences"></a>Tipos y cadencias de lanzamiento de Windows 10

Aunque Microsoft lanza compilaciones piloto para usuarios de Windows Insider, Microsoft publicará continuamente dos tipos de versiones de Windows 10 de manera amplia destinadas al público en general:

**Actualizaciones de características** que instalan las más recientes características, experiencias y funcionalidades nuevas en dispositivos que ya ejecutan Windows 10. Dado que las actualizaciones de características contienen una copia completa de Windows, los clientes también las usan para instalar Windows 10 en dispositivos existentes que ejecutan Windows 7 o Windows 8.1 y en dispositivos nuevos que no tienen instalado ningún sistema operativo. Microsoft espera poder publicar actualizaciones semestrales. 

**Actualizaciones de calidad** que proporcionan resoluciones de problemas de seguridad y otras correcciones de errores importantes. Se proporcionarán actualizaciones de calidad para mejorar cada característica con soporte técnico actualmente, con una cadencia de una o más veces al mes. Microsoft seguirá publicando actualizaciones de calidad los martes de actualización (Update Tuesday), conocidos a veces como martes de revisión (Patch Tuesday). Además, es posible que Microsoft publique actualizaciones de calidad adicionales para Windows 10 fuera del proceso Update Tuesday cuando sea necesario para satisfacer las necesidades de los clientes.

Durante el desarrollo de Windows 10, Microsoft perfeccionó el ciclo de ingeniería y lanzamiento de productos de Windows para permitir ofrecer las características, experiencias y funcionalidades que desean los clientes más rápido que nunca. También creó nuevas formas de entregar e instalar actualizaciones de características y actualizaciones de calidad para simplificar las implementaciones y la administración continua, ampliar la base de empleados, que se puede mantener actualizada con las funcionalidades y experiencias de Windows más recientes, y reducir el coste total de propiedad. Desde entonces hemos implementado nuevas opciones de mantenimiento, conocidas como Canal semianual y Canal de mantenimiento a largo plazo (LTSC), que ofrecen soluciones prácticas para mantener actualizado un mayor número de dispositivos en entornos empresariales de lo que se podía anteriormente.

La siguiente tabla describe los distintos canales de mantenimiento y sus atributos principales.

| Opción de mantenimiento | Disponibilidad de nuevas actualizaciones de características para la instalación | Duración del mantenimiento | Principales ventajas | Ediciones admitidas |
| --- | --- | --- | --- | --- |
| Canal semianual (dirigido) | Inmediatamente después de que Microsoft realice la publicación inicial. | 18 meses | Pone las nuevas característica a disposición de los usuarios lo antes posible. | Home, Pro, Education, Enterprise, Mobile, IoT Core, Windows 10 IoT Core Pro (IoT Core Pro) |
| Canal semianual | Aproximadamente 4 meses después de que Microsoft realice la publicación inicial. | 18 meses a partir de la publicación inicial | Proporciona tiempo adicional para probar las nuevas actualizaciones de características antes de la implementación. | Pro, Education, Enterprise, Mobile Enterprise, IoT Core Pro |
| Canal de mantenimiento a largo plazo (LTSC) | Inmediatamente después de que Microsoft realice la publicación | 10 años | Permite la implementación a largo plazo de versiones de Windows 10 seleccionadas en configuraciones en las que no se realizan cambios con frecuencia | Enterprise LTSB |

Para obtener más información, consulta [Opciones de mantenimiento de Windows 10 para actualizaciones](/windows/deployment/update/waas-overview#servicing-channels).

## <a name="supporting-apps-in-windows-as-a-service"></a>Compatibilidad de las aplicaciones en Windows como servicio

El método tradicional para lograr la compatibilidad de las aplicaciones era publicar una nueva versión de la aplicación en respuesta a una nueva versión de Windows. Con este método se presupone que se han producido cambios importantes en el sistema operativo subyacente que podrían provocar una regresión de la aplicación. Este modelo implica un ciclo de desarrollo y validación dedicado que requiere que nuestros partners ISV se alineen con la cadencia de las versiones de Windows.

En el modelo Windows como servicio, Microsoft se compromete a mantener la compatibilidad del sistema operativo subyacente. Esto significa que Microsoft realizará un esfuerzo coordinado para garantizar que no se produzcan cambios importantes que afecten negativamente al ecosistema de las aplicaciones. En este escenario, cuando se publique una compilación de Windows, la mayoría de las aplicaciones (aquellas que no tengan dependencias de kernel) seguirán funcionando.

En vista de este cambio, Microsoft recomienda a sus partners ISV que separen la versión y el soporte técnico de sus aplicaciones de compilaciones de Windows específicas. Los clientes comunes reciben un mejor servicio si el enfoque se centra en el ciclo de vida de las aplicaciones. Esto significa que cuando se publique una versión de la aplicación, esta contará con soporte técnico durante un determinado período de tiempo independientemente de que se publiquen muchas compilaciones de Windows de forma provisional. El ISV se compromete a proporcionar soporte técnico para esa versión específica de la aplicación mientras esta se admita en el ciclo de vida. Microsoft sigue un enfoque de ciclo de vida similar para Windows, que se puede consultar [aquí](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy?C2=14019).

Este enfoque reduce la carga que supone mantener una programación de la aplicación que se alinee con las versiones de Windows. Los partners ISV deberían poder publicar características o actualizaciones con su propia cadencia. Creemos que nuestros partners pueden mantener al día su base de clientes con las últimas actualizaciones de aplicaciones, con independencia de la versión de Windows. Además, nuestros clientes no tienen que buscar una declaración de soporte técnico explícita siempre que se publique una compilación de Windows. Este es un ejemplo de una declaración de soporte técnico que abarca cómo se puede admitir una aplicación en diferentes versiones del sistema operativo:

> **Ejemplo de declaración de soporte técnico del ciclo de vida de una aplicación**
>
> Contoso es una empresa de desarrollo de software, propietaria de la popular aplicación Mojave, que cuenta con una amplia difusión en el mundo empresarial. Contoso publica su próxima versión principal, Mojave 14.0, y declara que esta disfrutará de soporte estándar durante un período de tres años a partir de la fecha de lanzamiento. Durante el soporte estándar, todas las actualizaciones y el soporte técnico serán gratuitos para el producto con licencia. Contoso también declara un período adicional de dos años de soporte extendido durante el cual los clientes podrán comprar actualizaciones y soporte técnico para un período de gracia. A partir de la fecha de finalización del soporte extendido, ya no se ofrecerá soporte técnico para esta versión del producto. Durante el período de soporte estándar, Contoso mantendrá el soporte técnico para Mojave 14.0 en todas las compilaciones de Windows que se publiquen. Contoso también lanzará actualizaciones para Mojave cuando sea necesario y con independencia de los lanzamientos de productos Windows.

En las siguientes secciones, encontrarás información adicional acerca de las medidas que adopta Microsoft para mantener la compatibilidad del sistema operativo subyacente. También encontrarás instrucciones sobre los pasos que puedes seguir para ayudar a mantener la compatibilidad del ecosistema combinado de la aplicación y el sistema operativo. Hay una sección sobre cómo utilizar las compilaciones piloto de Windows para detectar regresiones de la aplicación antes de que se publique una compilación de Windows. Por último, se describe cómo usamos un enfoque controlado por instrumentación y telemetría para mejorar la calidad de las compilaciones de Windows. Recomendamos a los ISV que adopten un enfoque similar para su cartera de aplicaciones.

## <a name="key-changes-since-windows7-to-ensure-app-compatibility"></a>Cambios clave desde Windows 7 para garantizar la compatibilidad de las aplicaciones

Sabemos que la compatibilidad es importante para los desarrolladores. Los ISV y los desarrolladores quieren garantizar que sus aplicaciones se ejecuten según lo previsto en todas las versiones compatibles del sistema operativo Windows. Este aspecto es clave para la inversión de los consumidores y las empresas: desean saber con seguridad que las aplicaciones por las que han pagado continuarán funcionando. Sabemos que la compatibilidad es el criterio principal para la toma de decisiones de compra. Una escritura correcta de las aplicaciones, de acuerdo con los procedimientos recomendados, dará lugar a una menor renovación de código cuando se publique una nueva versión de Windows y también reducirá la fragmentación (estas aplicaciones cuentan con una inversión en ingeniería reducida para su mantenimiento y comercialización).

En el marco de Windows 7, el enfoque de la compatibilidad era en gran medida reactivo. En Windows 8 comenzó a cambiar nuestra perspectiva de este tema y trabajamos dentro de Windows para garantizar que la compatibilidad se incluyera desde la fase de diseño y no como una idea posterior. Windows 10 es la versión del sistema operativo más compatible por diseño hasta la fecha. A continuación se muestran algunas de los aspectos clave gracias a los que lo hemos conseguido:
-   **Telemetría de la aplicación**: Esto nos ayuda a comprender la popularidad de la aplicación en el ecosistema de Windows para notificar sobre las pruebas de compatibilidad.
-   **Asociaciones con ISV**: Trabajamos directamente con partners externos para proporcionarles datos y ayudar a resolver los problemas que sufren nuestros usuarios.
-   **Revisiones del diseño y detección en dirección ascendente**: Asociación con los equipos de características para reducir el número de cambios importantes en Windows. La revisión de la compatibilidad es una prueba que nuestros equipos de características deben superar.
-   **Comunicación**: Un mayor control sobre los cambios en la API y una comunicación mejorada.
-   **Bucle de distribución de paquetes piloto y comentarios**: Los usuarios de Windows Insider reciben compilaciones piloto que nos ayudan a mejorar la capacidad de encontrar problemas de compatibilidad antes de que una compilación final se ponga a disposición de los clientes. Este proceso de comentarios no solo expone errores, sino que también garantiza que publicamos características que nuestros usuarios desean.

## <a name="best-practices-for-app-compatibility"></a>Procedimientos recomendados para la compatibilidad de las aplicaciones

Microsoft utiliza datos de diagnóstico y uso para identificar y solucionar problemas, mejorar los productos y servicios, y proporcionar experiencias personalizadas a los usuarios. Los datos de uso que recopilamos también incluyen las aplicaciones que se ejecutan en los equipos del ecosistema de Windows. En función de lo que utilizan nuestros clientes, creamos nuestra lista para probar estos dispositivos, aplicaciones y controladores en las nuevas versiones del sistema operativo Windows. Windows 10 ha sido la versión más compatible de Windows hasta la fecha, con más de un 90 % de compatibilidad con miles de aplicaciones populares. El equipo de compatibilidad con Windows contacta habitualmente con los partners ISV para proporcionarles comentarios si se detectan problemas, a fin de buscar soluciones conjuntamente. Lo ideal sería que nuestros clientes comunes pudieran actualizar Windows sin problemas y sin perder ninguna funcionalidad ni en su sistema operativo ni en las aplicaciones de las que dependen para su productividad o entretenimiento.

Las secciones siguientes contienen algunos procedimientos recomendados que Microsoft sugiere para que puedas asegurarte de que tus aplicaciones sean compatibles con Windows 10.

### <a name="windows-version-check"></a>Comprobación de la versión de Windows

La versión del sistema operativo se ha incrementado con Windows 10. Esto significa que el número de versión interno ha cambiado a 10.0. Como ya sucedió en su día, hacemos todo lo posible para mantener la compatibilidad de los dispositivos y las aplicaciones tras un cambio de versión del sistema operativo. En la mayoría de las categorías de aplicaciones (sin ninguna dependencia de kernel) el cambio no afectará negativamente a la funcionalidad de la aplicación, y la mayor parte de las aplicaciones existentes seguirán funcionando correctamente en Windows 10.

La manifestación de este cambio es específica de cada aplicación. Esto significa que cualquier aplicación que compruebe específicamente la versión del sistema operativo obtendrá un número de versión superior, lo que puede ocasionar una o varias de las siguientes situaciones:
-   Es posible que los instaladores de aplicaciones no puedan instalar la aplicación y que las aplicaciones no puedan iniciarse.
-   Las aplicaciones podrían volverse inestables o bloquearse.
-   Las aplicaciones podrían generar mensajes de error, pero seguir funcionando correctamente.

Algunas aplicaciones realizan una comprobación de la versión y simplemente muestran una advertencia a los usuarios. Sin embargo, hay aplicaciones que están enlazadas muy estrechamente a una comprobación de la versión (por ejemplo, en los controladores o en el modo kernel para evitar la detección). En estos casos, si se encuentra una versión incorrecta, se producirá un error en la aplicación. En lugar de una comprobación de la versión, recomendamos uno de los siguientes métodos:
-   Si la aplicación depende de la funcionalidad de una API específica, asegúrate de que apuntas a la versión correcta de dicha API.
-   Asegúrate de detectar el cambio a través de APISet o de otra API pública y de no utilizar la versión como proxy de alguna característica o corrección. Si hay cambios importantes y no se expone una comprobación adecuada, se trata de un error.
-   Asegúrate de que la aplicación NO comprueba la versión de formas extrañas, como a través del registro, versiones de archivos, desplazamientos, el modo kernel, controladores ni otros medios. Si es absolutamente necesario que la aplicación compruebe la versión, usa las API GetVersion, que deberían devolver el número de la versión principal, de la secundaria y de la compilación.
-   Si utilizas la API [GetVersion](/windows/win32/api/sysinfoapi/nf-sysinfoapi-getversion), recuerda que el comportamiento de esta API ha cambiado desde la versión Windows 8.1.

Si tienes aplicaciones como, por ejemplo, aplicaciones antimalware o de firewall, deberías utilizar los canales de comentarios habituales y el programa Windows Insider.

### <a name="undocumented-apis"></a>API sin documentar

Las aplicaciones no deben llamar a API de Windows sin documentar ni depender de exportaciones de archivos o claves del Registro de Windows específicas, ya que esto podría provocar errores en la funcionalidad, pérdida de datos y posibles problemas de seguridad. Si tu aplicación necesita una funcionalidad y esta no se encuentra disponible, tienes la oportunidad de proporcionar comentarios a través de los canales de comentarios habituales y a través del programa Windows Insider.

### <a name="develop-universal-windows-platform-uwp-and-centennial-apps"></a>Desarrollo de aplicaciones para la plataforma universal de Windows (UWP) y Centennial

Animamos a todos los ISV de aplicaciones de Win32 a que en el futuro desarrollen aplicaciones para la [plataforma universal de Windows (UWP)](https://blogs.windows.com/windowsdeveloper/2016/02/25/an-update-on-the-developer-opportunity-and-windows-10/) y, específicamente, para [Centennial](https://channel9.msdn.com/Events/Build/2015/2-692). Desarrollar estos paquetes de la aplicación en lugar de utilizar los instaladores de Win32 tradicionales supone unas enormes ventajas. Las aplicaciones para UWP también se admiten en [Microsoft Store](https://blogs.windows.com/windowsdeveloper/2016/02/04/windows-store-trends-february-2016/), por lo que es más fácil actualizar automáticamente a los usuarios a una versión coherente, y esto reduce los costos de soporte técnico.

Si los tipos de aplicaciones de Win32 no funcionan con el modelo Centennial, es muy recomendable utilizar el instalador correcto y asegurarse de que este se prueba de forma exhaustiva. Un instalador es la primera experiencia del usuario o el cliente con la aplicación, por lo que debe asegurarte de que funciona bien. Con demasiada frecuencia, el instalador no funciona correctamente o no se ha probado de forma completa en todos los escenarios. El [Kit para la certificación de aplicaciones en Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) puede ayudarte a probar la instalación y la desinstalación de la aplicación de Win32, así como a identificar el uso de API sin documentar y otros problemas básicos de procedimientos recomendados relativos al rendimiento, antes de que lo hagan los usuarios.

**Procedimiento recomendado:**
-   Usa instaladores que funcionen para versiones de Windows de 32 bits y de 64 bits.
-   Diseña tus instaladores para que funcionen en varios escenarios (en el nivel de usuario o de equipo).
-   Mantén todos los redistribuibles de Windows en el paquete original; si cambias el paquete, es posible que el instalador no funcione correctamente.
-   Programa el tiempo de desarrollo para tus instaladores, que a menudo se omiten como un entregable durante el ciclo de vida de desarrollo de software.

## <a name="optimized-test-strategies-and-flighting"></a>Estrategias de pruebas optimizadas y distribución de paquetes piloto

La distribución de paquetes piloto del sistema operativo Windows se refiere a las compilaciones provisionales disponibles para los usuarios de Windows Insider antes de que una compilación final se ponga a disposición del público general. Cuantos más usuarios de Insider prueban estas compilaciones provisionales, más comentarios recibimos acerca de la calidad de la compilación, su compatibilidad, etc., lo que ayuda a mejorar la calidad de las compilaciones finales. Puedes participar en este programa de distribución de paquetes piloto para asegurarte de que tus aplicaciones funcionan según lo previsto en compilaciones iterativas del sistema operativo. También te animamos a proporcionar comentarios sobre cómo funcionan estas compilaciones piloto, con qué problemas te encuentras, etc.

Si tu aplicación está en Store, puedes distribuir paquetes piloto de esta desde dicha ubicación, lo que significa que tu aplicación estará disponible para que la instalen los usuarios de Windows Insider. Los usuarios pueden instalar la aplicación y tú puedes recibir comentarios preliminares sobre esta antes de su lanzamiento al público general. En las siguientes secciones se describen los pasos para probar las aplicaciones en compilaciones piloto de Windows.

### <a name="step-1-become-a-windows-insider-and-participate-in-flighting"></a>Paso 1: Conviértete en usuario de Windows Insider y participa en la distribución de paquetes piloto
Como usuario de [Windows Insider,](https://insider.windows.com/) puedes ayudar a definir el futuro de Windows: tus comentarios nos ayudarán a mejorar las características y la funcionalidad de la plataforma. Se trata de una comunidad dinámica en la que podrás conectarte con otros entusiastas, unirte a foros, intercambiar consejos y enterarte de los próximos eventos solo para usuarios de Insider.

Dado que tendrás acceso a versiones preliminares de Windows 10 y Windows 10 Mobile y a los más recientes SDK y emuladores de Windows, tendrás a tu disposición todas las herramientas necesarias para desarrollar excelentes aplicaciones y explorar las novedades de la Plataforma universal de Windows y la Microsoft Store.

También se trata de una excelente oportunidad para crear hardware fantástico, con versiones preliminares de los kits de desarrollo de hardware que te permitirán desarrollar controladores universales para Windows. Asimismo, IoT Core Insider Preview está disponible en los paneles de desarrollo de IoT admitidos, para que puedas crear magníficas soluciones conectadas mediante la plataforma universal de Windows.

Antes de convertirte en usuario de Windows Insider, ten en cuenta que la participación está pensada para usuarios que:
-   Deseen probar software que aún se encuentra en fase de desarrollo.
-   Deseen compartir sus comentarios sobre el software y la plataforma.
-   No les importe que haya un gran número de actualizaciones o un diseño de la interfaz de usuario que pueda cambiar significativamente con el tiempo.
-   Realmente sepan cómo usar el equipo y se sientan cómodos con la resolución de problemas, la realización de copias de seguridad de los datos, el formateo de un disco duro, la instalación de un sistema operativo desde cero o la restauración de uno anterior si es necesario.
-   Sepan lo que es un archivo ISO y cómo utilizarlo.
-   No lo instalen en su equipo o dispositivo de uso cotidiano.

### <a name="step-2-test-your-scenarios"></a>Paso 2: Prueba los escenarios

Cuando hayas actualizado a una compilación piloto, consulta los siguientes ejemplos de casos de prueba, que te servirán como introducción a la realización de pruebas y la recopilación de comentarios. Para la mayoría de estas pruebas, asegúrate de incluir tanto sistemas x86 como AMD64.
**Prueba de instalación limpia:** en una instalación limpia de Windows 10, asegúrate de que la aplicación es completamente funcional. Si la aplicación no supera esta prueba ni la prueba de la actualización, es probable que el problema se deba a cambios del sistema operativo subyacente o errores en la aplicación. Si tras investigarlo, esto es lo que sucede, asegúrate de emplear el programa Windows Insider para proporcionar comentarios y colaborar para buscar soluciones.

**Prueba de actualización:** comprueba que la aplicación funciona tras actualizar desde una versión de nivel inferior de Windows (es decir, Windows 7 o Windows 8.1) a Windows 10. La aplicación no debe provocar reversiones durante la actualización y, tras esta, debe seguir funcionando del modo esperado; esto es crucial para lograr una experiencia de actualización óptima.

**Prueba de reinstalación:** asegúrate de que la funcionalidad de la aplicación puede restaurarse si se reinstala tras actualizar el equipo a Windows 10 desde un sistema operativo de nivel inferior. Si la aplicación no supera la prueba de actualización y no has podido averiguar la causa de estos problemas, es posible que una reinstalación pueda restaurar la funcionalidad perdida. Si se supera la prueba de reinstalación, es posible que algunas partes de la aplicación no se hayan migrado a Windows 10.

**Prueba de características del sistema operativo\\el dispositivo:** asegúrate de que la aplicación funciona según lo esperado si depende de alguna funcionalidad específica del sistema operativo. Las áreas comunes de las pruebas incluyen las que se muestran a continuación, a menudo respecto un conjunto de los modelos de equipo más utilizados para garantizar la cobertura:
-   Audio
-   Funcionalidad de los dispositivos USB (teclado, mouse, memory stick, disco duro externo, etc.)
-   Bluetooth
-   Gráficos\\pantalla (varios monitores, proyección, rotación de pantalla, etc.)
-   Pantalla táctil (orientación, teclado en pantalla, lápiz, gestos, etc.)
-   Panel táctil (botones izquierdo\\derecho, pulsación, desplazamiento, etc.)
-   Lápiz (pulsación individual\\doble, presionar, mantener pulsado, borrador, etc.)
-   Impresión\\digitalización
-   Sensores (acelerómetro, fusión, etc.)
-   Cámara

### <a name="step-3-provide-feedback"></a>Paso 3: Proporciona comentarios

Infórmanos del rendimiento de tu aplicación en las compilaciones piloto. A medida que descubras problemas en la aplicación durante las pruebas, registra los errores a través del portal para partners, si tienes acceso a él, o a través de tu representante de Microsoft. Te animamos a que nos traslades esta información para poder crear juntos una experiencia de calidad para nuestros usuarios.


## <a name="related-topics"></a>Temas relacionados
[Opciones de mantenimiento de Windows 10 para actualizaciones](/windows/manage/introduction-to-windows-10-servicing)