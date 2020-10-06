---
title: Establecer roles o permisos personalizados para usuarios de cuenta
description: Obtenga información acerca de cómo establecer roles estándar o permisos personalizados al agregar usuarios a la cuenta del centro de Partners.
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, roles de usuario, permisos de usuario, roles personalizados, acceso de usuario, permisos de personalización, roles estándar
ms.localizationpriority: medium
ms.openlocfilehash: f8454587e31751e3653d983dbb1d45e21a2808d9
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2020
ms.locfileid: "91750181"
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>Establecer roles o permisos personalizados para usuarios de cuenta

Al [Agregar usuarios a la cuenta del centro de Partners](add-users-groups-and-azure-ad-applications.md), deberá especificar qué acceso tienen dentro de la cuenta. Puede hacerlo mediante la asignación de [roles estándar](#roles) que se aplican a toda la cuenta, o bien puede [personalizar sus permisos](#custom) para proporcionar el nivel de acceso adecuado. Algunos de los permisos personalizados se aplican a toda la cuenta y algunos se pueden limitar a uno o varios productos específicos (o concedidos a todos los productos, si lo prefiere).

> [!NOTE] 
> Se pueden aplicar los mismos roles y permisos independientemente de si está agregando un usuario, un grupo o una aplicación Azure AD.

A la hora de determinar qué rol o permisos aplicar, tenga en cuenta lo siguiente: 
-   Los usuarios (incluidos los grupos y las aplicaciones Azure AD) podrán acceder a toda la cuenta del centro de Partners con los permisos asociados a sus roles asignados, a menos que se [personalicen los permisos](#custom) y se asignen [permisos de nivel de producto](#product-level-permissions) para que solo funcionen con aplicaciones específicas y/o complementos.
-   Para permitir que un usuario, grupo o aplicación de Azure AD tengan acceso a las funciones de más de un rol, selecciona varios roles o utiliza permisos personalizados para conceder el acceso que quieras.
-   Un usuario con un determinado rol (o un conjunto de permisos personalizados) también puede formar parte de un grupo que tenga un rol diferente (o un conjunto de permisos). En ese caso, el usuario tendrá acceso a toda la funcionalidad asociada con el grupo y con la cuenta individual.

> [!TIP]
> Este tema es específico del programa para desarrolladores de aplicaciones de Windows en el [centro de Partners](https://partner.microsoft.com/dashboard). Para obtener información acerca de los roles de usuario en el programa para desarrolladores de hardware, consulte [Administración de funciones de usuario](/windows-hardware/drivers/dashboard/managing-user-roles). Para obtener información acerca de los roles de usuario en el programa de aplicación de escritorio de Windows, consulte [programa de aplicación de escritorio de Windows](/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users).


<span id="roles" />

## <a name="assign-roles-to-account-users"></a>Asignación de roles a usuarios de cuenta

De forma predeterminada, se muestra un conjunto de roles estándar al agregar un usuario, un grupo o una aplicación Azure AD a la cuenta del centro de Partners. Cada rol tiene un conjunto específico de permisos para realizar determinadas funciones en la cuenta. 

A menos que decida definir [Permisos personalizados](#custom) seleccionando **personalizar permisos**, se debe asignar al menos uno de los siguientes roles estándar a cada usuario, grupo o aplicación Azure ad que agregue a una cuenta. 

> [!NOTE]
> El **propietario** de la cuenta es la persona que la creó por primera vez con un cuenta de Microsoft (y no los usuarios agregados a través de Azure ad). Dicho propietario de la cuenta es la única persona con acceso completo a la cuenta, lo que incluye la capacidad de eliminar aplicaciones, crear y editar todos los usuarios de la cuenta y modificar todas las opciones financieras y de la cuenta. 


| Role                 | Descripción              |
|----------------------|--------------------------|
| Manager              | Tiene acceso completo a la cuenta, excepto para cambiar la configuración fiscal y de pago. Esto incluye la administración de usuarios en el centro de Partners, pero tenga en cuenta que la capacidad de crear y eliminar usuarios en el inquilino de Azure AD depende del permiso de la cuenta en Azure AD. Es decir, si un usuario tiene asignado el rol de administrador, pero no tiene permisos de administrador global en el Azure AD de la organización, no podrá crear nuevos usuarios ni eliminar usuarios del directorio (aunque pueden cambiar el rol del centro de Partners de un usuario). <p> Tenga en cuenta que si la cuenta del centro de Partners está asociada a más de un inquilino Azure AD, un administrador no podrá ver los detalles completos de un usuario (incluido el nombre, el apellido, el correo electrónico de recuperación de contraseña y si es un administrador global Azure AD) a menos que haya iniciado sesión en el mismo inquilino que ese usuario con una cuenta que tenga permisos de administrador global para ese inquilino Sin embargo, pueden agregar y quitar usuarios de cualquier inquilino que esté asociado a la cuenta del centro de Partners. |
| Desarrollador            | Puede cargar paquetes y enviar aplicaciones y complementos, así como ver el [Informe de uso](usage-report.md) para obtener información detallada de telemetría. Puede tener acceso a [la funcionalidad de experiencias entre dispositivos](https://developer.microsoft.com/windows/project-rome) . No puede ver la configuración de la cuenta ni la información financiera.   |
| Colaborador empresarial | Puede ver informes de [estado](health-report.md) y de [uso](usage-report.md). No puede crear ni enviar productos, cambiar la configuración de cuenta ni ver información financiera.   |
| Colaborador financiero  | Puede ver [informes de pago](payout-summary.md), información financiera e informes de adquisición. No puede modificar las aplicaciones, los complementos ni la configuración de la cuenta.    |
| Vendedor             | Puede [responder a las opiniones de los clientes](respond-to-customer-reviews.md) y ver [informes analíticos](analytics.md) que no sean financieros. No puede modificar las aplicaciones, los complementos ni la configuración de la cuenta.      |

La siguiente tabla muestra algunas de las características específicas disponibles para cada rol (y para el propietario de la cuenta).

|                                                       |    Propietario de cuenta                 |    Manager                       |    Desarrollador                     |    Colaborador empresarial    |    Colaborador financiero    |    Vendedor                      |
|-------------------------------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    **Informe de adquisición (incluidos los datos casi en tiempo real)** |    Puede ver                      |    Puede ver                      |    Sin acceso                     |    Sin acceso               |    Puede ver               |    Sin acceso                     |
|    **Respuesta/informe de comentarios**                          |    Puede ver y enviar comentarios    |    Puede ver y enviar comentarios    |    Puede ver y enviar comentarios    |    Sin acceso               |    Sin acceso              |    Puede ver y enviar comentarios    |
|    **Informe de mantenimiento (incluidos los datos casi en tiempo real)**      |    Puede ver                      |    Puede ver                      |    Puede ver                      |    Puede ver                |    Sin acceso              |    Sin acceso                     |
|    **Informe de uso**                                       |    Puede ver                      |    Puede ver                      |    Puede ver                      |    Puede ver                |    Sin acceso              |    Sin acceso                     |
|    **Cuenta de pago**                                     |    Puede actualizar                    |    Sin acceso                     |    Sin acceso                     |    Sin acceso               |    Puede actualizar             |    Sin acceso                     |
|    **Perfil fiscal**                                        |    Puede actualizar                    |    Sin acceso                     |    Sin acceso                     |    Sin acceso               |    Puede actualizar             |    Sin acceso                     |
|    **Resumen de pagos**                                     |    Puede ver                      |    Sin acceso                     |    Sin acceso                     |    Sin acceso               |    Puede ver               |    Sin acceso                     |

Si ninguno de los roles estándar es adecuado o desea limitar el acceso a aplicaciones y complementos específicos, puede conceder permisos personalizados al usuario seleccionando **personalizar permisos**, como se describe a continuación.


<span id="custom" />

## <a name="assign-custom-permissions-to-account-users"></a>Asignar permisos personalizados a usuarios de cuenta

Para asignar permisos personalizados en lugar de roles estándar, haga clic en **personalizar permisos** en la sección **roles** al agregar o editar la cuenta de usuario. 

Si quieres habilitar un permiso para el usuario, activa o desactiva la casilla con el ajuste que corresponda. 

![Guía de la configuración de acceso](images/permission_key.png)

- **Sin acceso**: el usuario no tendrá el permiso indicado.
- **Solo lectura**: el usuario tendrá acceso para ver las características relacionadas con el área indicada, pero no podrá realizar cambios. 
- **Lectura y escritura**: el usuario tendrá acceso tanto para realizar cambios relacionados con el área como para verla.
- **Mixto**: no puedes seleccionar esta opción directamente, pero el indicador **Mixto** te mostrará si has permitido a una combinación de acceso de dicho permiso. Por ejemplo, si concedes acceso de **Solo lectura** a **Precios y disponibilidad** para **Todos los productos**, pero luego concedes acceso de **Lectura y escritura** a **Precios y disponibilidad** para un producto específico, el indicador **Precios y disponibilidad** de **Todos los productos** se mostrará como mixto. Lo mismo sucede si algunos productos presentan **Sin acceso** para un permiso y otros, acceso de **Lectura y escritura** o de **Solo lectura**.

En el caso de algunos permisos, como los relacionados con la visualización de datos analíticos, solo se puede conceder acceso de **Solo lectura**. Ten en cuenta que en la implementación actual algunos permisos no distinguen en absoluto entre acceso de **Solo lectura** y de **Lectura y escritura**. Revise los detalles de cada permiso para comprender las capacidades específicas que concede el acceso de **solo lectura** o de **lectura y escritura** .

En las tablas siguientes se describen los datos concretos de cada permiso.

## <a name="account-level-permissions"></a>Permisos de nivel de cuenta

Los permisos de esta sección no se pueden limitar a productos específicos. Conceder acceso a uno de estos permisos permite que el usuario tenga ese permiso para toda la cuenta.

<table>
    <colgroup>
    <col width="20%" />
    <col width="40%" />
    <col width="40%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Nombre de permiso</th>
    <th align="left">Solo lectura</th>
    <th align="left">Lectura/escritura</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    <b>Configuración de la cuenta</b>                    </td><td align="left">  Permite ver todas las páginas de la sección <b>Configuración de la cuenta</b>, incluida la <a href="/windows/uwp/publish/manage-account-settings-and-profile">información de contacto</a>.       </td><td align="left">  Permite ver todas las páginas de la sección <b>Configuración de la cuenta</b>. Permite realizar cambios en la <a href="/windows/uwp/publish/manage-account-settings-and-profile">información de contacto</a> y en otras páginas, pero no en la cuenta de pago ni en el perfil fiscal (a menos que este permiso se haya concedido por separado).            </td></tr>
<tr><td align="left">    <b>Usuarios de cuenta</b>                       </td><td align="left">  Puede ver los usuarios que se han agregado a la cuenta en la sección <b>usuarios</b> .          </td><td align="left">  Puede Agregar usuarios a la cuenta y realizar cambios en los usuarios existentes en la sección <b>usuarios</b> .             </td></tr>
<tr><td align="left">    <b>Informe de rendimiento de ad de nivel de cuenta</b> </td><td align="left">  Permite ver el <a href="advertising-performance-report.md">Informe de rendimiento de la publicidad</a> de nivel de cuenta.      </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    <b>Campañas de ad</b>                        </td><td align="left">  Permite ver las <a href="create-an-ad-campaign-for-your-app.md">campañas de anuncios</a> que se han creado en la cuenta.      </td><td align="left">  Permite crear, administrar y ver las <a href="create-an-ad-campaign-for-your-app.md">campañas de anuncios</a> que se han creado en la cuenta.          </td></tr>
<tr><td align="left">    <b>Mediación de anuncios</b>                        </td><td align="left">  Permite ver las configuraciones de mediación de anuncios de todos los productos de la cuenta.    </td><td align="left">  Permite ver y cargar las configuraciones de mediación de anuncios de todos los productos de la cuenta.        </td></tr>
<tr><td align="left">    <b>Informes de mediación de anuncios</b>                </td><td align="left">  Permite ver el <a href="/windows/uwp/publish/advertising-performance-report">informe de mediación de anuncios</a> de todos los productos de la cuenta.    </td><td align="left">  N/D    </td></tr>
<tr><td align="left">    <b>Informes de rendimiento de ad</b>              </td><td align="left">  Permite ver los <a href="advertising-performance-report.md">informes de rendimiento de la publicidad</a> de todos los productos de la cuenta.       </td><td align="left">  N/D         </td></tr>
<tr><td align="left">    <b>Unidades de anuncio</b>                            </td><td align="left">  Permite ver las <a href="in-app-ads.md">unidades de anuncios</a> que se han creado para la cuenta.    </td><td align="left">  Permite crear, administrar y ver las <a href="in-app-ads.md">unidades de anuncios</a> de la cuenta.             </td></tr>
<tr><td align="left">    <b>Anuncios afiliados</b>                       </td><td align="left">  Permite ver la utilización de <a href="/windows/uwp/publish/in-app-ads">anuncios de filiales</a> de todos los productos de la cuenta.    </td><td align="left">  Permite administrar y ver la utilización de los <a href="/windows/uwp/publish/in-app-ads">anuncios de filiales</a> de todos los productos de la cuenta.                </td></tr>
<tr><td align="left">    <b>Informes de rendimiento de afiliados</b>      </td><td align="left">  Permite ver el <a href="/windows/uwp/publish/advertising-performance-report">informe de rendimiento de filiales</a> de todos los productos de la cuenta.   </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    <b>Informes de anuncios de instalación de aplicaciones</b>             </td><td align="left">  Puede ver el <a href="/windows/uwp/publish/ad-campaign-report">Informe de campaña de ad</a>.           </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    <b>Anuncios de la comunidad</b>                       </td><td align="left">  Permite ver de forma gratuita la utilización del <a href="about-community-ads.md">anuncio de la comunidad</a> de todos los productos de la cuenta.          </td><td align="left">  Permite crear, administrar y ver la utilización del <a href="about-community-ads.md">anuncio de la comunidad</a> gratuito de todos los productos de la cuenta.               </td></tr>
<tr><td align="left">    <b>Información de contacto</b>                        </td><td align="left">  Permite ver la <a href="/windows/uwp/publish/manage-account-settings-and-profile">información de contacto</a> en la sección de configuración de la cuenta.        </td><td align="left">  Permite ver y editar la <a href="/windows/uwp/publish/manage-account-settings-and-profile">información de contacto</a> en la sección de configuración de la cuenta.            </td></tr>
<tr><td align="left">    <b>Cumplimiento de COPPA</b>                    </td><td align="left">  Permite ver selecciones del <a href="in-app-ads.md#coppa-compliance">cumplimiento de COPPA</a> (que indica si los productos se destinan a niños menores de 13 años) de todos los productos de la cuenta.                                            </td><td align="left">  Permite ver y editar selecciones del <a href="in-app-ads.md#coppa-compliance">cumplimiento de COPPA</a> (que indica si los productos se destinan a niños menores de 13 años) de todos los productos de la cuenta.         </td></tr>
<tr><td align="left">    <b>Grupos de clientes</b>                     </td><td align="left">  Puede ver <a href="create-customer-groups.md">grupos de clientes</a> (segmentos y grupos de usuarios conocidos).      </td><td align="left">  Puede crear, editar y ver <a href="create-customer-groups.md">grupos de clientes</a> (segmentos y grupos de usuarios conocidos).       </td></tr>
<tr><td align="left">    <b>Administrar grupos de productos</b>&nbsp;*                            </td><td align="left">  Puede ver la nueva página de creación de grupos de productos, pero en realidad no puede crear nuevos grupos de productos.    </td><td align="left">  Puede crear y editar grupos de productos.     </td></tr>
<tr><td align="left">    <b>Nuevas aplicaciones</b>                            </td><td align="left">  Permite ver la nueva página de creación de aplicaciones, pero en realidad no permite crear nuevas aplicaciones en la cuenta.    </td><td align="left">  Permite <a href="create-your-app-by-reserving-a-name.md">crear nuevas aplicaciones</a> en la cuenta mediante la reserva de nuevos nombres de aplicación, y crear envíos y enviar aplicaciones a la Tienda.     </td></tr>
<tr><td align="left">    <b>Nuevas agrupaciones</b>&nbsp;*                       </td><td align="left">  Permite ver la nueva página de creación de conjuntos, pero en realidad no permite crear nuevos conjuntos en la cuenta.     </td><td align="left">  Permite crear nuevos conjuntos de productos.          </td></tr>
<tr><td align="left">    <b>Servicios de asociados</b>&nbsp;*                  </td><td align="left">  Permite ver certificados para la instalación en servicios con el fin de recuperar XTokens.     </td><td align="left">  Permite administrar y ver certificados para la instalación en servicios con el fin de recuperar XTokens.       </td></tr>
<tr><td align="left">    <b>Cuenta de pago</b>                      </td><td align="left">  Permite ver la <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">información de la cuenta de pago</a> en <b>Configuración de la cuenta</b>.     </td><td align="left">  Permite editar y ver la <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">información de la cuenta de pago</a> en <b>Configuración de la cuenta</b>.       </td></tr>
<tr><td align="left">    <b>Resumen de pagos</b>                      </td><td align="left">  Permite ver el <a href="payout-summary.md">resumen de pago</a> para acceder y descargar información de informes de pago.       </td><td align="left">  Permite ver el <a href="payout-summary.md">resumen de pago</a> para acceder y descargar información de informes de pago.   </td></tr>
<tr><td align="left">    <b>Usuarios de confianza</b>&nbsp;*                   </td><td align="left">  Permite ver los usuarios de confianza para recuperar XTokens.    </td><td align="left">  Permite administrar y ver los usuarios de confianza para recuperar XTokens.     </td></tr>
<tr><td align="left">    <b>Espacios aislados</b>&nbsp;*                         </td><td align="left">  Permite acceder a la página de <b>espacios aislados</b>, y ver los espacios aislados de la cuenta y cualquier configuración que se aplique a dichos espacios. No permite ver los productos ni los envíos de cada espacio aislado a menos que se otorguen los permisos de nivel de producto oportunos. </td><td align="left">  Permite acceder a la página de <b>espacios aislados</b>, y ver y administrar los espacios aislados en la cuenta, incluida la creación y la eliminación de espacios aislados y la administración de sus configuraciones. No permite ver los productos ni los envíos de cada espacio aislado a menos que se otorguen los permisos de nivel de producto oportunos.    </td></tr>
<tr><td align="left">    <b>Eventos de venta de la tienda</b>&nbsp;*                            </td><td align="left">  N/D    </td><td align="left">  Puede configurar la opción para incluir automáticamente los productos en los eventos de venta de la tienda.     </td></tr>
<tr><td align="left">    <b>Perfil de impuestos</b>                         </td><td align="left">  Permite ver la <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">información del perfil fiscal y los formularios</a> en <b>Configuración de la cuenta</b>.     </td><td align="left">  Permite rellenar formularios fiscales y actualizar la <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">información del perfil fiscal</a> en <b>Configuración de la cuenta</b>.     </td></tr>
<tr><td align="left">    <b>Cuentas de prueba</b>&nbsp;*                     </td><td align="left">  Permite ver las cuentas de la configuración de Xbox Live de prueba.      </td><td align="left">  Permite crear, administrar y ver las cuentas de configuración de Xbox Live de prueba.      </td></tr>
<tr><td align="left">    <b>Dispositivos Xbox</b>                        </td><td align="left">  Permite ver las consolas de desarrollo de Xbox habilitadas para la cuenta en la sección <b>Configuración de la cuenta</b>.       </td><td align="left">  Permite agregar, eliminar y ver las consolas de desarrollo de Xbox habilitadas para la cuenta en la sección <b>Configuración de la cuenta</b>.     </td></tr>
    </tbody>
    </table>

\* Los permisos marcados con un asterisco (*) conceden acceso a las características que no están disponibles para todas las cuentas. Si tu cuenta no se ha habilitado para acceder a estas características, las selecciones de estos permisos no tendrán efecto alguno.   


## <a name="product-level-permissions"></a>Permisos de nivel de producto

Los permisos de esta sección se pueden conceder a todos los productos de la cuenta o se pueden personalizar para que se conceda el permiso solo a uno o varios productos específicos. 

Los permisos de nivel de producto se agrupan en cuatro categorías: **análisis**, **monetización**, **publicación**y **Xbox Live**. Permite expandir cada una de estas categorías para ver los permisos individuales de cada categoría. También tiene la opción de habilitar **todos los permisos** para uno o varios productos específicos.

Para conceder un permiso para cada producto de la cuenta, realice las selecciones para ese permiso (alternando el cuadro para indicar **solo lectura** o **lectura/escritura**) en la fila marcada como **todos los productos**. 
 
> [!TIP]
> Las selecciones realizadas para **todos los productos** se aplicarán a todos los productos que se encuentren en la cuenta, así como a los productos futuros creados en la cuenta. Para evitar que se apliquen permisos a futuros productos, seleccione todos los productos individualmente en lugar de elegir **todos los productos**.

Debajo de la fila **Todos los productos**, verás todos los productos de la cuenta en una fila aparte. Para conceder un permiso solo a un producto en particular, realiza tus selecciones de dicho permiso en la fila del producto en cuestión.

Cada complemento aparece en una fila aparte debajo de su producto correspondiente, junto con una fila **Todos los complementos**. Las selecciones que se realicen para **Todos los complementos** se aplicarán a todos los complementos actuales de dicho producto, así como a cualquier complemento que se cree para dicho producto en el futuro.

Ten en cuenta que algunos permisos no se pueden establecer para los complementos. Esto se debe a que no se pueden aplicar a los complementos (por ejemplo, el permiso **Comentarios del usuario**) o porque los permisos que se conceden en el nivel de producto primario se aplican a todos los complementos de dicho producto (por ejemplo, los **códigos promocionales**). Sin embargo, ten en cuenta que ningún permiso que esté disponible para los complementos se debe establecer por separado; los complementos no heredan las selecciones que se realizan para el producto principal. Por ejemplo, si quieres permitir a un usuario realizar selecciones de precios y disponibilidad para un complemento, deberías habilitar el permiso **Precios y disponibilidad** para el complemento (o para **todos los complementos**), con independencia de que hayas concedido el permiso **Precios y disponibilidad** para el producto principal. 


### <a name="analytics"></a>Análisis

<table>
    <thead>
    <tr class="header">
    <th align="left">Nombre&nbsp;de permiso</th>
    <th align="left">Solo&nbsp;lectura</th>
    <th align="left">Lectura/escritura</th>
    <th align="left">Solo&nbsp;lectura&nbsp;(complemento) </th>
    <th align="left">Lectura y escritura&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Adquisiciones</b> (incluyendo datos casi en tiempo real) </td><td>    Permite ver los informes de <a href="acquisitions-report.md">adquisiciones</a> y de <a href="add-on-acquisitions-report.md">adquisiciones de complementos</a> del producto.        </td><td>    N/D    </td><td>    N/A (la configuración de producto principal incluye el informe de **adquisiciones de complementos** )        </td><td>    N/D                         </td></tr>
    <tr><td align="left">    <b>Uso</b> </td><td>    Permite ver el <a href="usage-report.md">informe de utilización</a> del producto.     </td><td>    N/D       </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    <b>Mantenimiento</b> (incluidos los datos casi en tiempo real) </td><td>    Permite ver el <a href="health-report.md">informe Mantenimiento</a> del producto.    </td><td>    N/D     </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    <b>Comentarios del cliente</b>    </td><td>    Puede ver los informes de <a href="reviews-report.md">opiniones</a> y <a href="feedback-report.md">comentarios</a> del producto.       </td><td>    N/D (para responder a los comentarios o a las críticas, se debe conceder el permiso correspondiente para <b>ponerse en contacto con los clientes</b>)   </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    <b>Análisis de Xbox</b> </td><td>    Puede ver el <a href="xbox-analytics-report.md">Informe de análisis de Xbox</a> del producto.    </td><td>    N/D   </td><td>    N/D       </td><td>    N/D          </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>Monetización

<table>
    <thead>
    <tr class="header">
    <th align="left">Nombre&nbsp;de permiso</th>
    <th align="left">Solo&nbsp;lectura</th>
    <th align="left">Lectura/escritura</th>
    <th align="left">Solo&nbsp;lectura&nbsp;(complemento) </th>
    <th align="left">Lectura y escritura&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Códigos promocionales</b>     </td><td>    Permite ver pedidos de <a href="generate-promotional-codes.md">código promocional</a> e información sobre la utilización del producto y sus complementos, y también información sobre su utilización.         </td><td>    Permite ver, administrar y crear pedidos de <a href="generate-promotional-codes.md">código promocional</a> del producto y sus complementos, y también ver información sobre su utilización.          </td><td>    N/D (la configuración relativa al producto principal se aplicará a todos los complementos)     </td><td>    N/D (la configuración relativa al producto principal se aplicará a todos los complementos)     </td></tr>
    <tr><td align="left">    <b>Ofertas de destino</b>     </td><td>    Puede ver las <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">ofertas de destino</a> del producto.         </td><td>    Puede ver, administrar y crear <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">ofertas de destino</a> para el producto.          </td><td>    N/D     </td><td>    N/D      </td></tr>
    <tr><td align="left">    <b>Contacto con el cliente</b>  </td><td>    Permite ver las <a href="respond-to-customer-feedback.md">respuestas a los comentarios de los clientes</a> y <a href="respond-to-customer-reviews.md">a las opiniones de los clientes</a>, siempre y cuando también se haya concedido permiso para <b>Comentarios del cliente</b>. También permite ver las <a href="send-push-notifications-to-your-apps-customers.md">notificaciones dirigidas</a> que se han creado para el producto.    </td><td>    Puede <a href="respond-to-customer-feedback.md">responder a los comentarios</a> de los clientes y <a href="respond-to-customer-reviews.md">responder a las opiniones</a>de los clientes, siempre y cuando se haya concedido también el permiso de comentarios del <b>cliente</b> . También permite <a href="send-push-notifications-to-your-apps-customers.md">crear y enviar notificaciones dirigidas</a> del producto.                   </td><td>    N/D         </td><td>    N/D                          </td></tr>
    <tr><td align="left">    <b>Experimentación</b></td><td>    Permite ver <a href="../monetize/run-app-experiments-with-a-b-testing.md">experimentos (pruebas A/B)</a> y los datos de los experimentos del producto.   </td><td>    Permite crear, administrar y ver <a href="../monetize/run-app-experiments-with-a-b-testing.md">experimentos (pruebas A/B)</a> del producto y ver datos de los experimentos.     </td><td>    N/D  </td><td>    N/D                 </td></tr>
    <tr><td align="left">    <b>Eventos de venta de la tienda</b>&nbsp;*</td><td>    Puede ver el estado de los eventos de venta del producto.   </td><td>    Puede Agregar el producto a los eventos de venta y configurar los descuentos.      </td><td>    Puede ver el estado de los eventos de venta del producto.   </td><td>    Puede Agregar el producto a los eventos de venta y configurar los descuentos.      </td></tr>
    </tbody>
    </table>

### <a name="publishing"></a>Publicación 

<table>
    <thead>
    <tr class="header">
    <th align="left">Nombre&nbsp;de permiso</th>
    <th align="left">Solo&nbsp;lectura</th>
    <th align="left">Lectura/escritura</th>
    <th align="left">Solo&nbsp;lectura&nbsp;(complemento) </th>
    <th align="left">Lectura y escritura&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Configuración del producto</b>  </td><td>    Puede ver la página de configuración del producto de los productos.     </td><td>    Puede ver y editar la página de configuración del producto de los productos. </td><td>    Puede ver la página Configuración del producto de los complementos.   </td><td>    Puede ver y editar los complementos de la página Configuración del producto.          </td></tr>
    <tr><td align="left">    <b>Precios y disponibilidad</b>  </td><td>    Puede ver la página de <a href="set-app-pricing-and-availability.md">precios y disponibilidad</a> de los productos.     </td><td>    Puede ver y editar la página de <a href="set-app-pricing-and-availability.md">precios y disponibilidad</a> de los productos. </td><td>    Puede ver la página de <a href="set-add-on-pricing-and-availability.md">precios y disponibilidad</a> de los complementos.   </td><td>    Puede ver y editar la página de <a href="set-add-on-pricing-and-availability.md">precios y disponibilidad</a> de los complementos.          </td></tr>
    <tr><td align="left">    <b>Propiedades</b>   </td><td>    Puede ver la página de <a href="enter-app-properties.md">propiedades</a> de los productos.      </td><td>    Puede ver y editar la página de <a href="enter-app-properties.md">propiedades</a> de los productos.       </td><td>    Puede ver la página <a href="enter-add-on-properties.md">propiedades</a> de los complementos.     </td><td>    Puede ver y editar la página de <a href="enter-add-on-properties.md">propiedades</a> de los complementos.               </td></tr>
    <tr><td align="left">    <b>Clasificaciones por edades</b>    </td><td>    Puede ver la página <a href="age-ratings.md">clasificaciones de edad</a> de los productos.       </td><td>    Puede ver y editar la página <a href="age-ratings.md">clasificaciones de edad</a> de los productos.    </td><td>    Puede ver la página clasificaciones de edad de los complementos.          </td><td>     Puede ver y editar la página clasificaciones de edad de los complementos.       </td></tr>
    <tr><td align="left">    <b>Paquetes</b>        </td><td>    Puede ver la página <a href="upload-app-packages.md">paquetes</a> de productos.  </td><td>    Puede ver y editar la página <a href="upload-app-packages.md">paquetes</a> de productos, incluidos los paquetes de carga.     </td><td>   Puede ver la página <a href="upload-app-packages.md">paquetes</a> de complementos (si procede).   </td><td>     Puede ver y editar los <a href="upload-app-packages.md">paquetes</a> de la página de complementos (si procede).             </td></tr>
    <tr><td align="left">    <b>Lista de tiendas</b>  </td><td>    Puede ver las <a href="create-app-store-listings.md">páginas</a> de la descripción de la tienda de los productos.  </td><td>    Puede ver y editar las <a href="create-app-store-listings.md">páginas</a> de la descripción de la tienda de los productos, y puede agregar nuevas listas de tiendas para distintos idiomas.     </td><td>    Puede ver las <a href="create-add-on-store-listings.md">páginas de Descripción</a> de la tienda de los complementos.            </td><td>    Puede ver y editar las <a href="create-add-on-store-listings.md">páginas de Descripción</a> de la tienda de los complementos y puede Agregar listas de tiendas para distintos idiomas.                 </td></tr>
    <tr><td align="left">    <b>Envío de la tienda</b>     </td><td>    No se concede ningún acceso si este permiso se establece como de solo lectura.           </td><td>    Permite enviar el producto a la Tienda y ver informes de certificación. Incluye los envíos nuevos y los actualizados. </td><td>No se concede ningún acceso si este permiso se establece como de solo lectura.     </td><td>    Permite enviar el complemento a la Tienda y ver informes de certificación. Incluye los envíos nuevos y los actualizados.</td></tr>
    <tr><td align="left">    <b>Creación de nuevo envío</b>       </td><td>    No se concede ningún acceso si este permiso se establece como de solo lectura.        </td><td>    Permite crear nuevos <a href="app-submissions.md">envíos</a> para el producto.  </td><td>    No se concede ningún acceso si este permiso se establece como de solo lectura.   </td><td>    Permite crear nuevos <a href="add-on-submissions.md">envíos</a> para el complemento.        </td></tr>
    <tr><td align="left">    <b>Nuevos complementos</b>    </td><td>    No se concede ningún acceso si este permiso se establece como de solo lectura. </td><td>    Permite <a href="set-your-add-on-product-id.md">crear nuevos complementos</a> para el producto. </td><td>    N/D    </td><td>    N/D        </td></tr>
    <tr><td align="left">    <b>Reservas de nombres</b>   </td><td>    Permite ver la página <a href="manage-app-names.md">Administrar nombres de aplicación</a> del producto.</td><td>    Permite ver y editar la página <a href="manage-app-names.md">Administrar nombres de aplicación</a> del producto, incluso reservar nombres adicionales y eliminar nombres reservados. </td><td>   Permite ver los nombres reservados del complemento.    </td><td>   Permite ver y editar nombres reservados del complemento.          </td></tr>
    <tr><td align="left">    <b>Solicitud de disco</b>   </td><td>    Puede ver el disco en la página de solicitud. </td><td>    Puede crear solicitudes de disco. </td><td>   N/D    </td><td>   N/D          </td></tr>
    <tr><td align="left">    <b>Regalías de disco </b>   </td><td>    Puede ver el disco en la página de regalías.</td><td>    Puede crear regalías de disco. </td><td>   N/D    </td><td>   N/D          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">Nombre&nbsp;de permiso</th>
    <th align="left">Solo&nbsp;lectura</th>
    <th align="left">Lectura/escritura</th>
    <th align="left">Solo&nbsp;lectura&nbsp;(complemento) </th>
    <th align="left">Lectura y escritura&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Usuarios de confianza</b>&nbsp;*</td><td>    Puede ver la página de usuarios de confianza de una cuenta.   </td><td>    Puede ver y editar la página de usuarios de confianza de una cuenta.    </td><td>    N/D    </td><td>    N/D                      </td></tr>
    <tr><td align="left">    <b>Servicios de asociados</b>&nbsp;*</td><td>    Puede ver la página servicios Web de una cuenta.  </td><td>    Puede ver y editar la página servicios Web de una cuenta.      </td><td>    N/D    </td><td>    N/D                      </td></tr>
    <tr><td align="left">    <b>Cuentas de prueba de Xbox</b>&nbsp;*</td><td>    Puede ver la página cuentas de prueba de Xbox de una cuenta.  </td><td>    Puede ver y editar la página de cuentas de prueba de Xbox de una cuenta.    </td><td>    N/D    </td><td>    N/D                      </td></tr>
    <tr><td align="left">    <b>Cuentas de prueba de Xbox por espacio aislado</b>&nbsp;*</td><td>    Puede ver la página cuentas de prueba de Xbox solo para los espacios aislados especificados de una cuenta.  </td><td>    Puede ver y editar la prueba de Xbox.   <tr><td align="left">    <b>Página cuentas solo para los espacios aislados especificados de una cuenta    </td><td>    N/D    </td><td>    N/D                      </td></tr>
    <tr><td align="left">    <b>Dispositivos Xbox</b>&nbsp;*</td><td>    Puede ver la página de las consolas de desarrollo de Xbox One de una cuenta.  </td><td>    Puede ver y editar la página de las consolas de desarrollo de Xbox One de una cuenta.    </td><td>    N/D    </td><td>    N/D                      </td></tr>
    <tr><td align="left">    <b>Dispositivos Xbox por espacio aislado</b>&nbsp;*</td><td>    Puede ver la página de consolas de desarrollo de Xbox One solo para los espacios aislados especificados de una cuenta.  </td><td>    Puede ver y editar la página de consolas de desarrollo de Xbox One solo para los espacios aislados especificados de una cuenta.    </td><td>    N/D    </td><td>    N/D                      </td></tr>
    <tr><td align="left">    <b>Canales de aplicación</b>&nbsp;*</td><td>    N/D  </td><td>    Permite publicar promocionales canales de vídeo en la consola Xbox para su visualización a través de OneGuide.    </td><td>    N/D    </td><td>    N/D                      </td></tr>
    <tr><td align="left">    <b>Configuración del servicio</b>&nbsp;*</td><td>    Puede ver la página de configuración del servicio Xbox Live de un producto.  </td><td>    Puede ver y editar la página de configuración del servicio Xbox Live de un producto.    </td><td>    N/D    </td><td>    N/D                      </td></tr>
    <tr><td align="left">    <b>Herramientas acceso</b>&nbsp;*</td><td>    Puede ejecutar las herramientas de Xbox Live en un producto para ver únicamente los datos.  </td><td>    Puede ejecutar las herramientas de Xbox Live en un producto para ver y editar los datos.    </td><td>    N/D    </td><td>    N/D                      </td></tr>
</tbody>
</table>

\* Los permisos marcados con un asterisco (*) conceden acceso a las características que no están disponibles para todas las cuentas. Si tu cuenta no se ha habilitado para acceder a estas características, las selecciones de estos permisos no tendrán efecto alguno.