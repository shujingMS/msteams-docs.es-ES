---
title: Enviar y recibir archivos a través del bot
description: Describe cómo enviar y recibir archivos a través del bot
keywords: recibir los archivos de bots de teams
ms.date: 05/20/2019
ms.topic: how-to
ms.openlocfilehash: 1699b9339bd6a49194240130d16795e8febcb76e
ms.sourcegitcommit: fa64b83c0b534bf7a89f256880d5b5ca193e4b04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2021
ms.locfileid: "50037059"
---
# <a name="send-and-receive-files-through-the-bot"></a>Enviar y recibir archivos a través del bot

> [!IMPORTANT]
> Los artículos de este documento se basan en el SDK de Bot Framework v4.

Hay dos formas de enviar y recibir archivos desde un bot:

* **Uso de las API de Microsoft Graph:** Este método funciona para bots en todos los ámbitos de Microsoft Teams:
  * `personal`
  * `channel`
  * `groupchat`

* **Uso de las API de bot de Teams:** Estos solo admiten archivos en `personal` contexto.

## <a name="using-the-graph-apis"></a>Uso de las API de Graph

Publique mensajes con datos adjuntos de tarjeta que hacen referencia a archivos de SharePoint existentes, con las API de Graph para [OneDrive y SharePoint.](/onedrive/developer/rest-api/) Para usar las API de Graph, obtenga acceso a cualquiera de las siguientes opciones a través del flujo de autorización estándar de OAuth 2.0:
* Carpeta de OneDrive de un usuario y `personal` `groupchat` archivos.
* Los archivos del canal de un equipo para `channel` los archivos.

Las API de Graph funcionan en todos los ámbitos de Teams.

## <a name="using-the-teams-bot-apis"></a>Uso de las API de bot de Teams

> [!NOTE]
> Las API de bot de Teams solo funcionan en el `personal` contexto. No funcionan en el `channel` contexto o en el `groupchat` contexto.

Con las API de Teams, el bot puede enviar y recibir directamente archivos con usuarios en el contexto, también conocido `personal` como chats personales. Implementar características, como informes de gastos, reconocimiento de imágenes, archivado de archivos y firmas electrónicas que impliquen la edición del contenido del archivo. Los archivos compartidos en Teams suelen aparecer como tarjetas y permiten la visualización enriquezca en la aplicación.

En las secciones siguientes se describe cómo enviar contenido de archivos como una interacción directa del usuario, como enviar un mensaje. Esta API se proporciona como parte de la plataforma de bots de Teams.

### <a name="configuring-the-bot-to-support-files"></a>Configuración del bot para admitir archivos

