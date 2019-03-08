---
title: Almacenamiento de información de Xbox Live título
description: Obtenga información sobre cómo usar el almacenamiento de título de Xbox Live para almacenar información de juegos para un título en la nube.
ms.assetid: a4182bc8-d232-4e77-93ae-97fe17ac71b1
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, juegos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 472a2131c113cdde5d7383e169e4fbe638e4e555
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623930"
---
# <a name="xbox-live-title-storage"></a>Almacenamiento de información de Xbox Live título

El servicio de almacenamiento de título de Xbox Live proporciona una manera de almacenar información de juegos para un título en la nube. Los juegos que se ejecutan en todas las plataformas pueden usar este servicio.

<a name="ID4EW"></a>

## <a name="features-of-xbox-live-title-storage"></a>Características de almacenamiento de título de Xbox Live

Algunas de las características de alto nivel de almacenamiento de título de Xbox Live incluyen, pero no se limitan a:

-   Se pueden compartir entre varias plataformas, títulos y los usuarios
-   Admite JSON, binario y los archivos de configuración

Las características principales de almacenamiento de título de Xbox Live se explican con más detalle en las secciones siguientes:

-   [Tipos de almacenamiento](#ID4ETB)
-   [Tipos de datos](#ID4ECF)
-   [URI de almacenamiento del título](#ID4EBEAC)
-   [Límite del Acelerador](#ID4ETEAC)

<a name="ID4ETB"></a>

Para socios administrados y ID@Xbox miembros:

| Tipo de almacenamiento       | Cuota (Managed Partners/ID@Xbox) | Cuota (programa de creadores en vivo de Xbox) |  Propósito                                                                                                                                                      | Plataformas                                                                                           | Usuarios                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| Plataforma segura   | 256 MB por usuario | 64 MB por usuario    | Juegos de datos, como guardados por el usuario o el estado del juego para reproducir, pausar y reanudar. Más seguro, pero con restricciones de la plataforma. | Puede leer cualquier plataforma, pero puede escribir sólo Xbox One, Xbox 360 o Windows Phone.  | Configurable a public o solo el propietario.       |
| Plataforma universal de | 64 MB por usuario | 64 MB por usuario    | Juegos de datos, como guardados por el usuario o el estado del juego para reproducir, pausar y reanudar. | Puede escribir cualquier plataforma, pero pueden leer solo las plataformas distintas de Xbox One, Xbox 360 o Windows Phone. | Configurable a public o solo el propietario.       |
| Global             | 256 MB | 256 MB            | Datos que puede leer todo el mundo, como listas, asignaciones, desafíos o recursos de arte. | Solo puede escribir a través del Portal para desarrolladores de Xbox o un centro de partners, cualquier plataforma puede leer.                                | Pueden leer todos los usuarios.

### <a name="deprecated-storage-types"></a>Tipos de almacenamiento en desuso

Los siguientes tipos de almacenamiento están en desuso. Se admiten únicamente para los títulos que actualmente están usando. No están disponibles para los títulos de nuevo.

| Tipo de almacenamiento       | Su propia Cuota  |   Propósito                                                                                                                                                      | Plataformas                                                                                           | Usuarios                                       |
|--------------------|--------------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|---------------------------------------------|
| JSON               | 64 MB por usuario     | Juegos de datos, como guardados por el usuario o el estado del juego para reproducir, pausar y reanudar. Más no seguro, restricciones de plataforma, pero con datos dar formato a las restricciones (solo en JSON). | Cualquier plataforma pueda leer o escribir.                                                                     | Configurable a public o solo el propietario.       |
| Dispositivo             | 64 MB por dispositivo   | Datos específicos de un dispositivo, como la configuración ni las preferencias de dispositivo.                                                                                            | Puede escribir sólo Xbox One, Xbox 360 o Windows Phone. Puede leer sólo el dispositivo que escribe los datos.  | Pueden leer todos los usuarios.                         |
| Almacenamiento de la sesión    | 256 MB por sesión | Datos para todo aquel que unirse a una determinada sesión de varios jugadores.                                                                                             | Cualquier plataforma que puede unirse a la sesión.                                                             | Todos los usuarios de la sesión pueden leer o escribir. |


<a name="ID4ECF"></a>

## <a name="types-of-data"></a>Tipos de datos

Juegos de especifican el tipo de datos que se va a usar en el **{type}** parámetro de un método GET o PUT. La siguiente sección describe los tres tipos admitidos:

-   [Información binaria](#ID4ENF)
-   [Información de JSON](#ID4EUF)
-   [información de configuración](#ID4ECAAC)

<a name="ID4ENF"></a>

#### <a name="binary-information"></a>Información binaria

Imágenes, sonidos y datos personalizados, usan el tipo binario. Dado que los datos se deben transmitir a través de HTTP, los datos binarios deben codificarse en caracteres que acepta HTTP. Por ejemplo, puede convertir los datos en cadenas hexadecimales o usar la codificación base64. El sistema de almacenamiento de título no procesa los datos codificados, por lo que su juego debe utilizar el mismo esquema de codificación y descodificación de datos al leer y escribir en el almacenamiento de título.

<a name="ID4EUF"></a>

#### <a name="json-information"></a>Información de JSON

Datos estructurados pueden usar el tipo JSON. Objetos JSON se pueden utilizar directamente en lenguajes como JavaScript, que los respaldan. Al recuperar datos desde archivos JSON, el juego puede proporcionar un *seleccione* parámetro para devolver los elementos específicos dentro de la estructura. Por ejemplo, use un archivo con formato JSON que contiene la información siguiente:

    {
    "difficulty" : 1,
    "level" :
        [
            { "number" : "1", "quest" : "swords" },
            { "number" : "2", "quest" : "iron" },
            { "number" : "3", "quest" : "gold" },
            { "number" : "4", "quest" : "queen" }
         ],
    "weapon" :
        {
             "name" : "poison",
             "timeleft" : "2mins"
        }
    }


| Nota                                                                                                                                              |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Por motivos de seguridad, el primer elemento de los datos JSON no debe ser una matriz. El servicio rechazará datos JSON enviadas con una matriz en la raíz. |

Juegos pueden seleccionar las partes de esta estructura con una consulta como esta:

             GET https://titlestorage.xboxlive.com/users/xuid(1234)/storage/titlestorage/titlegroups/
             faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=weapon.name
             Content-Type: application/octet-stream
             x-xbl-contract-version: 1
             Authorization: XBL3.0 x=<userHash>;<STSTokenString>
             Connection: Keep-Alive

El cuerpo de respuesta para esta consulta es:

    {
        "name" : "poison"
    }

Puede tener acceso a la matriz con una consulta como esta:

      GET https://titlestorage.xboxlive.com//users/xuid(1234)/storage/titlestorage/titlegroups/
      faa29d21-2b49-4908-96bf-b953157ac4fe/data/save1.dat,json?select=levels[3].quest
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

El cuerpo de respuesta para esta consulta es:

    {
        "quest" : "queen"
    }

Se aplican las siguientes restricciones de longitud para los datos JSON:

-   Valor numérico, longitud máxima = 32
-   Valor de longitud máxima de cadena = 1024
-   Nombre de propiedad, la longitud máxima = 64
-   Jerarquía, la profundidad máxima = 16
-   Tamaño máximo, de matriz = 1024
-   Las propiedades secundarias, máxima en un objeto = 1024

<a name="ID4ECAAC"></a>

#### <a name="configuration-information"></a>información de configuración

El **{type}** puede ser **config** para indicar que los datos están un blob de configuración. Los blobs de configuración son estructuras de datos que se almacenan en almacenamiento global del título. El formato del blob es similar a un objeto JSON.

Los blobs de configuración pueden incluir nodos virtuales que devuelven un valor de una lista de posibilidades. Los nodos virtuales son útiles para proporcionar opciones para situaciones específicas, como un título o la configuración regional. El nodo virtual incluye varias configuraciones posibles junto con los valores y las condiciones para la selección de los valores. En el ejemplo siguiente, la **defaultCardDesign** configuración puede tener uno de los valores en el nodo virtual.

    {
      "defaultCardDesign":
      {
        "_virtualNode":
       {
          "_selectBy":"titleId",
          "_sourceNodes":
          [
            {"_selector":"123456799", "_data":"RobotUnicornCard.png,binary"},
            {"_selector":"default", "_data":"StandardCard.png,binary"}
          ]
        }
      },
    }

Cuando un juego lee este archivo, el sistema selecciona uno de los valores de la  **\_sourceNodes** matriz. En este caso, el elemento se selecciona según el identificador del título del juego. Los usuarios que se ejecuta el juego **12456799** vea:

    {
      "defaultCardDesign":"RobotUnicornCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:1234567899"]
    }

Vea el resto de los usuarios:

    {
      "defaultCardDesign":"StandardCard.png,binary",
      "_sourceNodes":["defaultCardDesign:titleID:default"]
    }

Juegos pueden definir los selectores personalizados que coincide con un parámetro en la solicitud. Por ejemplo, en este blob de configuración:

    {
        "defaultCardDesign":
        {
            "_virtualNode":
            {
                "_selectBy":"custom:gameMode",
                "_sourceNodes":
                [
                    {"_selector":"silly", "_data":"RobotUnicornCard.png,binary"},
                    {"_selector":"serious", "_data":"SeriousCard.png,binary"},
                    {"_selector":"default", "_data":"StandardCard.png,binary"}
                 ]
            }
        },
        "backgroundColor":"green",
        "dealerHitsOnSoft17":true
    }

Juegos de pasan una cadena el **customSelector** parámetro para seleccionar qué elemento va a devolver. Por ejemplo, para obtener la segunda opción, solicita un juego:

      GET https://titlestorage.xboxlive.com/media/titlegroups/faa29d21-2b49-4908-96bf-b953157ac4fe
      /storage/data/config.json,config?customSelector=gameMode.serious
      Content-Type: application/octet-stream
      x-xbl-contract-version: 1
      Authorization: XBL3.0 x=<userHash>;<STSTokenString>
      Connection: Keep-Alive

El  **\_selectBy** valor indica qué tipo de selección para realizar y el  **\_selector** valor indica que los datos que se va a usar en la selección. Los valores posibles son:

<table>
<thead>
<tr>
<th>_selectBy</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr>
<td >titleId</td>
<td ><p>El <strong>_selector</strong> coincide con el identificador de título en la notificación proporcionada.</p></td>
</tr>
<tr>
<td >configuración regional</td>
<td ><p>El <strong>_selector</strong> coincide con la cadena de configuración regional del encabezado Accept-Language.</p></td>
</tr>
<tr>
<td >Personalizado</td>
<td ><p>El <strong>_selector</strong> coincide con una cadena personalizada que se pasa en el <strong>customSelector</strong> parámetro de consulta. El <strong>customSelector</strong> contiene una o varias consultas separadas por comas. Cada consulta es el nombre de la <strong>selectBy</strong> elemento y el valor de la <strong>_selector</strong> elemento.</p></td>
</tr>
</tbody>
</table>

<a name="ID4EBEAC"></a>

## <a name="title-storage-uris"></a>URI de almacenamiento del título

URI de almacenamiento de título tienen el siguiente formato:

    https://titlestorage.xboxlive.com/{path}

El **{path}** parte del URI es el tipo de solicitud realizada y debe estar 245 caracteres o menos.

<a name="ID4ETEAC"></a>

## <a name="throttle-limit"></a>Límite del Acelerador

No hay ningún límite fijo en cuántos lecturas o escrituras puede hacer que un título por minuto, pero generalmente no podrá realizar más de un minuto por término medio en una sesión de una hora. Por ejemplo, un título para hacer 60 lecturas o escrituras al principio de una sesión, pero no hay más para el resto de la hora. Títulos deben protegerse contra más llamadas más adelante, si necesita limitar las solicitudes de servicios de Xbox LIVE.

Si el título tiene requisitos especiales de partición, como adicionales lecturas o escrituras, póngase en contacto con Microsoft.

<a name="ID4E5EAC"></a>

## <a name="using-title-storage"></a>Uso de almacenamiento de título

Para empezar a trabajar con almacenamiento de información de título, determine primero qué tipo de datos que desea almacenar. Algunos ejemplos son juegos guardados, estado del juego, desafíos diarios, mapas de juegos y recursos de arte.

A continuación, determine qué plataformas y los títulos necesitará acceso a los datos. Título almacenamiento admite en la nube acceso a datos de un título en una única plataforma único y de varios títulos en varias plataformas.

Por último, use los temas de esta sección para configurar el almacenamiento, cargar los datos y establecer permisos de acceso adecuadamente en función de las opciones seleccionadas.

<a name="ID4EJFAC"></a>

## <a name="in-this-section"></a>En esta sección

[Leer un Blob de configuración en el almacenamiento de información de Xbox Live título](reading-configuration-blobs.md)  
Muestra cómo leer almacenamiento de blobs de la configuración de Xbox Live título.

[Almacenar un Blob binario en el almacenamiento de información de Xbox Live título](storing-binary-blobs.md)  
Se muestra cómo almacenar objetos BLOB binarios en el almacenamiento de título de Xbox Live.

[Leer un Blob binario en el almacenamiento de información de Xbox Live título](reading-binary-blobs.md)  
Muestra cómo leer blobs binarios desde el almacenamiento de título de Xbox Live.

[Almacena un Blob JSON en el almacenamiento de información de Xbox Live título](storing-jsonblobs.md)  
Muestra cómo almacenar los blobs JSON en el almacenamiento de título de Xbox Live.

[Leer un Blob JSON en el almacenamiento de información de Xbox Live título](reading-jsonblobs.md)  
Muestra cómo leer almacenamiento de blobs JSON de Xbox Live título.

<a name="ID4E4FAC"></a>
