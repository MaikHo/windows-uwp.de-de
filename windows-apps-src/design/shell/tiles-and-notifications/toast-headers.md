---
Description: Erfahren Sie, wie Sie mit Headern ihre Popup Benachrichtigungen im Aktions Center visuell gruppieren.
title: Popup-Header
label: Toast headers
template: detail.hbs
ms.date: 12/07/2017
ms.topic: article
keywords: Windows 10, UWP, Popup, Header, Popup-Header, Benachrichtigungen, Gruppen-Popups, Info-Center
ms.localizationpriority: medium
ms.openlocfilehash: af94b501b0e98515a1d3b3216f3a76a28fedab67
ms.sourcegitcommit: 6e7665b457ec4585db19b70acfa2554791ad6e10
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2019
ms.locfileid: "70987217"
---
# <a name="toast-headers"></a>Popup-Header

Sie können eine Reihe verwandter Benachrichtigungen visuell im Info-Center mit einem Popup-Header für Ihre Benachrichtigungen gruppieren.

> [!IMPORTANT]
> **Erfordert Desktop Creators Update und 1.4.0 der Benachrichtigungs Bibliothek**: Sie müssen den Desktop-Build 15063 oder höher ausführen, um Popup Header anzuzeigen. Sie müssen Version 1.4.0 oder höher der [UWP Community Toolkit Benachrichtigungen NuGet-Bibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) verwenden, um den Header der Popup-Inhalte zu erstellen. Header werden nur auf Desktop unterstützt.

Wie unten dargestellt wird, ist diese Gruppenunterhaltung unter einer einzelnen Kopfzeile vereint: "Camping!!". Jede einzelne Nachricht in der Unterhaltung ist eine separate Popupbenachrichtigung, die die gemeinsame Nutzung des gleichen Popup-Headers teilt.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

Sie können auch auswählen, Ihre Benachrichtigungen visuell nach Kategorie zu gruppieren, z. B. Test-Flight-Erinnerungen, Paketverfolgung und mehr.

## <a name="add-a-header-to-a-toast"></a>Hinzufügen eines Headers zu einem Popup

Hier erfahren Sie, wie Sie einer Popupbenachrichtigung einen Header hinzufügen.

> [!NOTE]
> Header werden nur auf Desktop unterstützt. Geräte, die Header nicht unterstützen, ignoriert den Header einfach.

```csharp
ToastContent toastContent = new ToastContent()
{
    Header = new ToastHeader()
    {
        Id = "6289",
        Title = "Camping!!",
        Arguments = "action=openConversation&id=6289",
    },

    Visual = new ToastVisual() { ... }
};
```

```xml
<toast>

    <header
        id="6289"
        title="Camping!!"
        arguments="action=openConversation&amp;id=6289"/>

    <visual>
        ...
    </visual>

</toast>
```

Zusammenfassung...

1. Fügen Sie den **Header** zu Ihrem **ToastContent** hinzu
2. Weisen Sie die erforderlichen Eigenschaften zu, wie **ID**, **Titel** und **Argumente**
3. Senden der Benachrichtigung ([Hier erfahren Sie mehr](send-local-toast.md))
4. Verwenden Sie auf einer anderen Benachrichtigung die gleiche Überschrift-**ID**, um diese unter dem Header zu vereinheitlichen. Die **ID** ist die einzige Eigenschaft, die bestimmt, ob die Benachrichtigungen gruppiert werden sollen, d. h. dass **Titel** und **Argumente** unterschiedlich sein können. Es werden die **Titel** und **Argumente** aus der letzten Benachrichtigung innerhalb einer Gruppe verwendet. Wenn die Benachrichtigung entfernt wird, greifen **Titel** und **Argumente** auf die nächste aktuelle Benachrichtigung zurück.


## <a name="handle-activation-from-a-header"></a>Behandeln der Aktivierung über einen Header

Header können von Benutzern angeklickt werden, damit der Benutzer den Header anklicken kann, um mehr über Ihre App zu erfahren.

Daher können Apps **Argumente** im Header enthalten, ähnlich wie die Startargumente des Popup.

Die Aktivierung wird identisch behandelt wie eine [normale Popup-Aktivierung](send-local-toast.md#activation-handling), d. h., Sie können diese Argumente in der **OnActivated**-Methode der `App.xaml.cs` genauso abrufen, als ob der Benutzer auf den Text des Popups oder die Schaltfläche des Popup klickt.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        // Arguments specified from the header
        string arguments = (e as ToastNotificationActivatedEventArgs).Argument;
    }
}
```


## <a name="additional-info"></a>Zusätzliche Infos

Der Header trennt und gruppiert visuell Gruppenbenachrichtigungen. Dadurch ändert sich die andere Logistik über die maximalen Anzahl der Benachrichtigungen und das Verhalten der Reihenfolge der Benachrichtigungsliste nicht, über die eine App verfügen kann (20).

Die Reihenfolge der Benachrichtigungen in den Headern lautet wie folgt... Für eine bestimmte APP wird die aktuellste Benachrichtigung von der APP (und die gesamte Header Gruppe, wenn Teil eines Headers vorhanden ist) zuerst angezeigt.

Die **ID** kann eine beliebige Zeichenfolge sein, die Sie auswählen. Es gelten keine Beschränkung für die Länge- oder die Zeichensätze auf den Eigenschaften des **ToastHeaders**. Die einzige Einschränkung besteht darin, dass der gesamte XML-Popupinhalt nicht größer als 5 KB sein kann.

Das Erstellen der Überschriften ändert die Anzahl der angezeigten Benachrichtigungen im Info-Center nicht, bevor die Schaltfläche "Weitere" angezeigt wird (diese Nummer ist standardmäßig 3 und kann vom Benutzer für jede App in den Systemeinstellungen für Benachrichtigungen konfiguriert werden).

Das Klicken auf die Überschrift, genau wie das Klicken auf den App-Titel, löscht keine Benachrichtigungen, die zu diesem Header gehören (Ihre App sollte die Popup-APIs verwenden,und die entsprechenden Benachrichtigungen zu löschen).


## <a name="related-topics"></a>Verwandte Themen

- [Senden eines lokalen Popups und behandeln der Aktivierung](send-local-toast.md)
- [Dokumentation zu Popup Inhalten](adaptive-interactive-toasts.md)
