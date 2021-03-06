---
ms.assetid: 40B97E0C-EB1B-40C2-A022-1AB95DFB085E
description: In diesem Artikel wird beschrieben, wie Sie Medien aus einer universellen Windows-App zu Remotegeräten umwandeln.
title: Medienumwandlung
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a23e6c58325a8679a8df2ec0d3f429f75a230602
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363923"
---
# <a name="media-casting"></a>Medienumwandlung



In diesem Artikel wird beschrieben, wie Sie Medien aus einer universellen Windows-App zu Remotegeräten umwandeln.

## <a name="built-in-media-casting-with-mediaplayerelement"></a>Integrierte Medienumwandlung mit MediaPlayerElement

Die einfachste Methode zum Umwandeln von Medien aus einer universellen Windows-App ist die Verwendung der integrierten Umwandlungsfunktion des [**MediaPlayerElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaPlayerElement)-Steuerelements.

Um dem Benutzer das Öffnen einer wiederzugebenden Videodatei im **MediaPlayerElement**-Steuerelement zu ermöglichen, fügen Sie Ihrem Projekt die folgenden Namespaces hinzu.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetBuiltInCastingUsing":::

Fügen Sie in der XAML-Datei der App ein **MediaPlayerElement** hinzu, und legen Sie [**AreTransportControlsEnabled**](/uwp/api/windows.ui.xaml.controls.mediaelement.aretransportcontrolsenabled) auf „true“ fest.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetMediaElement":::

Fügen Sie eine Schaltfläche hinzu, über die der Benutzer die Auswahl einer Datei initiieren kann.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetOpenButton":::

