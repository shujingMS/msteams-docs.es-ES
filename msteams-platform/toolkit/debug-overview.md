---
title: Depurar la aplicación de Teams
author: surbhigupta
description: En este módulo, aprenderá a depurar la aplicación de Teams en teams Toolkit y las características clave del kit de herramientas de Teams.
ms.author: v-amprasad
ms.localizationpriority: high
ms.topic: overview
ms.date: 03/21/2022
zone_pivot_groups: teams-app-platform
ms.openlocfilehash: d2bdebcb75a9e145cbb874b2d7cc263c78050e07
ms.sourcegitcommit: dccb48902e08484692ab927415bcd3d61dc50db2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2022
ms.locfileid: "67806925"
---
# <a name="debug-your-teams-app"></a>Depurar la aplicación de Teams


Teams Toolkit le ayuda a depurar y obtener una vista previa de la aplicación de Microsoft Teams. La depuración es el proceso de comprobar, detectar y corregir problemas o errores para garantizar que el programa se ejecuta correctamente en Teams.

::: zone pivot="visual-studio-code"

## <a name="debug-your-teams-app-for-visual-studio-code"></a>Depuración de la aplicación de Teams para Visual Studio Code

Teams Toolkit en Microsoft Visual Studio Code automatiza el proceso de depuración. Puede detectar errores y corregirlos, así como obtener una vista previa de la aplicación teams. También puede personalizar la configuración de depuración para crear la pestaña o el bot.

## <a name="debug-your-microsoft-teams-app-for-visual-studio-code"></a>Depuración de la aplicación de Microsoft Teams para Visual Studio Code

Teams Toolkit en Visual Studio Code automatiza el proceso de depuración. Puede detectar errores y corregirlos, así como obtener una vista previa de la aplicación teams. También puede personalizar la configuración de depuración para crear la pestaña o el bot.

Durante el proceso de depuración:

* El kit de herramientas de Teams inicia automáticamente los servicios de aplicaciones, inicia depuradores y descarga localmente la aplicación de Teams.
* Teams Toolkit comprueba los requisitos previos durante el proceso en segundo plano de depuración.
* La aplicación de Teams está disponible para la versión preliminar en el cliente web de Teams localmente después de la depuración.
* También puede personalizar la configuración de depuración para usar los puntos de conexión del bot, el certificado de desarrollo o el componente parcial de depuración para cargar la aplicación configurada.
* Microsoft Visual Studio Code permite depurar pestañas, bots, extensiones de mensajes y Azure Functions.

## <a name="key-debug-features-of-teams-toolkit"></a>Características clave de depuración del kit de herramientas de Teams

El kit de herramientas de Teams admite las siguientes características de depuración:

