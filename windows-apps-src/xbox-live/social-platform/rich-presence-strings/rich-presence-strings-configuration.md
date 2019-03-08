---
title: Configuración de presencia enriquecida
description: Obtenga información sobre cómo configurar cadenas de la presencia de Xbox Live enriquecido.
ms.assetid: 7b08d46d-d3aa-42eb-93f2-ecd9338fccea
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, presencia enriquecida
ms.localizationpriority: medium
ms.openlocfilehash: d69aaadaa1f64d1cb6eb40b52016a330d924b9b9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590480"
---
# <a name="rich-presence-configuration"></a>Configuración de presencia enriquecida

Habrá una parte de la Xbox Developer Portal (XDP) dedicada a la configuración de las cadenas de presencia enriquecida. Hasta que está disponible en XDP, se debe usar la configuración manual. Configuración manual implica definir las cadenas en una plantilla de hoja de cálculo proporcionado por el Administrador de cuentas de desarrollador y enviarlo a su administrador de cuentas de desarrollador para la ingesta de datos en el entorno.

-   **Cadenas**
-   **Enumeraciones**
-   **Ingesta**
-   **Juego de ejemplo**
-   **Configuración de estadísticas de presencia enriquecida**
-   **Ejemplo de configuración de la enumeración**
-   **Ejemplo de cadena de configuración**


## <a name="strings"></a>Strings

Para las cadenas de presencia enriquecida, debe definir los conjuntos de cadena. Cada conjunto de cadena debe tener un nombre descriptivo que se usará como identificador. Cada conjunto de cadena creado debe contener un grupo de cadenas traducidas para las configuraciones regionales en el que se va a publicar el título. Además de las cadenas localizadas, también debe proporcionar una cadena de referencia cultural neutra. Esta cadena se utiliza si una solicitud realizada para una variable local que no se proporcionó.

También puede hacer que cada conjunto de cadena usar de datos en el juego por parametrizar las cadenas para mejorar aún más en ellos. Un conjunto de cadena puede tener tantos parámetros como desee, siempre y cuando las cadenas resultantes no pasan por las limitaciones de caracteres. Consulte [presencia enriquecida de cadenas: Las directivas y las limitaciones](rich-presence-strings-policies-and-limitations.md) para obtener más detalles.


## <a name="enumerations"></a>Enumeraciones

Las enumeraciones que se describe a continuación se definen a través de los eventos de la plataforma de datos.


## <a name="statistics"></a>Estadísticas

Puede usar valores numéricos de estadística de las cadenas de presencia enriquecida. Por ejemplo, en un solucionador, tal vez desee la cadena de presencia enriquecida para mostrar cuántos derribos el Reproductor ha recibido hasta ahora. La configuración solo se necesita mostrar que el parámetro se debe reemplazar la estadística "Recuento de interrupción". La estadística "Kill Count" se creó a través del sistema de eventos de esquema comunes a través de la plataforma de datos y debe definirse a través de la configuración de estadísticas. A continuación, cuando se lee la cadena de otro usuario, el servicio ir y recuperar el valor más actualizado para la estadística "Recuento Kill" lo que permite la cadena siempre esté actualizada.


## <a name="state"></a>Estado

También puede utilizar las estadísticas que representan valores de estado real de las cadenas de presencia enriquecida. Por ejemplo, en un juego de carreras, podría utilizar una cadena de presencia enriquecida para mostrar qué automóvil actualmente está impulsando el Reproductor, o qué mapa o se acelera el Reproductor en el seguimiento. Hay dos pasos para configurar la información de estado:

1.  Para obtener información de estado, definir las enumeraciones para las cadenas del juego. Por ejemplo, si tiene cinco asignaciones en el juego y desea que los mapas para formar parte de las cadenas de presencia enriquecida, los mapas de cinco deben mostrarse en las cadenas localizadas también.
2.  Vincule las enumeraciones a las estadísticas. En este caso, de nuevo, el servicio recuperará el valor más actualizado de la plataforma de datos buscando el valor de la estadística. A continuación, determina qué valor de enumeración se convierte en la estadística.


