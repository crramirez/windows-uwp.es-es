---
author: normesta
ms.assetid: E2A1200C-9583-40FA-AE4D-C9E6F6C32BCF
title: Enviar un elemento de trabajo al grupo de subprocesos
description: Obtén información acerca de cómo trabajar en un subproceso distinto mediante el envío de un elemento de trabajo al grupo de subprocesos.
ms.author: normesta
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, subprocesos, grupo de subprocesos
ms.localizationpriority: medium
ms.openlocfilehash: fe73520782b18fb7419807695296bc6487f9c018
ms.sourcegitcommit: 9f8010fe67bb3372db1840de9f0be36097ed6258
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/16/2018
ms.locfileid: "7103153"
---
# <a name="submit-a-work-item-to-the-thread-pool"></a>Enviar un elemento de trabajo al grupo de subprocesos

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** API importantes **

-   [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593)
-   [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580)

Obtén información acerca de cómo realizar trabajo en un subproceso separado mediante el envío de un elemento de trabajo al grupo de subprocesos. Usa esto para mantener una interfaz de usuario con capacidad de respuesta mientras se completa un trabajo que tarda una cantidad de tiempo considerable, y para completar varias tareas en paralelo.

## <a name="create-and-submit-the-work-item"></a>Crear y enviar el elemento de trabajo

Crea un elemento de trabajo mediante una llamada a [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593). Proporciona un delegado que haga el trabajo (puedes usar una función lambda o una función delegada). Ten en cuenta que **RunAsync** devuelve un objeto [**IAsyncAction**](https://msdn.microsoft.com/library/windows/apps/BR206580). Almacena este objeto para usarlo en el paso siguiente.

Hay tres versiones de [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593) disponibles para que puedas especificar opcionalmente la prioridad del elemento de trabajo, así como controlar si se ejecuta simultáneamente con otros elementos de trabajo.

>[!NOTE]
>Usar [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) para acceder al subproceso de interfaz de usuario y mostrar el progreso del elemento de trabajo.

En el siguiente ejemplo se crea un elemento de trabajo y se envía un lambda para que realice el trabajo:

```csharp
// The nth prime number to find.
const uint n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
ulong nthPrime = 0;

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
IAsyncAction asyncAction = Windows.System.Threading.ThreadPool.RunAsync(
    (workItem) =>
{
    uint  progress = 0; // For progress reporting.
    uint  primes = 0;   // Number of primes found so far.
    ulong i = 2;        // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status == AsyncStatus.Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (uint j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            uint temp = progress;
            progress = (uint)(10.0*primes/n);

            if (progress != temp)
            {
                String updateString;
                updateString = "Progress to " + n + "th prime: "
                    + (10 * progress) + "%\n";

                // Update the UI thread with the CoreDispatcher.
                CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
                    CoreDispatcherPriority.High,
                    new DispatchedHandler(() =>
                {
                    UpdateUI(updateString);
                }));
            }
        }
    }

    // Return the nth prime number.
    nthPrime = i;
});

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

```cppwinrt
// The nth prime number to find.
const unsigned int n{ 9999 };

unsigned long nthPrime{ 0 };

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = Windows::System::Threading::ThreadPool::RunAsync([&](Windows::Foundation::IAsyncAction const& workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem.Status() == Windows::Foundation::AsyncStatus::Canceled)
        {
            break;
        }

        // Go to the next number.
        i++;

        // Check for prime.
        bool prime = true;
        for (unsigned int j = 2; j < i; ++j)
        {
            if ((i % j) == 0)
            {
                prime = false;
                break;
            }
        };

        if (prime)
        {
            // Found another prime number.
            primes++;

            // Report progress at every 10 percent.
            unsigned int temp = progress;
            progress = static_cast<unsigned int>(10.f*primes / n);

            if (progress != temp)
            {
                std::wstringstream updateString;
                updateString << L"Progress to " << n << L"th prime: " << (10 * progress) << std::endl;

                // Update the UI thread with the CoreDispatcher.
                Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
                    Windows::UI::Core::CoreDispatcherPriority::High,
                    Windows::UI::Core::DispatchedHandler([&]()
                {
                    UpdateUI(updateString.str());
                }));
            }
        }
    }
    // Return the nth prime number.
    nthPrime = i;
});
```

```cpp
// The nth prime number to find.
const unsigned int n = 9999;

// A shared pointer to the result.
// We use a shared pointer to keep the result alive until the
// thread is done.
std::shared_ptr<unsigned long> nthPrime = make_shared<unsigned long int>(0);

