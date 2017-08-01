---
author: mcleanbyron
description: "Hier erfahren Sie, wie Sie Ihre App aktualisieren, damit Sie die neuesten unterstützten Microsoft Advertising-Bibliotheken verwenden können und Ihre App weiterhin Banneranzeigen erhält."
title: Aktualisieren Ihrer App auf die neuesten Advertising-Bibliotheken
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP, Anzeigen, Werbung, AdControl, AdMediatorControl, Migrieren
ms.assetid: f8d5b2ad-fcdb-4891-bd68-39eeabdf799c
ms.openlocfilehash: 25435ebf314327db7288ac853819c90ebba35669
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="update-your-app-to-the-latest-advertising-libraries"></a>Aktualisieren Ihrer App auf die neuesten Advertising-Bibliotheken

Apps, die Banneranzeigen aus Microsoft Advertising verwenden, müssen **AdControl** oder **AdMediatorControl** von einem der folgenden SDKs verwenden, damit nach dem 1.April2017 weiterhin Banneranzeigen angezeigt werden:

  * [Microsoft Store Services SDK](http://aka.ms/store-services-sdk) (für UWP-Apps)
  * [Microsoft Advertising-SDK für Windows und Windows Phone 8.x](http://aka.ms/store-8-sdk) (für Windows 8.1- und Windows Phone 8.x-Apps)

Bevor diese SDKs verfügbar waren, haben wir verschiedene diese Steuerelemente in älteren Advertising SDK-Versionen für Windows- und Windows Phone-Apps freigegeben. Diese älteren Advertising SDK-Versionen werden nicht mehr unterstützt. Nach dem 1.April2017 kann Microsoft die Bereitstellung von Banneranzeigen für Apps, die ältere Advertising-SDK-Versionen verwenden, jederzeit und ohne vorherige Ankündigung einstellen.

Wenn Sie über eine App verfügen (bereits im Store oder noch in Entwicklung), die Banneranzeigen mithilfe von **AdControl** oder **AdMediatorControl** anzeigt, ermitteln Sie anhand der Anweisungen in diesem Artikel, ob Ihre App von dieser Änderung betroffen ist, und zu erfahren, wie Sie Ihre App gegebenenfalls aktualisieren können.

>**Hinweis**&nbsp;&nbsp;Darüber hinaus werden wir nach dem 1.April 2017 die Bereitstellung von Banneranzeigen für alle Anzeigeneinheiten einstellen, die in mehr als einer App verwendet werden. Stellen Sie zur Vorbereitung auf diese Änderung sicher, dass Ihre Anzeigeneinheiten jeweils nur in einer App verwendet werden.

## <a name="more-details-about-this-change"></a>Weitere Informationen zu dieser Änderung

Beachten Sie außerdem, dass wir die Unterstützung für ältere Advertising SDK-Versionen einstellen, die nicht einen mindestens erforderlichen Satz von Funktionen unterstützen. Dies umfasst auch die Möglichkeit, HTML5-Rich-Media über die [Mobile Rich Media Ad Interface Definitions (MRAID) 1.0-Spezifikation](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf) vom Interactive Advertising Bureau (IAB) zu verarbeiten. Viele unserer Werbekunden wünschen sich diese Funktionen, und wir nehmen diese Änderungen vor, damit unser App-Ökosystem für Werbekunden attraktiver wird und Ihr Umsatz wächst.

Wenn Ihre App von dieser Änderung betroffen ist und Sie Ihre App nicht so aktualisieren, dass sie die neueste Advertising SDK für Ihre Zielplattform verwendet, werden Sie das folgende Verhalten sehen, wenn wir die Bereitstellung von Banner-Anzeigen für Apps einstellen, die nicht unterstützte Advertising SDK-Versionen verwenden.

* Banneranzeigen werden nicht mehr für **AdControl**- oder **AdMediatorControl**-Steuerelemente in Ihrer App bereitgestellt, und Sie erhalten keinen Anzeigenumsatz mehr aus diesen Steuerelementen.

* Wenn **AdControl** oder **AdMediatorControl** in Ihrer App eine neue Anzeige anfordert, wird ein **ErrorOccurred**-Ereignis des Steuerelements ausgegeben, und die **ErrorCode**-Eigenschaft der Ereignisargumente hat den Wert **NoAdAvailable**.

Wenn Schwierigkeiten auftreten oder Sie Hilfe benötigen, [wenden Sie sich an den Support](http://go.microsoft.com/fwlink/?LinkId=393643).

>**Hinweis**&nbsp;&nbsp;Wenn Ihre App bereits das [Microsoft Store Services SDK](http://aka.ms/store-services-sdk) (für UWP-Apps) oder das [Microsoft Advertising SDK für Windows und Windows Phone 8.x](http://aka.ms/store-8-sdk) (für Windows 8.1- und Windows Phone 8.x-Apps) verwendet, oder Sie Ihre App bereits zur Nutzung eines dieser SDKs aktualisiert haben, verwendet diese bereits das neueste verfügbare Advertising SDK, und Sie müssen an Ihrer App keine weiteren Änderungen vornehmen.

## <a name="prerequisites"></a>Voraussetzungen

* Der komplette Quellcode und Visual Studio-Projektdateien für Ihre App, die **AdControl** oder **AdMediatorControl** verwendet.

* Das APPX- oder XAP-Paket für Ihre App.

  >**Hinweis**&nbsp;&nbsp;Wenn Sie über das APPX- oder XAP-Paket für Ihre App nicht mehr verfügen, Ihnen aber noch immer ein Entwicklungscomputer mit der Version von Visual Studio und dem Advertising SDK, das für die App-Erstellung eingesetzt wurde, zur Verfügung steht, können Sie das APPX- oder XAP-Paket in Visual Studio neu generieren.

<span id="part-1" />
## <a name="part-1-determine-whether-you-need-to-update-your-app"></a>Teil 1: Ermitteln, ob Ihre App aktualisiert werden muss

Befolgen Sie die Anweisungen in den folgenden Abschnitten, um festzustellen, ob Sie Ihre App aktualisieren müssen.

### <a name="your-app-uses-adcontrol"></a>Ihre App verwendet AdControl

Wenn Ihre App **AdControl** zum Anzeigen von Banneranzeigen verwendet, gehen Sie wie folgt vor.

**UWP-Apps für Windows 10**

1. Erstellen Sie eine Kopie des APPX-Pakets für Ihre App, damit das Original nicht beeinträchtigt wird, benennen Sie die Kopie um, sodass sie die Erweiterung „.zip“ erhält, und extrahieren Sie den Inhalt der Datei.

2. Überprüfen Sie den extrahierten Inhalt Ihres App-Pakets:

  * Wenn die Datei „Microsoft.Advertising.dll“ vorhanden ist, verwendet Ihre App ein altes SDK, und Sie müssen Ihr Projekt aktualisieren, indem Sie die Anweisungen in den folgenden Abschnitten ausführen. Fahren Sie mit [Teil 2](update-your-app-to-the-latest-advertising-libraries.md#part-2) fort.

  * Wenn die Datei „Microsoft.Advertising.dll“ nicht vorhanden ist, verwendet Ihre UWP-App bereits das neueste verfügbare Advertising SDK, und Sie müssen keine Änderungen an Ihrem Projekt vornehmen.

<span/>

**Windows 8.1- oder Windows Phone 8.x-Apps**

1. Erstellen Sie eine Kopie des APPX- oder XAP-Pakets für Ihre App, damit das Original nicht beeinträchtigt wird, benennen Sie die Kopie um, sodass sie die Erweiterung „.zip“ erhält, und extrahieren Sie den Inhalt der Datei.

2. Im Fall von XAML- oder JavaScript/HTML-Apps prüfen Sie den extrahierten Inhalt Ihres App-Pakets:

  * Wenn die Datei „Microsoft.Advertising.winmd“ vorhanden ist, die Datei „UniversalXamlAdControl.\*.dll“ (für XAML-Apps) oder „UniversalSharedLibrary.Windows.dll“ (für JavaScript/HTML-Apps) jedoch nicht, verwendet Ihre App ein altes SDK, und Sie müssen Ihr Projekt aktualisieren, indem Sie die Anweisungen in den folgenden Abschnitten ausführen. Fahren Sie mit [Teil 2](update-your-app-to-the-latest-advertising-libraries.md#part-2) fort.

  * Andernfalls fahren Sie bitte mit dem folgenden Schritt fort.

2. Öffnen Sie Windows PowerShell, geben Sie den folgenden Befehl ein, und weisen Sie das Argument ```-Path``` dem vollständigen Pfad zum extrahierten Inhalt des App-Pakets zu. Dieser Befehl zeigt alle Advertising-Bibliotheken an, auf die Ihr Projekt verweist, sowie die Version der einzelnen Bibliotheken.

  > [!div class="tabbedCodeSnippets"]
  ```syntax
  get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
  ```

2. Suchen Sie die Datei in der folgenden Tabelle für die Zielplattform Ihrer App, und vergleichen Sie die Versionen der Datei mit der in der Tabelle aufgeführten Version.

  <table>
    <colgroup>
      <col width="33%" />
      <col width="33%" />
      <col width="33%" />
    </colgroup>
    <thead>
      <tr class="header">
        <th align="left">Zielplattform</th>
        <th align="left">Dateien</th>
        <th align="left">Version</th>
      </tr>
    </thead>
    <tbody>
      <tr class="odd">
        <td align="left"><p>Windows8.1 XAML</p></td>
        <td align="left"><p>UniversalXamlAdControl.Windows.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone8.1 XAML</p></td>
        <td align="left"><p>UniversalXamlAdControl.WindowsPhone.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows 8.1 JavaScript/HTML<br/>Windows Phone 8.1 JavaScript/HTML</p></td>
        <td align="left"><p>UniversalSharedLibrary.Windows.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone8.1 Silverlight</p></td>
        <td align="left"><p>Microsoft.Advertising.\*.dll</p></td>
        <td align="left"><p>8.1.50112.0</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone8.0 Silverlight</p></td>
        <td align="left"><p>Microsoft.Advertising.\*.dll</p></td>
        <td align="left"><p>6.2.40501.0</p></td>
      </tr>
    </tbody>
  </table>

3. Wenn die Version der Datei mindestens genauso hoch ist wie die in der obigen Tabelle aufgeführte Version, müssen Sie keine Änderungen an Ihrem Projekt vornehmen.

  Wenn die Datei eine niedrigere Versionsnummer aufweist, müssen Sie Ihr Projekt aktualisieren, ‌indem Sie die Anweisungen in den folgenden Abschnitten ausführen. Fahren Sie mit [Teil 2](update-your-app-to-the-latest-advertising-libraries.md#part-2) fort.

<span/>

**Windows 8.0-Apps**

* Für Apps, die für Windows8.0 entwickelt wurden, werden künftig möglicherweise keine Banneranzeigen mehr bereitgestellt. Damit keine Aufrufe verloren gehen, empfehlen wir Ihnen, Ihr Projekt in eine UWP-App für Windows10 umzuwandeln. Der Großteil des Windows8.0-App-Datenverkehrs läuft nun auf Windows10-Geräten.

<span/>

**Windows Phone7.x-Apps**

* Für Apps, die für Windows Phone 7.x entwickelt wurden, werden künftig möglicherweise keine Banneranzeigen mehr bereitgestellt. Damit keine Aufrufe verloren gehen, empfehlen wir Ihnen, Ihr Projekt in eine Windows Phone 8.1- oder UWP-App für Windows10 umzuwandeln. Der Großteil des Windows7.x-App-Datenverkehrs läuft nun auf Windows Phone 8.1- oder Windows10-Geräten.

<span/>

### <a name="your-app-uses-admediatorcontrol"></a>Ihre App verwendet AdMediatorControl.

Wenn Ihre App **AdMediatorControl** zum Anzeigen von Banneranzeigen verwendet, befolgen Sie diese Anweisungen, um festzustellen, ob Ihre App aktualisiert werden muss.

**UWP-Apps für Windows 10**

* **AdMediatorControl** wird für UWP-Apps nicht mehr unterstützt. Führen Sie die Anweisungen in den folgenden Abschnitten aus, um zur Verwendung von **AdControl** zu migrieren. Fahren Sie mit [Teil 2](update-your-app-to-the-latest-advertising-libraries.md#part-2) fort.

<span/>

**Windows8.1- oder WindowsPhone8.1-Apps**

1. Erstellen Sie eine Kopie des APPX- oder XAP-Pakets für Ihre App, damit das Original nicht beeinträchtigt wird, benennen Sie die Kopie um, sodass sie die Erweiterung „.zip“ erhält, und extrahieren Sie den Inhalt der Datei.

2. Öffnen Sie Windows PowerShell, geben Sie den folgenden Befehl ein, und weisen Sie das Argument ```-Path``` dem vollständigen Pfad zum extrahierten Inhalt des App-Pakets zu. Dieser Befehl zeigt alle Advertising-Bibliotheken an, auf die Ihr Projekt verweist, sowie die Version der einzelnen Bibliotheken.

  > [!div class="tabbedCodeSnippets"]
  ```syntax
  get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
  ```

2. Wenn die Version der „Microsoft.AdMediator.\*.dll“-Dateien, die in der Ausgabe aufgelistet sind, Version 2.0.1603.18005 oder höher entsprechen, müssen Sie keine Änderungen an Ihrem Projekt vornehmen.

  Wenn die Dateien eine niedrigere Versionsnummer aufweisen, müssen Sie Ihr Projekt aktualisieren, ‌indem Sie die Anweisungen in den folgenden Abschnitten ausführen. Fahren Sie mit [Teil 2](update-your-app-to-the-latest-advertising-libraries.md#part-2) fort.

<span id="part-2" />
## <a name="part-2-install-the-latest-sdk"></a>Teil 2: Installieren der aktuellen SDK-Version

Wenn Ihre App eine alte SDK-Version verwendet, führen Sie diese Anweisungen aus, um sicherzustellen, dass Sie das aktuelle SDK auf Ihrem Entwicklungscomputer verwenden.

1. Auf Ihrem Entwicklungscomputer muss Visual Studio 2015 (für UWP-, Windows 8.1- oder Windows Phone 8.x-Projekte) oder Visual Studio 2013 (für Windows 8.1- oder Windows Phone 8.x-Projekte) installiert sein.

  >**Hinweis:**&nbsp;&nbsp;Wenn Visual Studio auf Ihrem Entwicklungscomputer geöffnet ist, schließen Sie es, bevor Sie die folgenden Schritte ausführen.

1.    Deinstallieren Sie alle früheren Versionen des Microsoft Advertising SDK und Ad Mediator SDK auf Ihrem Entwicklungscomputer.

2.    Öffnen Sie ein **Eingabeaufforderungsfenster**, und führen Sie diese Befehle aus, um alle SDK-Versionen zu löschen, die möglicherweise mit Visual Studio installiert wurden und nicht in der Liste der installierten Programme auf Ihrem Computer angezeigt werden:

  > [!div class="tabbedCodeSnippets"]
  ```syntax
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.    Installieren Sie das neueste SDK für Ihre App:
  * Installieren Sie für UWP-Apps unter Windows10 das [Microsoft Store Services SDK](http://aka.ms/store-services-sdk).
  * Installieren Sie für Apps, die für eine frühere Betriebssystemversion erstellt wurden, das [Microsoft Advertising SDK für Windows und Windows Phone 8.x](http://aka.ms/store-8-sdk).

## <a name="part-3-update-your-project"></a>Teil 3: Aktualisieren Ihres Projekts

Befolgen Sie diese Anweisungen, um Ihr Projekt zu aktualisieren.

### <a name="uwp-projects-for-windows-10"></a>UWP-Projekte für Windows 10

<span/>

Wenn Ihre App **AdMediatorControl** verwendet, [ändern Sie sie so, dass sie stattdessen AdControl nutzt](migrate-from-admediatorcontrol-to-adcontrol.md). **AdMediatorControl** wird für UWP-Apps nicht mehr unterstützt.

Wenn Ihre App **AdControl** verwendet, entfernen Sie alle vorhandenen Verweise auf Microsoft Advertising-Bibliotheken aus dem Projekt, und führen Sie zum Hinzufügen der erforderlichen Verweise die Anweisungen [AdControl in XAML](adcontrol-in-xaml-and--net.md) oder [AdControl in HTML](adcontrol-in-html-5-and-javascript.md) aus. Dadurch wird sichergestellt, dass Ihr Projekt die richtigen Bibliotheken verwendet. Sie können Ihr vorhandenes XAML-Markup und den Code beibehalten.

<span/>

### <a name="windows-81-or-windows-phone-81-xaml-or-javascripthtml-projects"></a>Windows8.1 oder Windows Phone8.1-Projekte (XAML oder JavaScript/HTML)

<span/>

1. Entfernen Sie alle „Microsoft.Advertising.\*“- und „Microsoft.AdMediator.\*“-Verweise aus Ihrem Projekt. Wenn Sie die Projektvorlage „Universal“ verwendet haben, verfügen Sie möglicherweise über zwei Verweise (eine für Windows und eine für Windows Phone).

2. Wenn Ihre App **AdMediatorControl** verwendet, fügen Sie die Bibliotheksverweise wieder ein, indem Sie die Anweisungen unter [Hinzufügen und Verwenden des Steuerelements für die Anzeigenvermittlung](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx) ausführen. Wenn Ihre App **AdControl** verwendet, fügen Sie die Bibliotheksverweise wieder ein, indem Sie die Anweisungen unter [AdControl in XAML](adcontrol-in-xaml-and--net.md) oder [AdControl in HTML](adcontrol-in-html-5-and-javascript.md) ausführen.

<span/>

Hinweis:

* Wenn Ihre App zuvor bereits zur **Any CPU**-Plattform kompiliert wurde, müssen Sie Ihr Projekt für eine architekturspezifische Plattform neu kompilieren (x86, x64 oder ARM).

* Wenn Sie über eine Windows Phone 8.x XAML-App verfügen, die zuvor eine SDK-Version verwendet hat, in der die Klasse **AdControl** im Namespace **Microsoft.Advertising.Mobile.UI** definiert wurde, müssen Sie Ihren Code so aktualisieren, dass er auf die Klasse **AdControl** im Namespace **Microsoft.Advertising.WinRT.UI** verweist (diese Klasse hat Namespaces in die neueren SDK-Versionen verschoben).

* Im Gegensatz zum zuvor beschriebenen Thema können Sie Ihr vorhandenes XAML-Markup und den Code jedoch beibehalten.

<span/>

### <a name="windows-phone-8x-silverlight-projects"></a>Windows Phone8.x Silverlight-Projekte

<span/>

1. Entfernen Sie alle „Microsoft.Advertising.\*“- und „Microsoft.AdMediator.\*“-Verweise aus Ihrem Projekt.

2. Wenn Ihre App **AdMediatorControl** verwendet, fügen Sie die Bibliotheksverweise wieder ein, indem Sie die Anweisungen unter [Hinzufügen und Verwenden des Steuerelements für die Anzeigenvermittlung](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx) ausführen. Wenn Ihre App **AdControl** verwendet, fügen Sie die Bibliotheksverweise wieder ein, indem Sie die Anweisungen unter [AdControl in Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md) ausführen.

<span/>

Hinweis:

* Sie können Ihr vorhandenes XAML-Markup und den Code beibehalten.

* Prüfen Sie im **Projektmappen-Explorer** die Eigenschaften für den Verweis **Microsoft.Advertising.Mobile.UI** in Ihrem Projekt. Wenn Ihre App für Windows Phone 8.0 entwickelt wurde, ist 6.2.40501.0 die korrekte Version; wenn die App für Windows Phone 8.1 entwickelt wurde, sollte die Version 8.1.50112.0 lauten.

* Für Windows Phone 8.x Silverlight-Apps wird das Testen von Produktionseinheiten auf einem Emulator nicht unterstützt. Daher wird das Testen auf einem Gerät empfohlen.

## <a name="part-4-test-and-republish-your-app"></a>Teil 4: Testen und erneutes Veröffentlichen Ihrer App

Testen Sie Ihre App, um sicherzustellen, dass sie Banneranzeigen korrekt anzeigt.

Wenn die vorherige Version Ihrer App bereits im Store verfügbar ist, erstellen Sie im Windows Dev Center-Dashboard [eine neue Übermittlung](../publish/app-submissions.md) für Ihre aktualisierte App, um diese erneut zu veröffentlichen.





 