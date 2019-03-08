---
title: Tratamiento de errores de la API de C++
description: Obtenga información sobre cómo controlar los errores al realizar una llamada de servicio de Xbox Live con las APIs de C++.
ms.assetid: 10b47e68-8b1f-4023-96a4-404f3f6a9850
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, control de errores
ms.localizationpriority: medium
ms.openlocfilehash: 90fd816f8d44b27c1df0ded9bee6473f642478a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636530"
---
# <a name="c-api-error-handling"></a>Tratamiento de errores de la API de C++

En la API de C++, en lugar de producir excepciones, la mayoría de las llamadas devolverá xbox_live_result < payload_type > según corresponda.

## <a name="xboxliveresult-structure"></a>estructura xbox_live_result
xbox_live_result tiene 3 elementos:
1. El error devuelto por la operación,
2. Mensaje de error específico que se usa con fines de depuración y
3. La carga del resultado (puede estar vacío si se produjo un error)"

Puede obtener más información sobre xbox_live_result como así como el error de códigos están en la documentación de Xbox Live.

La estructura es la siguiente:

```cpp
template<typename T>
class xbox_live_result
{
    const std::error_code& err();
    const std::string& err_message();
    T& payload();
};
```

**Err** -devuelve el error.  Será una referencia NULL con ningún error.  Este comportamiento es como la STL de C++ error se realiza en que se puede obtener el valor primitivo llamando value().  Llamar a message() obtendrá una representación de cadena.  Por tanto, si el código de error "Argumento no válido", significa que, a continuación, ```err().message()``` sería el texto "Argumento no válido".

**err_message** -se profundiza sobre el error.  Por ejemplo si **err** es "Argumento no válido", a continuación, **err_message** podría proporcionar información sobre qué argumento no es válido.

**carga** -devuelven el elemento de interés.  Por ejemplo, tenga en cuenta ```xbox_live_result<achievement>``` que podría obtener en get_achievement que realiza la llamada.  En este ejemplo, la carga sería la consecución de sí mismo (si no está presente ningún error).

## <a name="example"></a>Ejemplo

```cpp
// Function which returns an xbox_live_result
xbox_live_result<std::shared_ptr<title_presence_change_subscription>> presenceChangeSubscriptionResult =
xbox::services::presence::subscribe_to_title_presence_change(
    xboxUserId,
    titleId
    );

printf("Error value %d, string %s", achievementResult.err().value(), achievementResult.err().message());

// Would output:
// "0 Success" if successful
// "401 Unauthorized" if auth issue

if (!achievementResult.err())
{
  // Do things on success.  Payload will be populated if applicable.
  std::shared_ptr<title_presence_change_subscription> presenceChangeSubscription = presenceChangeSubscriptionResult->payload();

  // ...
}
else if (achievement.err() == xboxlive_error_code::http_status_403_forbidden)
{
  // Special handling for 403 errors
}
else if (achievementResult.err() == xbox_live_error_condition::auth)
{
  // Handle broad auth failures.  See below section for more info on xbox_live_error_condition
}

```

## <a name="using-xboxliveerrorcondition-to-test-against-broad-error-categories"></a>Uso de xbox_live_error_condition para pruebas en categorías de error más amplio
En el ejemplo anterior, se prueba el código de error frente a 403 errores, así como algo llamado ```xbox_live_error_condition::auth```.

 Cuando se usa la función de error() xbox_live_result, uno puede probar con los códigos de error individualmente.  Por ejemplo, 400 errores de la clase podría tener las pruebas individuales y control de flujo para:

* xbox_live_error_code::http_status_400_bad_request
* xbox_live_error_code::http_status_401_unauthorized
* xbox_live_error_code::http_status_403_forbidden
* etc

Pero normalmente esto es que no lo desea hacer y desea probar contra una clase de errores como uno.  Para que pueda probar contra una clase de errores mediante las enumeraciones disponibles en el ```xbox_live_error_condition``` clase.  Implementamos una sobrecarga del operador de igualdad que automatice las pruebas con muchos códigos de error.  Además ```auth```, hay categorías como ```rta``` y ```http```.  Encontrará la lista completa en *errors.h* o en *xblsdk_cpp.chm*.

Para ver un vídeo que cubre esta y otras características de la API de servicios de Xbox de C++, consulte nuestra charla XFest en [Xfest 2015 vídeos](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) en *XSAPI: ¡No hay excepciones de C++!*
