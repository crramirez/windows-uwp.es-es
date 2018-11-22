---
author: Xansky
ms.assetid: d305746a-d370-4404-8cde-c85765bf3578
description: Usa este método en la API de promociones de Microsoft Store para administrar los perfiles objetivo para las campañas de anuncios promocionales.
title: Administrar perfiles objetivo
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store promotions API, API de promociones de Microsoft Store, ad campaigns, campañas de anuncios
ms.localizationpriority: medium
ms.openlocfilehash: 271d60e6fbc0bd6336aa8aa8ec9edbb2b965c7f4
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "7573475"
---
# <a name="manage-targeting-profiles"></a>Administrar perfiles objetivo


Usa estos métodos en la API de promociones de Microsoft Store para seleccionar los usuarios, las zonas geográficas y los tipos de inventario que quieres como destino de cada línea de entrega de una campaña de anuncios promocionales. Los perfiles de destino se pueden crear y volver a usar en varias líneas de entrega.

Para obtener más información sobre la relación entre los perfiles objetivo y las campañas de anuncios, las líneas de entrega y los creativos, consulta [Ejecutar campañas de anuncios con los servicios de Microsoft Store](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

## <a name="prerequisites"></a>Requisitos previos

Para usar estos métodos, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](run-ad-campaigns-using-windows-store-services.md#prerequisites) de la API de promociones de Microsoft Store.
* [Obtén un token de acceso de Azure AD](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para estos métodos. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. Después de que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud

Estos métodos tienen los siguientes URI.

| Tipo de método | URI de la solicitud                                                      |  Descripción  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile``` |  Crea un nuevo perfil de destino.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  Edita el perfil de destino especificado por *targetingProfileId*.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  Obtiene el perfil de destino especificado por *targetingProfileId*.  |


### <a name="header"></a>Encabezado

| Encabezado        | Tipo   | Descripción         |
|---------------|--------|---------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |
| Id. de seguimiento   | GUID   | Opcional. Un id. que realiza un seguimiento del flujo de llamadas.                                  |


### <a name="request-body"></a>Cuerpo de la solicitud

Los métodos POST y PUT necesitan un cuerpo de la solicitud JSON con los campos obligatorios de un objeto de [perfil de destino](#targeting-profile) y los campos adicionales que quieras establecer o cambiar.


### <a name="request-examples"></a>Ejemplos de solicitud

En el siguiente ejemplo se muestra cómo llamar al método POST para crear un perfil de destino.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652],
    "gender": [
      700
    ],
    "country": [
      11,
      12
    ],
    "osVersion": [
      504
    ],
    "deviceType": [
      710
    ],
    "supplyType": [
      11470
    ]
}
```

En el siguiente ejemplo se muestra cómo llamar al método GET para recuperar un perfil de destino.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/310023951  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/>

## <a name="response"></a>Respuesta

Estos métodos devuelven un cuerpo de respuesta JSON con un objeto de [perfil de destino](#targeting-profile) que contiene información sobre el perfil de destino que se creó, actualizó o recuperó. En el siguiente ejemplo se muestra un cuerpo de respuesta para estos métodos.

```json
{
  "Data": {
    "id": 310021746,
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652
    ],
    "gender": [
      700
    ],
    "country": [
      6,
      13,
      29
    ],
    "osVersion": [
      504,
      505,
      506,
      507,
      508
    ],
    "deviceType": [
      710,
      711
    ],
    "supplyType": [
      11470
    ]
  }
}
```

<span id="targeting-profile"/>

## <a name="targeting-profile-object"></a>Objeto de perfil de destino

Los cuerpos de solicitud y respuesta para estos métodos contienen los siguientes campos. En esta tabla se muestran los campos que son de solo lectura (es decir, no se pueden cambiar en el método PUT) y los campos que son obligatorios en el cuerpo de la solicitud para el método POST.

| Campo        | Tipo   |  Descripción      |  Solo lectura  | Valor predeterminado  | Obligatorio para POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  número entero   |  El id. del perfil de destino.     |   Sí    |       |   No      |       
|  name   |  cadena   |   El nombre del perfil de destino.    |    No   |      |  Sí     |       
|  targetingType   |  cadena   |  Uno de los siguientes valores: <ul><li>**Auto**: especifica este valor para permitir que Microsoft elija el perfil de destino en función de la configuración de la aplicación en el centro de partners.</li><li>**Manual**: especifica este valor para definir tu propio perfil de destino.</li></ul>     |  No     |  Automático    |   Sí    |       
|  age   |  matriz   |   Uno o más enteros que identifican los intervalos de edad de los usuarios de destino. Para obtener una lista completa de enteros, consulta [valores de edad](#age-values) en este artículo.    |    No    |  nulo    |     No    |       
|  gender   |  matriz   |  Uno o más enteros que identifican el sexo de los usuarios de destino. Para obtener una lista completa de enteros, consulta [valores de sexo](#gender-values) en este artículo.       |  No    |  nulo    |     No    |       
|  country   |  matriz   |  Uno o más enteros que identifican los códigos de país de los usuarios de destino. Para obtener una lista completa de enteros, consulta [valores de códigos de país](#country-code-values) en este artículo.    |  No    |  nulo   |      No   |       
|  osVersion   |  matriz   |   Uno o más enteros que identifican las versiones de los sistemas operativos de los usuarios de destino. Para obtener una lista completa de enteros, consulta [valores de versión de sistema operativo](#osversion-values) en este artículo.     |   No    |  nulo   |     No    |       
|  deviceType   | matriz    |  Uno o más enteros que identifican los tipos de dispositivos de los usuarios de destino. Para obtener una lista completa de enteros, consulta [valores de tipo de dispositivo](#devicetype-values) en este artículo.       |   No    |  nulo    |    No     |       
|  supplyType   |  matriz   |  Uno o más enteros que identifican el tipo de inventario en el que se mostrarán los anuncios de la campaña. Para obtener una lista completa de enteros, consulta [valores de tipo de suministro](#supplytype-values) en este artículo.      |   No    |  nulo   |     No    |   |  


<span id="age-values"/>

### <a name="age-values"></a>Valores de edad

El campo *age* del objeto [TargetingProfile](#targeting-profile) contiene uno o varios de los siguientes enteros que identifican los intervalos de edad de los usuarios de destino.

|  Valor entero del campo *age*  |  Intervalo de edad correspondiente  |  
|---------------------------------|---------------------------|
|     651     |            13 a 17             |
|     652     |           18 a 24             |
|     653     |            25 a 34             |
|     654     |            35 a 49             |
|     655     |            50 o más             |

Para obtener los valores que admite el campo *age* mediante programación, puedes llamar al siguiente método GET.  Para el encabezado ```Authorization```, pasa el token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/age
Authorization: Bearer <your access token>
```

En el siguiente ejemplo se muestra el cuerpo de la respuesta para este método.

```json
{
  "Data": {
    "Age": {
      "651": "Age13To17",
      "652": "Age18To24",
      "653": "Age25To34",
      "654": "Age35To49",
      "655": "Age50AndAbove"
    }
  }
}
```

<span id="gender-values"/>

### <a name="gender-values"></a>Valores de sexo

El campo *gender* del objeto [TargetingProfile](#targeting-profile) contiene uno o varios de los siguientes enteros que identifican los sexos de los usuarios de destino.

|  Valor entero para el campo *gender*  |  Sexo correspondiente  |  
|---------------------------------|---------------------------|
|     700     |            Hombre             |
|     701     |           Mujer             |

Para obtener los valores que admite el campo *gender* mediante programación, puedes llamar al siguiente método GET.  Para el encabezado ```Authorization```, pasa el token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/gender
Authorization: Bearer <your access token>
```

En el siguiente ejemplo se muestra el cuerpo de la respuesta para este método.

```json
{
  "Data": {
    "Gender": {
      "700": "Male",
      "701": "Female"
    }
  }
}
```


<span id="osversion-values"/>

### <a name="os-version-values"></a>Valores de versión de sistema operativo

El campo *osVersion* del objeto [TargetingProfile](#targeting-profile) contiene uno o varios de los siguientes enteros que identifican las versiones del sistema operativo de los usuarios de destino.

|  Valor entero para el campo *osVersion*  |  Versión del sistema operativo correspondiente  |  
|---------------------------------|---------------------------|
|     500     |            Windows Phone 7             |
|     501     |           Windows Phone 7.1             |
|     502     |           Windows Phone7.5             |
|     503     |           Windows Phone7.8             |
|     504     |           Windows Phone8.0             |
|     505     |           Windows Phone 8.1             |
|     506     |           Windows8.0             |
|     507     |           Windows8.1             |
|     508     |           Windows10             |
|     509     |           Windows10 Mobile             |

Para obtener los valores que admite el campo *osVersion* mediante programación, puedes llamar al siguiente método GET.  Para el encabezado ```Authorization```, pasa el token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/osversion
Authorization: Bearer <your access token>
```

En el siguiente ejemplo se muestra el cuerpo de la respuesta para este método.

```json
{
  "Data": {
    "OsVersion": {
      "500": "WindowsPhone70",
      "501": "WindowsPhone71",
      "502": "WindowsPhone75",
      "503": "WindowsPhone78",
      "504": "WindowsPhone80",
      "505": "WindowsPhone81",
      "506": "Windows80",
      "507": "Windows81",
      "508": "Windows10",
      "509": "WindowsPhone10"
    }
  }
}
```


<span id="devicetype-values"/>

### <a name="device-type-values"></a>Valores de tipo de dispositivo

El campo *deviceType* del objeto [TargetingProfile](#targeting-profile) contiene uno o varios de los siguientes enteros que identifican los tipos de dispositivos de los usuarios de destino.

|  Valor entero para el campo *deviceType*  |  Tipo de dispositivo correspondiente  |  Descripción  |
|---------------------------------|---------------------------|---------------------------|
|     710     |  Windows   |  Representa los dispositivos que ejecutan una versión de escritorio de Windows10 o Windows8.x.  |
|     711     |  Teléfono     |  Representa los dispositivos que ejecutan Windows10 Mobile, Windows Phone8.x o Windows Phone7.x.

Para obtener los valores que admite el campo *deviceType* mediante programación, puedes llamar al siguiente método GET.  Para el encabezado ```Authorization```, pasa el token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/devicetype
Authorization: Bearer <your access token>
```

En el siguiente ejemplo se muestra el cuerpo de la respuesta para este método.

```json
{
  "Data": {
    "DeviceType": {
      "710": "Windows",
      "711": "Phone"
    }
  }
}
```


<span id="supplytype-values"/>

### <a name="supply-type-values"></a>Valores de tipo de suministro

El campo *supplyType* del objeto [TargetingProfile](#targeting-profile) contiene uno o varios de los siguientes enteros que identifican el tipo de inventario en el que se mostrarán los anuncios de la campaña.

|  Valor entero para el campo *supplyType*  |  Tipo de suministro correspondiente  |  Descripción  |
|---------------------------------|---------------------------|---------------------------|
|     11470     |  Aplicación        |  Hace referencia a los anuncios que aparecen solo en aplicaciones.  |
|     11471     |  Universal        |  Hace referencia a los anuncios que aparecen en aplicaciones, en Internet y en otras superficies de pantalla.  |

Para obtener los valores que admite el campo *supplyType* mediante programación, puedes llamar al siguiente método GET.  Para el encabezado ```Authorization```, pasa el token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/supplytype
Authorization: Bearer <your access token>
```

En el siguiente ejemplo se muestra el cuerpo de la respuesta para este método.

```json
{
  "Data": {
    "SupplyType": {
      "11470": "App",
      "11471": "Universal"
    }
  }
}
```

<span id="country-code-values"/>

### <a name="country-code-values"></a>Valores de código de país

El campo *country* del objeto [TargetingProfile](#targeting-profile) contiene uno o varios de los siguientes enteros que identifican los códigos de país [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) de los usuarios de destino.

|  Valor entero para el campo *country*  |  Código de país correspondiente  |  
|-------------------------------------|------------------------------|
|     1      |            US                  |
|     2      |            AU                  |
|     3      |            AT                  |
|     4      |            BE                  |
|     5      |            BR                  |
|     6      |            CA                  |
|     7      |            DK                  |
|     8      |            FI                  |
|     9      |            FR                  |
|     10      |            DE                  |
|     11      |            GR                  |
|     12      |            HK                  |
|     13      |            IN                  |
|     14      |            IE                  |
|     15      |            IT                  |
|     16      |            JP                  |
|     17      |            LU                  |
|     18      |            MX                  |
|     19      |            NL                  |
|     20      |            NZ                  |
|     21      |            NO                  |
|     22      |            PL                  |
|     23      |            PT                  |
|     24      |            SG                  |
|     25      |            ES                  |
|     26      |            SE                  |
|     27      |            CH                  |
|     28      |            TW                  |
|     29      |            GB                  |
|     30      |            RU                  |
|     31      |            CL                  |
|     32      |            CO                  |
|     33      |            CZ                  |
|     34      |            HU                  |
|     35      |            ZA                  |
|     36      |            KR                  |
|     37      |            CN                  |
|     38      |            RO                  |
|     39      |            TR                  |
|     40      |            SK                  |
|     41      |            IL                  |
|     42      |            ID                  |
|     43      |            AR                  |
|     44      |            MY                  |
|     45      |            PH                  |
|     46      |            PE                  |
|     47      |            UA                  |
|     48      |            AE                  |
|     49      |            TH                  |
|     50      |            IQ                  |
|     51      |            VN                  |
|     52      |            CR                  |
|     53      |            VE                  |
|     54      |            QA                  |
|     55      |            SI                  |
|     56      |            BG                  |
|     57      |            LT                  |
|     58      |            RS                  |
|     59      |            HR                  |
|     60      |            HR                  |
|     61      |            LV                  |
|     62      |            EE                  |
|     63      |            IS                  |
|     64      |            KZ                  |
|     65      |            SA                  |
|     67      |            AL                  |
|     68      |            DZ                  |
|     70      |            AO                  |
|     72      |            AM                  |
|     73      |            AZ                  |
|     74      |            BS                  |
|     75      |            BD                  |
|     76      |            BB                  |
|     77      |            BY                  |
|     81      |            BO                  |
|     82      |            BA                  |
|     83      |            BW                  |
|     87      |            KH                  |
|     88      |            CM                  |
|     94      |            CD                  |
|     95      |            CI                  |
|     96      |            CY                  |
|     99      |            DO                  |
|     100      |            EC                  |
|     101      |            EG                  |
|     102      |            SV                  |
|     107      |            FJ                  |
|     108      |            GA                  |
|     110      |            GE                  |
|     111      |            GH                  |
|     114      |            GT                  |
|     118      |            HT                  |
|     119      |            HN                  |
|     120      |            JM                  |
|     121      |            JO                  |
|     122      |            KE                  |
|     124      |            KW                  |
|     125      |            KG                  |
|     126      |            LA                  |
|     127      |            LB                  |
|     133      |            MK                  |
|     135      |            MW                  |
|     138      |            MT                  |
|     141      |            MU                  |
|     145      |            ME                  |
|     146      |            MA                  |
|     147      |            MZ                  |
|     148      |            NA                  |
|     150      |            NP                  |
|     151.      |            NI                  |
|     153      |            NG                  |
|     154      |            OM                  |
|     155      |            PK                  |
|     157      |            PA                  |
|     159      |            PY                  |
|     167      |            SN                  |
|     172      |            LK                  |
|     176      |            TZ                  |
|     180      |            TT                  |
|     181      |            TN                  |
|     184      |            UG                  |
|     185      |            UY                  |
|     186      |            UZ                  |
|     189      |            ZM                  |
|     190      |            ZW                  |
|     219      |            MD                  |
|     224      |            PS                  |
|     225      |            RE                  |
|     246      |            PR                  |

Para obtener los valores que admite el campo *country* mediante programación, puedes llamar al siguiente método GET.  Para el encabezado ```Authorization```, pasa el token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/country
Authorization: Bearer <your access token>
```

En el siguiente ejemplo se muestra el cuerpo de la respuesta para este método.

```json
{
  "Data": {
    "Country": {
      "1": "US",
      "2": "AU",
      "3": "AT",
      "4": "BE",
      "5": "BR",
      "6": "CA",
      "7": "DK",
      "8": "FI",
      "9": "FR",
      "10": "DE",
      "11": "GR",
      "12": "HK",
      "13": "IN",
      "14": "IE",
      "15": "IT",
      "16": "JP",
      "17": "LU",
      "18": "MX",
      "19": "NL",
      "20": "NZ",
      "21": "NO",
      "22": "PL",
      "23": "PT",
      "24": "SG",
      "25": "ES",
      "26": "SE",
      "27": "CH",
      "28": "TW",
      "29": "GB",
      "30": "RU",
      "31": "CL",
      "32": "CO",
      "33": "CZ",
      "34": "HU",
      "35": "ZA",
      "36": "KR",
      "37": "CN",
      "38": "RO",
      "39": "TR",
      "40": "SK",
      "41": "IL",
      "42": "ID",
      "43": "AR",
      "44": "MY",
      "45": "PH",
      "46": "PE",
      "47": "UA",
      "48": "AE",
      "49": "TH",
      "50": "IQ",
      "51": "VN",
      "52": "CR",
      "53": "VE",
      "54": "QA",
      "55": "SI",
      "56": "BG",
      "57": "LT",
      "58": "RS",
      "59": "HR",
      "60": "BH",
      "61": "LV",
      "62": "EE",
      "63": "IS",
      "64": "KZ",
      "65": "SA",
      "67": "AL",
      "68": "DZ",
      "70": "AO",
      "72": "AM",
      "73": "AZ",
      "74": "BS",
      "75": "BD",
      "76": "BB",
      "77": "BY",
      "81": "BO",
      "82": "BA",
      "83": "BW",
      "87": "KH",
      "88": "CM",
      "94": "CD",
      "95": "CI",
      "96": "CY",
      "99": "DO",
      "100": "EC",
      "101": "EG",
      "102": "SV",
      "107": "FJ",
      "108": "GA",
      "110": "GE",
      "111": "GH",
      "114": "GT",
      "118": "HT",
      "119": "HN",
      "120": "JM",
      "121": "JO",
      "122": "KE",
      "124": "KW",
      "125": "KG",
      "126": "LA",
      "127": "LB",
      "133": "MK",
      "135": "MW",
      "138": "MT",
      "141": "MU",
      "145": "ME",
      "146": "MA",
      "147": "MZ",
      "148": "NA",
      "150": "NP",
      "151": "NI",
      "153": "NG",
      "154": "OM",
      "155": "PK",
      "157": "PA",
      "159": "PY",
      "167": "SN",
      "172": "LK",
      "176": "TZ",
      "180": "TT",
      "181": "TN",
      "184": "UG",
      "185": "UY",
      "186": "UZ",
      "189": "ZM",
      "190": "ZW",
      "219": "MD",
      "224": "PS",
      "225": "RE",
      "246": "PR"
    }
  }
}
```

## <a name="related-topics"></a>Temas relacionados

* [Ejecutar campañas de anuncios con los servicios de Microsoft Store](run-ad-campaigns-using-windows-store-services.md)
* [Administrar campañas de anuncios](manage-ad-campaigns.md)
* [Administrar líneas de entrega de campañas de anuncios](manage-delivery-lines-for-ad-campaigns.md)
* [Administrar creativos de campañas de anuncios](manage-creatives-for-ad-campaigns.md)
* [Obtener los datos de rendimiento de la campaña de anuncios](get-ad-campaign-performance-data.md)
