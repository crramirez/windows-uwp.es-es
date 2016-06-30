---
title: Automatizar el inicio de las aplicaciones para la Plataforma universal de Windows (UWP) de Windows 10
description: "Los desarrolladores pueden usar la activación de protocolos y la activación del inicio para automatizar el inicio de los juegos o aplicaciones para UWP para las pruebas automatizadas."
author: listurm
translationtype: Human Translation
ms.sourcegitcommit: adf2d16f9c208631f91fbcad19d1ea8087cd9cb5
ms.openlocfilehash: ae2f80a915f4aed90c269c37a11d01a2f6c9849e

---

# Automatizar el inicio de aplicaciones de Windows 10 para UWP

## Introducción

Los desarrolladores tienen varias opciones para lograr el inicio automatizado de las aplicaciones para la Plataforma universal de Windows (UWP). En este documento exploramos los métodos para iniciar una aplicación mediante la activación de protocolos y del inicio.

La *activación de protocolos* permite que una aplicación se registre como controlador para un protocolo determinado. 

La *activación del inicio* es el inicio normal de una aplicación, como, por ejemplo, al iniciarla desde el icono de la aplicación.

Con cada método de activación, tienes la posibilidad de usar la línea de comandos o una aplicación de selector. Para todos los métodos de activación, si la aplicación se está ejecutando, la activación la llevará al primer plano (lo que la reactiva) y proporcionará argumentos de activación nuevos. Esto ofrece flexibilidad para usar los comandos de activación para proporcionar mensajes nuevos a la aplicación. Es importante tener en cuenta que el proyecto debe compilarse e implementarse para que el método de activación ejecute la aplicación recién actualizada. 

## Activación de protocolos

Sigue estos pasos para configurar la activación de protocolos para las aplicaciones: 

1. Abre el archivo **Package.appxmanifest** en Visual Studio.
2. Selecciona la pestaña **Declaraciones**.
3. En la lista desplegable **Declaraciones disponibles**, selecciona **Protocolo** y, a continuación, **Agregar**.
4. En **Propiedades**, en el campo **Nombre**, escribe un nombre único para iniciar la aplicación. 

    ![Activación de protocolos](images/automate-uwp-apps-1.png)

5. Guarda el archivo e implementa el proyecto. 
6. Una vez implementado el proyecto, la activación de protocolos debería estar lista. 
7. Ve a **Control Panel\All Control Panel Items\Default Programs** y selecciona **Asociar un tipo de archivo o protocolo con un determinado programa**. Desplázate hasta la sección **Protocolos** para ver si aparece el protocolo en la lista. 

Ahora que la activación de protocolos está configurada, tienes dos opciones (la línea de comandos o la aplicación de selector) para activar la aplicación mediante el protocolo. 

### Línea de comandos

Para activar la aplicación con un protocolo, usa la línea de comandos con el inicio de comando seguido del nombre de protocolo definido previamente, dos puntos (":") y cualquier parámetro. Los parámetros pueden ser cualquier cadena arbitraria. Sin embargo, para aprovechar las funcionalidades del identificador uniforme de recursos (URI), es recomendable seguir el formato de URI estándar: 

  ```
  scheme://username:password@host:port/path.extension?query#fragment
  ```

