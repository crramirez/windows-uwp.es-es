---
title: Desarrollo de aplicaciones para Windows como servicio
description: Separa el lanzamiento de la aplicación y el soporte técnico de las compilaciones específicas de Windows.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f384ca56-f2b2-4793-b251-f7f5735376bb
ms.localizationpriority: medium
ms.openlocfilehash: 0629201b695f6df6f7f3e2084a73d72b10b82be5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658640"
---
# <a name="application-development-for-windows-as-a-service"></a>Desarrollo de aplicaciones para Windows como servicio

**Se aplica a**
-   Windows 10
-   Windows 10 Mobile
-   Windows 10 IoT Core 

En el entorno actual, donde las expectativas del usuario con frecuencia las establecen las experiencias centradas en los dispositivos, los ciclos completos de un producto se deben medir en meses, no en años. Además, las nuevas versiones deben estar disponibles de forma continua y se deben poder implementar con el mínimo impacto para los usuarios. Microsoft diseñó Windows 10 para satisfacer estos requisitos al implementar un nuevo método de desarrollo de innovación y entrega denominado [Windows como servicio (WaaS)](https://docs.microsoft.com/windows/deployment/update/waas-overview). La clave para permitir ciclos de producto significativamente más cortos a la vez que se mantienen niveles de alta calidad es un innovador método de pruebas centrado en la comunidad que Microsoft puso en marcha para Windows 10. La comunidad, conocida como Windows Insider, incluye millones de usuarios en todo el mundo. Cuando los usuarios de Windows Insider deciden participar en la comunidad, prueban muchas compilaciones a lo largo del ciclo de un producto y proporcionan comentarios a Microsoft a través de una metodología iterativa denominada distribución de paquetes piloto.

Las compilaciones que se distribuyen como paquetes piloto proporcionan al equipo de ingeniería de Windows datos significativos sobre el funcionamiento en uso real de las compilaciones. El Programa Windows Insider con los usuarios de Windows Insider también permite a Microsoft probar compilaciones en entornos de hardware, aplicaciones y redes mucho más diversos que lo podía hacer en el pasado y también identificar los problemas de manera mucho más rápida. Como resultado, Microsoft cree que la distribución de paquetes piloto centrada en la comunidad permitirá una entrega de innovación mucho más rápida, así como una calidad mucho más elevada de versiones destinadas al público en general.

## <a name="windows-10-release-types-and-cadences"></a>Tipos y cadencias de lanzamiento de Windows 10

Aunque Microsoft lanza compilaciones piloto para usuarios de Windows Insider, Microsoft publicará continuamente dos tipos de versiones de Windows 10 de manera amplia destinadas al público en general:

**Las actualizaciones de características** instale las últimas características, experiencias y capacidades en dispositivos que ya ejecutan Windows 10. Dado que las actualizaciones de funciones contienen una copia completa de Windows, también son lo que los clientes usan para instalar Windows 10 en dispositivos existentes que ejecutan Windows 7 o Windows 8.1 y en dispositivos nuevos que no tienen instalado ningún sistema operativo. Microsoft espera poder publicar actualizaciones semestrales. 

**Actualizaciones de calidad** que proporcionan resoluciones de problemas de seguridad y otras correcciones de errores importantes. Se proporcionarán actualizaciones de calidad para mejorar cada característica compatible actualmente, a un ritmo de una o más veces al mes. Microsoft seguirá publicando actualizaciones de calidad los martes de actualización (conocidos a veces como martes de revisiones). Además, es posible que Microsoft publique actualizaciones de calidad adicionales para Windows 10 fuera del proceso de los martes de actualización cuando sea necesario para satisfacer las necesidades de los clientes.

Durante el desarrollo de Windows 10, Microsoft perfeccionó el ciclo de ingeniería y lanzamiento de los productos de Windows para que se puedan ofrecer las características, las experiencias y la funcionalidad que desean los clientes, y de manera más rápida que nunca. También creó nuevas formas de entregar e instalar actualizaciones de características y actualizaciones de calidad que simplifican las implementaciones y la administración continua, amplían la base de empleados que se puede mantener actualizada con las capacidades y experiencias de Windows más recientes y reducen el coste total de propiedad. Por lo tanto, hemos implementado nuevas opciones de mantenimiento: contempladas como canal semianual y el canal de mantenimiento de a largo plazo (LTSC) – que ofrecen soluciones pragmáticas para mantener la más actual de más dispositivos en los entornos empresariales que antes era posible.

La siguiente tabla se describen los distintos canales de mantenimiento y sus atributos claves.

| Opción de mantenimiento | Disponibilidad de actualizaciones de nuevas funciones para la instalación | Duración de mantenimiento | Principales ventajas | Ediciones admitidas |
| --- | --- | --- | --- | --- |
| Canal semianual (dirigido) | Inmediatamente después de la publicación inicial de parte de Microsoft | 18 meses | Pone nuevas funciones a la disposición de los usuarios tan pronto como sea posible | Home, Pro, Education, Enterprise, Mobile, IoT Core, Windows 10 IoT Core Pro (IoT Core Pro) |
| Canal semianual | 4 meses, aproximadamente, después de la publicación inicial de parte de Microsoft | 18 meses a partir de cuándo se publicó por primera vez | Proporciona tiempo adicional para probar las actualizaciones de nuevas funciones antes de la implementación. | Pro, Education, Enterprise, Mobile Enterprise, IoT Core Pro |
| Canal de mantenimiento a largo plazo (LTSC) | Inmediatamente después de la publicación de parte de Microsoft | 10 años | Permite la implementación a largo plazo de versiones de Windows 10 seleccionadas en configuraciones en las que no se realizan cambios con frecuencia | Enterprise LTSB |

Para obtener más información, consulta [Opciones de actualizaciones y mantenimiento de Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels).

## <a name="supporting-apps-in-windows-as-a-service"></a>Compatibilidad con las aplicaciones en Windows como servicio

El método tradicional para que las aplicaciones fueran compatibles era publicar una nueva versión de la aplicación en respuesta a una nueva versión de Windows. Esto presupone que se han producido cambios importantes en el sistema operativo subyacente que podrían provocar una regresión de la aplicación. Este modelo implica un ciclo de desarrollo y validación dedicado que requiere que nuestros ISV asociados se alineen con el ritmo de publicación de las versiones de Windows.

En el modelo Windows como servicio, Microsoft se compromete a mantener la compatibilidad del sistema operativo subyacente. Esto significa que Microsoft hará un esfuerzo coordinado para garantizar que no se produzcan cambios importantes que afecten negativamente al ecosistema de las aplicaciones. En este escenario, cuando se publique una compilación de Windows, la mayoría de las aplicaciones (aquellas que no tengan dependencias de kernel) seguirán funcionando.

En vista de este cambio, Microsoft recomienda a sus ISV asociados que desacoplen la versión y la compatibilidad de sus aplicaciones con compilaciones específicas de Windows. Los clientes reciben un mejor servicio si el enfoque se centra en el ciclo de vida de las aplicaciones. Esto significa que cuando se publique una versión de la aplicación, esta se admitirá durante un determinado período de tiempo independientemente del modo en que muchas compilaciones de Windows se publiquen de forma provisional. El ISV se compromete a proporcionar compatibilidad para esa versión específica de la aplicación mientras esta se admita en el ciclo de vida. Microsoft sigue un enfoque similar del ciclo de vida para Windows, que se puede consultar [aquí](https://go.microsoft.com/fwlink/?LinkID=780549).

Este enfoque reduce la carga que supone mantener una programación de la aplicación que se alinee con las versiones de Windows. Los ISV asociados deberían poder publicar características o actualizaciones a su propio ritmo. Creemos que nuestros partners pueden tener al día a todos sus clientes con las últimas actualizaciones de sus aplicaciones, con independencia de la versión de Windows. Además, nuestros clientes no tienen que buscar una declaración de compatibilidad explícita siempre que se publique una compilación de Windows. Este es un ejemplo de una declaración de compatibilidad que abarca cómo puede ser la compatibilidad de una aplicación en diferentes versiones del sistema operativo:

| Ejemplo de declaración de compatibilidad del ciclo de vida de una aplicación | |
| --- | --- |
| Contoso es una empresa de desarrollo de software y es propietaria de la popular aplicación de Mojave, que cuenta con una amplia difusión en el mundo empresarial. Contoso publica su próxima versión principal, Mojave 14.0, y declara que esta disfrutará de soporte estándar durante un periodo de tres años a partir de la fecha de lanzamiento. Durante el soporte estándar, todas las actualizaciones y el soporte técnico serán gratuitos para el producto con licencia. Contoso también declara que existirán dos años adicionales de soporte extendido durante los cuales los clientes podrán comprar actualizaciones y soporte técnico durante un período de gracia. A partir de la fecha de finalización del soporte extendido, esta versión del producto ya no será compatible. Durante el período de soporte estándar, Contoso mantendrá la compatibilidad de Mojave 14.0 en todas las compilaciones de Windows que se publiquen. Contoso también lanzará actualizaciones para Mojave cuando sea necesario y con independencia de las versiones de los productos de Windows. | |

En las siguientes secciones, encontrarás información adicional acerca de las medidas que adopta Microsoft para mantener la compatibilidad del sistema operativo subyacente. También encontrarás instrucciones sobre los pasos que puedes seguir para ayudar a mantener la compatibilidad del ecosistema combinado de la aplicación y el sistema operativo. Hay una sección sobre cómo sacar provecho de las compilaciones de distribución de paquetes piloto de Windows para detectar regresiones de la aplicación antes de que se publique una compilación de Windows. Por último, se describe cómo usamos un enfoque controlado por instrumentación y telemetría para aumentar la calidad de las compilaciones de Windows. Recomendamos a los ISV que adopten un enfoque similar para su cartera de aplicaciones.

## <a name="key-changes-since-windows7-to-ensure-app-compatibility"></a>Cambios clave desde Windows 7 para garantizar la compatibilidad de las aplicaciones

Sabemos que la compatibilidad es importante para los desarrolladores. Los ISV y los desarrolladores quieren garantizar que sus aplicaciones funcionen según lo previsto en todas las versiones compatibles del SO Windows. Este aspecto es clave para la inversión de los consumidores y las empresas: desean saber con seguridad que las aplicaciones por las que han pagado continuarán funcionando. Sabemos que la compatibilidad es el criterio principal para la toma de decisiones de compra. Las aplicaciones que estén bien escritas, basándose en los procedimientos recomendados, obligarán a una menor renovación del código cuando se publique una nueva versión de Windows, lo que reducirá la fragmentación; además, se reducirá la inversión en ingeniería para el mantenimiento de estas aplicaciones y se lanzará al mercado mucho más rápido.

En el marco de Windows 7, el enfoque hacia la compatibilidad era mucho más reactivo. En Windows 8, comenzamos a entrar en esto diferente, trabajar en Windows para garantizar la compatibilidad de esa era por diseño en lugar de algo. Windows 10 es la versión del sistema operativo más compatible por diseño hasta la fecha. A continuación se muestran algunas de los aspectos clave gracias a los que lo hemos conseguido:
-   **Telemetría de aplicación**: Esto nos ayuda a entender la popularidad de la aplicación en el ecosistema de Windows para informar a las pruebas de compatibilidad.
-   **Las asociaciones de ISV**: Trabajar directamente con socios externos para proporcionar datos y ayudar a solucionar los problemas que experimentan los usuarios.
-   **Revisiones de diseño, la detección de nivel superior**: Los equipos asociados con la característica para reducir el número de cambios importantes en Windows. La revisión de la compatibilidad es una prueba que nuestros equipos de características deben superar.
-   **Comunicación**: Un mayor control sobre los cambios en la API y mejorar la comunicación.
-   **Bucle de comentarios y distribución de paquetes piloto**: Insider de Windows recibe las compilaciones no finales que ayudan a mejorar nuestra capacidad para detectar problemas de compatibilidad antes de una compilación final se libera a los clientes. Este proceso de comentarios no solo expone errores, sino que también garantiza que publicamos características que quieren nuestros usuarios.

## <a name="best-practices-for-app-compatibility"></a>Procedimientos recomendados para la compatibilidad de las aplicaciones

Microsoft utiliza datos de diagnóstico y uso para identificar y solucionar problemas, mejorar los productos y servicios y proporcionar experiencias personalizadas a los usuarios. Los datos de uso que recopilamos también incluyen las aplicaciones que se ejecutan en los equipos del ecosistema de Windows. Creamos nuestra lista en función de lo que utilizan nuestros clientes y probamos estas aplicaciones, dispositivos y controladores en las nuevas versiones del SO Windows. Windows 10 ha sido la versión más compatible de Windows hasta la fecha, con más de un 90 % de compatibilidad con miles de aplicaciones populares. El equipo de compatibilidad de Windows contacta con los ISV asociados para proporcionarles comentarios si se detectan problemas, de forma que podamos colaborar en la búsqueda de soluciones. Lo ideal sería que nuestros clientes comunes pudieran actualizar Windows sin problemas y sin perder ninguna funcionalidad ni en su sistema operativo ni en las aplicaciones de las que dependen para su productividad o entretenimiento.

Las secciones siguientes contienen algunos procedimientos recomendados que Microsoft sugiere para que puedas asegurarte de que tus aplicaciones sean compatibles con Windows 10.

### <a name="windows-version-check"></a>Comprobación de la versión de Windows

La versión del sistema operativo ha aumentado con Windows 10. Esto significa que el número de versión interno se ha cambiado a 10.0. Como ya hicimos en su día, hacemos todo lo posible para mantener la compatibilidad de los dispositivos y las aplicaciones tras un cambio de la versión del sistema operativo. Para la mayoría de categorías de aplicaciones (sin las dependencias de kernel), el cambio no afectará negativamente la funcionalidad de la aplicación y las aplicaciones existentes seguirán funcionando correctamente en Windows 10.

La manifestación de este cambio es específico de cada aplicación. Esto significa que cualquier aplicación que compruebe específicamente la versión del sistema operativo obtendrá un número de versión superior, lo que puede ocasionar una o varias de las siguientes situaciones:
-   Los instaladores de las aplicaciones podrían no ser capaces de instalar la aplicación y las aplicaciones podrían no iniciarse.
-   Las aplicaciones podrían volverse inestables o bloquearse.
-   Las aplicaciones podrían generar mensajes de error pero seguir funcionando correctamente.

Algunas aplicaciones realizan una comprobación de la versión y simplemente transmiten una advertencia a los usuarios. Sin embargo, hay aplicaciones que están enlazadas muy estrechamente a una comprobación de la versión (en los controladores, o en el modo kernel para evitar la detección). En dichos casos, si se encuentra una versión incorrecta, se producirá un error en la aplicación. En lugar de una comprobación de la versión, recomendamos uno de los siguientes métodos:
-   Si la aplicación depende de la funcionalidad de una API específica, asegúrate de que apuntas a la versión correcta de dicha API.
-   Asegúrate de detectar el cambio a través de APISet o de otra API pública y de no utilizar la versión como proxy de alguna característica o corrección. Si hay cambios importantes y no se expone una comprobación adecuada, se trata de un error.
-   Asegúrate de que la aplicación NO comprueba la versión de formas extrañas, como a través del registro, versiones de archivos, desplazamientos, el modo kernel, controladores ni otros medios. Si es absolutamente necesario que la aplicación compruebe la versión, usa las API GetVersion, que deberían devolver el número de la versión principal, de la secundaria y de la compilación.
-   Si utilizas la API [GetVersion](https://go.microsoft.com/fwlink/?LinkID=780555), recuerda que el comportamiento de esta API ha cambiado desde Windows 8.1.

Si tienes aplicaciones como aplicaciones antimalware o de firewall, deberías utilizar los canales de comentarios habituales y el programa Windows Insider.

### <a name="undocumented-apis"></a>API sin documentar

Las aplicaciones no deben llamar a API de Windows sin documentar ni depender de exportaciones de archivos o claves del Registro de Windows específicas, ya que esto podría provocar errores en la funcionalidad, pérdida de datos y posibles problemas de seguridad. Si tu aplicación necesita una funcionalidad y esta no se encuentra disponible, tienes la oportunidad de proporcionar comentarios a través de los canales de comentarios habituales y a través del programa Windows Insider.

### <a name="develop-universal-windows-platform-uwp-and-centennial-apps"></a>Desarrollo de aplicaciones para la plataforma universal de Windows (UWP) y Centennial

Animamos a todos los ISV de aplicaciones de Win32 a que en el futuro desarrollen aplicaciones para la [plataforma universal de Windows (UWP)](https://go.microsoft.com/fwlink/?LinkID=780560) y, específicamente, para [Centennial](https://go.microsoft.com/fwlink/?LinkID=780562). Desarrollar estos paquetes de aplicación en lugar de utilizar los instaladores de Win32 tradicionales supone unas enormes ventajas. También se admiten las aplicaciones para UWP en el [Microsoft Store](https://go.microsoft.com/fwlink/?LinkID=780563), por lo que es más fácil de actualizar automáticamente, los usuarios a una versión coherente, reduce los costos de soporte técnico.

Si los tipos de aplicaciones de Win32 no funcionan con el modelo Centennial, es muy recomendable utilizar el instalador correcto y asegurarse de que este se prueba de forma exhaustiva. Un instalador es la primera experiencia del usuario o el cliente con la aplicación, por lo que deberías asegurarte de que funciona bien. Con demasiada frecuencia, el instalador no funciona correctamente o no se ha probado de forma completa en todos los escenarios. El [Kit para la certificación de aplicaciones en Windows](https://go.microsoft.com/fwlink/?LinkID=780565) puede ayudarte a probar la instalación y la desinstalación de la aplicación de Win32, así como a identificar el uso de API sin documentar y otros problemas básicos de procedimientos recomendados relativos al rendimiento, antes de que lo hagan los usuarios.

**Procedimientos recomendados:**
-   Usa instaladores que funcionen para versiones de Windows de 32 bits y de 64 bits.
-   Diseña tus instaladores para que funcionen en varios escenarios (en nivel de usuario o de equipo).
-   Mantén todos los redistribuibles de Windows en el paquete original; si cambias tu paquete, es posible que el instalador no funcione correctamente.
-   Programa el tiempo de desarrollo para tus instaladores, a menudo, durante el ciclo de vida de desarrollo de software se olvida que los instaladores son un Entregable.

## <a name="optimized-test-strategies-and-flighting"></a>Estrategias de pruebas optimizadas y distribución de paquetes piloto

La distribución de paquetes piloto del SO Windows se refiere a las compilaciones provisionales disponibles para los usuarios de Windows Insider antes de que una compilación final se ponga a disposición del gran público. Cuantos más usuarios de Insider prueben estas compilaciones provisionales, más comentarios recibimos acerca de la calidad de la compilación, su compatibilidad, etc., lo que ayuda a mejorar la calidad de las compilaciones finales. Puedes participar en este programa de distribución de paquetes piloto para asegurarte de que tus aplicaciones funcionen según lo esperado en compilaciones iterativas del sistema operativo. También te animamos a que proporciones comentarios sobre cómo te funcionan estas compilaciones piloto, con qué problemas te encuentras, etc.

Si tu aplicación está en la Tienda, puedes distribuir paquetes piloto de ella a través de la propia Tienda, lo que significará que tu aplicación estará disponible para que la instalen los usuarios de Windows Insider. Los usuarios podrán instalar la aplicación y tú podrás recibir comentarios preliminares sobre ella antes de su lanzamiento al gran público. Las siguientes secciones describen los pasos para probar las aplicaciones en compilaciones piloto de Windows.

### <a name="step-1-become-a-windows-insider-and-participate-in-flighting"></a>Paso 1: Convertirse en un especialista en Windows y participar en la distribución de paquetes piloto
Como usuario de [Windows Insider,](https://go.microsoft.com/fwlink/p/?LinkId=521639) podrás ayudar a determinar el futuro de Windows: tus comentarios nos ayudarán a mejorar las características y la funcionalidad de la plataforma. Se trata de una comunidad dinámica en la que podrás conectarte con otros entusiastas, unirte a foros, intercambiar consejos y enterarte de los próximos eventos solo para usuarios de Insider.

Puesto que tendrá acceso a la vista previa de las compilaciones de Windows 10, Windows 10 Mobile y el último SDK de Windows y el emulador, tendrá todas las herramientas a su disposición para desarrollar aplicaciones increíbles y explorar Novedades de la plataforma Universal de Windows y la Microsoft Store.

También se trata de una excelente oportunidad para crear un hardware fantástico, con versiones preliminares de los kits de desarrollo de hardware que te permitirán desarrollar controladores universales para Windows. Asimismo, IoT Core Insider Preview está disponible en los paneles de desarrollo de IoT admitidos, para que puedas generar magníficas soluciones conectadas mediante la plataforma universal de Windows.

Antes de convertirte en usuario de Windows Insider, ten en cuenta que la participación está pensada para usuarios que:
-   Deseen probar software que aún se encuentra en fase de desarrollo.
-   Deseen compartir sus comentarios sobre el software y la plataforma.
-   No les importe que haya un gran número de actualizaciones o un diseño de la interfaz de usuario que pueda cambiar significativamente con el tiempo.
-   Realmente sepan cómo usar el equipo y se sientan cómodos con la solución de problemas, la realización de copias de seguridad de los datos, el formateo de un disco duro, la instalación de un sistema operativo desde cero o la restauración de uno anterior si es necesario.
-   Sepan lo que es un archivo ISO y cómo utilizarlo.
-   No lo instalen en su equipo o dispositivo de uso cotidiano.

### <a name="step-2-test-your-scenarios"></a>Paso 2: Los escenarios de prueba

Cuando hayas actualizado a una compilación piloto, a continuación hay algunos ejemplos de casos de prueba que te servirán como introducción a la realización de pruebas y la recopilación de comentarios. Para la mayoría de estas pruebas, asegúrate de incluir tanto sistemas x86 como sistemas AMD64.
**Una instalación limpia de prueba:** En una instalación limpia de Windows 10, asegúrate de que la aplicación es completamente funcional. Si la aplicación no supera esta prueba ni la prueba de la actualización, es probable que el problema se deba a cambios del sistema operativo subyacente o errores en la aplicación. Si tras investigarlo, esto es lo que sucede, asegúrate de emplear el programa Windows Insider para proporcionar comentarios y asociarte para buscar soluciones.

**Prueba de actualización:** Comprueba que la aplicación funciona tras actualizar desde una versión de nivel inferior de Windows (es decir, Windows 7 o Windows 8.1) a Windows 10. La aplicación no debe provocar reversiones durante la actualización y, tras esta, debe seguir funcionando del modo esperado; esto es crucial para lograr una experiencia de actualización óptima.

**Vuelva a instalar la prueba:** Asegúrate de que la funcionalidad de la aplicación puede restaurarse si se reinstala tras actualizar el equipo a Windows 10 desde un sistema operativo de nivel inferior. Si la aplicación no supera la prueba de actualización y no has podido averiguar la causa de estos problemas, es posible que una reinstalación sea capaz de restaurar la funcionalidad perdida. Si se supera la prueba de reinstalación, quiere decir que posiblemente algunas partes de la aplicación no se hayan migrado a Windows 10.

**Sistema operativo\\características del dispositivo de prueba:** Asegúrate de que la aplicación funciona según lo esperado si se basa en alguna funcionalidad específica del sistema operativo. Las áreas comunes de las pruebas incluyen las que se muestran a continuación, a menudo respecto un conjunto de los modelos de equipo más utilizados para garantizar la cobertura:
-   Audio
-   Funcionalidad de los dispositivos USB (teclado, mouse, stick de memoria, disco duro externo etc.)
-   Bluetooth
-   Gráficos\\mostrar (varios monitores, proyección, rotación de pantalla etc.)
-   Pantalla táctil (orientación, teclado en pantalla, lápiz, gestos, etc.)
-   Panel táctil (izquierdo\\botones derechos, pulse, desplazamiento y así sucesivamente)
-   Lápiz (solo\\doble punteo, presione, suspensión, borrador etc.)
-   Impresión\\Scan
-   Sensores (acelerómetro, fusión, etc.)
-   Cámara

### <a name="step-3-provide-feedback"></a>Paso 3: Enviar comentarios

Infórmanos del rendimiento de tu aplicación en las compilaciones piloto. A medida que descubras problemas en la aplicación durante las pruebas, registra los errores a través del portal para partners, si tienes acceso a él, o a través de tu representante de Microsoft. Te animamos a que nos traslades esta información para poder crear juntos una experiencia de calidad para nuestros usuarios.

### <a name="step-4-register-on-ready-for-windows"></a>Paso 4: Registrar en listo para Windows
El sitio web [Ready For Windows](https://go.microsoft.com/fwlink/?LinkID=780580) es un directorio del software compatible con Windows 10. Está destinado a los administradores de TI de empresas y organizaciones de todo el mundo que están pensando en la implementación de Windows 10. Los administradores de TI pueden consultar el sitio para ver que si el software implementado en su empresa es compatible con Windows 10.

## <a name="related-topics"></a>Temas relacionados
[Las opciones para las actualizaciones de mantenimiento de Windows 10](https://technet.microsoft.com/itpro/windows/manage/introduction-to-windows-10-servicing)
