---
title: Empaquetar los juegos DirectX para la Plataforma universal de Windows (UWP)
description: Los juegos para la Plataforma universal de Windows UWP de gran tamaño, especialmente aquellos que admiten varios idiomas con activos específicos de alguna región o activos de alta definición como característica opcional, pueden crecer hasta alcanzar tamaños considerables.
ms.assetid: 68254203-c43c-684f-010a-9cfa13a32a77
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, juegos, DirectX, paquete
ms.localizationpriority: medium
ms.openlocfilehash: 8a1e255a5219e39d866915bce25778dc8f1dfdba
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175249"
---
#  <a name="package-your-universal-windows-platform-uwp-directx-game"></a>Empaquetar los juegos DirectX para la Plataforma universal de Windows (UWP)

Los juegos para la Plataforma universal de Windows UWP de gran tamaño, especialmente aquellos que admiten varios idiomas con activos específicos de alguna región o activos de alta definición como característica opcional, pueden crecer hasta alcanzar tamaños considerables. En este tema, aprende cómo usar los paquetes de la aplicación y los lotes de aplicaciones para personalizar tu aplicación de modo que los clientes solamente reciban los recursos que de verdad necesitan.

Aparte del modelo de paquetes de aplicaciones, Windows 10 admite lotes de aplicaciones que reúnen dos tipos de paquetes:

-   Paquetes de la aplicación que contienen bibliotecas y ejecutables específicos de la plataforma. Generalmente, un juego para UWP puede tener hasta tres paquetes de la aplicación: uno para cada una de las siguientes arquitecturas de x86, x64 y CPU ARM. Todo el código y los datos específicos de esa plataforma de hardware deben incluirse en su paquete de la aplicación. Dicho paquete también debe contener todos los activos clave para que el juego se ejecute con un nivel de línea base de fidelidad y rendimiento.
-   Los paquetes de recursos contienen datos expandidos u opcionales independientes de la plataforma, como activos de juegos (texturas, redes, sonido, texto). Un juego para UWP puede tener uno o más paquetes de recursos, incluidos paquetes de recursos para texturas o activos de alta definición, recursos de nivel de característica 11+ de DirectX o recursos y activos específicos del idioma.

Para obtener más información sobre los lotes de aplicaciones y los paquetes de la aplicación, lee [Definición de recursos de la aplicación](/previous-versions/windows/apps/hh965321(v=win.10)).

Si bien puedes colocar todo el contenido en tus paquetes de la aplicación, esto no es eficiente y es redundante. Entonces, ¿por qué hacer que se replique tres veces el mismo gran archivo de textura en cada plataforma, especialmente para plataformas de ARM que probablemente no lo usen? Un buen objetivo es intentar minimizar lo que el cliente tiene que descargar para que pueda comenzar a jugar con más rapidez, ahorrar espacio en el dispositivo y evitar posibles gastos de ancho de banda de uso medido.

Para usar esta característica del instalador de aplicaciones para UWP, es importante que tengas en cuenta las convenciones de nomenclatura de archivos y el diseño del directorio, para empaquetar los recursos y las aplicaciones en una etapa temprana del desarrollo del juego, con el fin de que las herramientas y el origen puedan producirlos correctamente de forma que el empaquetado se realice con más facilidad. Sigue las reglas detalladas en este documento cuando desarrolles o configures la creación de activos y la administración de scripts y herramientas, y cuando crees código que se carga o recursos de referencia.

## <a name="why-create-resource-packs"></a>¿Por qué crear paquetes de recursos?


Cuando creas una aplicación, en particular una aplicación de juegos que puede venderse en muchas configuraciones regionales o en una gran variedad de plataformas de hardware para UWP, a menudo necesitas incluir varias versiones de muchos archivos para que esas configuraciones regionales o plataformas sean compatibles. Por ejemplo, si estás lanzando tu juego tanto en los Estados Unidos como en Japón, probablemente necesites un conjunto de archivos de voz en Inglés para las configuraciones regionales de en-us y otro en japonés para la configuración regional jp-jp. O, si quieres usar una imagen en el juego para dispositivos de ARM, así como para plataformas de x86 y x64, debes cargar los mismos activos de imagen tres veces, una por cada arquitectura de CPU.

