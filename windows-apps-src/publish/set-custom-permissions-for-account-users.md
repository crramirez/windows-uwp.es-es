---
Description: Conjunto de roles o permisos personalizados para usuarios de la cuenta.
title: Establecer roles o permisos personalizados para usuarios de cuentas
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, roles de usuario, permiso de usuario, personalizar roles, acceso de usuario, personalizar permisos, roles estándar
ms.localizationpriority: medium
ms.openlocfilehash: 450fc4d016debb72364cbefedb6b69a80cc6235b
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63790604"
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>Establecer roles o permisos personalizados para usuarios de cuentas

Cuando se [agregar usuarios a la cuenta del centro de partners](add-users-groups-and-azure-ad-applications.md), deberá especificar qué acceso tiene dentro de la cuenta. Para ello, asígnales [roles estándar](#roles) que se aplican a toda la cuenta, o puedes [personalizar sus permisos](#custom) para proporcionar el nivel de acceso adecuado. Algunos de los permisos personalizados se aplican a toda la cuenta y algunos pueden estar limitados a uno o varios productos específicos (o concedidos a todos los productos, si lo prefieres).

> [!NOTE] 
> Pueden aplicarse los mismos roles y permisos con independencia de que vayas a agregar un usuario, un grupo o una aplicación de Azure AD.

Al determinar los roles o los permisos que desees aplicar, ten en cuenta: 
-   Los usuarios (incluidos los grupos y aplicaciones de Azure AD) podrán obtener acceso a toda la cuenta del centro de partners con los permisos asociados con sus roles asignados, a menos que [personalizar permisos](#custom) y asignar [ permisos de nivel de producto](#product-level-permissions) para que solo puede funcionar con aplicaciones específicas o los complementos.
-   Para permitir que un usuario, grupo o aplicación de Azure AD tengan acceso a las funciones de más de un rol, selecciona varios roles o utiliza permisos personalizados para conceder el acceso que quieras.
-   Un usuario con un determinado rol (o un conjunto de permisos personalizados) también puede formar parte de un grupo que tenga un rol diferente (o un conjunto de permisos). En ese caso, el usuario tendrá acceso a toda la funcionalidad asociada con el grupo y con la cuenta individual.

> [!TIP]
> Este tema es específico para el programa para desarrolladores de aplicaciones de Windows en [centro de partners](https://partner.microsoft.com/dashboard). Para obtener información sobre los roles de usuario del Programa de desarrolladores de hardware, consulta [Administración de roles de usuario](https://docs.microsoft.com/windows-hardware/drivers/dashboard/managing-user-roles). Para obtener información sobre los roles de usuario en el Programa de aplicaciones de escritorio de Windows, consulta [Programa de aplicaciones de escritorio de Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users).


<span id="roles" />

## <a name="assign-roles-to-account-users"></a>Asignar roles a usuarios de la cuenta

De forma predeterminada, se presenta un conjunto de roles estándares para que pueda elegir al agregar un usuario, grupo o aplicación de Azure AD a su cuenta de centro de partners. Cada rol tiene un conjunto específico de permisos para realizar determinadas funciones en la cuenta. 

A menos que decidas definir [permisos personalizados](#custom) seleccionando **Permisos personalizados**, a cada usuario, grupo o aplicación de Azure AD que agregas a una cuenta debes asignarle al menos uno de los siguientes roles. 

> [!NOTE]
> El **propietario** de la cuenta es la persona que la creó inicialmente con una cuenta de Microsoft (y no los usuarios agregados a través de Azure AD). Dicho propietario de la cuenta es la única persona con acceso completo a la cuenta, lo que incluye la capacidad de eliminar aplicaciones, crear y editar todos los usuarios de la cuenta y modificar todas las opciones financieras y de la cuenta. 


| Rol                 | Descripción              |
|----------------------|--------------------------|
| Manager              | Tiene acceso completo a la cuenta, excepto para cambiar la configuración fiscal y de pago. Esto incluye la administración de usuarios en el centro de partners, pero tenga en cuenta que la capacidad de crear y eliminar los usuarios del inquilino de Azure AD depende de los permisos de la cuenta de Azure AD. Es decir, si un usuario está asignado el rol de administrador, pero no tiene permisos de administrador global de la organización Azure AD, no podrán crear nuevos usuarios o eliminar usuarios del directorio (aunque puede cambiar una función de usuario centro de partners). <p> Tenga en cuenta que si la cuenta del centro de partners está asociada a más de un inquilino de Azure AD, un administrador no puede ver los detalles completos de un usuario (incluidos nombre, apellido, correo electrónico de la recuperación de contraseña, y si son administradores globales de Azure AD) a menos que estén iniciar sesión en el mismo inquilino que ese usuario con una cuenta que tenga permisos de administrador global para ese inquilino. Sin embargo, puede agregar y quitar usuarios en todos los inquilinos que está asociado con la cuenta del centro de partners. |
| Desarrollador            | Puede cargar paquetes y enviar aplicaciones y complementos, así como ver el [Informe de uso](usage-report.md) para obtener información detallada de telemetría. Puede tener acceso a [experiencias multidispositivo](https://go.microsoft.com/fwlink/?linkid=874042) funcionalidad. No puede ver la configuración de la cuenta ni la información financiera.   |
| Colaborador empresarial | Puede ver informes de [estado](health-report.md) y de [uso](usage-report.md). No puede crear ni enviar productos, cambiar la configuración de cuenta ni ver información financiera.   |
| Colaborador financiero  | Puede ver [informes de pago](payout-summary.md), información financiera e informes de adquisición. No puede modificar las aplicaciones, los complementos ni la configuración de la cuenta.    |
| Vendedor             | Puede [responder a las opiniones de los clientes](respond-to-customer-reviews.md) y ver [informes analíticos](analytics.md) que no sean financieros. No puede modificar las aplicaciones, los complementos ni la configuración de la cuenta.      |

La siguiente tabla muestra algunas de las características específicas disponibles para cada rol (y para el propietario de la cuenta).

|                                 |    Propietario de la cuenta                 |    Manager                       |    Desarrollador                     |    Colaborador empresarial    |    Colaborador financiero    |    Vendedor                      |
|---------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    Informe de adquisiciones           |    Puede ver                      |    Puede ver                      |     Sin acceso                    |     Sin acceso              |    Puede ver               |    Sin acceso                     |
|    Respuestas/informe de comentarios    |    Puede ver y enviar comentarios    |    Puede ver y enviar comentarios    |    Puede ver y enviar comentarios    |     Sin acceso              |     Sin acceso             |    Puede ver y enviar comentarios    |
|    Informe de estado                |    Puede ver                      |    Puede ver                      |    Puede ver                      |    Puede ver                |     Sin acceso             |    Sin acceso                     |
|    Informe de uso                 |    Puede ver                      |    Puede ver                      |    Puede ver                      |    Puede ver                |     Sin acceso             |    Sin acceso                     |
|    Cuenta de pago               |    Puede actualizar                    |    Sin acceso                     |    Sin acceso                     |    Sin acceso               |    Puede ver               |    Sin acceso                     |
|    Perfil fiscal                  |    Puede actualizar                    |    Sin acceso                     |    Sin acceso                     |    Sin acceso               |    Puede ver               |    Sin acceso                     |
|    Resumen de pago               |    Puede ver                      |    Sin acceso                     |    Sin acceso                     |    Sin acceso               |    Puede ver               |    Sin acceso                     |

Si ninguno de los roles estándar es adecuado o si quieres limitar el acceso a complementos o aplicaciones determinados, puedes seleccionar **Personalizar permisos** para conceder permisos personalizados al usuario, tal y como se describe a continuación.


<span id="custom" />

## <a name="assign-custom-permissions-to-account-users"></a>Asignar permisos personalizados a usuarios de cuentas

Para asignar permisos personalizados en lugar de roles estándar, haz clic en **Personalizar permisos** en la sección **Roles** al agregar o modificar la cuenta del usuario. 

Si quieres habilitar un permiso para el usuario, activa o desactiva la casilla con el ajuste que corresponda. 

![Guía de la configuración de acceso](images/permission_key.png)

- **Sin acceso**: El usuario no tendrá el permiso indicado.
- **De sólo lectura**: El usuario tendrá acceso para ver las características relacionadas con el área indicado, pero no podrá realizar cambios. 
- **Read/write**: El usuario tendrá acceso para realizar cambios asociados con el área, así como para verlo.
- **Mixto**: No se selecciona esta opción directamente, pero la **mixto** indicador mostrará si ha permitido una combinación de acceso para ese permiso. Por ejemplo, si concedes acceso de **Solo lectura** a **Precios y disponibilidad** para **Todos los productos**, pero luego concedes acceso de **Lectura y escritura** a **Precios y disponibilidad** para un producto específico, el indicador **Precios y disponibilidad** de **Todos los productos** se mostrará como mixto. Lo mismo sucede si algunos productos presentan **Sin acceso** para un permiso y otros, acceso de **Lectura y escritura** o de **Solo lectura**.

En el caso de algunos permisos, como los relacionados con la visualización de datos analíticos, solo se puede conceder acceso de **Solo lectura**. Ten en cuenta que en la implementación actual algunos permisos no distinguen en absoluto entre acceso de **Solo lectura** y de **Lectura y escritura**. Revisa los detalles de cada permiso para comprender las capacidades específicas que concede el acceso de **Solo lectura** y de **Lectura y escritura**.

En las tablas siguientes se describen los datos concretos de cada permiso.

## <a name="account-level-permissions"></a>Permisos de nivel de cuenta

Los permisos de esta sección no se pueden limitar a productos específicos. La concesión de acceso a uno de estos permisos otorga al usuario permiso para toda la cuenta.

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
    <th align="left">Lectura y escritura</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    <b>Configuración de la cuenta</b>                    </td><td align="left">  Permite ver todas las páginas de la sección <b>Configuración de la cuenta</b>, incluida la <a href="managing-your-profile.md">información de contacto</a>.       </td><td align="left">  Permite ver todas las páginas de la sección <b>Configuración de la cuenta</b>. Permite realizar cambios en la <a href="managing-your-profile.md">información de contacto</a> y en otras páginas, pero no en la cuenta de pago ni en el perfil fiscal (a menos que este permiso se haya concedido por separado).            </td></tr>
<tr><td align="left">    <b>Usuarios de cuentas</b>                       </td><td align="left">  Permite ver los usuarios que se han agregado a la cuenta en la sección <b>Usuarios</b>.          </td><td align="left">  Permite agregar usuarios a la cuenta y realizar cambios en los usuarios existentes en la sección <b>Usuarios</b>.             </td></tr>
<tr><td align="left">    <b>Informe de rendimiento de nivel de cuenta de ad</b> </td><td align="left">  Permite ver el <a href="advertising-performance-report.md">Informe de rendimiento de la publicidad</a> de nivel de cuenta.      </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    <b>Campañas de anuncios</b>                        </td><td align="left">  Permite ver las <a href="create-an-ad-campaign-for-your-app.md">campañas de anuncios</a> que se han creado en la cuenta.      </td><td align="left">  Permite crear, administrar y ver las <a href="create-an-ad-campaign-for-your-app.md">campañas de anuncios</a> que se han creado en la cuenta.          </td></tr>
<tr><td align="left">    <b>Mediación de AD</b>                        </td><td align="left">  Puede ver las configuraciones de mediación de ad para todos los productos de la cuenta.    </td><td align="left">  Puede ver y cambiar las configuraciones de mediación de ad para todos los productos de la cuenta.        </td></tr>
<tr><td align="left">    <b>Informes de mediación de AD</b>                </td><td align="left">  Permite ver el <a href="ad-mediation-report.md">informe de mediación de anuncios</a> de todos los productos de la cuenta.    </td><td align="left">  N/D    </td></tr>
<tr><td align="left">    <b>Informes de rendimiento de AD</b>              </td><td align="left">  Permite ver los <a href="advertising-performance-report.md">informes de rendimiento de la publicidad</a> de todos los productos de la cuenta.       </td><td align="left">  N/D         </td></tr>
<tr><td align="left">    <b>Unidades de anuncios</b>                            </td><td align="left">  Permite ver las <a href="in-app-ads.md">unidades de anuncios</a> que se han creado para la cuenta.    </td><td align="left">  Permite crear, administrar y ver las <a href="in-app-ads.md">unidades de anuncios</a> de la cuenta.             </td></tr>
<tr><td align="left">    <b>Afiliada de anuncios</b>                       </td><td align="left">  Permite ver la utilización de <a href="about-affiliate-ads.md">anuncios de filiales</a> de todos los productos de la cuenta.    </td><td align="left">  Permite administrar y ver la utilización de los <a href="about-affiliate-ads.md">anuncios de filiales</a> de todos los productos de la cuenta.                </td></tr>
<tr><td align="left">    <b>Informes de rendimiento de afiliados</b>      </td><td align="left">  Permite ver el <a href="affiliates-performance-report.md">informe de rendimiento de filiales</a> de todos los productos de la cuenta.   </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    <b>Informes de anuncios de instalación de aplicaciones</b>             </td><td align="left">  Puedes ver el <a href="promote-your-app-report.md">Informe Campaña publicitaria</a>.           </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    <b>Anuncios de la Comunidad</b>                       </td><td align="left">  Permite ver de forma gratuita la utilización del <a href="about-community-ads.md">anuncio de la comunidad</a> de todos los productos de la cuenta.          </td><td align="left">  Permite crear, administrar y ver la utilización del <a href="about-community-ads.md">anuncio de la comunidad</a> gratuito de todos los productos de la cuenta.               </td></tr>
<tr><td align="left">    <b>Información de contacto</b>                        </td><td align="left">  Permite ver la <a href="managing-your-profile.md">información de contacto</a> en la sección de configuración de la cuenta.        </td><td align="left">  Permite ver y editar la <a href="managing-your-profile.md">información de contacto</a> en la sección de configuración de la cuenta.            </td></tr>
<tr><td align="left">    <b>Cumplimiento COPPA</b>                    </td><td align="left">  Permite ver selecciones del <a href="in-app-ads.md#coppa-compliance">cumplimiento de COPPA</a> (que indica si los productos se destinan a niños menores de 13 años) de todos los productos de la cuenta.                                            </td><td align="left">  Permite ver y editar selecciones del <a href="in-app-ads.md#coppa-compliance">cumplimiento de COPPA</a> (que indica si los productos se destinan a niños menores de 13 años) de todos los productos de la cuenta.         </td></tr>
<tr><td align="left">    <b>Grupos de clientes</b>                     </td><td align="left">  Puede ver <a href="create-customer-groups.md">grupos de clientes</a> (segmentos y grupos de usuarios conocidos).      </td><td align="left">  Puede crear, editar y ver <a href="create-customer-groups.md">grupos de clientes</a> (segmentos y grupos de usuarios conocidos).       </td></tr>
<tr><td align="left">    <b>Administrar grupos de productos</b>&nbsp;*                            </td><td align="left">  Permite ver la nueva página de creación de grupos de productos, pero en realidad no permite crear nuevos grupos de productos.    </td><td align="left">  Permite crear y editar grupos de productos.     </td></tr>
<tr><td align="left">    <b>Nuevas aplicaciones</b>                            </td><td align="left">  Permite ver la nueva página de creación de aplicaciones, pero en realidad no permite crear nuevas aplicaciones en la cuenta.    </td><td align="left">  Permite <a href="create-your-app-by-reserving-a-name.md">crear nuevas aplicaciones</a> en la cuenta mediante la reserva de nuevos nombres de aplicación, y crear envíos y enviar aplicaciones a la Tienda.     </td></tr>
<tr><td align="left">    <b>Nuevos conjuntos</b>&nbsp;*                       </td><td align="left">  Permite ver la nueva página de creación de conjuntos, pero en realidad no permite crear nuevos conjuntos en la cuenta.     </td><td align="left">  Permite crear nuevos conjuntos de productos.          </td></tr>
<tr><td align="left">    <b>Servicios de partners</b>&nbsp;*                  </td><td align="left">  Permite ver certificados para la instalación en servicios con el fin de recuperar XTokens.     </td><td align="left">  Permite administrar y ver certificados para la instalación en servicios con el fin de recuperar XTokens.       </td></tr>
<tr><td align="left">    <b>Cuenta de pago</b>                      </td><td align="left">  Permite ver la <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">información de la cuenta de pago</a> en <b>Configuración de la cuenta</b>.     </td><td align="left">  Permite editar y ver la <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">información de la cuenta de pago</a> en <b>Configuración de la cuenta</b>.       </td></tr>
<tr><td align="left">    <b>Resumen de pago</b>                      </td><td align="left">  Permite ver el <a href="payout-summary.md">resumen de pago</a> para acceder y descargar información de informes de pago.       </td><td align="left">  Permite ver el <a href="payout-summary.md">resumen de pago</a> para acceder y descargar información de informes de pago.   </td></tr>
<tr><td align="left">    <b>Usuarios de confianza</b>&nbsp;*                   </td><td align="left">  Permite ver los usuarios de confianza para recuperar XTokens.    </td><td align="left">  Permite administrar y ver los usuarios de confianza para recuperar XTokens.     </td></tr>
<tr><td align="left">    <b>Solicitar discos</b>&nbsp;*                   </td><td align="left">  Puedes ver las solicitudes de discos de juegos.    </td><td align="left">  Puedes crear y ver las solicitudes de discos de juegos.     </td></tr>
<tr><td align="left">    <b>Espacios aislados</b>&nbsp;*                         </td><td align="left">  Permite acceder a la página de <b>espacios aislados</b>, y ver los espacios aislados de la cuenta y cualquier configuración que se aplique a dichos espacios. No permite ver los productos ni los envíos de cada espacio aislado a menos que se otorguen los permisos de nivel de producto oportunos. </td><td align="left">  Permite acceder a la página de <b>espacios aislados</b>, y ver y administrar los espacios aislados en la cuenta, incluida la creación y la eliminación de espacios aislados y la administración de sus configuraciones. No permite ver los productos ni los envíos de cada espacio aislado a menos que se otorguen los permisos de nivel de producto oportunos.    </td></tr>
<tr><td align="left">    <b>Eventos de ventas de la Store</b>&nbsp;*                            </td><td align="left">  N/D    </td><td align="left">  Permite configurar la opción de incluir productos automáticamente en los eventos de venta de la Store.     </td></tr>
<tr><td align="left">    <b>Perfil de impuestos</b>                         </td><td align="left">  Permite ver la <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">información del perfil fiscal y los formularios</a> en <b>Configuración de la cuenta</b>.     </td><td align="left">  Permite rellenar formularios fiscales y actualizar la <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">información del perfil fiscal</a> en <b>Configuración de la cuenta</b>.     </td></tr>
<tr><td align="left">    <b>Cuentas de prueba</b>&nbsp;*                     </td><td align="left">  Permite ver las cuentas de la configuración de Xbox Live de prueba.      </td><td align="left">  Permite crear, administrar y ver las cuentas de configuración de Xbox Live de prueba.      </td></tr>
<tr><td align="left">    <b>Dispositivos de Xbox</b>                        </td><td align="left">  Permite ver las consolas de desarrollo de Xbox habilitadas para la cuenta en la sección <b>Configuración de la cuenta</b>.       </td><td align="left">  Permite agregar, eliminar y ver las consolas de desarrollo de Xbox habilitadas para la cuenta en la sección <b>Configuración de la cuenta</b>.     </td></tr>
    </tbody>
    </table>

\* Los permisos se marcan con un asterisco (*) conceder acceso a las características que no están disponibles para todas las cuentas. Si tu cuenta no se ha habilitado para acceder a estas características, las selecciones de estos permisos no tendrán efecto alguno.   


## <a name="product-level-permissions"></a>Permisos de nivel de producto

Los permisos de esta sección se pueden conceder a todos los productos de la cuenta o se pueden personalizar para que se conceda el permiso solo a uno o varios productos específicos. 

Permisos de nivel de producto se agrupan en cuatro categorías: **Análisis**, **monetización**, **publicación**, y **Xbox Live**. Permite expandir cada una de estas categorías para ver los permisos individuales de cada categoría. También tienes la opción de habilitar **Todos los permisos** para uno o más productos específicos.

Para conceder un permiso para cada producto de la cuenta, realiza las selecciones de ese permiso (activando o desactivando la casilla para indicar **Solo lectura** o **Lectura y escritura**) en la fila marcada con **Todos los productos**. 
 
> [!TIP]
> Las selecciones que se realicen para **Todos los productos** se aplicarán a todos los productos actuales de la cuenta, así como a cualquier producto que se cree en la cuenta en el futuro. Para evitar que los permisos se apliquen a futuros productos, selecciona todos los productos de forma individual, en lugar de elegir **Todos los productos**.

Debajo de la fila **Todos los productos**, verás todos los productos de la cuenta en una fila aparte. Para conceder un permiso solo a un producto en particular, realiza tus selecciones de dicho permiso en la fila del producto en cuestión.

Cada complemento aparece en una fila aparte debajo de su producto correspondiente, junto con una fila **Todos los complementos**. Las selecciones que se realicen para **Todos los complementos** se aplicarán a todos los complementos actuales de dicho producto, así como a cualquier complemento que se cree para dicho producto en el futuro.

Ten en cuenta que algunos permisos no se pueden establecer para los complementos. Esto se debe a que no se pueden aplicar a los complementos (por ejemplo, el permiso **Comentarios del usuario**) o porque los permisos que se conceden en el nivel de producto primario se aplican a todos los complementos de dicho producto (por ejemplo, los **códigos promocionales**). Sin embargo, ten en cuenta que ningún permiso que esté disponible para los complementos se debe establecer por separado; los complementos no heredan las selecciones que se realizan para el producto principal. Por ejemplo, si quieres permitir a un usuario realizar selecciones de precios y disponibilidad para un complemento, deberías habilitar el permiso **Precios y disponibilidad** para el complemento (o para **todos los complementos**), con independencia de que hayas concedido el permiso **Precios y disponibilidad** para el producto principal. 


### <a name="analytics"></a>Análisis

<table>
    <thead>
    <tr class="header">
    <th align="left">Nombre&nbsp;de permiso</th>
    <th align="left">Solo&nbsp;lectura</th>
    <th align="left">Lectura y escritura</th>
    <th align="left">Solo&nbsp;lectura&nbsp;(complemento) </th>
    <th align="left">Lectura y escritura&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Adquisiciones</b>     </td><td>    Permite ver los informes de <a href="acquisitions-report.md">adquisiciones</a> y de <a href="add-on-acquisitions-report.md">adquisiciones de complementos</a> del producto.        </td><td>    N/D    </td><td>    N/D (la configuración para el producto padre incluye el **adquisiciones de complemento** informes)        </td><td>    N/D                         </td></tr>
    <tr><td align="left">    <b>Uso de</b> </td><td>    Permite ver el <a href="usage-report.md">informe de utilización</a> del producto.     </td><td>    N/D       </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    <b>Estado</b> </td><td>    Permite ver el <a href="health-report.md">informe Mantenimiento</a> del producto.    </td><td>    N/D     </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    <b>Comentarios del cliente</b>    </td><td>    Permite ver los informes <a href="reviews-report.md">Valoraciones</a> y <a href="feedback-report.md">Comentarios</a> del producto.       </td><td>    N/D (para responder a los comentarios o a las críticas, se debe conceder el permiso correspondiente para <b>ponerse en contacto con los clientes</b>)   </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    <b>Análisis de Xbox</b> </td><td>    Puede ver el <a href="xbox-analytics-report.md">informe de análisis de Xbox</a> para el producto.    </td><td>    N/D   </td><td>    N/D       </td><td>    N/D          </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>Monetización

<table>
    <thead>
    <tr class="header">
    <th align="left">Nombre&nbsp;de permiso</th>
    <th align="left">Solo&nbsp;lectura</th>
    <th align="left">Lectura y escritura</th>
    <th align="left">Solo&nbsp;lectura&nbsp;(complemento) </th>
    <th align="left">Lectura y escritura&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Códigos promocionales</b>     </td><td>    Permite ver pedidos de <a href="generate-promotional-codes.md">código promocional</a> e información sobre la utilización del producto y sus complementos, y también información sobre su utilización.         </td><td>    Permite ver, administrar y crear pedidos de <a href="generate-promotional-codes.md">código promocional</a> del producto y sus complementos, y también ver información sobre su utilización.          </td><td>    N/D (la configuración relativa al producto principal se aplicará a todos los complementos)     </td><td>    N/D (la configuración relativa al producto principal se aplicará a todos los complementos)     </td></tr>
    <tr><td align="left">    <b>Ofertas de destino</b>     </td><td>    Puedes ver las <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">ofertas dirigidas</a> del producto.         </td><td>    Permite ver, administrar y crear las <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">ofertas dirigidas</a> de la cuenta.          </td><td>    N/D     </td><td>    N/D      </td></tr>
    <tr><td align="left">    <b>Póngase en contacto con el cliente</b>  </td><td>    Permite ver las <a href="respond-to-customer-feedback.md">respuestas a los comentarios de los clientes</a> y <a href="respond-to-customer-reviews.md">a las opiniones de los clientes</a>, siempre y cuando también se haya concedido permiso para <b>Comentarios del cliente</b>. También permite ver las <a href="send-push-notifications-to-your-apps-customers.md">notificaciones dirigidas</a> que se han creado para el producto.    </td><td>    Puede <a href="respond-to-customer-feedback.md">responder a los comentarios de clientes</a> y <a href="respond-to-customer-reviews.md">responder a las revisiones de cliente</a>, siempre y cuando la <b>comentarios de los clientes</b> también se ha concedido permiso. También permite <a href="send-push-notifications-to-your-apps-customers.md">crear y enviar notificaciones dirigidas</a> del producto.                   </td><td>    N/D         </td><td>    N/D                          </td></tr>
    <tr><td align="left">    <b>Experimentación</b></td><td>    Permite ver <a href="../monetize/run-app-experiments-with-a-b-testing.md">experimentos (pruebas A/B)</a> y los datos de los experimentos del producto.   </td><td>    Permite crear, administrar y ver <a href="../monetize/run-app-experiments-with-a-b-testing.md">experimentos (pruebas A/B)</a> del producto y ver datos de los experimentos.     </td><td>    N/D  </td><td>    N/D                 </td></tr>
    <tr><td align="left">    <b>Eventos de ventas de la Store</b>&nbsp;*</td><td>    Permite ver el estado del evento de venta para el producto.   </td><td>    Permite agregar el producto a los eventos de venta y configurar descuentos.      </td><td>    Permite ver el estado del evento de venta para el producto.   </td><td>    Permite agregar el producto a los eventos de venta y configurar descuentos.      </td></tr>
    </tbody>
    </table>

### <a name="publishing"></a>Publishing 

<table>
    <thead>
    <tr class="header">
    <th align="left">Nombre&nbsp;de permiso</th>
    <th align="left">Solo&nbsp;lectura</th>
    <th align="left">Lectura y escritura</th>
    <th align="left">Solo&nbsp;lectura&nbsp;(complemento) </th>
    <th align="left">Lectura y escritura&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Precios y disponibilidad</b>  </td><td>    Permite ver la página <a href="set-app-pricing-and-availability.md">Precios y disponibilidad</a> de los envíos de productos.     </td><td>    Permite ver y editar la página <a href="set-app-pricing-and-availability.md">Precios y disponibilidad</a> de los envíos de productos. </td><td>    Permite ver la página <a href="set-add-on-pricing-and-availability.md">Precios y disponibilidad</a> de los envíos de complementos.   </td><td>    Permite ver y editar la página <a href="set-add-on-pricing-and-availability.md">Precios y disponibilidad</a> de los envíos de complementos.          </td></tr>
    <tr><td align="left">    <b>Propiedades</b>   </td><td>    Permite ver la página <a href="enter-app-properties.md">Propiedades</a> de envíos de productos.      </td><td>    Permite ver y editar la página <a href="enter-app-properties.md">Propiedades</a> de envíos de productos.       </td><td>    Permite ver la página <a href="enter-add-on-properties.md">Propiedades</a> de envíos de complementos.     </td><td>    Permite ver y editar la página <a href="enter-add-on-properties.md">Propiedades</a> de envíos de complementos.               </td></tr>
    <tr><td align="left">    <b>Clasificaciones por edades</b>    </td><td>    Permite ver la página <a href="age-ratings.md">Clasificaciones por edades</a> de envíos de productos.       </td><td>    Permite ver y editar la página <a href="age-ratings.md">Clasificaciones por edades</a> de envíos de productos.    </td><td>    Permite ver la página Clasificaciones por edades de envíos de complementos.          </td><td>     Permite ver y editar la página Clasificaciones por edades de envíos de complementos.       </td></tr>
    <tr><td align="left">    <b>Paquetes</b>        </td><td>    Permite ver la página <a href="upload-app-packages.md">Paquetes</a> de envíos de productos.  </td><td>    Permite ver y editar la página <a href="upload-app-packages.md">Paquetes</a> de envíos de productos, incluidos los paquetes que se estén cargando.     </td><td>   Permite ver objetivos y paquetes de familias de dispositivos (si procede) de envíos de complementos.   </td><td>     Permite ver y editar los objetivos de las familias de dispositivos de envíos de complementos, incluidos los paquetes que se estén cargando (si corresponde).             </td></tr>
    <tr><td align="left">    <b>Anuncios de Store</b>  </td><td>    Permite ver las <a href="create-app-store-listings.md">páginas de descripción de la Tienda</a> de envíos de productos.  </td><td>    Permite ver y editar las <a href="create-app-store-listings.md">páginas de descripción de la Tienda</a> de envíos de productos y agregar nuevas descripciones de la Tienda en idiomas distintos.     </td><td>    Permite ver las <a href="create-add-on-store-listings.md">páginas de descripción de la Tienda</a> de envíos de complementos.            </td><td>    Permite ver y editar las <a href="create-add-on-store-listings.md">páginas de descripción de la Tienda</a> de envíos de complementos y agregar descripciones de la Tienda en idiomas distintos.                 </td></tr>
    <tr><td align="left">    <b>Envío de Store</b>     </td><td>    No se concede ningún acceso si este permiso se establece como de solo lectura.           </td><td>    Permite enviar el producto a la Tienda y ver informes de certificación. Incluye los envíos nuevos y los actualizados. </td><td>No se concede ningún acceso si este permiso se establece como de solo lectura.     </td><td>    Permite enviar el complemento a la Tienda y ver informes de certificación. Incluye los envíos nuevos y los actualizados.</td></tr>
    <tr><td align="left">    <b>Creación de nuevo envío</b>       </td><td>    No se concede ningún acceso si este permiso se establece como de solo lectura.        </td><td>    Permite crear nuevos <a href="app-submissions.md">envíos</a> para el producto.  </td><td>    No se concede ningún acceso si este permiso se establece como de solo lectura.   </td><td>    Permite crear nuevos <a href="add-on-submissions.md">envíos</a> para el complemento.        </td></tr>
    <tr><td align="left">    <b>Nuevos complementos</b>    </td><td>    No se concede ningún acceso si este permiso se establece como de solo lectura. </td><td>    Permite <a href="set-your-add-on-product-id.md">crear nuevos complementos</a> para el producto. </td><td>    N/D    </td><td>    N/D        </td></tr>
    <tr><td align="left">    <b>Reservas de direcciones de nombre</b>   </td><td>    Permite ver la página <a href="manage-app-names.md">Administrar nombres de aplicación</a> del producto.</td><td>    Permite ver y editar la página <a href="manage-app-names.md">Administrar nombres de aplicación</a> del producto, incluso reservar nombres adicionales y eliminar nombres reservados. </td><td>   Permite ver los nombres reservados del complemento.    </td><td>   Permite ver y editar nombres reservados del complemento.          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">Nombre&nbsp;de permiso</th>
    <th align="left">Solo&nbsp;lectura</th>
    <th align="left">Lectura y escritura</th>
    <th align="left">Solo&nbsp;lectura&nbsp;(complemento) </th>
    <th align="left">Lectura y escritura&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Canales de aplicación</b>&nbsp;*</td><td>    N/D  </td><td>    Permite publicar promocionales canales de vídeo en la consola Xbox para su visualización a través de OneGuide.  </td><td>  N/D </td><td> N/D </td></tr>
    <tr><td align="left">    <b>Configuración del servicio</b>&nbsp;*    </td><td>    Permite ver la configuración relacionada con los logros, la opción multijugador, los marcadores y otra configuración de Xbox Live del producto.  </td><td>    Permite ver y editar la configuración relacionada con los logros, la opción multijugador, los marcadores y otra configuración de Xbox Live del producto.  </td><td>    N/D     </td><td>    N/D                      </td></tr>
</tbody>
</table>

\* Los permisos se marcan con un asterisco (*) conceder acceso a las características que no están disponibles para todas las cuentas. Si tu cuenta no se ha habilitado para acceder a estas características, las selecciones de estos permisos no tendrán efecto alguno.  
