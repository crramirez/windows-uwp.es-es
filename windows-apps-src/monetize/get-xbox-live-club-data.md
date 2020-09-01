---
description: Use este método en la API de Microsoft Store Analytics para obtener datos del Club de Xbox Live.
title: Obtener datos del club de Xbox Live
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, servicios de tienda, Microsoft Store API de análisis, análisis de Xbox Live, tréboles
ms.localizationpriority: medium
ms.openlocfilehash: 155e8a48f9e9e207709af4e55bb8e052b4419867
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162369"
---
# <a name="get-xbox-live-club-data"></a>Obtener datos del club de Xbox Live

Use este método en la API de Microsoft Store Analytics para obtener datos del Club para su [juego habilitado para Xbox Live](/gaming/xbox-live/index.md). Esta información también está disponible en el [Informe de análisis de Xbox](../publish/xbox-analytics-report.md) del centro de Partners.

> [!IMPORTANT]
> Este método solo admite juegos para Xbox o juegos que usan los servicios de Xbox Live. Estos juegos deben pasar por el [proceso de aprobación del concepto](../gaming/concept-approval.md), que incluye juegos publicados por [asociados de Microsoft](/gaming/xbox-live/developer-program-overview.md#microsoft-partners) y juegos enviados a través del [ ID@Xbox programa](/gaming/xbox-live/developer-program-overview.md#id). Este método no admite actualmente juegos publicados a través del [programa de creadores de Xbox Live](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="prerequisites"></a>Requisitos previos

Para usar este método, primero debes hacer lo siguiente:

* Si aún no lo ha hecho, complete todos los [requisitos previos](access-analytics-data-using-windows-store-services.md#prerequisites) de la API de Microsoft Store Analytics.
* [Obtén un token de acceso de Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token) para usarlo en el encabezado de la solicitud de este método. Una vez que haya obtenido un token de acceso, tiene 60 minutos para usarlo antes de que expire. Si el token expira, puedes obtener uno nuevo.

## <a name="request"></a>Solicitud


### <a name="request-syntax"></a>Sintaxis de la solicitud

| Método | URI de solicitud       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Encabezado de solicitud

| Encabezado        | Tipo   | Descripción                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | string | Necesario. El token de acceso de Azure AD del formulario **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Parámetros de solicitud


| Parámetro        | Tipo   |  Descripción      |  Obligatorio  
|---------------|--------|---------------|------|
| applicationId | string | El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del juego para el que desea recuperar los datos de la consola Xbox Live Club.  |  Sí  |
| metricType | string | Cadena que especifica el tipo de datos de análisis de Xbox Live que se va a recuperar. Para este método, especifique el valor **communitymanagerclub**.  |  Sí  |
| startDate | date | Fecha de inicio del intervalo de fechas de datos del Club que se va a recuperar. El valor predeterminado es 30 días antes de la fecha actual. |  No  |
| endDate | date | Fecha de finalización del intervalo de fechas de los datos del Club que se va a recuperar. La fecha predeterminada es la actual. |  No  |
| top | int | Número de filas de datos que se devuelven en la solicitud. El valor máximo y el valor predeterminado, si no se especifican, es 10 000. Si hay más filas en la consulta, el cuerpo de la respuesta incluye un vínculo que puedes usar para solicitar la siguiente página de datos. |  No  |
| skip | int | Número de filas que se omiten en la consulta. Usa este parámetro para consultar grandes conjuntos de datos. Por ejemplo, los valores top=10000 y skip=0 recuperan las primeras 10 000 filas de datos, los valores top=10000 y skip=10000 recuperan las siguientes 10 000 filas de datos, y así sucesivamente. |  No  |


### <a name="request-example"></a>Ejemplo de solicitud

En el ejemplo siguiente se muestra una solicitud para obtener datos del Club para el juego habilitado para Xbox Live. Reemplace el valor de *ApplicationID* por el identificador de almacén del juego.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=communitymanagerclub&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Response

| Value      | Tipo   | Descripción                  |
|------------|--------|-------------------------------------------------------|
| Value      | array  | Una matriz que contiene un objeto [ProductData](#productdata) que contiene los datos de los tréboles relacionados con el juego y un objeto [XboxwideData](#xboxwidedata) que contiene los datos de los clubs de todos los clientes de Xbox Live. Estos datos se incluyen con fines de comparación con los datos del juego.  |
| @nextLink  | string | Si hay páginas adicionales de datos, esta cadena contiene un URI que puedes usar para solicitar la siguiente página de datos. Por ejemplo, este valor se devuelve si el parámetro **Top** de la solicitud se establece en 10000, pero hay más de 10000 filas de datos para la consulta. |
| TotalCount | int    | El número total de filas del resultado de datos de la consulta. |


### <a name="productdata"></a>ProductData

Este recurso contiene datos del Club del juego.

| Value           | Tipo    | Descripción        |
|-----------------|---------|------|
| date            |  string |   La fecha de los datos del Club.   |
|  applicationId               |    string     |  El [identificador de almacén](in-app-purchases-and-trials.md#store-ids) del juego para el que ha recuperado los datos del Club.   |
|  clubsWithTitleActivity               |    int     |  El número de tréboles que participan en el juego.   |     
|  clubsExclusiveToGame               |   int      |  El número de tréboles que están comprometidos exclusivamente con su juego.   |     
|  clubFacts               |   array      |   Contiene uno o más objetos [ClubFacts](#clubfacts) sobre cada club que participa en el equipo social.   |


### <a name="xboxwidedata"></a>XboxwideData

Este recurso contiene datos de Club medios en todos los clientes de Xbox Live.

| Value           | Tipo    | Descripción        |
|-----------------|---------|------|
| date            |  string |   La fecha de los datos del Club.   |
|  applicationId  |    string     |   En el objeto **XboxwideData** , esta cadena siempre es el valor **XBOXWIDE**.  |
|  clubsWithTitleActivity               |   int     |  En promedio, el número de clubs con clientes que están comprometidos con un juego habilitado para Xbox Live.    |     
|  clubsExclusiveToGame               |   int      |  Por término medio, el número de clubs que son socialmente dedicados a un juego habilitado para Xbox Live exclusivamente.   |     
|  clubFacts               |   object      |  Contiene un objeto [ClubFacts](#clubfacts) . Este objeto no tiene sentido en el contexto del objeto **XboxwideData** y tiene valores predeterminados.  |


### <a name="clubfacts"></a>ClubFacts

En el objeto **ProductData** , este objeto contiene los datos de un club específico que tiene una actividad relacionada con el juego. En el objeto **XboxwideData** , este objeto no tiene sentido y tiene valores predeterminados.

| Value           | Tipo    | Descripción        |
|-----------------|---------|--------------------|
|  name            |  string  |   En el objeto **ProductData** , es el nombre del Club. En el objeto **XboxwideData** , siempre es el valor **XBOXWIDE**.           |
|  memberCount               |    int     | En el objeto **ProductData** , este es el número de miembros del Club, excluidos los que no son miembros que solo visitan el Club. En el objeto **XboxwideData** , siempre es 0.    |
|  titleSocialActionsCount               |    int     |  En el objeto **ProductData** , este es el número de acciones sociales que han realizado los miembros del Club y que están relacionadas con el juego. En el objeto **XboxwideData** , siempre es 0   |
|  isExclusiveToGame               |    Boolean     |  En el objeto **ProductData** , indica si el Club actual es social embragado exclusivamente con su juego. En el objeto **XboxwideData** , siempre es true.  |


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

## <a name="related-topics"></a>Temas relacionados

* [Acceder a datos de análisis mediante servicios de Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Obtener datos de análisis de Xbox Live](get-xbox-live-analytics.md)
* [Obtener datos de logros de Xbox Live](get-xbox-live-achievements-data.md)
* [Obtener datos de estado de Xbox Live](get-xbox-live-health-data.md)
* [Obtener datos del concentrador de juegos de Xbox Live](get-xbox-live-game-hub-data.md)
* [Obtener datos de multijugador de Xbox Live](get-xbox-live-multiplayer-data.md)