Además, si tu juego tiene muchos recursos de alta definición que no se aplican en plataformas con niveles de características de DirectX más bajos, ¿por qué incluirlos en el paquete de la aplicación de línea base y exigir al usuario que se descargue un gran volumen de componentes que el dispositivo no puede usar? Si separas estos recursos de alta definición en un paquete de recursos opcional, los clientes con dispositivos que sean compatibles con esos recursos de alta definición pueden obtenerlos a costa de su ancho de banda (posiblemente medido), mientras que aquellos que no tengan dispositivos más avanzados pueden obtener el juego con rapidez y usando menos recursos de red.

Los candidatos de contenido para paquetes de recursos de juego son, entre otros:

-   Activos específicos de una configuración regional internacional (imágenes, audio o texto localizado)
-   Activos de alta resolución para factores de escalado de dispositivos diferentes (1.0x, 1.4x y 1.8x)
-   Activos de alta definición para niveles de característica de DirectX más altos (9, 10 y 11)

Todo esto se define en el package.appxmanifest que forma parte de tu proyecto de UWP, y en la estructura de directorios del paquete final. Debido a la nueva interfaz de usuario de Visual Studio, si sigues el proceso descrito en este documento, no necesitarás editarlo manualmente.

> **Importante**    La carga y la administración de estos recursos se administran a través de las API de **Windows. ApplicationModel. Resources** \* . Si usas estas API de recursos del modelo de aplicaciones para cargar el archivo correcto de una configuración regional, factor de escalado o nivel de características de DirectX, no necesitas cargar los activos mediante rutas de archivo explícitas. En su lugar, debes proporcionar las API de recursos con el nombre de archivo del activo generalizado que quieres, y permitir que el sistema de administración de recursos obtenga la variante correcta del recurso para la plataforma actual del usuario y su configuración regional (que también puedes especificar directamente con estas mismas API).

 

Los recursos para el empaquetado de recursos se especifican de una de dos maneras básicas:

-   Los archivos de activos tienen el mismo nombre de archivo y las versiones específicas del paquete de recursos se encuentran en directorios con nombres específicos. El sistema reserva estos nombres de directorio. Por ejemplo, \\ en-US, \\ scale-140, \\ dxfl-DX11.
-   Los archivos de activos se almacenan en carpetas con nombres arbitrarios, pero los nombres de los archivos en sí tienen una etiqueta común que se anexa a las cadenas que el sistema reserva para indicar el idioma u otro calificador. En concreto, las cadenas de calificador se colocan en el nombre de archivo generalizado después de un carácter de subrayado (" \_ "). Por ejemplo, \\ el \\ menú activos \_ Opción1 \_lang-en-us.png, el \\ \\ menú recursos \_ Opción1 \_scale-140.png, \\ assets \\ coolsign \_ dxfl-DX11. DDS. También puedes combinar estas cadenas. Por ejemplo, \\ \\ el menú recursos \_ opción1 \_ Scale-140 \_lang-en-us.png.
    > **Nota:**    Cuando se usa en un nombre de archivo en lugar de solo en un nombre de directorio, un calificador de lenguaje debe tener la forma "lang- <tag> ", por ejemplo, "lang-en-US", tal y como se describe en [adaptar los recursos para el idioma, la escala y otros calificadores](../app-resources/tailor-resources-lang-scale-contrast.md).

     

Puedes combinar los nombres de directorio de una especificidad adicional en el empaquetado de recursos. No obstante, no pueden ser redundantes. Por ejemplo, \\ el menú en-US \\ \_ Opción1 \_lang-en-us.png es redundante.