// Simulates work by searching for the nth prime number. Uses a
// naive algorithm and counts 2 as the first prime number.
auto workItem = ref new WorkItemHandler(
    \[this, n, nthPrime](IAsyncAction^ workItem)
{
    unsigned int progress = 0; // For progress reporting.
    unsigned int primes = 0;   // Number of primes found so far.
    unsigned long int i = 2;   // Number iterator.

    if ((n >= 0) && (n <= 2))
    {
        *nthPrime = n;
        return;
    }

    while (primes < (n - 1))
    {
        if (workItem->Status == AsyncStatus::Canceled)
       {
           break;
       }

       // Go to the next number.
       i++;

       // Check for prime.
       bool prime = true;
       for (unsigned int j = 2; j < i; ++j)
       {
           if ((i % j) == 0)
           {
               prime = false;
               break;
           }
       };

       if (prime)
       {
           // Found another prime number.
           primes++;

           // Report progress at every 10 percent.
           unsigned int temp = progress;
           progress = static_cast<unsigned int>(10.f*primes / n);

           if (progress != temp)
           {
               String^ updateString;
               updateString = "Progress to " + n + "th prime: "
                   + (10 * progress).ToString() + "%\n";

               // Update the UI thread with the CoreDispatcher.
               CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
                   CoreDispatcherPriority::High,
                   ref new DispatchedHandler([this, updateString]()
               {
                   UpdateUI(updateString);
               }));
           }
       }
   }
   // Return the nth prime number.
   *nthPrime = i;
});

auto asyncAction = ThreadPool::RunAsync(workItem);

// A reference to the work item is cached so that we can trigger a
// cancellation when the user presses the Cancel button.
m_workItem = asyncAction;
```

Después de la llamada a [**RunAsync**](https://msdn.microsoft.com/library/windows/apps/BR230593), el grupo de subprocesos pone el elemento de trabajo en cola y este se ejecuta cuando hay un subproceso disponible. Los elementos de trabajo del grupo de subprocesos se ejecutan de manera asincrónica en cualquier orden, de manera que debes asegurarte de que los elementos de trabajo funcionen de manera independiente.

Ten en cuenta que el elemento de trabajo comprueba la propiedad [**IAsyncInfo.Status**](https://msdn.microsoft.com/library/windows/apps/BR206593) y existe si el elemento de trabajo se cancela.

## <a name="handle-work-item-completion"></a>Controlar la finalización del elemento de trabajo

Proporciona un controlador de finalización mediante la configuración de la propiedad [**IAsyncAction.Completed**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.iasyncaction.completed.aspx) del elemento de trabajo. Proporciona un delegado (puedes usar una función lambda o una función delegada) para controlar que el elemento de trabajo se complete. Por ejemplo, usa [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/Hh750317) para acceder al subproceso de interfaz de usuario y mostrar el resultado.

En el siguiente ejemplo se actualiza la interfaz de usuario con el resultado del elemento de trabajo enviado en el paso 1:

```cpp
asyncAction->Completed = ref new AsyncActionCompletedHandler(
    \[this, n, nthPrime](IAsyncAction^ asyncInfo, AsyncStatus asyncStatus)
{
    if (asyncStatus == AsyncStatus::Canceled)
    {
        return;
    }

    String^ updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + (*nthPrime).ToString() + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication::MainView->CoreWindow->Dispatcher->RunAsync(
        CoreDispatcherPriority::High,
        ref new DispatchedHandler([this, updateString]()
    {
        UpdateUI(updateString);
    }));
});
```

```cppwinrt
m_workItem.Completed([&](Windows::Foundation::IAsyncAction const& asyncInfo, Windows::Foundation::AsyncStatus const& asyncStatus)
{
    if (asyncStatus == Windows::Foundation::AsyncStatus::Canceled)
    {
        return;
    }

    std::wstringstream updateString;
    updateString << std::endl << L"The " << n << L"th prime number is " << nthPrime << std::endl;

    // Update the UI thread with the CoreDispatcher.
    Windows::ApplicationModel::Core::CoreApplication::MainView().CoreWindow().Dispatcher().RunAsync(
        Windows::UI::Core::CoreDispatcherPriority::High,
        Windows::UI::Core::DispatchedHandler([&]()
    {
        UpdateUI(updateString.str());
    }));
});
```

```c#
asyncAction.Completed = new AsyncActionCompletedHandler(
    (IAsyncAction asyncInfo, AsyncStatus asyncStatus) =>
{
    if (asyncStatus == AsyncStatus.Canceled)
    {
        return;
    }

    String updateString;
    updateString = "\n" + "The " + n + "th prime number is "
        + nthPrime + ".\n";

    // Update the UI thread with the CoreDispatcher.
    CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(
        CoreDispatcherPriority.High,
        new DispatchedHandler(()=>
    {
        UpdateUI(updateString);
    }));
});
```

Ten en cuenta que el controlador de finalización comprueba si el elemento de trabajo se ha cancelado antes de enviar una actualización de interfaz de usuario.

## <a name="summary-and-next-steps"></a>Resumen y pasos siguientes

Puedes obtener más información, descarga el código de este inicio rápido en la [creación de un grupo de subprocesos funcionan elemento muestra](http://go.microsoft.com/fwlink/p/?LinkID=328569) escrita para Windows8.1 y vuelve a usar el código fuente en una aplicación Windows 10 win\_unap.

## <a name="related-topics"></a>Temas relacionados

* [Enviar un elemento de trabajo al grupo de subprocesos](submit-a-work-item-to-the-thread-pool.md)
* [Procedimientos recomendados para usar el grupo de subprocesos](best-practices-for-using-the-thread-pool.md)
* [Enviar un elemento de trabajo con un temporizador](use-a-timer-to-submit-a-work-item.md)
 