## <a name="ingestion"></a>Ingesta

Una vez haya completado la hoja de cálculo, enviarlo a su administrador de cuentas de desarrollador para la ingesta. El equipo de presencia enriquecida revisará la configuración y asegúrese de que esté configurado correctamente y, a continuación, haber introducidos en el servicio.


## <a name="example-game"></a>Juego de ejemplo

El resto del ejemplo, supongamos que estoy trabajando en el depósito más reciente de entrar en juego. Hay una gran cantidad de tipos diferentes de depósitos de mi juego como cubos de madera, plata depósitos y golden depósitos. Decidir crear una estadística para cada tipo de depósito expulsado. También hay distintos lugares en mi juego en el que se va a iniciar los depósitos, como las Montañas, el desierto de y la playa. Una vez que se inicie un depósito, destruye, por lo que debe siempre se puede buscar más cubos. Algunos cubos requieren que tenga especial de engranaje (necesita un arranque suficientemente seguro para resistir una puesta en marcha) para iniciar correctamente el cubo. El juego incluye un modo multijugador, por lo que si suficiente reproductores están intentando morir al mismo tiempo, no es necesario el arranque adecuado.

Echemos un vistazo a la configuración de la cadena de presencia enriquecida es similar a este juego.


## <a name="configuring-statistics-for-rich-presence"></a>Configuración de estadísticas de presencia enriquecida

Para usar una estadística generada por la plataforma de datos en las cadenas de presencia enriquecida, el servicio debe conocer los nombres de las estadísticas. Debe incluir esta información en la sección de la plantilla de hoja de cálculo que le pide que especifique las estadísticas que se usa en las cadenas.


## <a name="enumeration-configuration-example"></a>Ejemplo de configuración de la enumeración

Este es un ejemplo de definición de un par de enumeraciones. En primer lugar, creamos una enumeración de las asignaciones en el juego. El juego tiene tres asignaciones que obtención un nombre descriptivo para que podemos hacer referencia a ellos fácilmente. A continuación, para cada configuración regional, creamos una cadena traducida para ese nombre de mapa. Tenga en cuenta que hay una localización predeterminada global que también una referencia cultural neutra.

A continuación, veremos la armas en el juego. Hay tres armas en mi juego, donde estos nombres descriptivos y las cadenas localizadas. Hacemos esto para todas las enumeraciones.

Enumeración | Estadísticas relacionadas | Nombre descriptivo | Locale | Cadena
----------- | ----------------- | ------------- | ------ | ----
Asignar | CurrentMap | Map_Mountains | valor predeterminado | Montañas
 |  |  |  | es | Montañas
 |  |  |  | en-US | Montañas
 |  |  |  | en-GB | Montañas
 |  |  |  |  de | Gebirge
 | |  |  | etc |
 | |  | Map_Desert | valor predeterminado | Desert
 |  ||  |  es | Desert
 |  ||  |  en-US | Desert
 |  ||  |   en-GB | Desert
 |  ||  |  de | Wuste
 |  ||  |  etc |
| |  |  Map_Beach | valor predeterminado | Playa
 ||    |  | es | Playa
 ||    |  | en-US | Playa
 ||    |  | en-GB | Playa
 ||    |  | de | Strand
 | |   |  | etc |
Arranque | CurrentWeapon | Boot_Light | valor predeterminado | Claro
 |  ||  |  es | Claro
 |  ||  |   en-US | Claro
 |  ||  |   en-GB | Claro
 |  ||  |   de | Leicht
 |  ||  |   etc  |
 |  | |  Boot_Medium | valor predeterminado | Medio
 |  |  |  | es | Medio
 |  |  |  | en-US | Medio
 |  |  |  | en-GB | Medio
 |  |  |  | de | Mittel
 |  |  |  | etc |
 |  | |  Boot_Strong | valor predeterminado | Seguro
 |  |  |  | es | Seguro
 |  |  |  | en-US | Seguro
 |  |  |  | en-GB | Seguro
 |  |  |  | de | Descolorido
 |  ||  | etc