Puedes especificar cualquier nombre de subdirectorio no reservado que necesites debajo de un directorio de recursos, siempre y cuando la estructura del directorio sea idéntica en cada directorio de recursos. Por ejemplo, \\ dxfl-contenido DX10 \\ assets \\ Textures \\ coolsign. DDS. Cuando se carga un activo o se hace referencia a él, el nombre de ruta de acceso debe ser generalizado; esto es, se debe quitar cualquier calificador de idioma, escala o nivel de características de DirectX, independientemente de si están en nodos de carpeta o en los nombres de archivo. Por ejemplo, para hacer referencia en el código a un recurso para el que una de las variantes es \\ dxfl-contenido DX10 de las \\ \\ texturas de activos \\ coolsign. DDS, use \\ \\ texturas de activos \\ coolsign. DDS. Del mismo modo, para hacer referencia a un recurso con un \\ fondo de imágenes variantes \\ \_scale-140.png, use \\ imágenes \\background.png.

He aquí los siguientes nombres de directorio y prefijos de carácter de subrayado de nombre de archivo reservados:

| Tipo de recurso                   | Nombre de directorio del paquete de recursos                                                                                                                  | Sufijo del nombre de archivo del paquete de recursos                                                                                                    |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| Activos localizados             | Todos los idiomas posibles, o las combinaciones de configuración regional e idioma, de Windows 10. (El prefijo de calificador "lang-" no es necesario en un nombre de carpeta). | Un " \_ " seguido del especificador de idioma, configuración regional o idioma-configuración regional. Por ejemplo, " \_ en", " \_ EE. UU." o " \_ en-US", respectivamente. |
| Activos de factores de escalado        | scale-100, scale-140, scale-180. Estos son para los factores de escalado de interfaz de usuario de 1.0x, 1.4x y 1.8x, respectivamente.                                     | " \_ " Seguido de "Scale-100", "Scale-140" o "Scale-180".                                                                    |
| Activos de nivel de característica de DirectX | dxfl-dx9, dxfl-dx10 y dxfl-dx11. Estos son para los niveles de característica 9, 10 y 11 de DirectX respectivamente.                                     | Un " \_ " seguido de "dxfl-DX9", "dxfl-contenido DX10" o "dxfl-DX11".                                                                     |

 

## <a name="defining-localized-language-resource-packs"></a>Definir paquetes de recursos de idiomas localizados


Los archivos específicos de la configuración regional se colocan en directorios de proyecto cuyo nombre es el idioma (por ejemplo, "en").

Cuando configures tu aplicación para admitir activos localizados para múltiples idiomas, debes hacer lo siguiente:

-   Crea un subdirectorio de aplicación (o de la versión del archivo) para cada idioma y configuración regional que admitirás (por ejemplo, en-us, jp-jp, zh-cn, fr-fr, etc.).
-   Durante el desarrollo, coloca copias de TODOS los activos (como gráficos de menú, texturas y archivos de audio localizado) en el correspondiente subdirectorio de configuración regional e idioma, incluso si no son diferentes en los distintos idiomas y configuraciones regionales. Para que la experiencia del usuario sea la mejor, asegúrate de que este sepa si no obtuvo un paquete de recursos de idioma disponible para su configuración regional si existiera uno (o si accidentalmente lo eliminó después la descarga y la instalación).
-   Asegúrate de que cada archivo de recursos de cadena o activo (.resw) tenga el mismo nombre en cada directorio. Por ejemplo, los \_option1.png de menú deben tener el mismo nombre en los \\ directorios en-US y \\ JP-JP aunque el contenido del archivo sea para un idioma diferente. En este caso, los vería como menú \\ en-us \\ \_option1.png y \\ el menú JP-JP \\ \_option1.png.
    > **Nota:**    Opcionalmente, puede anexar la configuración regional al nombre de archivo y almacenarla en el mismo directorio. por ejemplo, \\ el \\ menú activos \_ Opción1 \_lang-en-us.png, el \\ \\ menú recursos \_ Opción1 \_lang-jp-jp.png.

     

