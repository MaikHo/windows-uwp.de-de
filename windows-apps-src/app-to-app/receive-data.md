---
description: In diesem Artikel wird erläutert, wie Sie in Ihrer UWP-App (Universelle Windows-Plattform) Inhalte empfangen, die in einer anderen App mithilfe des Freigabe-Vertrags freigegeben wurden. Mit diesem Freigabe-Vertrag kann Ihre App als Option angezeigt werden, wenn der Benutzer „Freigeben“ aufruft.
title: Empfangen von Daten
ms.assetid: 0AFF9E0D-DFF4-4018-B393-A26B11AFDB41
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2bf345f3b8f72043c72f47d681aa3ed174eb0dc5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175744"
---
# <a name="receive-data"></a>Empfangen von Daten



In diesem Artikel wird erläutert, wie Sie in Ihrer UWP-App (Universelle Windows-Plattform) Inhalte empfangen, die in einer anderen App mithilfe des Freigabe-Vertrags freigegeben wurden. Mit diesem Freigabe-Vertrag kann Ihre App als Option angezeigt werden, wenn der Benutzer „Freigeben“ aufruft.

## <a name="declare-your-app-as-a-share-target"></a>Deklarieren der App als Freigabeziel

Das System zeigt eine Liste der möglichen Ziel-Apps, wenn ein Benutzer „Freigeben“ aufruft. Damit sie in der Liste angezeigt wird, muss Ihre App deklarieren, dass sie den Freigabe-Vertrag unterstützt. Dies informiert das System darüber, dass Ihre App für das Empfangen von Inhalten zur Verfügung steht.

1.  Öffnen Sie die Manifestdatei. Ihr Name lautet in etwa **package.appxmanifest**.
2.  Öffnen Sie die Registerkarte **Deklarationen**.
3.  Wählen Sie in der Liste **Verfügbare Deklarationen** die Option **Ziel freigeben** aus, und klicken Sie auf **Hinzufügen**.

## <a name="choose-file-types-and-formats"></a>Auswählen von Dateitypen und Formaten

Als Nächstes müssen Sie entscheiden, welche Dateitypen und Datenformate Sie unterstützen. Die Freigabe-APIs unterstützen verschiedene Standardformate wie Text, HTML und Bitmaps. Sie können auch benutzerdefinierte Dateitypen und Datenformate angeben. Denken Sie in solchen Fällen jedoch daran, dass die Quell-Apps wissen müssen, welcher Art diese Typen und Formate sind, da sie diese andernfalls nicht für das Freigeben von Daten verwenden können.

Führen Sie die Registrierung nur für Formate durch, die für Ihre App geeignet sind. Wenn der Benutzer „Freigeben“ aufruft, werden nur Ziel-Apps angezeigt, die die freigegebenen Daten auch unterstützen.

So legen Sie Dateitypen fest

1.  Öffnen Sie die Manifestdatei. Ihr Name lautet in etwa **package.appxmanifest**.
2.  Klicken Sie im Abschnitt **Unterstützte Dateitypen** der Seite **Deklarationen** auf **Neue Hinzufügen**.
3.  Geben Sie die zu unterstützenden Dateierweiterungen ein, beispielsweise „.docx“, Sie müssen den Punkt angeben. Wenn Sie alle Dateitypen unterstützen möchten, aktivieren Sie das Kontrollkästchen **SupportsAnyFileType**.

So richten Sie Datenformate ein

1.  Öffnen Sie die Manifestdatei.
2.  Öffnen Sie den Abschnitt **Datenformate** der Seite **Deklarationen**, und klicken Sie auf **Neue Hinzufügen**.
3.  Geben Sie den Namen des Datenformats ein, das Sie unterstützen, zum Beispiel „Text“.

## <a name="handle-share-activation"></a>Handhabung der Freigabeaktivierung

Wenn ein Benutzer Ihre App auswählt (i. d. R. durch die Auswahl aus einer Liste verfügbarer Ziel-Apps auf der Benutzeroberfläche für das Freigeben), wird ein [**OnShareTargetActivated**](/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_)-Ereignis ausgelöst. Ihre App muss dieses Ereignis behandeln, um die Daten, die der Benutzer freigeben möchte, verarbeiten zu können.

<!-- For some reason, the snippets in this file are all inline in the WDCML topic. Suggest moving to VS project with rest of snippets. -->
```cs
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    // Code to handle activation goes here. 
} 
```

Die Daten, die ein Benutzer freigeben möchte, sind in einem [**ShareOperation**](/uwp/api/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation)-Objekt enthalten. Mithilfe dieses Objekts können Sie das Format der enthaltenen Daten ermitteln.

```cs
ShareOperation shareOperation = args.ShareOperation;
if (shareOperation.Data.Contains(StandardDataFormats.Text))
{
    string text = await shareOperation.Data.GetTextAsync();

    // To output the text from this example, you need a TextBlock control
    // with a name of "sharedContent".
    sharedContent.Text = "Text: " + text;
} 
```

## <a name="report-sharing-status"></a>Bericht über den Freigabestatus

In einigen Fällen kann es eine Weile dauern, bis Ihre App die freizugebenden Daten verarbeitet hat. Beispiele umfassen die Freigabe von Datei- oder Bildsammlungen. Diese Elemente sind größer als einfache Textzeichenfolgen, sodass auch ihre Verarbeitung länger dauert.

```cs
shareOperation.ReportStarted(); 
```

Erwarten Sie nach dem Aufrufen von [**ReportStarted**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted) keine Benutzerinteraktion mit der App mehr. Führen Sie diesen Aufruf daher nur durch, wenn Ihre App vom Benutzer geschlossen werden kann.

