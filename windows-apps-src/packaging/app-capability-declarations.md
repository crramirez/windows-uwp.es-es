---
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: Declaraciones de funcionalidades de las aplicaciones
description: Las funcionalidades deben declararse en el manifiesto del paquete de la aplicación de la Plataforma universal de Windows (UWP) para acceder a determinadas API o ciertos recursos como imágenes, música o dispositivos, como la cámara o el micrófono.
---
# Declaraciones de funcionalidades de las aplicaciones

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Las funcionalidades deben declararse en el [manifiesto del paquete](https://msdn.microsoft.com/library/windows/apps/BR211474) de la aplicación de la Plataforma universal de Windows (UWP) para acceder a determinadas API o ciertos recursos como imágenes, música o dispositivos, como la cámara o el micrófono.

Para solicitar acceso a los recursos o API específicos, declara funcionalidades en el [manifiesto del paquete](https://msdn.microsoft.com/library/windows/apps/BR211474) de tu aplicación. Puedes declarar funcionalidades generales mediante el [Diseñador de manifiestos](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx) en Microsoft Visual Studio o puedes agregarlas manualmente. Para obtener más información, consulta [Cómo especificar funcionalidades en un manifiesto del paquete](https://msdn.microsoft.com/library/windows/apps/BR211477). Es importante saber que cuando los clientes compren tu aplicación en la Tienda, se les notificarán todas las funcionalidades que la aplicación declara. Evita declarar funcionalidades que la aplicación no necesite.

Algunas funcionalidades otorgan a las aplicaciones acceso a un *recurso con información confidencial*. Estos recursos se consideran confidenciales porque pueden acceder a datos personales del usuario o costarle dinero al usuario. La configuración de privacidad, administrada por la aplicación Configuración, permite al usuario controlar dinámicamente el acceso a recursos con información confidencial. Por lo tanto, es importante que la aplicación no suponga que un recurso con información confidencial está siempre disponible. Para obtener más información sobre cómo acceder a recursos confidenciales, consulta las [Directrices para aplicaciones compatibles con la privacidad](https://msdn.microsoft.com/library/windows/apps/Hh768223). Las capacidades que proporcionan las aplicaciones con acceso a un *recurso con información confidencial* están marcadas con un asterisco (\*) junto al escenario de la capacidad.

En este artículo, se estudian cuatro categorías de funcionalidades que se describen a continuación.

-   Funcionalidades de uso general que se aplican a la mayoría de escenarios de las aplicaciones.

-   Funcionalidades de dispositivo que permiten a tu aplicación acceder a los periféricos y dispositivos internos.

-   Funcionalidades de uso especial que requieren una cuenta de empresa especial para enviarlas a la Tienda para usarlas. Para obtener más información sobre las cuentas de empresa, consulta [Tipos de cuenta, ubicaciones y precios](https://msdn.microsoft.com/library/windows/apps/JJ863494).

-   Funcionalidades restringidas que solo están disponibles para Microsoft y sus asociados.

## Funcionalidades de uso general

Las funcionalidades de uso general se aplican a la mayoría de escenarios de las aplicaciones.

<table>
        <thead>
            <tr>
                <th>Escenario de funcionalidad</th>
                <th>Uso de la funcionalidad</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>**Música***</td>
                <td>
                    The **musicLibrary** capability provides programmatic access to the user's Music, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in jukebox apps that make use of the entire Music library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **musicLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **musicLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="musicLibrary"/&gt;
&lt;/Funcionalidades&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Imágenes***</td>
                <td>
                    La funcionalidad **picturesLibrary** proporciona acceso mediante programación a las imágenes del usuario, permitiendo a la aplicación enumerar todos los archivos de la biblioteca y acceder a ellos sin que haya interacción del usuario. Esta funcionalidad se usa normalmente en aplicaciones de fotografías que hacen uso de la biblioteca de imágenes completa.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **picturesLibrary** capability only when the scenarios for your app require programmatic access and can't be realized them by using the **file picker**.

                    The **picturesLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="picturesLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Vídeos***</td>
                <td>
                    The **videosLibrary** capability provides programmatic access to the user's Videos, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in movie-playback apps that make use of the entire Videos library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **videosLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **videosLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="videosLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Almacenamiento extraíble**</td>
                <td>
                    The **removableStorage** capability provides programmatic access to files on removable storage, like USB keys and external hard drives, filtered to the file-type associations declared in the package manifest. For example, if a document-reader app declares a .doc file-type association, it can open .doc files on the removable storage device, but not other types of files. Be careful when you declare this capability, because users may include a variety of info in their removable storage devices, and will expect your app to provide a valid justification for programmatic access to the removable storage for all files of the declared type.

                    Users will expect your app to handle any file associations that you declare. So don't declare file associations that your app cannot handle responsibly. The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app.

                    Declare the **removableStorage** capability only when the scenarios for your app require programmatic access and can't be realized by using the [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847).

                    The **removableStorage** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="removableStorage"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Internet y redes públicas***</td>
                <td>
                    There are two capabilities that provide different levels of access to the Internet and public networks.

                    The **internetClient** capability indicates that apps can receive incoming data from the Internet. Cannot act as a server. No local network access.

                    The **internetClientServer** capability indicates that apps can receive incoming data from the Internet. Can act as a server. No local network access.

                    Most apps that have a web service component will use **internetClient**. Apps that enable peer-to-peer (P2P) scenarios where the app needs to listen for incoming network connections should use **internetClientServer**. The **internetClientServer** capability includes the access that the **internetClient** capability provides, so you don't need to specify **internetClient** when you specify **internetClientServer**.
                </td>
            </tr>
            <tr>
                <td>**Homes and work networks***</td>
                <td>
                    The **privateNetworkClientServer** capability provides inbound and outbound access to home and work networks through the firewall. This capability is typically used for games that communicate across the local area network (LAN), and for apps that share data across a variety of local devices. If your app specifies **musicLibrary**, **picturesLibrary**, or **videosLibrary**, you don't need to use this capability to access the corresponding library in a Home Group. On Windows, this capability does not provide access to the Internet.
                </td>
            </tr>
            <tr>
                <td>**Appointments**</td>
                <td>
                    The **appointments** capability provides access to the user’s appointment store. This capability allows read access to appointments obtained from the synced network accounts and to other apps that write to the appointment store. With this capability, your app can create new calendars and write appointments to calendars that it creates.

                    The **appointments** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="appointments"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Contactos***</td>
                <td>
                    The **contacts** capability provides access to the aggregated view of the contacts from various contacts stores. This capability gives the app limited access (network permitting rules apply) to contacts that were synced from various networks and the local contact store.

                    The **contacts** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="contacts"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Generación de código**</td>
                <td>
                    The **codeGeneration** capability allows apps to access the following functions which provide JIT capabilities to apps.

                    - [**VirtualProtectFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169846)
                    - [**CreateFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994453)
                    - [**OpenFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169844)
                    - [**MapViewOfFileFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994454)
                </td>
            </tr>
            <tr>
                <td>**AllJoyn**</td>
                <td>
                    The **allJoyn** capability allows AllJoyn-enabled apps and devices on a network to discover and interact with each other.

                    All apps that access APIs in the [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) namespace must use this capability.
                </td>
            </tr>
            <tr>
                <td>**Phone calls**</td>
                <td>
                    The **phoneCall** capability allows apps to access all of the phone lines on the device and perform the following functions.

                    - Place a call on the phone line and show the system dialer without prompting the user.
                    - Access line-related metadata.
                    - Access line-related triggers.
                    - Allows the user-selected spam filter app to set and check block list and call origin information.

                    The **phoneCall** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="phoneCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    The **phoneCallHistoryPublic** capability allows apps to read cellular and some VOIP call history information on the device. This capability also allows the app to write VOIP call history entries. This capability is required to access all members of the [**PhoneCallHistoryStore**](https://msdn.microsoft.com/library/windows/apps/Dn705931) class.
                </td>
            </tr>
            <tr>
                <td>**Carpeta de llamadas grabadas***</td>
                <td>
                    <p>La funcionalidad de dispositivo **recordedCallsFolder** permite que las aplicaciones accedan a la carpeta de llamadas grabadas.</p>
                    <p>La funcionalidad **recordedCallsFolder** debe incluir el espacio de nombres **mobile** cuando la declares en el manifiesto del paquete de la aplicación, como se muestra a continuación.</p>
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;mobile:Capability Name="recordedCallsFolder"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Información de cuenta de usuario***</td>
                <td>
                    <p>La funcionalidad **userAccountInformation** permite a las aplicaciones acceder al nombre del usuario y a la imagen.</p>
                    <p>Esta funcionalidad es necesaria para acceder a algunas API del espacio de nombres Windows.System.User.</p>
                    <p>La funcionalidad **userAccountInformation** debe incluir el espacio de nombres **uap** cuando la declares en el manifiesto del paquete de la aplicación, como se muestra a continuación.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="userAccountInformation"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Llamada VOIP**</td>
                <td>
                    <p>La funcionalidad **voipCall** permite que las aplicaciones accedan a las API de llamadas de VOIP en el espacio de nombres [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266).</p>
                    <p>La funcionalidad **voipCall** debe incluir el espacio de nombres **uap** cuando la declares en el manifiesto del paquete de la aplicación, como se muestra a continuación.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="voipCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Objetos 3D**</td>
                <td>
                    <p>La funcionalidad **objects3D** permite a las aplicaciones acceder mediante programación a los archivos de objetos 3D. Esta funcionalidad se usa normalmente en aplicaciones 3D y juegos que necesitan tener acceso a toda la biblioteca de objetos 3D.</p>
                    <p>Esta funcionalidad es necesario para acceder a la carpeta que contiene los objetos 3D mediante las API en el espacio de nombres [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346).</p>
                    <p>La funcionalidad **objects3D** debe incluir el espacio de nombres **uap** cuando la declares en el manifiesto del paquete de la aplicación, como se muestra a continuación.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="objects3d"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Leer mensajes bloqueados***</td>
                <td>
                    <p>La funcionalidad **blockedChatMessages** permite que las aplicaciones lean mensajes SMS y MMS bloqueados por la aplicación Filtro de correo no deseado.</p>
                    <p>Esta funcionalidad es necesaria para acceder a los mensajes bloqueados mediante las API del espacio de nombres [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p>
                    <p>La funcionalidad **blockedChatMessages** debe incluir el espacio de nombres **uap** cuando la declares en el manifiesto del paquete de la aplicación, como se muestra a continuación.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="blockedChatMessages"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Acceso a mensajes de chat**</td>
                <td>
                    <p>La funcionalidad **chat** permite que las aplicaciones lean y eliminen mensajes de texto. Esta funcionalidad también permite que las aplicaciones almacenen los mensajes de chat en el almacén de datos del sistema.</p>
                    <p>Esta funcionalidad es necesaria para usar algunas API del espacio de nombres [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p>
                    <p>La funcionalidad **chat** debe incluir el espacio de nombres **uap** cuando la declares en el manifiesto del paquete de la aplicación, como se muestra a continuación.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="chat"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Hardware de bus de nivel bajo IoT**</td>
                <td>
                    <p>La funcionalidad **lowLevelDevices** permite a las aplicaciones que se ejecutan en dispositivos de IoT acceder al hardware de bus de nivel bajo, como GPIO, I2C, SPI, ADC y PWM.</p>
                    <p>Esta funcionalidad es necesaria para acceder a algunas API del espacio de nombres [**Windows.Devices.Spi**](https://msdn.microsoft.com/library/windows/apps/Dn708178).</p>
                    <p>La funcionalidad **lowLevelDevices** debe incluir el espacio de nombres **iot** cuando la declares en el manifiesto del paquete de la aplicación, como se muestra a continuación.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="lowLevelDevices"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Administración del sistema IoT**</td>
                <td>
                    <p>La funcionalidad **systemManagement** permite que las aplicaciones tengan privilegios básicos de administración del sistema, como apagar o reiniciar, configuración regional y zona horaria.</p>
                    <p>Esta funcionalidad es necesaria para acceder a algunas de las API del espacio de nombres [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/BR241814).</p>
                    <p>La funcionalidad **systemManagement** debe incluir el espacio de nombres **iot** cuando la declares en el manifiesto del paquete de la aplicación, como se muestra a continuación.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="systemManagement"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
        </tbody>
</table>

 
## Funcionalidades del dispositivo

Las funcionalidades de dispositivo permiten a tu aplicación acceder a los periféricos y dispositivos internos. Las funcionalidades de dispositivo se especifican mediante el elemento **DeviceCapability** en el manifiesto del paquete de la aplicación. Este elemento puede necesitar elementos secundarios adicionales y algunas funcionalidades de dispositivos deben agregarse al manifiesto del paquete manualmente. Para más información, consulta [Cómo especificar funcionalidades de dispositivos en un manifiesto del paquete](https://msdn.microsoft.com/library/windows/apps/Dn263092) y la [**referencia de esquema DeviceCapability**](https://msdn.microsoft.com/library/windows/apps/BR211430).

| Escenario de funcionalidad | Uso de la funcionalidad |
|---------------------|------------------|
| **Ubicación**\* | La funcionalidad **location** proporciona acceso a las funciones de ubicación, la cual se obtiene desde hardware dedicado, como un sensor GPS en el equipo, o se deriva de la información de red disponible. Las aplicaciones deben administrar el caso en el que el usuario haya deshabilitado los servicios de ubicación desde el acceso a **Configuración**. |
| **Micrófono** | La funcionalidad **microphone** proporciona acceso a la alimentación de audio del micrófono, que permite a la aplicación grabar desde micrófonos conectados. Las aplicaciones deben administrar el caso en el que el usuario haya deshabilitado el micrófono desde el acceso a **Configuración**. |
| **Proximidad** | La funcionalidad **proximity** permite la comunicación entre varios dispositivos que se encuentran cerca unos de otros. Esta funcionalidad se utiliza normalmente en juegos esporádicos de varios jugadores y en aplicaciones que intercambian información. Los dispositivos intentan usar la tecnología de comunicación que proporcione la mejor conexión posible, lo que incluye Bluetooth, Wi-Fi e Internet. Esta funcionalidad se usa solo para iniciar la comunicación entre los dispositivos. |
| **Cámara web** | La funcionalidad **webcam** proporciona acceso a la fuente de vídeo de una cámara integrada o a una cámara web externa, que permite a la aplicación capturar fotos y vídeos. En Windows, las aplicaciones deben controlar el caso en el que el usuario haya deshabilitado la cámara desde el acceso a **Configuración**.<br/>La funcionalidad **webcam** solo concede acceso a la secuencia de vídeo. Para conceder acceso también a la secuencia de audio, debe agregarse la funcionalidad **microphone**. |
| **USB** | La funcionalidad del dispositivo **usb** permite acceder a las API que se indican en [How to add USB device capabilities to the app manifest](http://go.microsoft.com/fwlink/p/?LinkId=302259) (Cómo agregar capacidades de dispositivo USB al manifiesto de la aplicación). |
| **Dispositivo de interfaz de usuario (HID)** | La funcionalidad del dispositivo **humaninterfacedevice** permite acceder a las API que se indican en [Cómo especificar funcionalidades de dispositivo para HID](https://msdn.microsoft.com/library/windows/apps/Dn263091). |
| **Punto de servicio (POS)** | La funcionalidad del dispositivo **pointOfService** permite acceder a las API del espacio de nombres [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071). Este espacio de nombres permite a tu aplicación acceder a lectores de códigos de barras POS y lectores de bandas magnéticas. El espacio de nombres ofrece una interfaz independiente del proveedor para acceder a dispositivos POS de diferentes fabricantes desde una aplicación de la Tienda Windows. |
| **Bluetooth** | La funcionalidad de dispositivos **Bluetooth** permite a las aplicaciones comunicarse con dispositivos Bluetooth ya emparejados sobre el protocolo de atributo genérico (GATT) o de velocidad básica clásico (RFCOMM).<br/>Esta funcionalidad es necesaria para usar algunas API del espacio de nombres [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413). |
| **Redes Wi-Fi** | La funcionalidad del dispositivo **wiFiControl** permite que las aplicaciones analicen y se conecten a redes Wi-Fi.<br/>Esta funcionalidad es necesaria para usar algunas API del espacio de nombres [**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/Dn975224). |
| **Estado de radio** | La funcionalidad del dispositivo **radios** permite que las aplicaciones alternen entre las señales de radio Wi-Fi y Bluetooth.<br/>Esta funcionalidad es necesaria para usar las API en el espacio de nombres [**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/Dn996447).  |
| **Disco óptico** | La funcionalidad del dispositivo **optical** permite a las aplicaciones acceder a las funciones de las unidades de disco ópticas como CD, DVD y Blu-Ray.<br/>Esta funcionalidad es necesaria para usar algunas API del espacio de nombres [**Windows.Devices.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn263667). |
| **Actividad de movimiento** | La funcionalidad del dispositivo **activity** permite que las aplicaciones detecten el movimiento actual del dispositivo.<br/>Esta funcionalidad es necesaria para usar algunas API del espacio de nombres [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408). |

## Funcionalidades especiales y restringidas

**Importante**  
Las funcionalidades especiales y restringidas están indicadas para escenarios muy específicos. El uso de estas funcionalidades está muy restringido y está sujeto a revisión y a directivas de incorporación adicionales de la Tienda.

Hay casos en los que estas funcionalidades son necesarias y apropiadas, como en aplicaciones de banca con autenticación de dos factores, en las que los usuarios proporcionan una tarjeta inteligente con un certificado digital que confirme su identidad. Es posible que otras aplicaciones se hayan diseñado principalmente para empresas y precisen acceder a recursos corporativos a los que no es posible acceder sin las credenciales de dominio del usuario.

Las aplicaciones que aplican las funcionalidades de uso especial requieren una cuenta de empresa para enviarlas a la Tienda. En cambio, las funcionalidades restringidas no requieren una cuenta de empresa especial para la tienda, y no están disponibles para que las usen los desarrolladores. Las funcionalidades restringidas solo están disponibles para las aplicaciones que están desarrolladas por Microsoft y sus socios. Para más información sobre las cuentas de empresa, consulta [Tipos de cuentas, ubicaciones y precios](https://msdn.microsoft.com/library/windows/apps/JJ863494).

Todas las funcionalidades restringidas deben incluir el espacio de nombres **rescap** cuando las declaras en el manifiesto del paquete de la aplicación de forma diferente a las otras funcionalidades del espacio de nombres. En el siguiente ejemplo, se muestra la manera de declarar la funcionalidad **appCaptureSettings**.

```xml
<Capabilities>
    <rescap:Capability Name="appCaptureSettings"/>
</Capabilities>
```

También se debe agregar la declaración de espacio de nombres **xmlns:rescap** en la parte superior del archivo Package.appxmanifest, como se muestra a continuación.

```xml
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
    xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
    xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp wincap rescap">
```

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Escenario de funcionalidad</th>
<th align="left">Uso de la funcionalidad</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**Empresa**</td>
<td align="left"><p>Las credenciales de dominio de Windows permiten al usuario usar sus credenciales para iniciar sesión en recursos remotos, y funcionan como si el usuario hubiera proporcionado su nombre de usuario y contraseña. La funcionalidad especial **enterpriseAuthentication** se usa normalmente en aplicaciones de línea de negocio que se conectan a los servidores de una empresa.</p>
<p>No necesitas esta funcionalidad para la comunicación genérica a través de Internet.</p>

<p>La funcionalidad especial **enterpriseAuthentication** se usa para admitir las aplicaciones comunes de línea de negocio. No la declares en aplicaciones que no necesiten acceder a recursos de empresa. El [**selector de archivos**](https://msdn.microsoft.com/library/windows/apps/BR207847) proporciona un sólido mecanismo de interfaz de usuario que permite a los usuarios abrir archivos en un recurso compartido de red para usarlos con una aplicación. Declara la funcionalidad especial **enterpriseAuthentication** solo cuando los escenarios de la aplicación requieran acceso mediante programación y no puedas llevarlos a cabo con el **selector de archivos**.</p>
<p>La funcionalidad **enterpriseAuthentication** debe incluir el espacio de nombres **uap** cuando la declares en el manifiesto del paquete de la aplicación, como se muestra a continuación.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="enterpriseAuthentication"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div>
<p>La funcionalidad **enterpriseDataPolicy** permite a las aplicaciones definir y usar directivas específicas de la empresa para el dispositivo. Esta funcionalidad es necesaria para usar a todos los miembros de las siguientes clases.</p>
<ul>
<li>[**FileProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn705151)</li>
<li>[**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn706017)</li>
<li>[**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/Dn705170)</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">**Certificados de usuario compartidos**</td>
<td align="left"><p>La funcionalidad especial **sharedUserCertificates** permite a una aplicación agregar certificados de software y hardware al almacén de usuario compartido, tales como certificados almacenados en una tarjeta inteligente, así como acceder a estos. Esta funcionalidad suele usarse para aplicaciones empresariales o financieras que requieren una tarjeta inteligente para la autenticación.</p>
<p>La funcionalidad **sharedUserCertificates** debe incluir el espacio de nombres **uap** cuando la declares en el manifiesto del paquete de la aplicación, como se muestra a continuación.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="sharedUserCertificates"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="odd">
<td align="left">**Documentos***</td>
<td align="left"><p>La funcionalidad especial **documentsLibrary** proporciona acceso con programación a los documentos del usuario, filtrados según las asociaciones de tipo de archivo declaradas en el manifiesto del paquete, para admitir acceso sin conexión a OneDrive. Por ejemplo, si una aplicación de lector de DOC declaró una asociación de tipo de archivo .doc, puede abrir archivos .doc en Documentos, pero no puede abrir otros tipos de archivo.</p>
<p>Las aplicaciones que declaran la funcionalidad especial **documentsLibrary** no pueden acceder a Documentos en los equipos del grupo en el hogar. El [file picker](https://msdn.microsoft.com/library/windows/apps/Hh465174) proporciona un sólido mecanismo de interfaz de usuario que permite a los usuarios abrir archivos para usarlos con una aplicación. Declara la funcionalidad especial **documentsLibrary** solo cuando no puedas usar el selector de archivos.</p>
<p>Para usar la funcionalidad especial **documentsLibrary**, una aplicación debe hacer lo siguiente:</p>
<ul>
<li>Facilitar el acceso sin conexión entre plataformas a contenido de OneDrive específico con direcciones URL o id. de recurso de OneDrive válidos.</li>
<li>Guardar los archivos abiertos en el OneDrive del usuario automáticamente en el modo sin conexión.</li>
</ul>
<p>Las aplicaciones que usan la funcionalidad especial **documentsLibrary** para estos dos propósitos también pueden usar opcionalmente la funcionalidad para abrir contenido insertado en otro documento. Solo se aceptan los usos de la funcionalidad especial **documentsLibrary** mencionados anteriormente.</p>
<ul>
<li><p>La aplicación no puede acceder a la biblioteca Documentos en el almacenamiento interno del teléfono. Sin embargo, si otra aplicación crea una carpeta Documentos en la tarjeta SD opcional, tu aplicación podrá ver esa carpeta.</p></li>
</ul>
<p>La funcionalidad **documentsLibrary** debe incluir el espacio de nombres **uap** cuando la declares en el manifiesto del paquete de la aplicación, como se muestra a continuación.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="documentsLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="even">
<td align="left">**Configuración de Game DVR**</td>
<td align="left"><p>La funcionalidad restringida **appCaptureSettings** permite a las aplicaciones controlar la configuración de usuario del Game DVR.</p>
<p>Esta funcionalidad es necesaria para usar algunas API del espacio de nombres [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/BR226738).</p></td>
</tr>
<tr class="odd">
<td align="left">**Red de telefonía móvil**</td>
<td align="left"><p>La funcionalidad restringida **cellularDeviceControl** permite a las aplicaciones controlar el dispositivo de telefonía móvil.</p>
<p>La funcionalidad **cellularDeviceIdentity** permite que las aplicaciones tengan acceso a los datos de identificación del dispositivo móvil.</p>
<p>La funcionalidad **cellularMessaging** permite a las aplicaciones hacer uso de SMS y el RCS.</p>
<p>Estas funcionalidades son necesarias para usar algunas API en los espacios de nombres [**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/BR206567).</p>
<p>A partir de Windows 10, las aplicaciones llaman a [**AppIDList**](https://msdn.microsoft.com/library/windows/apps/Dn393996)).</p></td>
</tr>
<tr class="even">
<td align="left">**Desbloqueo del dispositivo**</td>
<td align="left"><p>La funcionalidad restringida **deviceUnlock** permite a las aplicaciones desbloquear un dispositivo para escenarios de instalación de prueba de empresa y para desarrolladores.</p></td>
</tr>
<tr class="odd">
<td align="left">**Iconos de SIM dual**</td>
<td align="left"><p>La funcionalidad restringida **dualSimTiles** permite a las aplicaciones crear una entrada de lista de aplicaciones adicional en dispositivos que dispongan de varias tarjetas SIM.</p>
<p>Esta funcionalidad es necesaria para usar algunas API del espacio de nombres [**Windows.UI.StartScreen**](https://msdn.microsoft.com/library/windows/apps/BR242235).</p></td>
</tr>
<tr class="even">
<td align="left">**Almacenamiento compartido de empresa**</td>
<td align="left"><p>La funcionalidad restringida **enterpriseDeviceLockdown** permite a las aplicaciones usar la API de bloqueo del dispositivo y acceder a las carpetas de almacenamiento compartido de empresa.</p></td>
</tr>
<tr class="odd">
<td align="left">**Inserción de entrada del sistema**</td>
<td align="left"><p>La funcionalidad restringida **inputInjection** permite que las aplicaciones inserten diversas formas de entrada (como HID, entrada táctil, lápiz, teclado o mouse) en el sistema mediante programación. Esta funcionalidad se suele usar con las aplicaciones de colaboración que pueden tomar el control del sistema.</p>
<div class="alert">
**Nota**  En un equipo, la inserción de entrada de una aplicación con esta funcionalidad solo la recibirán los procesos que residan en el mismo contenedor de aplicación.
</div>
</td>
</tr>
<tr class="even">
<td align="left">**Observar entrada***</td>
<td align="left"><p>La funcionalidad restringida **inputObservation** permite a las aplicaciones observar diversas formas de entrada sin procesar (como HID, entrada táctil, lápiz, teclado o mouse) que reciba el sistema, independientemente de su destino final.</p></td>
</tr>
<tr class="odd">
<td align="left">**Suprimir entrada**</td>
<td align="left"><p>La funcionalidad restringida **inputSuppression** permite a las aplicaciones suprimir diversas formas de entrada sin procesar (como HID, entrada táctil, lápiz, teclado o mouse) para evitar que el sistema las reciba.</p></td>
</tr>
<tr class="even">
<td align="left">**Aplicación VPN**</td>
<td align="left"><p>La funcionalidad restringida **networkingVpnProvider** permite a las aplicaciones tener acceso completo a las características VPN, por ejemplo, la capacidad para administrar las conexiones y proporcionar funcionalidad de complementos de VPN.</p>
<p>Esta funcionalidad es necesaria para usar algunas API del espacio de nombres [**Windows.Networking.Vpn**](https://msdn.microsoft.com/library/windows/apps/Dn434040).</p></td>
</tr>
<tr class="odd">
<td align="left">**Administración de otras aplicaciones**</td>
<td align="left"><p>La funcionalidad restringida **packageManagement** permite a las aplicaciones administrar otras aplicaciones directamente.</p>
<p>La funcionalidad del dispositivo **packageQuery** permite que las aplicaciones recopilen información sobre otras aplicaciones.</p>
<p>Estas funcionalidades son necesarias para acceder a algunos métodos y propiedades de la clase [**PackageManager**](https://msdn.microsoft.com/library/windows/apps/BR240960).</p></td>
</tr>
<tr class="even">
<td align="left">**Proyección de pantalla**</td>
<td align="left"><p>La funcionalidad restringida **screenDuplication** permite a las aplicaciones proyectar la pantalla en otro dispositivo.</p>
<p>Esta funcionalidad es necesaria para usar API en el espacio de nombres DirectX.</p></td>
</tr>
<tr class="odd">
<td align="left">**Nombre principal del usuario**</td>
<td align="left"><p>La funcionalidad restringida **userPrincipalName** permite a las aplicaciones modificar la memoria caché de miniaturas de fotos y acceder a ella.</p>
<p>Esta funcionalidad es necesaria para llamar a la función [**GetUserNameEx**](https://msdn.microsoft.com/library/windows/desktop/ms724435).</p></td>
</tr>
<tr class="even">
<td align="left">**Cartera**</td>
<td align="left"><p>La funcionalidad restringida **walletSystem** permite a las aplicaciones tener acceso completo a las tarjetas de bolsillo almacenadas.</p>
<p>Esta funcionalidad es necesaria para usar algunas API del espacio de nombres [**Windows.ApplicationModel.Wallet.System**](https://msdn.microsoft.com/library/windows/apps/Mt171610).</p></td>
</tr>
<tr class="odd">
<td align="left">**Historial de ubicaciones**</td>
<td align="left"><p>La funcionalidad restringida **locationHistory** permite a las aplicaciones acceder al historial de ubicación del dispositivo.</p>
<p>Esta funcionalidad es necesaria para usar las API en el espacio de nombres [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/BR225603).</p></td>
</tr>
<tr class="even">
<td align="left">**Confirmación de cierre de la aplicación**</td>
<td align="left"><p>La funcionalidad restringida **confirmAppClose** permite a las aplicaciones cerrar sus propias ventanas y a sí mismas, y retrasar el cierre de la aplicación.</p>
<p>Esta funcionalidad es necesaria para usar algunas API del espacio de nombres [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/BR242295).</p></td>
</tr>
<tr class="odd">
<td align="left">**Historial de llamadas***</td>
<td align="left"><p>La funcionalidad restringida **phoneCallHistory** permite a las aplicaciones leer el historial de llamadas y eliminar entradas en el historial de llamadas.</p>
<p>Esta funcionalidad es necesaria para usar las API del espacio de nombres [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p></td>
</tr>
<tr class="even">
<td align="left">**Acceso a citas de nivel de sistema**</td>
<td align="left"><p>La funcionalidad restringida **appointmentsSystem** permite a las aplicaciones leer y modificar todas las citas del calendario del usuario.</p>
<p>Esta funcionalidad es necesaria para usar las API del espacio de nombres [**Windows.ApplicationModel.Appointment**](https://msdn.microsoft.com/library/windows/apps/Dn263359).</p></td>
</tr>
<tr class="odd">
<td align="left">**Acceso a mensajes de chat de nivel de sistema***</td>
<td align="left"><p>La funcionalidad restringida **chatSystem** permite a las aplicaciones leer y escribir todos los mensajes SMS y MMS.</p>
<p>Esta funcionalidad es necesaria para usar las API del espacio de nombres [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p></td>
</tr>
<tr class="even">
<td align="left">**Acceso a contactos de nivel del sistema**</td>
<td align="left"><p>La funcionalidad restringida **contactsSystem** permite a las aplicaciones leer información de contactos que se haya establecido como confidencial o restringida y modificar la información de contactos existente.</p>
<p>Esta funcionalidad es necesaria para usar las API del espacio de nombres [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p></td>
</tr>
<tr class="odd">
<td align="left">**Acceso a correo electrónico***</td>
<td align="left"><p>La funcionalidad restringida **email** permite a las aplicaciones leer, clasificar y enviar correos electrónicos del usuario.</p>
<p>Esta funcionalidad es necesaria para usar las API del espacio de nombres [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285).</p></td>
</tr>
<tr class="even">
<td align="left">**Acceso a correo electrónico de nivel del sistema**</td>
<td align="left"><p>La funcionalidad restringida **emailSystem** permite a las aplicaciones leer, clasificar y enviar correos electrónicos del usuario confidenciales o restringidos.</p>
<p>Esta funcionalidad es necesaria para usar las API del espacio de nombres [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285).</p></td>
</tr>
<tr class="odd">
<td align="left">**Acceso al historial de llamadas de nivel del sistema**</td>
<td align="left"><p>La funcionalidad restringida **phoneCallHistorySystem** permite a las aplicaciones modificar completamente el historial de llamadas al cambiar las entradas existentes y escribir otras nuevas.</p>
<p>Esta funcionalidad es necesaria para usar las API del espacio de nombres [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266).</p></td>
</tr>
<tr class="even">
<td align="left">**Enviar mensajes de texto***</td>
<td align="left"><p>La funcionalidad restringida **smsSend** permite a las aplicaciones enviar mensajes SMS y MMS.</p>
<p>Esta funcionalidad es necesaria para usar las API del espacio de nombres [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p></td>
</tr>
<tr class="odd">
<td align="left">**Acceso a todos los datos de usuario de nivel de sistema**</td>
<td align="left"><p>La funcionalidad restringida **userDataSystem** permite a las aplicaciones acceder al almacén de datos del sistema del usuario.</p></td>
</tr>
<tr class="even">
<td align="left">**Funciones de vista previa de la Tienda**</td>
<td align="left"><p>La funcionalidad restringida **previewStore** permite a las aplicaciones recuperar y comprar SKU de productos desde la aplicación.</p>
<p>Esta funcionalidad es necesaria para usar algunas API del espacio de nombres [**Windows.ApplicationModel.Store.Preview**](https://msdn.microsoft.com/library/windows/apps/Mt185546).</p></td>
</tr>
<tr class="odd">
<td align="left">**Configuración de primer inicio de sesión**</td>
<td align="left"><p>La funcionalidad restringida **firstSignInSettings** permite a las aplicaciones acceder a la configuración de usuario que se establecieron cuando el usuario inició sesión por primera en su dispositivo.</p></td>
</tr>
<tr class="even">
<td align="left">**Experiencia del Equipo de Windows**</td>
<td align="left"><p>La funcionalidad restringida **teamEditionExperience** permite que las aplicaciones obtengan acceso a API internas que controlan muchos aspectos experimentales de una sesión del Equipo de Windows. Es probable que se ejecute una sesión del equipo de Windows en un dispositivo de equipo como, por ejemplo, Microsoft Surface Hub.</p></td>
</tr>
<tr class="odd">
<td align="left">**Desbloqueo remoto**</td>
<td align="left"><p>La funcionalidad restringida **remotePassportAuthentication** permite que las aplicaciones accedan a las credenciales que pueden usarse para desbloquear un equipo remoto.</p></td>
</tr>
<tr class="even">
<td align="left">**Composición de vista previa**</td>
<td align="left"><p>La funcionalidad restringida **previewUiComposition** permite a las aplicaciones obtener una vista previa del espacio de nombres [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878) para su interfaz de usuario de modo que puedan proporcionar comentarios sobre la API antes de que se complete. Ponte en contacto con wincomposition@microsoft.com para más información.</p></td>
</tr>
<tr class="odd">
<td align="left">**Bloqueo de evaluaciones seguras**</td>
<td align="left"><p>La funcionalidad restringida **secureAssessment** permite que las aplicaciones bloqueen Windows en un modo de aplicación única para que las evaluaciones sean seguras.</p></td>
</tr>
<tr class="even">
<td align="left">**Aprovisionamiento del administrador de conexiones**</td>
<td align="left"><p>La funcionalidad restringida **networkConnectionManagerProvisioning** permite que las aplicaciones definan las directivas que conectan el dispositivo a las interfaces WWAN y WLAN. Los operadores de telefonía móvil crean aplicaciones que usan esta funcionalidad para controlar los dispositivos que se conectan a su red móvil.</p></td>
</tr>
<tr class="odd">
<td align="left">**Aprovisionamiento de plan de datos**</td>
<td align="left"><p>La funcionalidad restringida **networkDataPlanProvisioning** permite que las aplicaciones recopilen información sobre los planes de datos en el dispositivo y lean el uso de la red. Los operadores de telefonía móvil crean las aplicaciones que usan esta funcionalidad para integrar el uso real de datos de sus clientes en la configuración de uso de datos del sistema operativo.</p></td>
</tr>
<tr class="even">
<td align="left">**Licencias de software**</td>
<td align="left"><p>La funcionalidad restringida **slapiQueryLicenseValue** permite que las aplicaciones consulten directivas de licencias de software.</p></td>
</tr>
<tr class="odd">
<td align="left">**Ejecución ampliada**</td>
<td align="left"><p>La funcionalidad restringida **extendedExecutionBackgroundAudio** permite que las aplicaciones reproduzcan audio cuando la aplicación no está en primer plano.</p>
<p>La funcionalidad restringida **extendedExecutionCritical** permite que las aplicaciones comiencen una sesión de ejecución crítica ampliada.</p>
<p>La funcionalidad restringida **extendedExecutionUnconstrained** permite que las aplicaciones comiencen una sesión de ejecución ampliada sin restricciones.</p></td>
</tr>
<tr class="even">
<td align="left">**Administración de dispositivos móviles**</td>
<td align="left"><p>La funcionalidad restringida **deviceManagementDmAccount** permite que las aplicaciones aprovisionen y configuren cuentas de Mobile Operator Open Mobile Alliance - Device Management (MO OMA-DM).</p>
<p>La funcionalidad restringida **deviceManagementFoundation** permite que las aplicaciones tengan acceso básico a la infraestructura de proveedor de servicios de configuración (CSP) de Mobile Device Management (MDM) en el dispositivo. Ten en cuenta que se necesitan otras funcionalidades para obtener acceso a los CSP específicos.</p>
<p>La funcionalidad restringida **deviceManagementWapSecurityPolicies** permite que las aplicaciones configuren servicios basados en el protocolo de aplicación inalámbrica (WAP), como MMS, Service Indication/Service Loading (SI/SL) y Open Mobile Alliance - Client Provisioning (OMA-CP).</p>
<p>La funcionalidad restringida **deviceManagementEmailAccount** permite que las aplicaciones creadas por los operadores de telefonía móvil agreguen y administren una cuenta de correo electrónico en dispositivos que aprovisionen a los usuarios.</p></td>
</tr>
<tr class="odd">
<td align="left">**Control de directivas de paquete**</td>
<td align="left"><p>La funcionalidad restringida **packagePolicySystem** permite que las aplicaciones tengan el control de directivas del sistema relacionadas con aplicaciones instaladas en el dispositivo.</p></td>
</tr>
<tr class="even">
<td align="left">**Lista de juegos**</td>
<td align="left"><p>La funcionalidad restringida **gameList** permite que las aplicaciones obtengan una lista de juegos conocidos instalados en el sistema.</p></td>
</tr>
<tr class="odd">
<td align="left">**Accesorio de Xbox**</td>
<td align="left"><p>La funcionalidad restringida **xboxAccessoryManagement** permite a las aplicaciones administrar directamente dispositivos de Xbox que cumplan con las especificaciones de hardware de Xbox.</p></td>
</tr>
<tr class="even">
<td align="left">**Reconocimiento de voz para accesorios**</td>
<td align="left"><p>La funcionalidad restringida **cortanaSpeechAccessory** permite que las aplicaciones invoquen y pasen comandos a Cortana.</p></td>
</tr>
<tr class="odd">
<td align="left">**Administración de accesorios**</td>
<td align="left"><p>La funcionalidad restringida **accessoryManager** permite que las aplicaciones se registren como una aplicación para accesorio y opten por recibir notificaciones de aplicaciones específicas, para que puedan reenviarse a los accesorios y mostrarse al usuario.</p></td>
</tr>
<tr class="even">
<td align="left">**Acceso a controladores**</td>
<td align="left"><p>La funcionalidad restringida **interopServices** permite que las aplicaciones interactúen directamente con los controladores.</p></td>
</tr>
<tr class="odd">
<td align="left">**Observación en primer plano**</td>
<td align="left"><p>La funcionalidad restringida **inputForegroundObservation** permite que las aplicaciones en primer plano intercepten la entrada de teclado y omite el procesamiento de todas las entradas de teclado que no son de la aplicación. Esta funcionalidad no puede interceptar las combinaciones SAS. Esta funcionalidad es necesaria para acceder a todos los miembros de la clase [**KeyboardDeliveryInterceptor**](https://msdn.microsoft.com/library/windows/apps/Mt608395).</p></td>
</tr>
<tr class="even">
<td align="left">**Aplicaciones de socios OEM y OM**</td>
<td align="left"><p>La funcionalidad restringida **oemDeployment** permite a las aplicaciones que crean los socios de Microsoft instalar aplicaciones nuevas y consultar las aplicaciones instaladas actualmente en el dispositivo.</p>
<p>La funcionalidad restringida **oemPublicDirectory** permite a las aplicaciones que crean los socios de Microsoft acceder a la carpeta de aplicaciones compartida.</p></td>
</tr>
<tr class="odd">
<td align="left">**Licencias de aplicaciones**</td>
<td align="left"><p>La funcionalidad restringida **appLicensing** permite ejecutar aplicaciones sin necesidad de una licencia. No puedes enviar tu aplicación a la Tienda si declaras esta funcionalidad en el manifiesto. Las solicitudes de acceso a esta funcionalidad para el envío a la Tienda siempre se denegarán.</p></td>
</tr>
<tr class="even">
<td align="left">**Sistema de ubicación**</td>
<td align="left"><p>La funcionalidad restringida **locationSystem** permite que las aplicaciones realicen determinadas configuraciones de ubicación privilegiadas, como establecer la ubicación predeterminada del dispositivo. No puedes enviar tu aplicación a la Tienda si declaras esta funcionalidad en el manifiesto. Las solicitudes de acceso a esta funcionalidad para el envío a la Tienda siempre se denegarán.</p></td>
</tr>
</tbody>
</table>

**Nota**  
Este artículo está orientado a desarrolladores de Windows 10 que programan aplicaciones para UWP. Si estás desarrollando para Windows 8.x o Windows Phone 8.x, consulta la [documentación archivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Temas relacionados

* [Diseñador de manifiestos](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx)
* [Directrices para aplicaciones compatibles con la privacidad](https://msdn.microsoft.com/library/windows/apps/Hh768223)
* [Cómo especificar funcionalidades en un manifiesto del paquete](https://msdn.microsoft.com/library/windows/apps/BR211477)
* [Cómo especificar funcionalidades de dispositivo en un manifiesto del paquete](https://msdn.microsoft.com/library/windows/apps/Dn263092)
 


<!--HONumber=Mar16_HO5-->


