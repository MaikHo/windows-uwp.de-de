---
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: Erfahren Sie mehr über bekannte Probleme bei der aktuellen Version des Microsoft Advertising SDK.
title: Bekannte Probleme und Problembehandlung für ADS in apps
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, UWP, Werbung, Werbung, bekannte Probleme, Problembehandlung
ms.localizationpriority: medium
ms.openlocfilehash: c69c61cc1db0796edbaedb2f8e2970e1100c5774
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158534"
---
# <a name="known-issues-and-troubleshooting-for-ads-in-apps"></a>Bekannte Probleme und Problembehandlung für ADS in apps

>[!WARNING]
> Ab dem 1. Juni 2020 wird die Microsoft AD-Monetarisierungsplattform für Windows UWP-apps heruntergefahren. [Weitere Informationen](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

In diesem Thema werden die bekannten Probleme mit der aktuellen Version des Microsoft Advertising SDK aufgelistet. Weitere Anleitungen zur Problembehandlung finden Sie in den folgenden Themen.

* [Handbuch zur Problembehandlung für HTML und JavaScript](html-and-javascript-troubleshooting-guide.md)
* [XAML- und C#-Handbuch zur Problembehandlung](xaml-and-c-troubleshooting-guide.md)

## <a name="adcontrol-interface-unknown-in-xaml"></a>AdControl-Schnittstelle in XAML nicht bekannt

Das XAML-Markup für eine [AdControl](/uwp/api/microsoft.advertising.winrt.ui.adcontrol) zeigt möglicherweise fälschlicherweise eine blaue Kurvenlinie an, die anzeigt, dass die Schnittstelle nicht bekannt ist. Dies geschieht nur im Zusammenhang mit x86 und kann ignoriert werden.

## <a name="lasterror-from-previous-ad-request"></a>„lastError“ aus vorheriger Anzeigenanforderung

Wenn es einen **lastError** aus der vorherigen Anzeigenanforderung gibt, wird das Ereignis möglicherweise während des nächsten Anzeigenaufrufs zweimal ausgelöst. Obwohl die neue Anzeigenanforderung dennoch ausgeführt werden kann und möglicherweise zu einer gültigen Anzeige führt, kann dieses Verhalten zu Verwirrung führen.

## <a name="interstitial-ads-and-navigation-buttons-on-phones"></a>Interstitielle Anzeigen und Navigationsschaltflächen auf Telefonen

Auf Telefonen (oder Emulatoren **), die**über die Schaltflächen "Software", " **starten**" und " **Suchen** " anstelle von Hardware Schaltflächen verfügen, können der Countdowntimer und die Schaltflächen "durch Klicken" für Interstitial

## <a name="recently-created-ads-are-not-being-served-to-your-app"></a>Vor kurzem erstellte Anzeigen werden Ihrer App nicht bereitgestellt

Wenn Sie vor kurzem (weniger als einem Tag) eine Anzeige erstellt haben, ist diese möglicherweise nicht sofort verfügbar. Wenn die Anzeige hinsichtlich ihrer redaktionellen Inhalte genehmigt wurde, wird sie bereitgestellt, nachdem der Anzeigenserver sie verarbeitet hat und die Anzeige als Bestand verfügbar ist.

## <a name="no-ads-are-shown-in-your-app"></a>In Ihrer App werden keine Anzeigen angezeigt

Es gibt viele Gründe, warum möglicherweise keine Anzeigen angezeigt werden, einschließlich Netzwerkfehlern. Andere Gründe können sein:

* Wählen Sie eine Ad-Einheit im Partner Center mit einer Größe aus, die größer oder kleiner als die Größe der **adcontrol** im Code Ihrer APP ist.

* Anzeigen werden nicht angezeigt, wenn Sie einen [Testmoduswert](set-up-ad-units-in-your-app.md#test-ad-units) für Ihre Anzeigeneinheiten-ID verwenden, wenn eine Live-App ausgeführt wird.

* Wenn Sie in der letzten halben Stunde eine neue Anzeigeneinheiten-ID erstellt haben, wird eine Anzeige möglicherweise erst angezeigt, wenn der Server neue Daten durch das System propagiert hat. Vorhandene IDs, die zuvor bereits Anzeigen angezeigt haben, sollten Anzeigen sofort anzeigen.

Wenn Sie in der App Testanzeigen sehen können, funktioniert Ihr Code und kann Anzeigen anzeigen. Bei Problemen wenden Sie sich an den [Produktsupport](https://developer.microsoft.com/windows/support). Wählen Sie auf dieser Seite die Option **kontaktieren Sie uns**.

Sie können auch im [Forum](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps) eine Frage stellen.

## <a name="test-ads-are-showing-in-your-app-instead-of-live-ads"></a>In Ihrer App werden Testanzeigen anstelle von Liveanzeigen angezeigt.

Testanzeigen können angezeigt werden, auch wenn Sie Liveanzeigen erwarten. Dies kann in den folgenden Situationen geschehen:

* Unsere Werbeplattform kann die im Store verwendete Live-Anwendungs-ID nicht überprüfen oder ermitteln. Wenn eine Anzeigeneinheit von einem Benutzer erstellt wird, kann in diesem Fall der Status als live (Nicht-Test) beginnen, jedoch innerhalb von 6 Stunden nach der ersten Anzeigenanforderung in den Teststatus wechseln. Er wechselt zurück zum Livestatus, wenn es 10 Tage keine Anforderungen von Test-Apps gibt.

* Quergeladene Apps oder im Emulator ausgeführte Apps zeigen keine Liveanzeigen an.

Wenn eine aktive Ad-Einheit Test anzeigen bedient, wird der Status der Ad-Einheit aktiv angezeigt, und es werden **Test anzeigen** in Partner Center bedient. Dies gilt zurzeit nicht für Telefon-Apps.


<span id="reference_errors"/>

## <a name="reference-errors-caused-by-targeting-any-cpu-in-your-project"></a>Referenzfehler, die durch die Ausrichtung auf eine beliebige CPU (Any CPU) in Ihrem Projekt verursacht werden

Wenn Sie das Microsoft Advertising SDK verwenden, können Sie im Projekt **keine CPU** als Ziel verwenden. Wenn Ihr Projekt auf die Plattform **Any CPU** ausgerichtet ist, wird Ihnen möglicherweise eine Warnung angezeigt, nachdem Sie einen Verweis wie diesen hinzugefügt haben.

![ReferenceError \- SolutionExplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

Um diese Warnung zu entfernen, müssen Sie eine architekturspezifische Buildausgabe verwenden (beispielsweise **x86**) und das Projekt entsprechend aktualisieren. Verwenden Sie den **Konfigurations-Manager**, um die Plattformziele für Debug- und Releasekonfigurationen festzulegen.

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

Achten Sie darauf, die beabsichtigten Architekturen einzuschließen, wenn Sie App-Pakete für die Übermittlung an den Store erstellen (wie in den folgenden Bildern gezeigt). Sie können x64 auslassen, wenn Sie x86-Builds auf dem x64-Betriebssystem ausführen möchten.

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## <a name="z-order-in-javascripthtml-apps"></a>Z-Reihenfolge in JavaScript/HTML-Apps

JavaScript/HTML-Apps müssen Elemente nicht in den reservierten MAX 10-Bereich der Z-Reihenfolge platzieren. Die einzigen Ausnahmen sind Interrupt-Overlays wie eingehende Anrufbenachrichtigungen für Skype-Apps.

<span id="bkmk-ui"/>

## <a name="do-not-use-borders"></a>Verwenden Sie keine Rahmen

Die Festlegung von Rahmeneigenschaften, die **AdControl** von seiner übergeordneten Klasse erbt, führt zu einer falschen Platzierung der Anzeige.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu den neuesten bekannten Problemen und zum Veröffentlichen von Fragen im Zusammenhang mit dem Microsoft Advertising SDK finden Sie im [Forum](https://social.msdn.microsoft.com/forums/windowsapps/en-US/home?category=windowsapps).

 

 