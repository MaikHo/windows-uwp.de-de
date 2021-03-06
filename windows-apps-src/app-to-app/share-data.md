---
description: In diesem Artikel wird erläutert, wie der Freigabe-Vertrag in einer UWP-App (Universelle Windows-Plattform) unterstützt wird.
title: Freigeben von Daten
ms.assetid: 32287F5E-EB86-4B98-97FF-8F6228D06782
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4ed74149552e6582bf133550d4db1a45625e8c39
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364043"
---
# <a name="share-data"></a>Freigeben von Daten


In diesem Artikel wird erläutert, wie der Freigabe-Vertrag in einer UWP-App (Universelle Windows-Plattform) unterstützt wird. Der Freigabe-Vertrag ist eine einfache Möglichkeit, Daten wie z. B. Text, Links, Fotos und Videos schnell für andere Apps freizugeben. Ein Benutzer möchte beispielsweise mit einer App für ein soziales Netzwerk eine Webseite mit seinen Freunden teilen, oder er möchte in einer Notiz-App einen Link für eine spätere Verwendung speichern.

> [!NOTE]
> Die Codebeispiele in diesem Artikel sind für UWP-apps geschrieben. WPF, Windows Forms und C++/Win32 Desktop-Apps müssen die [idatatransfermanagerinterop](/windows/win32/api/shobjidl_core/nn-shobjidl_core-idatatransfermanagerinterop) -Schnittstelle verwenden, um das [datatransfermanager](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager) -Objekt für ein bestimmtes Fenster zu erhalten. Weitere Informationen finden Sie im Beispiel zu [sharesource](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/ShareSource) .

## <a name="set-up-an-event-handler"></a>Einrichten eines Ereignishandlers

Fügen Sie einen [**DataRequested**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)-Ereignishandler hinzu, der aufgerufen werden soll, wenn der Benutzer das Teilen-Feature verwendet. Dies kann durch Tippen auf ein Steuerelement in Ihrer App (etwa eine Schaltfläche oder ein App-Leistenbefehl) oder automatisch in einem bestimmten Szenario geschehen (etwa wenn der Benutzer einen Level mit einer hohen Punktzahl abschließt).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetPrepareToShare":::

Wenn ein [**DataRequested**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)-Ereignis eintritt, empfängt Ihre App ein [**DataRequest**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest)-Objekt. Dieses enthält ein [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage), mit dem Sie den Inhalt bereitstellen können, den der Benutzer freigeben möchte. Sie müssen einen Titel und die freizugebenden Daten angeben. Eine Beschreibung ist optional, wird aber empfohlen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetCreateRequest":::

## <a name="choose-data"></a>Wählen von Daten

Sie können verschiedene Arten von Daten freigeben, einschließlich:

-   Nur-Text
-   URIs (Uniform Resource Identifiers)
-   HTML
-   Formatierter Text
-   Bitmaps
-   Dateien
-   Vom Entwickler definierte Daten

Das [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage)-Objekt kann mehrere dieser Formate in beliebiger Kombination enthalten. Im folgenden Beispiel wird das Freigeben von Text veranschaulicht.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetSetContent":::

## <a name="set-properties"></a>Eigenschaften festlegen

Beim Verpacken von Daten für die Freigabe können Sie eine Vielzahl von Eigenschaften angeben, die weitere Informationen zum freigegebenen Inhalt enthalten. Mit diesen Eigenschaften kann die Benutzererfahrung für Ziel-Apps verbessert werden. Die Angabe einer Beschreibung kann beispielsweise nützlich sein, wenn der Benutzer Inhalte für mehrere Apps freigibt. Eine Miniaturansicht beim Freigeben eines Bilds oder eines Links für eine Webseite stellt eine visuelle Referenz für den Benutzer dar. Weitere Informationen finden Sie unter [**DataPackagePropertySet**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet).

Alle Eigenschaften mit Ausnahme des Titels sind optional. Die title-Eigenschaft ist erforderlich und muss festgelegt werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetSetProperties":::

## <a name="launch-the-share-ui"></a>Starten der Benutzeroberfläche für das Freigeben

Eine Benutzeroberfläche für das Freigeben wird vom System bereitgestellt. Zum Starten rufen Sie die [**ShowShareUI**](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)-Methode auf.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/app-to-app/share_data/cs/MainPage.xaml.cs" id="SnippetShowUI":::

## <a name="handle-errors"></a>Behandeln von Fehlern

In den meisten Fällen ist das Freigeben von Inhalten ein einfacher Prozess. Es besteht jedoch immer die Möglichkeit, dass unerwartet ein Problem auftritt. So kann in der App etwa vorgesehen sein, dass der Benutzer etwas zum Teilen auswählt, der Benutzer hat aber nichts ausgewählt. Um diese Situationen zu behandeln, verwenden Sie die [**FailWithDisplayText**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataRequest#Windows_ApplicationModel_DataTransfer_DataRequest_FailWithDisplayText_System_String_)-Methode, die dem Benutzer eine Meldung anzeigt, wenn ein Fehler auftritt.

## <a name="delay-share-with-delegates"></a>Verzögern der Freigabe mit Delegaten

Manchmal ist es nicht sinnvoll, die Daten, die der Benutzer freigeben möchte, direkt vorzubereiten. Wenn Ihre App zum Beispiel das Senden von großen Bilddateien in verschiedenen Formaten unterstützt, ist es ineffizient, alle diese Bilder zu erstellen, bevor der Benutzer seine Auswahl trifft.

Zum Lösen dieses Problems kann ein [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) einen Delegaten enthalten. Dabei handelt es sich um eine Funktion, die aufgerufen wird, wenn die empfangende App Daten anfordert. Es wird empfohlen, einen Delegaten immer dann zu verwenden, wenn die von einem Benutzer freigegebenen Daten ressourcenintensiv sind.

<!-- For some reason, this snippet was inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
async void OnDeferredImageRequestedHandler(DataProviderRequest request)
{
    // Provide updated bitmap data using delayed rendering
    if (this.imageStream != null)
    {
        DataProviderDeferral deferral = request.GetDeferral();
        InMemoryRandomAccessStream inMemoryStream = new InMemoryRandomAccessStream();

        // Decode the image.
        BitmapDecoder imageDecoder = await BitmapDecoder.CreateAsync(this.imageStream);

        // Re-encode the image at 50% width and height.
        BitmapEncoder imageEncoder = await BitmapEncoder.CreateForTranscodingAsync(inMemoryStream, imageDecoder);
        imageEncoder.BitmapTransform.ScaledWidth = (uint)(imageDecoder.OrientedPixelWidth * 0.5);
        imageEncoder.BitmapTransform.ScaledHeight = (uint)(imageDecoder.OrientedPixelHeight * 0.5);
        await imageEncoder.FlushAsync();

        request.SetData(RandomAccessStreamReference.CreateFromStream(inMemoryStream));
        deferral.Complete();
    }
}
```

## <a name="see-also"></a>Siehe auch 

* [App-zu-App-Kommunikation](index.md)
* [Empfangen von Daten](receive-data.md)
* [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackagePropertySet](/uwp/api/windows.applicationmodel.datatransfer.datapackagepropertyset)
* [Datarequest](/uwp/api/windows.applicationmodel.datatransfer.datarequest)
* [DataRequested](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested)
* [FailWithDisplayText](/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
 