-   Usa las API en [**Windows.ApplicationModel.Resources**](/uwp/api/Windows.ApplicationModel.Resources) y [**Windows.ApplicationModel.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) para especificar y cargar los recursos específicos de la configuración regional de tu aplicación. De igual modo, usa referencias de activo que no incluyan una configuración regional específica, ya que estas API determinan la configuración regional adecuada según la configuración del usuario, para recuperar a continuación el recurso apropiado para dicho usuario.
-   En Microsoft Visual Studio 2015, selecciona **PROYECTO->Tienda->Crear paquete de la aplicación...** y crea el paquete.

## <a name="defining-scaling-factor-resource-packs"></a>Definir paquetes de recursos de factor de escalado


Windows 10 proporciona tres factores de escalado de la interfaz de usuario: 1.0x, 1.4x y 1.8x. Los valores de ajuste de escala de cada pantalla se establecen durante la instalación en función de una serie de factores combinados: el tamaño de la pantalla, la resolución de esta y el promedio de distancia asumida del usuario respecto de la pantalla. El usuario también puede ajustar los factores de escala para mejorar la legibilidad. El juego debe ser compatible con el reconocimiento de PPP y con el factor de escalado para que la experiencia sea la mejor posible. Parte de esta compatibilidad significa crear versiones de activos visuales críticos para cada uno de los tres factores de escalado. Esto también incluye interacción del puntero y prueba de acceso.

Cuando configures tu aplicación para que admita paquetes de recursos de diferentes factores de escalado de aplicaciones para UWP, debes hacer lo siguiente:

-   Crea un subdirectorio de aplicaciones (o versión de archivo) para cada factor de escalado que admitirás (scale-100, scale-140 y scale-180).
-   Durante el desarrollo, coloca copias apropiadas para el factor de escalado de TODOS los archivos en cada directorio de recursos de factor de escalado, incluso si no son diferentes entre dichos factores.
-   Asegúrate de que cada activo tenga el mismo nombre en cada directorio. Por ejemplo, los \_option1.png de menú deben tener el mismo nombre en los \\ directorios scale-100 y \\ Scale-180, incluso si el contenido del archivo es diferente. En este caso, los vería comooption1.png de \\ menú Scale- \\ 100 \_ y \\ el menú Scale- \\ 140 \_option1.png.
    > **Nota:**    De nuevo, puede anexar opcionalmente el sufijo de factor de escala al nombre de archivo y almacenarlo en el mismo directorio. por ejemplo, \\ el \\ menú activos \_ Opción1 \_scale-100.png, el \\ \\ menú recursos \_ Opción1 \_scale-140.png.

     

-   Usa las API en [**Windows.ApplicationModel.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) para cargar los activos. Las referencias a activos deben ser generalizadas (sin sufijos) y dejar fuera la variación de escala específica. El sistema recuperará el activo de escala adecuado de acuerdo con la pantalla y la configuración de usuario.
-   En Visual Studio 2015, selecciona **PROYECTO->Tienda->Crear paquete de la aplicación** y crea el paquete.

## <a name="defining-directx-feature-level-resource-packs"></a>Definir paquetes de recursos de nivel de característica de DirectX


Los niveles de características de DirectX corresponden a conjuntos de características de GPU para versiones anteriores o actuales de DirectX (específicamente, Direct3D). Estos incluyen funcionalidades y especificaciones de modelo de sombreador, compatibilidad con el idioma del sombreador, compatibilidad con la compresión de textura y características de canalización de gráficos generales.

El paquete de la aplicación de línea base debe usar los formatos de compresión de textura de línea base: BC1, BC2 o BC3. Cualquier dispositivo para UWP puede usarlos: desde plataformas de ARM de gama baja hasta equipos multimedia y estaciones de trabajo de múltiples GPU exclusivas.

La compatibilidad de formato de textura en el nivel de característica 10 o superior de DirectX debe agregarse a un paquete de recursos para conservar el ancho de banda de descarga y el espacio del disco local. Esto permite usar los esquemas de compresión más avanzados para 11, como BC6H y BC7. (Para ver más detalles, consulta [Texture block compression in Direct3D 11 (Compresión de bloques de textura en Direct3D 11)](/windows/desktop/direct3d11/texture-block-compression-in-direct3d-11)). Estos formatos son más eficientes para los activos de textura de resolución alta admitidos por los GPU modernos y su uso mejora los requisitos de espacio, rendimiento y apariencia del juego en plataformas de gama alta.

