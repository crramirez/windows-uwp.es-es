---
description: Usa este método en la API de análisis de Microsoft Store para obtener datos de club de Xbox Live.
title: Obtener datos del club de Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Store services, servicios de Store, Microsoft Store analytics API, API de análisis de Microsoft Store, Xbox Live analytics, análisis de Xbox Live, clubs
ms.localizationpriority: medium
ms.openlocfilehash: dbf9d06f96632237c10de0fe3b6c4723a2501254
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "7845910"
---
# <a name="get-xbox-live-club-data"></a>Obtener datos del club de Xbox Live

Usa este método en la API de análisis de Microsoft Store para obtener datos de club de tu [juego habilitado para Xbox Live](../xbox-live/index.md). Esta información también está disponible en el [informe de análisis de Xbox](../publish/xbox-analytics-report.md) en el centro de partners.

> [!IMPORTANT]
> Este método solo admite juegos para Xbox o juegos que usan servicios de Xbox Live. Estos juegos debe pasar por el [proceso de aprobación de concepto](../gaming/concept-approval.md), que incluye juegos publicados por [partners de Microsoft](../xbox-live/developer-program-overview.md#microsoft-partners) y juegos enviados a través del [programa ID@Xbox](../xbox-live/developer-program-overview.md#id). Este método no admite actualmente juegos publicados mediante el [Programa de creadores de Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo has hecho, completa todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) para la API de análisis de Microsoft Store.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud para este método. Después de obtener un token de acceso, tienes 60 minutos para usarlo antes de que expire. De todos modos, una vez que el token expire, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Encabezado de la solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | cadena | Obligatorio. Token de acceso de Azure AD con formato **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud


| Parámetro        | Tipo   |  Descripción      |  Necesario  
|---------------|--------|---------------|------|
| applicationId | cadena | El [Id. de Store](in-app-purchases-and-trials.md#store-ids) del juego para la que quieres recuperar los datos de club de Xbox Live.  |  Sí  |
| metricType | cadena | Una cadena que especifica el tipo de datos de análisis de Xbox Live que recuperar. En este método, especifica el valor **communitymanagerclub**.  |  Sí  |
| startDate | fecha | La fecha de inicio del intervalo de fechas de los datos de club que se han de recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | fecha | Fecha de finalización del intervalo de fechas de los datos de club que se han de recuperar. El valor siempre es la fecha actual. |  No  |
| top | entero | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | entero | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

El siguiente ejemplo muestra una solicitud para obtener datos de club para tu juego habilitado para Xbox Live. Sustituye el valor *applicationId* por el Id de Store del juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagerclub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Respuesta

| Valor      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Valor      | matriz  | Una matriz que contiene un objeto [ProductData](#productdata) que contiene los datos para clubs relacionadas con tu juego y un objeto [XboxwideData](#xboxwidedata) que contiene datos de club para todos los clientes de Xbox Live. Estos datos se incluyen para compararlos con los datos de tu juego.  |
| @nextLink  | cadena | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, se devuelve este valor si el parámetro **top** de la solicitud está establecido en 10000, pero resulta que hay más de 10000 filas de datos de la consulta. |
| TotalCount | entero    | El número total de filas del resultado de datos de la consulta. |


### <a name="productdata"></a>ProductData

Este recurso contiene datos de club para tu juego.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
| fecha            |  cadena |   La fecha de los datos de club.   |
|  applicationId               |    cadena     |  El [Id. de Store](in-app-purchases-and-trials.md#store-ids) del juego para la que has recuperado los datos de club.   |
|  clubsWithTitleActivity               |    int     |  El número de clubs que están implicados socialmente en tu juego.   |     
|  clubsExclusiveToGame               |   int      |  El número de clubs que están implicados socialmente exclusivamente en tu juego.   |     
|  clubFacts               |   matriz      |   Contiene uno o más objetos [ClubFacts](#clubfacts) sobre cada uno de lo clubs socialmente implicados en el juego.   |


### <a name="xboxwidedata"></a>XboxwideData

Este recurso contiene datos medios del club en todos los clientes de Xbox Live.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|------|
| fecha            |  cadena |   La fecha de los datos de club.   |
|  applicationId  |    cadena     |   En el objeto **XboxwideData**, esta cadena es siempre el valor **XBOXWIDE**.  |
|  clubsWithTitleActivity               |   int     |  De media, el número de clubs con clientes socialmente implicados en el juego habilitado para Xbox Live.    |     
|  clubsExclusiveToGame               |   int      |  De media, el número de clubs con clientes socialmente implicados exclusivamente en el juego habilitado para Xbox Live.   |     
|  clubFacts               |   objeto      |  Contiene un objeto [ClubFacts](#clubfacts). Este objeto no tiene sentido en el contexto del objeto **XboxwideData** y tiene los valores predeterminados.  |


### <a name="clubfacts"></a>ClubFacts

En el objeto **ProductData**, este objeto contiene datos de un club específico que tiene actividad relacionada con tu juego. En el objeto **XboxwideData**, este objeto no tiene sentido y tiene valores predeterminados.

| Valor           | Tipo    | Descripción        |
|-----------------|---------|--------------------|
|  name            |  cadena  |   En el objeto **ProductData**, este es el nombre del club. En el objeto **XboxwideData**, este es siempre el valor **XBOXWIDE**.           |
|  memberCount               |    int     | En el objeto **ProductData**, este es el número de miembros en el club, salvo los que no son miembros y que simplemente están visitando el club. En el objeto **XboxwideData**, este es siempre 0.    |
|  titleSocialActionsCount               |    int     |  En el objeto **ProductData**, este es el número de acciones sociales realizadas por miembros del club relacionadas con tu juego. En el objeto **XboxwideData**, este es siempre 0   |
|  isExclusiveToGame               |    Booleano     |  En el objeto **ProductData**, este indica si el club actual está socialmente comprometido exclusivamente con tu juego. En el objeto **XboxwideData**, este es siempre true.  |


### <a name="response-example"></a>Ejemplo de respuesta

En el ejemplo siguiente se muestra el cuerpo de una respuesta JSON de ejemplo realizada para esta solicitud.

```json
{
  "Value": [
    {
      "ProductData": [
        {
          "date": "2018-01-30",
          "applicationId": "9NBLGGGZ5QDR",
          "clubsWithTitleActivity": 3,
          "clubsExclusiveToGame": 1,
          "clubFacts": [
            {
              "name": "Club Contoso Racing",
              "memberCount": 1,
              "titleSocialActionsCount": 1,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Sports",
              "memberCount": 12,
              "titleSocialActionsCount": 9,
              "isExclusiveToGame": false
            },
            {
              "name": "Club Contoso Maze",
              "memberCount": 4,
              "titleSocialActionsCount": 2,
              "isExclusiveToGame": false
            }
          ]
        }
      ],
      "XboxwideData": [
        {
          "date": "2018-01-30",
          "applicationId": "XBOXWIDE",
          "clubsWithTitleActivity": 25,
          "clubsExclusiveToGame": 3,
          "clubFacts": [
            {
              "name": "XBOXWIDE",
              "memberCount": 0,
              "titleSocialActionsCount": 0,
              "isExclusiveToGame": true
            }
          ]
        }
      ]
    }
  ],
  "@nextLink": "gameanalytics?applicationId=9NBLGGGZ5QDR&metricType=communitymanagerclub&top=1&skip=1&startDate=2018/01/04&endDate=2018/02/02&orderby=date desc",
  "TotalCount": 27
}
```

## <a name="related-topics"></a>Artículos relacionados

* [Acceder a los datos de análisis mediante los servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del hub de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)