Erstellen Sie im [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignishandler für die Schaltfläche eine neue Instanz des [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker)-Objekts, fügen Sie der [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter)-Sammlung Videodateitypen hinzu, und legen die Ausgangsposition auf die Videobibliothek des Benutzers fest.

Rufen Sie [**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) auf, um das Dialogfeld für die Dateiauswahl zu starten. Diese Methode gibt ein [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)-Objekt zurück, das die Videodatei darstellt. Stellen Sie sicher, dass die Datei nicht NULL ist; dies ist der Fall, wenn der Benutzer den Auswahlvorgang abbricht. Rufen Sie die [**OpenAsync**](/uwp/api/windows.storage.storagefile.openasync)-Methode der Datei auf, um einen [**IRandomAccessStream**](/uwp/api/Windows.Storage.Streams.IRandomAccessStream) für die Datei abzurufen. Abschließend erstellen Sie ein neues **MediaSource**-Objekt aus der ausgewählten Datei durch Aufrufen von [**CreateFromStorageFile**](/uwp/api/windows.media.core.mediasource.createfromstoragefile) und weisen es der [**Source**](/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source)-Eigenschaft des **MediaPlayerElement**-Objekts zu, um die Videodatei zur Videoquelle für das Steuerelement zu machen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetOpenButtonClick":::

Nachdem das Video in das **MediaPlayerElement** geladen wurde, kann der Benutzer einfach die Umwandlungsschaltfläche in den Transportsteuerelementen wählen, um ein integriertes Dialogfeld zu öffnen, in dem er ein Gerät auswählen kann, für das die geladenen Medien umgewandelt werden.

![MediaElement-Umwandlungsschaltfläche](images/media-element-casting-button.png)

> [!NOTE] 
> Ab Windows 10, Version 1607, wird die Verwendung der **MediaPlayer**-Klasse zum Wiedergeben von Medienelementen empfohlen. **MediaPlayerElement** ist ein einfaches XAML-Steuerelement, das zum Rendern des Inhalts eines **MediaPlayer**-Objekts auf einer XAML-Seite verwendet wird. Das **MediaElement**-Steuerelement wird aus Gründen der Abwärtskompatibilität weiterhin unterstützt. Weitere Informationen zur Verwendung von **MediaPlayer** und **MediaPlayerElement** zum Wiedergeben von Medieninhalten finden Sie unter [Wiedergeben von Audio- und Videoinhalten mit „MediaPlayer“](play-audio-and-video-with-mediaplayer.md). Informationen zur Verwendung von **MediaSource** und dazugehörigen APIs für die Arbeit mit Medieninhalten finden Sie unter [Medienelemente, Wiedergabelisten und Titel](media-playback-with-mediasource.md).

## <a name="media-casting-with-the-castingdevicepicker"></a>Medienumwandlung mit CastingDevicePicker

Eine zweite Methode zum Umwandeln von Medien für ein Gerät ist die Verwendung der [**CastingDevicePicker**](/uwp/api/Windows.Media.Casting.CastingDevicePicker)-Klasse. Zum Verwenden dieser Klasse schließen Sie den [**Windows.Media.Casting**](/uwp/api/Windows.Media.Casting)-Namespace in Ihr Projekt ein.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetCastingNamespace":::

Deklarieren Sie eine Membervariable für das **CastingDevicePicker**-Objekt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareCastingPicker":::

Wenn die Seite initialisiert wird, erstellen Sie eine neue Instanz der Umwandlungsauswahl, und legen Sie die [**Filter**](/uwp/api/windows.media.casting.castingdevicepicker.filter)-Eigenschaft auf [**SupportsVideo**](/uwp/api/Windows.Media.Casting.CastingDevicePickerFilter) fest, um anzugeben, dass die von der Auswahl aufgeführten Umwandlungsgeräte Video unterstützen müssen. Registrieren Sie einen Handler für das [**CastingDeviceSelected**](/uwp/api/windows.media.casting.castingdevicepicker.castingdeviceselected)-Ereignis, das ausgelöst wird, wenn der Benutzer ein Gerät für die Umwandlung auswählt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetInitCastingPicker":::

Fügen Sie in der XAML-Datei eine Schaltfläche hinzu, über die der Benutzer die Auswahl starten kann.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetCastPickerButton":::

Rufen Sie im **Click**-Ereignishandler für die Schaltfläche [**TransformToVisual**](/uwp/api/windows.ui.xaml.uielement.transformtovisual) auf, um die Transformation eines UI-Elements relativ zu einem anderen Element abzurufen. In diesem Beispiel ist die Transformation die Position der Umwandlungsauswahl-Schaltfläche relativ zum visuellen Stamm des Anwendungsfensters. Rufen Sie die [**Show**](/uwp/api/windows.media.casting.castingdevicepicker.show)-Methode des [**CastingDevicePicker**](/uwp/api/Windows.Media.Casting.CastingDevicePicker)-Objekts auf, um das Dialogfeld der Umwandlungsauswahl zu öffnen. Geben Sie die Position und die Abmessungen der Umwandlungsauswahl-Schaltfläche an, sodass das Dialogfeld als Flyout der vom Benutzer gewählten Schaltfläche angezeigt werden kann.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetCastPickerButtonClick":::

Rufen Sie im **CastingDeviceSelected**-Ereignishandler die [**CreateCastingConnection**](/uwp/api/windows.media.casting.castingdevice.createcastingconnection)-Methode der [**SelectedCastingDevice**](/uwp/api/windows.media.casting.castingdeviceselectedeventargs.selectedcastingdevice)-Eigenschaft der Ereignisargumente auf, die das vom Benutzer ausgewählte Umwandlungsgerät darstellt. Registrieren Sie Handler für die Ereignisse [**ErrorOccurred**](/uwp/api/windows.media.casting.castingconnection.erroroccurred) und [**StateChanged**](/uwp/api/windows.media.casting.castingconnection.statechanged). Rufen Sie abschließend [**RequestStartCastingAsync**](/uwp/api/windows.media.casting.castingconnection.requeststartcastingasync) auf, um die Umwandlung zu starten, indem das Ergebnis für die [**GetAsCastingSource**](/uwp/api/windows.ui.xaml.controls.mediaelement.getascastingsource)-Methode des **MediaPlayer**-Objekts des **MediaPlayerElement**-Steuerelements übergeben wird, um anzugeben, dass die Medien, die umgewandelt werden sollen, der Inhalt des **MediaPlayer** sind, der dem **MediaPlayerElement** zugeordnet ist.

> [!NOTE] 
> Die Umwandlungsverbindung muss im UI-Thread initiiert werden. Da **CastingDeviceSelected** nicht für den UI-Thread aufgerufen wird, müssen Sie diese Aufrufe innerhalb eines Aufrufs von [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) platzieren, sodass diese für den UI-Thread aufgerufen werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetCastingDeviceSelected":::

Aktualisieren Sie die Benutzeroberfläche in den Ereignishandlern **ErrorOccurred** und **StateChanged**, um den Benutzer über den aktuellen Status der Umwandlung zu informieren. Diese Ereignisse werden im folgenden Abschnitt zum Erstellen einer benutzerdefinierten Umwandlungsgeräteauswahl ausführlich erläutert.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetEmptyStateHandlers":::

## <a name="media-casting-with-a-custom-device-picker"></a>Medienumwandlung mit einer benutzerdefinierten Geräteauswahl

Im folgenden Abschnitt wird beschrieben, wie Sie ein eigenes Benutzeroberflächenelement zur Umwandlungsgeräteauswahl erstellen, indem Sie die Umwandlungsgeräte aufzählen und die Verbindung im Code initiieren.

Um die verfügbaren Umwandlungsgeräte aufzuzählen, schließen Sie den [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)-Namespace in Ihr Projekt ein.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetEnumerationNamespace":::

Fügen Sie der XAML-Seite die folgenden Steuerelemente hinzu, um die einfache Benutzeroberfläche für dieses Beispiel zu implementieren:

-   Eine Schaltfläche zum Starten des Geräteüberwachungselements, das nach verfügbaren Umwandlungsgeräten sucht.
-   Ein [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)-Steuerelement, das den Benutzer über die laufende Umwandlungsenumeration informiert.
-   Ein [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) zum Auflisten der erkannten Umwandlungsgeräte. Definieren Sie eine [**ItemTemplate**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) für das Steuerelement, sodass die Umwandlungsgeräteobjekte direkt dem Steuerelement zugewiesen werden können und die [**FriendlyName**](/uwp/api/windows.media.casting.castingdevice.friendlyname)-Eigenschaft trotzdem weiterhin angezeigt wird.
-   Eine Schaltfläche, über die der Benutzer das Umwandlungsgerät trennen kann.

:::code language="xml" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml" id="SnippetCustomPickerXAML":::

Deklarieren Sie im CodeBehind Membervariablen für [**DeviceWatcher**](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) und [**CastingConnection**](/uwp/api/Windows.Media.Casting.CastingConnection).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetDeclareDeviceWatcher":::

Aktualisieren Sie im **Click**-Handler für die *startWatcherButton* zuerst die Benutzeroberfläche, indem Sie die Schaltfläche deaktivieren und das Statussignal aktivieren, während die Geräteenumeration ausgeführt wird. Löschen Sie den Inhalt des Listenfelds mit Umwandlungsgeräten.

Erstellen Sie als Nächstes ein Geräteüberwachungselement durch Aufruf von [**DeviceInformation.CreateWatcher**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher). Diese Methode kann verwendet werden, um viele verschiedene Arten von Geräten zu überwachen. Geben Sie mithilfe der von [**CastingDevice.GetDeviceSelector**](/uwp/api/windows.media.casting.castingdevice.getdeviceselector) zurückgegebenen Geräteauswahlzeichenfolge an, dass Sie Geräte überwachen möchten, die die Videoumwandlung unterstützen.

Registrieren Sie abschließend Ereignishandler für die Ereignisse [**Added**](/uwp/api/windows.devices.enumeration.devicewatcher.added), [**Removed**](/uwp/api/windows.devices.enumeration.devicewatcher.removed), [**EnumerationCompleted**](/uwp/api/windows.devices.enumeration.devicewatcher.enumerationcompleted) und [**Stopped**](/uwp/api/windows.devices.enumeration.devicewatcher.stopped).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetStartWatcherButtonClick":::

Das **Added**-Ereignis wird ausgelöst, wenn ein neues Gerät vom Überwachungselement erkannt wird. Erstellen Sie im Handler für dieses Ereignis ein neues [**CastingDevice**](/uwp/api/Windows.Media.Casting.CastingDevice)-Objekt, indem Sie [**CastingDevice.FromIdAsync**](/uwp/api/windows.media.casting.castingdevice.fromidasync) aufrufen und die ID des erkannten Umwandlungsgeräts übergeben, die in dem an den Handler übergebenen **DeviceInformation**-Objekt enthalten ist.

Fügen Sie das **CastingDevice** zum **ListBox** für Umwandlungsgeräte hinzu, damit der Benutzer es auswählen kann. Aufgrund der in XAML definierten [**ItemTemplate**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemtemplate) wird die [**FriendlyName**](/uwp/api/windows.media.casting.castingdevice.friendlyname)-Eigenschaft als Elementtext im Listenfeld verwendet. Da dieser Ereignishandler nicht für den UI-Thread aufgerufen wird, müssen Sie die Benutzeroberfläche innerhalb eines Aufrufs von [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) aktualisieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherAdded":::

Das **Removed**-Ereignis wird ausgelöst, wenn das Überwachungselement erkennt, dass ein Umwandlungsgerät nicht mehr vorhanden ist. Vergleichen Sie die ID-Eigenschaft des **Added**-Objekts, die an den Handler übergeben wird, mit der ID jedes **Added**-Objekts in der [**Items**](/uwp/api/windows.ui.xaml.controls.itemscontrol.items)-Sammlung des Listenfelds. Wenn die ID übereinstimmt, entfernen Sie das Objekt aus der Sammlung. Zur Erinnerung: Da die Benutzeroberfläche aktualisiert wird, muss dieser Aufruf innerhalb eines **RunAsync**-Aufrufs erfolgen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherRemoved":::

Das **EnumerationCompleted**-Ereignis wird ausgelöst, wenn das Überwachungselement die Erkennung von Geräten abgeschlossen hat. Aktualisieren Sie im Handler für dieses Ereignis die Benutzeroberfläche, um den Benutzer darüber zu informieren, dass die Enumeration abgeschlossen ist, und beenden Sie das Geräteüberwachungselement durch Aufruf von [**Stop**](/uwp/api/windows.devices.enumeration.devicewatcher.stop).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherEnumerationCompleted":::

Das Stopped-Ereignis wird ausgelöst, wenn das Beenden des Geräteüberwachungselements abgeschlossen ist. Beenden Sie im Handler für dieses Ereignis das [**ProgressRing**](/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)-Steuerelement, und aktivieren Sie die *startWatcherButton* wieder, damit der Benutzer den Vorgang der Geräteenumeration neu starten kann.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetWatcherStopped":::

Wenn der Benutzer eines der Umwandlungsgeräte im Listenfeld auswählt, wird das [**SelectionChanged**](/uwp/api/windows.ui.xaml.controls.primitives.selector.selectionchanged)-Ereignis ausgelöst. Innerhalb dieses Handlers werden die Umwandlungsverbindung erstellt und die Umwandlung gestartet.

Stellen Sie zunächst sicher, dass das Geräteüberwachungselement beendet wurde, sodass kein Konflikt zwischen Geräteenumeration und Medienumwandlung auftritt. Erstellen Sie eine Umwandlungsverbindung, indem Sie [**CreateCastingConnection**](/uwp/api/windows.media.casting.castingdevice.createcastingconnection) für das vom Benutzer ausgewählte **CastingDevice**-Objekt aufrufen. Fügen Sie Ereignishandler für die Ereignisse [**StateChanged**](/uwp/api/windows.media.casting.castingconnection.statechanged) und [**ErrorOccurred**](/uwp/api/windows.media.casting.castingconnection.erroroccurred) hinzu.

Starten Sie die Medienumwandlung durch Aufrufen von [**RequestStartCastingAsync**](/uwp/api/windows.media.casting.castingconnection.requeststartcastingasync), und übergeben Sie dabei die Umwandlungsquelle, die durch den Aufruf der **MediaPlayer**-Methode [**GetAsCastingSource**](/uwp/api/windows.ui.xaml.controls.mediaelement.getascastingsource) zurückgegeben wurde. Machen Sie zum Schluss die Schaltfläche zum Trennen sichtbar, damit der Benutzer die Medienumwandlung beenden kann.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetSelectionChanged":::

Im Ereignishandler für geänderten Zustand hängt die ausgeführte Aktion vom neuen Zustand der Umwandlungsverbindung ab:

-   Beim Zustand **Connected** oder **Rendering** stellen Sie sicher, dass das **ProgressRing**-Steuerelement inaktiv ist und die Schaltfläche zum Trennen angezeigt wird.
-   Beim Zustand **Disconnected** heben Sie die Auswahl des aktuellen Umwandlungsgerät im Listenfeld auf, deaktivieren Sie das **ProgressRing**-Steuerelement, und blenden Sie die Schaltfläche zum Trennen aus.
-   Beim Zustand **Connecting** aktivieren Sie das **ProgressRing**-Steuerelement, und blenden Sie die Schaltfläche zum Trennen aus.
-   Beim Zustand **Disconnecting** aktivieren Sie das **ProgressRing**-Steuerelement, und blenden Sie die Schaltfläche zum Trennen aus.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetStateChanged":::

Aktualisieren Sie im Handler für das **ErrorOccurred**-Ereignis die Benutzeroberfläche, um den Benutzer darüber zu informieren, dass ein Umwandlungsfehler aufgetreten ist, und heben Sie die Auswahl des aktuellen **CastingDevice**-Objekts im Listenfeld auf.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetErrorOccurred":::

Implementieren Sie zum Schluss den Handler für die Schaltfläche zum Trennen. Beenden Sie die Medienumwandlung, und trennen Sie die Verbindung mit dem Umwandlungsgerät, indem Sie die [**DisconnectAsync**](/uwp/api/windows.media.casting.castingconnection.disconnectasync)-Methode des **CastingConnection**-Objekts aufrufen. Dieser Aufruf muss durch Aufrufen von [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) an den UI-Thread weitergeleitet werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/MediaCasting_RS1/cs/MainPage.xaml.cs" id="SnippetDisconnectButton":::

 

 