## <a name="string-configuration-example"></a>Ejemplo de cadena de configuración

Ahora que se han definido las enumeraciones y las estadísticas, puede crear las cadenas.

En esta tabla de ejemplo siguiente, la primera columna es un nombre descriptivo para ayudar a hacer referencia a un conjunto de cadena frente a otro. En este ejemplo, hemos decidido utilizar la cadena "playingMap" para el primer conjunto de cadena y, a continuación, "totalKicked" para el segundo conjunto de cadena y, después, "participan varios jugadores" para la tercera.

Las dos columnas van juntas. Estos son los pares de cadena de configuración regional de la cadena de presencia enriquecida. En el primer ejemplo, queremos usar la cadena en inglés "jugar en {0}". El {0} símbolo (token) se reemplazará por un valor. A continuación, nos localizar la cadena en otras configuraciones regionales. Al igual que en las enumeraciones, debe ser un valor predeterminado, así como las cadenas de referencia cultural neutra para usarse en el caso de que un usuario está usando una configuración regional que no ha especificado un par de cadena de configuración regional. Las configuraciones regionales se utilizan para las enumeraciones, debe coincidir con las configuraciones regionales para las cadenas.

Es la última columna para los parámetros utilizados para rellenar los espacios en blanco en la cadena de presencia enriquecida. Si desea que el servicio de presencia para buscar el valor del parámetro en el servicio de estadísticas, debe usar el nombre de DO para hacer referencia al parámetro aquí. Uso de las estadísticas en las cadenas de, podrá establecer la cadena y, a continuación, no se preocupe. Si la cadena tiene el nombre del mapa en ella, y usa el CurrentMap Stat para rellenar el espacio en blanco, a continuación, el servicio actualizará la cadena como corresponda, durante la transmisión de los jugadores de asignación para asignar en el juego. Si no utiliza las estadísticas en las cadenas de presencia enriquecida, no se puede especificar ningún parámetro.

En el ejemplo siguiente, desea utilizar el objeto map actual generado por las estadísticas de la primera cadena y, a continuación, la estadística BucketsKicked en la segunda cadena. Tenga en cuenta que el tercer ejemplo no incluye ningún parámetro. La cadena se define por sí mismo. No hace referencia a las estadísticas.

Nombre descriptivo | Locale | Cadena | Parámetros
--- | --- | --- | ---
playingMap | valor predeterminado | Reproducción en curso en el mapa:{0} | CurrentMap
 |  | es | Reproducción en curso en el mapa:{0} |
 |  | en-US | Reproducción en curso en el mapa:{0} |
 |  | en-GB | Reproducción en curso en el mapa:{0} |
 |  | de | Spielt auf Karte: {0} |
 |  | etc. | |
totalKicked | valor predeterminado | Inicie {0} depósitos! | BucketsKicked
 |  | es | Inicie {0} depósitos! |
 |  | en-US | Inicie {0} depósitos! |
 |  | en-GB | Inicie {0} depósitos! |
 |  | de | {0} ¡Eimer getreten! |
 |  | etc. | |
multijugador | valor predeterminado | Reproducción de varios jugadores |
 |  | es | Reproducción de varios jugadores |
 |  | en-US | Reproducción de varios jugadores |
 |  | en-GB | Reproducción de varios jugadores |
 |  | de | Spielt Mehrspieler |
 |  | etc. | 

No hay ningún límite sobre cuántas cadenas puede crear, pero debe tener al menos una cadena del título.

"BucketsKicked" es simplemente un número que obtenga puede reemplazar en la cadena. "CurrentMap" es un ejemplo de una estadística de la cadena de procedencia de la enumeración. Cuando el título de conjuntos de presencia enriquecida a una de estas cadenas, usamos la configuración para saber qué parámetros son necesarios.