Para enviar y recibir archivos en el bot, establezca la `supportsFiles` propiedad en el manifiesto en `true` . Esta propiedad se describe en la [sección bots](~/resources/schema/manifest-schema.md#bots) de la referencia de manifiesto.

La definición tiene este `"supportsFiles": true` aspecto. Si el bot no lo habilita, las características que se `supportsFiles` enumeran en esta sección no funcionan.

### <a name="receiving-files-in-personal-chat"></a>Recepción de archivos en chat personal

Cuando un usuario envía un archivo al bot, el archivo se carga primero en el almacenamiento de OneDrive para la Empresa del usuario. A continuación, el bot recibe una actividad de mensaje que notifica al usuario sobre la carga del usuario. La actividad contiene metadatos de archivo, como su nombre y la dirección URL de contenido. El usuario puede leer directamente desde esta dirección URL para capturar su contenido binario.

#### <a name="message-activity-with-file-attachment-example"></a>Ejemplo de actividad de mensajes con datos adjuntos de archivo

```json
{
  "attachments": [{
    "contentType": "application/vnd.microsoft.teams.file.download.info",
    "contentUrl": "https://contoso.sharepoint.com/personal/johnadams_contoso_com/Documents/Applications/file_example.txt",
    "name": "file_example.txt",
    "content": {
      "downloadUrl" : "https://download.link",
      "uniqueId": "1150D938-8870-4044-9F2C-5BBDEBA70C9D",
      "fileType": "txt",
      "etag": "123"
    }
  }]
}
```

En la tabla siguiente se describen las propiedades de contenido de los datos adjuntos:

| Propiedad | Finalidad |
| --- | --- |
| `downloadUrl` | Dirección URL de OneDrive para capturar el contenido del archivo. El usuario puede emitir una `HTTP GET` directamente desde esta dirección URL. |
| `uniqueId` | Identificador de archivo único. Este es el identificador de elemento de unidad de OneDrive, en caso de que el usuario envíe un archivo al bot. |
| `fileType` | Tipo de archivo, como .pdf o .docx. |

Como procedimiento recomendado, confirme la carga de archivos enviando un mensaje al usuario.

### <a name="uploading-files-to-personal-chat"></a>Cargar archivos en un chat personal

Se requieren los siguientes pasos para cargar un archivo a un usuario:

1. Envíe un mensaje al usuario que solicita permiso para escribir el archivo. Este mensaje debe contener datos `FileConsentCard` adjuntos con el nombre del archivo que se va a cargar.
2. Si el usuario acepta la descarga de archivos, el bot recibe una actividad de invocación con una dirección URL de ubicación.
3. Para transferir el archivo, el bot realiza una operación `HTTP POST` directamente en la dirección URL de ubicación proporcionada.
4. Opcionalmente, quite la tarjeta de consentimiento original si no desea que el usuario acepte más cargas del mismo archivo.

#### <a name="message-requesting-permission-to-upload"></a>Mensaje que solicita permiso para cargar

El siguiente mensaje de escritorio contiene un objeto de datos adjuntos sencillo que solicita permiso de usuario para cargar el archivo:

![Tarjeta de consentimiento que solicita permiso de usuario para cargar archivo](../../assets/images/bots/bot-file-consent-card.png)

El siguiente mensaje móvil contiene un objeto de datos adjuntos que solicita permiso de usuario para cargar el archivo:

![Tarjeta de consentimiento que solicita permiso de usuario para cargar un archivo en un dispositivo móvil](../../assets/images/bots/mobile-bot-file-consent-card.png)

```json
{
  "attachments": [{
    "contentType": "application/vnd.microsoft.teams.card.file.consent",
    "name": "file_example.txt",
    "content": {
      "description": "<Purpose of the file, such as: this is your monthly expense report>",
      "sizeInBytes": 1029393,
      "acceptContext": {
      },
      "declineContext": {
      }
    }
  }]
}
```

En la tabla siguiente se describen las propiedades de contenido de los datos adjuntos:

| Propiedad | Finalidad |
| --- | --- |
| `description` | Describe el propósito del archivo o resume su contenido. |
| `sizeInBytes` | Proporciona al usuario una estimación del tamaño del archivo y la cantidad de espacio que ocupa en OneDrive. |
| `acceptContext` | Contexto adicional que se transmite silenciosamente al bot cuando el usuario acepta el archivo. |
| `declineContext` | Contexto adicional que se transmite silenciosamente al bot cuando el usuario rechaza el archivo. |

#### <a name="invoke-activity-when-the-user-accepts-the-file"></a>Invocar actividad cuando el usuario acepta el archivo

Se envía una actividad de invocación al bot si el usuario acepta el archivo y cuándo lo acepta. Contiene la dirección URL de marcador de posición de OneDrive para la Empresa en la que el bot puede emitir una entrada `PUT` para transferir el contenido del archivo. Para obtener información sobre cómo cargar en la dirección URL de OneDrive, vea [Cargar bytes en la sesión de carga.](/onedrive/developer/rest-api/api/driveitem_createuploadsession#upload-bytes-to-the-upload-session)

En el siguiente ejemplo se muestra una versión concisa de la actividad de invocación que recibe el bot:

```json
{
  "name": "fileConsent/invoke",
  "value": {
    "type": "fileUpload",
    "action": "accept",
    "context": {
    },
    "uploadInfo": {
      "contentUrl": "https://contoso.sharepoint.com/personal/johnadams_contoso_com/Documents/Applications/file_example.txt",
      "name": "file_example.txt",
      "uploadUrl": "https://upload.link",
      "uniqueId": "1150D938-8870-4044-9F2C-5BBDEBA70C8C",
      "fileType": "txt",
      "etag": "123"
    }
  }
}
```

De forma similar, si el usuario rechaza el archivo, el bot recibe el siguiente evento con el mismo nombre de actividad general:

```json
{
  "name": "fileConsent/invoke",
  "value": {
    "type": "fileUpload",
    "action": "decline",
    "context": {
    }
  }
}
```

### <a name="notifying-the-user-about-an-uploaded-file"></a>Notificar al usuario sobre un archivo cargado

Después de cargar un archivo en el OneDrive del usuario, envíe un mensaje de confirmación al usuario. El mensaje debe contener los siguientes datos adjuntos que el usuario puede seleccionar, ya sea para obtener una vista previa o abrirlo en `FileCard` OneDrive, o descargar localmente:

```json
{
  "attachments": [{
    "contentType": "application/vnd.microsoft.teams.card.file.info",
    "contentUrl": "https://contoso.sharepoint.com/personal/johnadams_contoso_com/Documents/Applications/file_example.txt",
    "name": "file_example.txt",
    "content": {
      "uniqueId": "1150D938-8870-4044-9F2C-5BBDEBA70C8C",
      "fileType": "txt",
    }
  }]
}
```

En la tabla siguiente se describen las propiedades de contenido de los datos adjuntos:

| Propiedad | Finalidad |
| --- | --- |
| `uniqueId` | Id. de elemento de unidad de OneDrive o SharePoint. |
| `fileType` | Tipo de archivo, como .pdf o .docx. |

### <a name="basic-example-in-c"></a>Ejemplo básico en C #

En el ejemplo siguiente se muestra cómo controlar las cargas de archivos y enviar solicitudes de consentimiento de archivos en el cuadro de diálogo del bot:

```csharp

protected override async Task OnMessageActivityAsync(ITurnContext<IMessageActivity> turnContext, CancellationToken cancellationToken)
{
    string filename = "teams-logo.png";
    string filePath = Path.Combine("Files", filename);
    long fileSize = new FileInfo(filePath).Length;
    await SendFileCardAsync(turnContext, filename, fileSize, cancellationToken);
}

private async Task SendFileCardAsync(ITurnContext turnContext, string filename, long filesize, CancellationToken cancellationToken)
{
    var consentContext = new Dictionary<string, string>
    {
        { "filename", filename 
        },
    };

    var fileCard = new FileConsentCard
    {
        Description = "This is the file I want to send you",
        SizeInBytes = filesize,
        AcceptContext = consentContext,
        DeclineContext = consentContext,
    };

    var asAttachment = new Attachment
    {
        Content = fileCard,
        ContentType = FileConsentCard.ContentType,
        Name = filename,
    };

    var replyActivity = turnContext.Activity.CreateReply();
    replyActivity.Attachments = new List<Attachment>() { asAttachment 
    };
    await turnContext.SendActivityAsync(replyActivity, cancellationToken);
}

protected override async Task OnTeamsFileConsentAcceptAsync(ITurnContext<IInvokeActivity> turnContext, FileConsentCardResponse fileConsentCardResponse, CancellationToken cancellationToken)
{
    try
    {
        JToken context = JObject.FromObject(fileConsentCardResponse.Context);

        string filePath = Path.Combine("Files", context["filename"].ToString());
        long fileSize = new FileInfo(filePath).Length;
        var client = _clientFactory.CreateClient();
        using (var fileStream = File.OpenRead(filePath))
        {
            var fileContent = new StreamContent(fileStream);
            fileContent.Headers.ContentLength = fileSize;
            fileContent.Headers.ContentRange = new ContentRangeHeaderValue(0, fileSize - 1, fileSize);
            await client.PutAsync(fileConsentCardResponse.UploadInfo.UploadUrl, fileContent, cancellationToken);
        }

        await FileUploadCompletedAsync(turnContext, fileConsentCardResponse, cancellationToken);
    }
    catch (Exception e)
    {
        await FileUploadFailedAsync(turnContext, e.ToString(), cancellationToken);
    }
}

protected override async Task OnTeamsFileConsentDeclineAsync(ITurnContext<IInvokeActivity> turnContext, FileConsentCardResponse fileConsentCardResponse, CancellationToken cancellationToken)
{
    JToken context = JObject.FromObject(fileConsentCardResponse.Context);

    var reply = MessageFactory.Text($"Declined. We won't upload file <b>{context["filename"]}</b>.");
    reply.TextFormat = "xml";
    await turnContext.SendActivityAsync(reply, cancellationToken);
}

private async Task FileUploadCompletedAsync(ITurnContext turnContext, FileConsentCardResponse fileConsentCardResponse, CancellationToken cancellationToken)
{
    var downloadCard = new FileInfoCard
    {
        UniqueId = fileConsentCardResponse.UploadInfo.UniqueId,
        FileType = fileConsentCardResponse.UploadInfo.FileType,
    };

    var asAttachment = new Attachment
    {
        Content = downloadCard,
        ContentType = FileInfoCard.ContentType,
        Name = fileConsentCardResponse.UploadInfo.Name,
        ContentUrl = fileConsentCardResponse.UploadInfo.ContentUrl,
    };

    var reply = MessageFactory.Text($"<b>File uploaded.</b> Your file <b>{fileConsentCardResponse.UploadInfo.Name}</b> is ready to download");
    reply.TextFormat = "xml";
    reply.Attachments = new List<Attachment> { asAttachment 
    };

    await turnContext.SendActivityAsync(reply, cancellationToken);
}

private async Task FileUploadFailedAsync(ITurnContext turnContext, string error, CancellationToken cancellationToken)
{
    var reply = MessageFactory.Text($"<b>File upload failed.</b> Error: <pre>{error}</pre>");
    reply.TextFormat = "xml";
    await turnContext.SendActivityAsync(reply, cancellationToken);
}
```