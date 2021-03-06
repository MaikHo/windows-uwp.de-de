---
ms.assetid: BF929A68-9C82-4866-BC13-A32B3A550005
title: Nachverfolgen kürzlich verwendeter Dateien und Ordner
description: Sie können Dateien nachverfolgen, auf die häufig zugegriffen wird, indem Sie sie der Liste mit den zuletzt verwendeten Elementen (MRU) der App hinzufügen.
ms.date: 12/19/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 29d140672e80304bb0adb7081ba616e75d8d8ecd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173204"
---
# <a name="track-recently-used-files-and-folders"></a>Nachverfolgen kürzlich verwendeter Dateien und Ordner

**Wichtige APIs**

- [**MostRecentlyUsedList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist)
- [**FileOpenPicker**](/uwp/schemas/appxpackage/appxmanifestschema/element-fileopenpicker)

Sie können Dateien nachverfolgen, auf die häufig zugegriffen wird, indem Sie sie der Liste mit den zuletzt verwendeten Elementen (MRU) der App hinzufügen. Die Plattform verwaltet die MRU für Sie. Dabei werden Elemente nach der Zeit und dem Ort des letzten Zugriffs sortiert und die ältesten Elemente entfernt, wenn das Limit von 25 Elementen erreicht ist. Alle Apps besitzen eine eigene MRU.

Ihre App-MRU-Liste wird durch die [**StorageItemMostRecentlyUsedList**](/uwp/api/Windows.Storage.AccessCache.StorageItemMostRecentlyUsedList)-Klasse repräsentiert, die Sie aus der statischen [**StorageApplicationPermissions.MostRecentlyUsedList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist)-Eigenschaft abrufen können. MRU-Elemente werden als [**IStorageItem**](/uwp/api/Windows.Storage.IStorageItem)-Objekte gespeichert. Das bedeutet, dass sowohl [**StorageFile**](/uwp/api/Windows.Storage.StorageFile)-Objekte (die Dateien darstellen) als auch [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder)-Objekte (die Ordner darstellen) der MRU-Liste hinzugefügt werden können.

> [!NOTE]
> Vollständige Beispiele finden Sie im [Beispiel zur Dateiauswahl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker) und im [Beispiel zum Dateizugriff](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess).

## <a name="prerequisites"></a>Voraussetzungen

-   **Kenntnisse in der asynchronen Programmierung für Apps für die universelle Windows-Plattform (UWP)**

    Informationen zum Schreiben von asynchronen Apps in C# oder Visual Basic finden Sie unter [Aufrufen asynchroner APIs in C# oder Visual Basic](../threading-async/call-asynchronous-apis-in-csharp-or-visual-basic.md). Informationen zum Schreiben von asynchronen Apps in C++ finden Sie unter [Asynchrone Programmierung in C++](../threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps.md).

-   **Zugriffsberechtigungen für den Speicherort**

    Weitere Informationen finden Sie unter [Berechtigungen für den Dateizugriff](file-access-permissions.md).

-   [Öffnen von Dateien und Ordnern mit einer Auswahl](quickstart-using-file-and-folder-pickers.md)

    Die ausgewählten Dateien sind meist diejenigen, auf die Benutzer immer wieder zugreifen.

 ## <a name="add-a-picked-file-to-the-mru"></a>Hinzufügen der ausgewählten Dateien zu MRU