Bei einer erweiterten Freigabe ist es möglich, dass der Benutzer die Quell-App schließt, bevor Ihre App alle Daten aus dem DataPackage-Objekt abgerufen hat. Es wird daher empfohlen, dass Sie das System informieren, wenn Ihre App die erforderlichen Daten erhalten hat. So kann das System die Quell-App ggf. anhalten oder beenden.

```cs
shareOperation.ReportSubmittedBackgroundTask(); 
```

Rufen Sie [**ReportError**](/uwp/api/Windows.ApplicationModel.DataTransfer.ShareTarget.ShareOperation#Windows_ApplicationModel_DataTransfer_ShareTarget_ShareOperation_ReportError_System_String_) auf, falls ein Fehler auftritt, um eine Fehlermeldung an das System zu senden. Die Meldung wird angezeigt, wenn der Benutzer den Status der Freigabe überprüft. An dieser Stelle wird die App heruntergefahren und die Freigabe beendet. Der Benutzer muss zum Freigeben des Inhalts für die App von vorn beginnen. In Abhängigkeit vom Szenario können Sie entscheiden, dass ein bestimmter Fehler nicht schwerwiegend genug ist, um den Freigabevorgang zu beenden. In diesem Fall können Sie sich gegen das Aufrufen von **ReportError** entscheiden und mit dem Freigeben fortfahren.

```cs
shareOperation.ReportError("Could not reach the server! Try again later."); 
```

Rufen Sie zum Abschluss der erfolgreichen Verarbeitung der freigegebenen Daten durch Ihre App [**ReportCompleted**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted) auf, um das System zu informieren.

```cs
shareOperation.ReportCompleted();
```

Halten Sie bei Verwendung dieser Methoden unbedingt die angegebene Reihenfolge ein, und rufen Sie die Methoden nicht mehrmals auf. Unter bestimmten Umständen kann [**ReportDataRetrieved**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportdataretrieved) von einer Ziel-App vor [**ReportStarted**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted) aufgerufen werden. Beispielweise können die Daten von der App im Rahmen einer Aufgabe des Aktivierungshandlers aufgerufen werden. **ReportStarted** wird jedoch erst aufgerufen, wenn der Benutzer auf eine **Freigeben**-Schaltfläche klickt.

## <a name="return-a-quicklink-if-sharing-was-successful"></a>Zurückgeben eines QuickLink-Objekts nach erfolgreicher Freigabe

Wählt ein Benutzer Ihre App aus, um Inhalte zu empfangen, sollten Sie einen [**QuickLink**](/uwp/api/Windows.ApplicationModel.DataTransfer.ShareTarget.QuickLink) erstellen. Ein **QuickLink** ähnelt einer Verknüpfung, über die Benutzer leichter Informationen mit Ihrer App austauschen können. Sie könnten beispielsweise einen **QuickLink** erstellen, der eine neue E-Mail erstellt, die bereits die E-Mail-Adresse eines Freundes enthält.

Ein **Quicklink** muss über einen Titel, ein Symbol und eine ID verfügen. Der Titel (z.b. "e-Mail MOM") und das Symbol werden angezeigt, wenn der Benutzer auf den Charm "teilen" tippt. Die ID verwendet Ihre App für den Zugriff auf benutzerdefinierte Informationen wie eine E-Mail-Adresse oder Anmeldeinformationen. Wenn Ihre App einen **QuickLink** erstellt, gibt sie den **QuickLink** an das System zurück, indem sie [**ReportCompleted**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted) aufruft.

Von einem **QuickLink** werden keine Daten gespeichert. Stattdessen enthält er eine ID, die beim Auswählen an die App gesendet wird. Ihre App ist selbst für das Speichern der ID für den **QuickLink** und die damit zusammenhängenden Benutzerdaten verantwortlich. Wenn der Benutzer auf den **QuickLink** tippt, können Sie seine ID über die [**QuickLinkId**](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.quicklinkid)-Eigenschaft abrufen.

```cs
async void ReportCompleted(ShareOperation shareOperation, string quickLinkId, string quickLinkTitle)
{
    QuickLink quickLinkInfo = new QuickLink
    {
        Id = quickLinkId,
        Title = quickLinkTitle,

        // For quicklinks, the supported FileTypes and DataFormats are set 
        // independently from the manifest
        SupportedFileTypes = { "*" },
        SupportedDataFormats = { StandardDataFormats.Text, StandardDataFormats.Uri, 
                StandardDataFormats.Bitmap, StandardDataFormats.StorageItems }
    };

    StorageFile iconFile = await Windows.ApplicationModel.Package.Current.InstalledLocation.CreateFileAsync(
            "assets\\user.png", CreationCollisionOption.OpenIfExists);
    quickLinkInfo.Thumbnail = RandomAccessStreamReference.CreateFromFile(iconFile);
    shareOperation.ReportCompleted(quickLinkInfo);
}
```

## <a name="see-also"></a>Weitere Informationen 

* [App-zu-App-Kommunikation](index.md)
* [Freigeben von Daten](share-data.md)
* [OnShareTargetActivated](/uwp/api/windows.ui.xaml.application.onsharetargetactivated)
* [ReportStarted](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted)
* [ReportError](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reporterror)
* [ReportCompleted](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportcompleted)
* [ReportDataRetrieved](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportdataretrieved)
* [ReportStarted](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation.reportstarted)
* [QuickLink](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.quicklink)
* [QuickLInkId](/uwp/api/windows.applicationmodel.datatransfer.sharetarget.quicklink.id)