| Nivel de característica de DirectX | Compresión de textura admitida |
|-----------------------|-------------------------------|
| 9                     | BC1, BC2, BC3                 |
| 10                    | BC4, BC5                      |
| 11                    | BC6H, BC7                     |

 

Además, cada nivel de característica de DirectX admite diferentes versiones de modelo de sombreador. Se pueden crear recursos de sombreador compilados en una base de nivel por característica, y se los puede incluir en paquetes de recursos de nivel de característica de DirectX. Además, algunos modelos de sombreador de versiones posteriores pueden usar activos, como mapas normales, que las versiones de modelo de sombreador anteriores no pueden. Estos activos específicos del modelo de sombreador también deben incluirse en un paquete de recursos del nivel de característica de DirectX.

El mecanismo de recurso se centra principalmente en los formatos de textura admitidos para los activos, para que admita solamente los tres niveles de característica generales. Si necesita tener sombreadores independientes para subniveles (versiones de punto) como DX9 \_ 1 frente a DX9 \_ 3, el código de representación y administración de recursos debe controlarlos explícitamente.

Cuando configures tu aplicación para que admita paquetes de recursos para diferentes niveles de característica de DirectX, debes realizar lo siguiente:

-   Crea un subdirectorio de aplicaciones (o versión de archivo) para cada nivel de característica de DirectX que admitirás (dxfl-dx9, dxfl-dx10 y dxfl-dx11).
-   Durante el desarrollo, coloca activos específicos del nivel de característica en cada directorio de recursos de nivel de característica. A diferencia de los factores de escalado y configuraciones regionales, probablemente tengas diferentes ramas de código de representación para cada nivel de característica de tu juego, y si tienes texturas, sombreadores compilados u otros activos que solamente se usan en un nivel de característica admitido o en un subconjunto de estos, coloca los activos correspondientes solamente en los directorios para los niveles de característica que los usan. Para los activos que se cargan en todos los diferentes niveles de característica, asegúrate de que cada directorio de recursos de nivel de característica tenga una versión de estos con el mismo nombre. Por ejemplo, en el caso de una textura independiente de nivel de característica denominada "coolsign. DDS", coloque la versión comprimida de BC3 en el \\ directorio dxfl-DX9 y la versión de BC7 comprimido en el \\ directorio dxfl-DX11.
-   Asegúrate de que cada activo (si está disponible para varios niveles de características) tenga el mismo nombre en cada directorio. Por ejemplo, coolsign. DDS debe tener el mismo nombre en los \\ directorios dxfl-DX9 y \\ dxfl-DX11, incluso si el contenido del archivo es diferente. En este caso, los vería como \\ dxfl-DX9 \\ coolsign. DDS y \\ dxfl-DX11 \\ coolsign. DDS.
    > **Nota:**    De nuevo, puede anexar opcionalmente el sufijo de nivel de característica al nombre de archivo y almacenarlo en el mismo directorio. por ejemplo, \\ Textures \\ coolsign \_ dxfl-DX9. DDS, \\ texturizations \\ coolsign \_ dxfl-DX11. DDS.

     

-   Declara los niveles de características de DirectX admitidos cuando configures tus recursos gráficos.
    ```cpp
    D3D_FEATURE_LEVEL featureLevels[] = 
    {
      D3D_FEATURE_LEVEL_11_1,
      D3D_FEATURE_LEVEL_11_0,
      D3D_FEATURE_LEVEL_10_1,
      D3D_FEATURE_LEVEL_10_0,
      D3D_FEATURE_LEVEL_9_3,
      D3D_FEATURE_LEVEL_9_1
    };
    ```

    ```cpp
    ComPtr<ID3D11Device> device;
    ComPtr<ID3D11DeviceContext> context;
    D3D11CreateDevice(
        nullptr,                    // Use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                      // Use 0 unless it is a software device.
        creationFlags,          // defined above
        featureLevels,          // What the app will support.
        ARRAYSIZE(featureLevels),
        D3D11_SDK_VERSION,      // This should always be D3D11_SDK_VERSION.
        &device,                    // created device
        &m_featureLevel,            // The feature level of the device.
        &context                    // The corresponding immediate context.
    );
    ```