* [Iniciar depuración](#start-debugging)
* [ Depuración de varios destinos](#multi-target-debugging)
* [Alternar puntos de interrupción](#toggle-breakpoints)
* [Recarga activa ](#hot-reload)
* [Detener depuración](#stop-debugging)

Teams Toolkit realiza funciones en segundo plano durante el proceso de depuración, que incluyen la comprobación de los requisitos previos necesarios para la depuración. Puede ver el progreso del proceso de verificación en el canal de salida del kit de herramientas de Teams. En el proceso de instalación, puede registrar y configurar la aplicación de Teams.

### <a name="start-debugging"></a>Iniciar depuración

Puede presionar **F5** como una sola operación para iniciar la depuración. El kit de herramientas de Teams empieza a comprobar los requisitos previos, registra la aplicación de Azure AD, la aplicación de Teams, registra el bot, inicia los servicios e inicia el explorador.

### <a name="multi-target-debugging"></a>Depuración de varios destinos

El kit de herramientas de Teams usa la característica de depuración de varios destinos para depurar pestañas, bots, extensiones de mensajes y Azure Functions al mismo tiempo.

### <a name="toggle-breakpoints"></a>Alternar puntos de interrupción

Los puntos de interrupción en los códigos fuente de pestañas, bots, extensiones de mensajes y Azure Functions se pueden alternar. Los puntos de interrupción se ejecutan al interactuar con la aplicación Teams en un explorador web. En la imagen siguiente se muestra el punto de interrupción de alternancia:

   :::image type="content" source="../assets/images/teams-toolkit-v2/debug/toggle-points.png" alt-text="Alternar puntos de interrupción" lightbox="../assets/images/teams-toolkit-v2/debug/toggle-points.png":::

### <a name="hot-reload"></a>Recarga activa

Puede actualizar y guardar los códigos de origen de tabulación, bot, extensión de mensaje y Azure Functions al mismo tiempo que depura la aplicación teams. La aplicación se vuelve a cargar y el depurador vuelve a asociarse a los lenguajes de programación.

   :::image type="content" source="../assets/images/teams-toolkit-v2/debug/hot-reload.png" alt-text="recarga activa para códigos fuente" lightbox="../assets/images/teams-toolkit-v2/debug/hot-reload.png":::

### <a name="stop-debugging"></a>Detener depuración

Cuando complete la depuración local, puede seleccionar **Detener (Mayús+F5)** o **[Alt] Desconectar (Mayús+F5)** en la barra de herramientas de depuración flotante para detener todas las sesiones de depuración y finalizar las tareas. En la imagen siguiente se muestra la acción de detención de depuración:

   :::image type="content" source="../assets/images/teams-toolkit-v2/debug/stop-debug.png" alt-text="detener depuración":::

## <a name="prepare-for-debug"></a>Preparación para la depuración

Los pasos siguientes le ayudan a prepararse para la depuración:

### <a name="sign-in-to-microsoft-365"></a>Iniciar sesión en Microsoft 365

Si ya se ha registrado en Microsoft 365, inicie sesión en Microsoft 365. Para obtener más información, consulte [Programa para desarrolladores de Microsoft 365](tools-prerequisites.md#microsoft-365-developer-program).

### <a name="toggle-breakpoints"></a>Alternar puntos de interrupción

Asegúrese de que puede alternar los puntos de interrupción en los códigos de origen de pestañas, bots, extensiones de mensaje y Azure Functions para obtener más información, consulte [Alternar puntos de interrupción](#toggle-breakpoints).

## <a name="customize-debug-settings"></a>Personalizar la configuración de depuración

El kit de herramientas de Teams desactiva algunos requisitos previos y le permite personalizar la configuración de depuración para crear su pestaña o bot:

<br>

<details>
<summary><b>Use el punto de conexión del bot</b></summary>

1. En Visual Studio Code configuración, debe desactivar **Asegurarse de que Ngrok está instalado e iniciado (ngrok)**.

1. Puede establecer la `siteEndpoint` configuración en en el `.fx/configs/config.local.json` punto de conexión.

```json
{
    "bot": {
        "siteEndpoint": "https://your-bot-tunneling-url"
    }
}

```

:::image type="content" source="../assets/images/teams-toolkit-v2/debug/bot-endpoint.png" alt-text="Personalice el punto de conexión del bot":::

</details>

<details>
<summary><b>Use el certificado de desarrollo</b></summary>

1. En Visual Studio Code configuración, debe desactivar **Asegurarse de que el certificado de desarrollo es de confianza (devCert).**

1. Puede establecer `sslCertFile` y `sslKeyFile` configurar en la ruta de acceso del archivo de certificado y la `.fx/configs/config.local.json` ruta de acceso del archivo de clave.

```json
{
    "frontend": {
        "sslCertFile": "",
        "sslKeyFile": ""
    }
}
```

:::image type="content" source="../assets/images/teams-toolkit-v2/debug/development-certificate-customize.png" alt-text="Personalizar el certificado":::

</details>

<details>
<summary><b>Use los scripts de inicio para iniciar los servicios de aplicaciones</b></summary>

1. Para la pestaña , debe actualizar `dev:teamsfx` el script en `tabs/package.json`.

1. Para la extensión de bot o mensaje, debe actualizar `dev:teamsfx` el script en `bot/package.json`.

1. Para Azure Functions, debe actualizar `dev:teamsfx` el script en `api/package.json` y para el script de actualización `watch:teamsfx` de TypeScript.

   > [!NOTE]
   > Actualmente, la pestaña, el bot, las aplicaciones de extensión de mensajes y los puertos de Azure Functions no admiten la personalización.

</details>

<details>
<summary><b>Agregar variables de entorno</b></summary>

Puede agregar variables de entorno al archivo `.env.teamsfx.local` para pestañas, bots, extensión de mensajes y Azure Functions. Teams Toolkit carga las variables de entorno que agregó para iniciar los servicios durante la depuración local.

 > [!NOTE]
 > Asegúrese de iniciar una nueva depuración local después de agregar nuevas variables de entorno, ya que las variables de entorno no admiten la recarga activa.

</details>

<details>
<summary><b>Depurar componente parcial</b></summary>

El kit de herramientas de Teams usa la depuración de varios destinos de Visual Studio Code para depurar la pestaña, el bot, la extensión de mensajes y Azure Functions al mismo tiempo. Puede actualizar `.vscode/launch.json` y `.vscode/tasks.json` para depurar componentes parciales. Si desea depurar la pestaña solo en una pestaña más un bot con un proyecto de Azure Functions, siga estos pasos:

1. Comentario **`Attach to Bot`** y **`Attach to Backend`** del compuesto de depuración en `.vscode/launch.json`.

   ```json
   {
       "name": "Debug (Edge)",
        "configurations": [
           "Attach to Frontend (Edge)",
           // "Attach to Bot",
           // "Attach to Backend""
           ],
           "preLaunchTask": "Pre Debug Check & Start All",
           "presentation": {
               "group": "all",
               "order": 1
           },
           "stopAll": true

   }
   ```

2. Comentario **`Start Backend`** e inicio del bot desde la tarea Iniciar todo en .vscode/tasks.json.

   ```json
   {
                                           
       "label": "Start All",
       "dependsOn": [
           "Start Frontend",
             // "Start Backend",
             // "Start Bot"

         ]
              
   }
   ```

</details>

## <a name="next"></a>Next

> [!div class="nextstepaction"]
> [Depurar la aplicación localmente](debug-local.md)

::: zone-end

::: zone pivot="visual-studio"

## <a name="debug-your-teams-app-using-visual-studio"></a>Depuración de la aplicación de Teams mediante Visual Studio

Teams Toolkit automatiza los servicios de inicio de aplicaciones, inicia la depuración y carga en paralelo la aplicación de Teams. Después de la depuración, puede obtener una vista previa de la aplicación de Teams en el cliente web de Teams. También puede personalizar la configuración de depuración para usar los puntos de conexión del bot o las variables de entorno para cargar la aplicación configurada. Visual Studio permite depurar la pestaña, el bot y la extensión de mensaje. Durante el proceso de depuración, Teams Toolkit admite las siguientes características de depuración:

* Preparación de dependencias de aplicaciones de Teams
* Iniciar depuración
* Alternar puntos de interrupción
* Recarga activa
* Detener depuración

## <a name="prerequisites"></a>Requisitos previos

| &nbsp; | Instalar | Para usar... |
| --- | --- | --- |
| &nbsp; | **Required** | &nbsp; |
| &nbsp; | Visual Studio 2022, versión 17.3 | Puede instalar la edición enterprise de Visual Studio e instalar la carga de trabajo "ASP.NET" y las herramientas de desarrollo de Microsoft Teams. |
| &nbsp; | Kit de herramientas de Teams | Extensión de Visual Studio que crea un scaffolding de proyecto para la aplicación. Use la versión más reciente. |
| &nbsp; | [Microsoft Teams](https://www.microsoft.com/microsoft-teams/download-app) | Microsoft Teams para colaborar con todos los usuarios con los que trabaja a través de aplicaciones de chat, reuniones, llamadas, todo en un solo lugar. |
| &nbsp; | [Preparar el espacio empresarial de Microsoft 365](../concepts/build-and-test/prepare-your-o365-tenant.md) | Acceso a la cuenta de Teams con los permisos adecuados para instalar una aplicación. |
| &nbsp; | [Cuenta de desarrollador de Microsoft 365](/../concepts/build-and-test/prepare-your-o365-tenant) | Acceso a la cuenta de Teams con los permisos adecuados para instalar una aplicación. |
| &nbsp; | Herramientas de Azure y [cli de Microsoft Azure](/cli/azure/install-azure-cli) | Herramientas de Azure para acceder a datos almacenados o para implementar un back-end basado en la nube para la aplicación de Teams en Azure. |
|&nbsp;  | **Optional** | &nbsp; |
|&nbsp; |[Ngrok](https://ngrok.com/) | Ngrok se usa para reenviar mensajes externos de Azure Bot Framework a la máquina local.|

## <a name="key-features-of-teams-toolkit"></a>Características clave del kit de herramientas de Teams

Puede ver las siguientes características clave del kit de herramientas de Teams, que automatizan el proceso de depuración local de la aplicación de Teams:

### <a name="prepare-teams-app-dependencies"></a>Preparación de dependencias de aplicaciones de Teams

Teams Toolkit prepara dependencias de depuración local y registra la aplicación de Teams en el inquilino de la cuenta. En el caso de las aplicaciones bot y extensión de mensajes, Teams Toolkit registrará y configurará el bot.

### <a name="start-debugging"></a>Iniciar depuración

Puede realizar la depuración con una sola operación, presione **F5** para iniciar la depuración. Teams Toolkit compila código, inicia servicios y la aplicación se inicia en el explorador.

### <a name="toggle-breakpoints"></a>Alternar puntos de interrupción

Puede alternar los puntos de interrupción en los códigos de origen de pestañas, bots, extensiones de mensaje y funciones de Azure. Los puntos de interrupción se ejecutan al interactuar con la aplicación Teams en el explorador web.
En la imagen siguiente se muestran la alternancia de puntos de interrupción:

:::image type="content" source="../assets/images/debug-teams-app/vs-localdebug-toggle-breakpoint.png" alt-text="Puntos de interrupción de alternancia de depuración local" lightbox="../assets/images/debug-teams-app/vs-localdebug-toggle-breakpoint.png":::

### <a name="hot-reload"></a>Recarga activa

Seleccione **Recarga activa** para aplicar los cambios en la aplicación de Teams cuando quiera actualizar y guardar los códigos de origen simultáneamente durante la depuración.

:::image type="content" source="../assets/images/debug-teams-app/vs-localdebug-hot-reload.png" alt-text="Seleccionar icono de recarga activa":::

Seleccione la opción **Recarga activa en Guardar** archivo en la lista desplegable para habilitar la recarga activa automática.

:::image type="content" source="../assets/images/debug-teams-app/vs-localdebug-hot-reload-filesave.png" alt-text="Seleccionar recarga activa al guardar archivos":::
  
   > [!Tip]
   > Para obtener más información sobre Recarga activa función de Visual Studio durante la depuración, puede visitar <https://aka.ms/teamsfx-vs-hotreload>.

### <a name="stop-debugging"></a>Detener depuración

Seleccione **Detener depuración** cuando se complete la depuración local.

:::image type="content" source="../assets/images/debug-teams-app/vs-localdebug-Stopdebug.png" alt-text="Seleccionar icono de detención de depuración":::

## <a name="customize-debug-settings"></a>Personalizar la configuración de depuración

Puede personalizar la configuración de depuración para que la aplicación de Teams use los puntos de conexión del bot y agregue variables de entorno:

### <a name="use-your-bot-endpoint"></a>Use el punto de conexión del bot

Puede establecer la configuración de siteEndpoint en **.fx/configs/config.local.json** en el punto de conexión.

```
"bot": {
    "siteEndpoint": "https://baidu.com"
}
```

### <a name="add-environment-variables"></a>Agregar variables de entorno

Puede agregar **environmentVariables** al archivo **launchSettings.json** .

:::image type="content" source="../assets/images/debug-teams-app/vs-localdebug-environment-variables.png" alt-text="Adición de variables de entorno personalizadas":::

### <a name="launch-teams-app-as-a-web-app"></a>Inicio de una aplicación de Teams como una aplicación web

Puede iniciar la aplicación de Teams como una aplicación web en lugar de ejecutarse en el cliente de Teams.

1. Seleccione **Propiedades** > **launchSettings.json** en Explorador de soluciones panel del proyecto.
1. Quite " **launchUrl"** del archivo.

   :::image type="content" source="../assets/images/debug-teams-app/vs-localdebug-launch-teamsapp-webapp.png" alt-text="Iniciar equipos como una aplicación web mediante la eliminación de launchurl" lightbox="../assets/images/debug-teams-app/vs-localdebug-launch-teamsapp-webapp.png":::

1. Haga clic con el botón derecho en **Solución** y seleccione **Propiedades**.

   :::image type="content" source="../assets/images/debug-teams-app/vs-localdebug-solution-properties.png" alt-text="Haga clic con el botón derecho en la solución y seleccione las propiedades" lightbox="../assets/images/debug-teams-app/vs-localdebug-solution-properties.png":::

1. Seleccione **Configuración de propiedades** > **de** configuración en el cuadro de diálogo.
1. Desactive la casilla **Implementar** .
1. Seleccione **Aceptar**.

   :::image type="content" source="../assets/images/debug-teams-app/vs-localdebug-disable-deploy.png" alt-text="Desactivar la implementación en las propiedades de configuración" lightbox="../assets/images/debug-teams-app/vs-localdebug-disable-deploy.png":::

::: zone-end

## <a name="see-also"></a>Vea también

* [Depurar procesos en segundo plano](debug-background-process.md)
* [usar el kit de herramientas de Teams para aprovisionar recursos en la nube](provision.md)
* [Implementar en la nube](deploy.md)
* [Vista previa y personalización del manifiesto de la aplicación de Teams](TeamsFx-preview-and-customize-app-manifest.md)