El objeto de URI tiene métodos para analizar una cadena URI en este formato. Para obtener más información, consulta [Uri class (MSDN)](https://msdn.microsoft.com/en-us/library/windows/apps/windows.foundation.uri.aspx). 

Ejemplos:

  ```
  >start bingnews:
  >start myapplication:protocol-parameter
  >start myapplication://single-player/level3?godmode=1&ammo=200
  ```

La activación de línea de comandos de protocolo admite caracteres Unicode hasta un límite 2038 caracteres en el URI sin procesar. 

### Aplicación de selector

Para iniciar, crea una aplicación independiente que admita la API de WinRT. En el ejemplo siguiente se muestra código de C++ para iniciar con la activación de protocolos en un programa de selector, donde **PackageURI** es el URI de la aplicación con los argumentos correspondientes; por ejemplo `myapplication:` o `myapplication:protocol activation arguments`.

```
bool ProtocolLaunchURI(Platform::String^ URI)
{
       IAsyncOperation<bool>^ protocolLaunchAsyncOp;
       try
       {
              protocolLaunchAsyncOp = Windows::System::Launcher::LaunchUriAsync(ref new 
Uri(URI));
       }
       catch (Platform::Exception^ e)
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI Exception Thrown: " 
+ e->ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }

       concurrency::create_task(protocolLaunchAsyncOp).wait();

       if (protocolLaunchAsyncOp->Status == AsyncStatus::Completed)
       {
              bool LaunchResult = protocolLaunchAsyncOp->GetResults();
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI 
+ " completed. Launch result " + LaunchResult + "\n";
              OutputDebugString(dbgStr->Data());
              return LaunchResult;
       }
       else
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI + " failed. Status:" 
+ protocolLaunchAsyncOp->Status.ToString() + " ErrorCode:" 
+ protocolLaunchAsyncOp->ErrorCode.ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }
}
```
La activación de protocolos con la aplicación de selector tiene las mismas limitaciones de argumentos que la activación de protocolos con la línea de comandos. Ambas admiten caracteres Unicode hasta un límite 2038 caracteres en el URI sin procesar. 

## Activación de selector

También puedes iniciar la aplicación mediante la activación de selector. No se requiere ninguna configuración, pero se necesita el identificador de modelo de usuario de aplicación (AUMID) de la aplicación para UWP. El AUMID es el nombre de familia de paquete seguido de un signo de exclamación y el identificador de la aplicación. 

La mejor forma de obtener el nombre de familia de paquete es completar estos pasos:

1. Abre el archivo **Package.appxmanifest**.
2. En la pestaña **Empaquetado**, escribe el **Nombre del paquete**.

    ![Activación de selector](images/automate-uwp-apps-2.png)

3. Si el **nombre de familia de paquete** no se muestra, abre PowerShell y ejecuta `>get-appxpackage MyPackageName` para encontrar **PackageFamilyName**.

El identificador de aplicación se puede encontrar en el archivo **Package.appxmanifest** (abierto en la vista XML), bajo el elemento `<Applications>`.

### Línea de comandos

Una herramienta para realizar una activación de inicio de una aplicación para UWP se instala con el SDK de Windows 10. Se puede ejecutar desde la línea de comandos y usa el AUMID de la aplicación para iniciarse como argumento.

```
C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe <AUMID>
```

Debe tener este aspecto:

```
"C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe" MyPackageName_ph1m9x8skttmg!AppId
```

Esta opción no admite argumentos de línea de comandos. 

### Aplicación de selector

Puedes crear una aplicación independiente que admita el uso de COM para el inicio. En el ejemplo siguiente se muestra el código de C++ para iniciarse con la activación de inicio en un programa selector. Con este código, puedes crear un objeto **ApplicationActivationManager** y llamar a **ActivateApplication** pasando el AUMID encontrado anteriormente y los argumentos correspondientes. Para obtener más información acerca de los otros parámetros, consulta el tema sobre el [método IApplicationActivationManager::ActivateApplication (MSDN)](https://msdn.microsoft.com/en-us/library/windows/desktop/hh706903(v=vs.85).aspx).

```
#include <ShObjIdl.h>
#include <atlbase.h>

HRESULT LaunchApp(LPCWSTR AUMID)
{
     HRESULT hr = CoInitializeEx(nullptr, COINIT_APARTMENTTHREADED);
     if (FAILED(hr))
     {
            wprintf(L"LaunchApp %s: Failed to init COM. hr = 0x%08lx \n", AUMID, hr);
     }
     {
            CComPtr<IApplicationActivationManager> AppActivationMgr = nullptr;
            if (SUCCEEDED(hr))
            {
                   hr = CoCreateInstance(CLSID_ApplicationActivationManager, nullptr,  
CLSCTX_LOCAL_SERVER, IID_PPV_ARGS(&AppActivationMgr));
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to create Application Activation 
Manager. hr = 0x%08lx \n", AUMID, hr);
                   }
            }
            if (SUCCEEDED(hr))
            {
                   DWORD pid = 0;
                   hr = AppActivationMgr->ActivateApplication(AUMID, nullptr, AO_NONE, 
&pid);
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to Activate App. hr = 0x%08lx 
\n", AUMID, hr);
                   }
            }
     }
     CoUninitialize();
     return hr;
}
```

Este método admite los argumentos que se pasan, a diferencia del método para iniciar anterior (es decir, la línea de comandos).

## Aceptar argumentos

Para aceptar los argumentos pasados en la activación de la aplicación para UWP, debes agregar código a la aplicación. Para determinar si se produjo la activación de protocolo o la activación de inicio, invalida el evento **OnActivated** y comprueba el tipo de argumento y, después, obtén la cadena sin procesar o los valores analizados previamente del objeto de URI. 

En este ejemplo se muestra cómo obtener la cadena sin procesar.

```
void OnActivated(IActivatedEventArgs^ args)
{
        // Check for launch activation
        if (args->Kind == ActivationKind::Launch)
        {
            auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args); 
Platform::String^ argval = launchArgs->Arguments;
            // Manipulate arguments …
        }

        // Check for protocol activation
        if (args->Kind == ActivationKind::Protocol)
        {
            auto protocolArgs = static_cast< ProtocolActivatedEventArgs^>(args);
            Platform::String^ argval = protocolArgs->Uri->ToString();
            // Manipulate arguments …
        }
    }
```

## Resumen
En resumen, puedes usar distintos métodos para iniciar la aplicación para UWP. Dependiendo de los requisitos y de los casos de uso, algunos métodos pueden resultar más adecuados que otros. 



<!--HONumber=Jun16_HO4-->