-   Usa las API en [**Windows.ApplicationModel.Resources.Core**](/uwp/api/Windows.ApplicationModel.Resources.Core) para cargar los recursos. Las referencias a activos deben ser generalizadas (sin sufijos) y dejar fuera el nivel de características. No obstante, y al contrario de lo que sucede con el idioma y la escala, el sistema no determina automáticamente cuál es el mejor nivel de característica para una pantalla en concreto, sino que dependerá de lo que indiques en la lógica del código. Una vez que hayas decidido esto, usa las API para informar al SO del nivel de característica que prefieres. De este modo, el sistema podrá recuperar el activo adecuado de acuerdo a dicha preferencia. He aquí un código de ejemplo que muestra cómo informar a tu aplicación del nivel de características de DirectX actual para la plataforma:
    
    ```cpp
    // Set the current UI thread's MRT ResourceContext's DXFeatureLevel with the right DXFL. 

    Platform::String^ dxFeatureLevel;
        switch (m_featureLevel)
        {
        case D3D_FEATURE_LEVEL_9_1:
        case D3D_FEATURE_LEVEL_9_2:
        case D3D_FEATURE_LEVEL_9_3:
            dxFeatureLevel = L"DX9";
            break;
        case D3D_FEATURE_LEVEL_10_0:
        case D3D_FEATURE_LEVEL_10_1:
            dxFeatureLevel = L"DX10";
            break;
        default:
            dxFeatureLevel = L"DX11";
        }

        ResourceContext::SetGlobalQualifierValue(L"DXFeatureLevel", dxFeatureLevel);
    ```

    > **Nota:**    En el código, cargue la textura directamente por nombre (o ruta de acceso debajo del directorio de nivel de característica). No incluyas ni el nombre del directorio de niveles de características ni el sufijo. Por ejemplo, cargue "Textures \\ coolsign. DDS", no "dxfl-DX11 \\ Textures \\ coolsign. DDS" o "Textures \\ coolsign \_ dxfl-DX11. DDS".

     

-   A continuación, usa [**ResourceManager**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) para ubicar el archivo que coincide con el nivel de características actual de DirectX. La clase **ResourceManager** devuelve una clase [**ResourceMap**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap), que consultas con [**ResourceMap::GetValue**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getvalue) (o [**ResourceMap::TryGetValue**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.trygetvalue)) y una clase [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext) suministrada. Esto devuelve una clase [**ResourceCandidate**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceCandidate) lo más cercana posible al nivel de características de DirectX especificado, llamando a [**SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue).
    
    ```cpp
    // An explicit ResourceContext is needed to match the DirectX feature level for the display on which the current view is presented.

    auto resourceContext = ResourceContext::GetForCurrentView();
    auto mainResourceMap = ResourceManager::Current->MainResourceMap;

    // For this code example, loader is a custom ref class used to load resources.
    // You can use the BasicLoader class from any of the 8.1 DirectX samples similarly.


    auto possibleResource = mainResourceMap->GetValue(
        L"Files/BumpPixelShader.cso",
        resourceContext
    );
    Platform::String^ resourceName = possibleResource->ValueAsString;
    ```

-   En Visual Studio 2015, selecciona **PROYECTO->Tienda->Crear paquete de la aplicación** y crea el paquete.
-   Asegúrate de habilitar los lotes de aplicaciones en la configuración del manifiesto package.appxmanifest.

## <a name="related-topics"></a>Temas relacionados


* [Definir los recursos de una aplicación](/previous-versions/windows/apps/hh965321(v=win.10))
* [Empaquetado de aplicaciones](../packaging/index.md)
* [Paquete de aplicaciones (MakeAppx.exe)](/windows/desktop/appxpkg/make-appx-package--makeappx-exe-)

 

 