-   Die Dateien, die der Benutzer wählt sind häufig Dateien, denen sie wiederholt zurückgeben werden. Erwägen Sie also, die ausgewählten Dateien Ihrer App-MRU-Liste hinzufügen, sobald sie ausgewählt werden. Gehen Sie dazu wie folgt vor:

    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    var mru = Windows.Storage.AccessCache.StorageApplicationPermissions.MostRecentlyUsedList;
    string mruToken = mru.Add(file, "profile pic");
    ```

    [**StorageItemMostRecentlyUsedList.Add**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add) ist überladen. Im Beispiel verwenden wir [**Add(IStorageItem, String)** ](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add), sodass wir der Datei Metadaten zuordnen können. Wenn Sie Metadaten festlegen, können Sie den Zweck des Elements festlegen, z. B. Profilauswahl. Sie können die Datei durch einen Aufruf von [**Add(IStorageItem)** ](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.add) auch ohne Metadaten der MRU-Liste hinzufügen. Beim Hinzufügen eines Elements zur MRU-Liste gibt die Methode eine eindeutig identifizierende Zeichenfolge zurück. Mit diesem sogenannten Token wird das Element abgerufen.

> [!TIP]
> Sie benötigen das Token zum Abrufen eines Elements aus der MRU-Liste. Speichern Sie es daher an einem beliebigen Ort. Weitere Informationen zu App-Daten finden Sie unter [Verwalten von Anwendungsdaten](/previous-versions/windows/apps/hh465109(v=win.10)).

## <a name="use-a-token-to-retrieve-an-item-from-the-mru"></a>Verwenden von Token zum Abrufen von Elementen aus der MRU-Liste

Verwenden Sie die Abrufmethode, die für das abzurufende Element am besten geeignet ist.

-   Rufen Sie mit [**GetFileAsync**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getfileasync) eine Datei als [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) ab.
-   Rufen Sie mit [**GetFolderAsync**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getfolderasync) einen Ordner als [**StorageFolder**](/uwp/api/Windows.Storage.StorageFolder) ab.
-   Rufen Sie mit [**GetItemAsync**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.getitemasync) ein allgemeines [**IStorageItem**](/uwp/api/Windows.Storage.IStorageItem)-Element ab, das eine Datei oder einen Ordner darstellen kann.

Im Folgenden erfahren Sie, wie Sie die Datei abrufen können, die gerade hinzugefügt wurde.

```cs
StorageFile retrievedFile = await mru.GetFileAsync(mruToken);
```

Hier wird erläutert, wie alle Einträge zum Abrufen von Token und anschließend Elementen durchlaufen werden.

```cs
foreach (Windows.Storage.AccessCache.AccessListEntry entry in mru.Entries)
{
    string mruToken = entry.Token;
    string mruMetadata = entry.Metadata;
    Windows.Storage.IStorageItem item = await mru.GetItemAsync(mruToken);
    // The type of item will tell you whether it's a file or a folder.
}
```

Mit [**AccessListEntryView**](/uwp/api/Windows.Storage.AccessCache.AccessListEntryView) können Sie Einträge in der MRU-Liste durchlaufen. Diese Einträge sind [**AccessListEntry**](/uwp/api/Windows.Storage.AccessCache.AccessListEntry)-Strukturen, die das Token und Metadaten für ein Element enthalten.

## <a name="removing-items-from-the-mru-when-its-full"></a>Entfernen von Elementen aus der MRU-Liste beim Erreichen des Grenzwerts

Wenn das Limit von 25 Elementen der MRU-Liste erreicht ist und ein neues Element hinzugefügt werden soll, wird das älteste Element (bei dem der letzte Zugriff am längsten zurück liegt) automatisch entfernt. Sie müssen also nie ein Element entfernen, bevor Sie ein neues hinzufügen.

## <a name="future-access-list"></a>Liste für zukünftigen Zugriff

Genau wie eine MRU-Liste besitzt auch Ihre App eine Liste für zukünftigen Zugriff. Durch die Auswahl von Dateien und Ordnern erteilt der Benutzer Ihrer App die Berechtigung zum Zugreifen auf Elemente, auf die andernfalls nicht zugegriffen werden kann. Wenn Sie diese Elemente zu Ihrer Liste für zukünftigen Zugriff hinzufügen, behalten Sie die Berechtigung, wenn Ihre App später erneut auf diese Elemente zugreifen möchte. Ihre App-Liste für zukünftigen Zugriff wird durch die [**StorageItemAccessList**](/uwp/api/Windows.Storage.AccessCache.StorageItemAccessList)-Klasse dargestellt, die Sie aus der statischen [**StorageApplicationPermissions.FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist)-Eigenschaft abrufen.

Wenn ein Benutzer ein Element auswählt, sollten Sie es Ihrer Liste für zukünftigen Zugriff und der MRU-Liste hinzufügen.

-   [  **FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) kann bis zu 1.000 Elemente enthalten. Denken Sie daran: Die Liste kann Ordner und Dateien enthalten. Das können also eine ganze Menge Ordner sein.
-   Die Plattform entfernt niemals Elemente aus der [**FutureAccessList**](/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) für Sie. Wenn das Limit von 1.000 Elementen erreicht ist, können Sie kein weiteres hinzufügen, bis Sie mit der Methode [**Remove**](/uwp/api/windows.storage.accesscache.storageitemmostrecentlyusedlist.remove) Platz geschaffen haben.