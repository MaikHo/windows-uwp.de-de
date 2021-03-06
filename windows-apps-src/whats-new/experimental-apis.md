---
title: Experimentelle APIs
description: Erfahren Sie, wie Entwickler experimentelle APIs mithilfe von Test-Flight unter Verwendung von Windows Insider-SDKs ausprobieren können.
ms.date: 11/13/2017
ms.topic: article
keywords: Windows 10, UWP, experimentell, API
ms.localizationpriority: medium
ms.openlocfilehash: 5a4813e7b4ae1e3dd16017066758aa8a35d0570a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170804"
---
# <a name="experimental-apis"></a>Experimentelle APIs

Experimentelle APIs befinden sich in der Anfangsphase des Entwurfs und ändern sich wahrscheinlich, wenn die Besitzer Feedback einarbeiten und Unterstützung für zusätzliche Szenarien hinzufügen.

Für diese APIs werden externe Test-Flights mit [Windows-Insider-SDKs](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK) zur Verfügung gestellt, damit Entwickler sie ausprobieren und Feedback bereitstellen können, bevor sie Teil der offiziellen Plattform werden. Während der Test-Flight-Phase können Stabilität und Verbindlichkeit nicht garantiert werden.

## <a name="consuming-experimental-apis"></a>Nutzung experimenteller APIs
IntelliSense informiert dich, ob eine API experimentell ist. Außerdem erhältst du eine Compilerwarnung, wenn du eine experimentelle API verwendest, z. B. „... ist nur zu Testzwecken vorgesehen und kann in zukünftigen Updates geändert oder entfernt werden“.

Diese Warnungen schützt dich vor der Erstellung von Abhängigkeiten von experimentellen APIs in Produktionsprojekten. Bei experimentellen Projekten kannst du diese Warnungen ignorieren oder deaktivieren.

Standardmäßig sind diese APIs zur Laufzeit deaktiviert, und ein Aufruf der APIs führt zu einer Laufzeitausnahme. Das ist eine weitere Vorsichtsmaßnahme, um unbeabsichtigte Abhängigkeiten und eine umfangreiche Verteilung von Apps zu verhindern, die experimentelle APIs nutzen.

Um diese APIs für experimentelle Zwecke zu aktivieren, verwendest du das Plug-In für Features des [Windows-Geräteportals (Windows Device Portal, WDP)](../debug-test-perf/device-portal.md), um das Feature zu aktivieren, das der aufzurufenden API entspricht.

Für die Dokumentation der jeweiligen experimentellen API ist das entsprechende Team zuständig.

## <a name="providing-feedback"></a>Abgeben von Feedback

Wenn du eine experimentelle API ausprobiert hast und Feedback dazu abgeben möchten, verwende die Kategorie **Entwicklerplattform** in [Windows-Feedback-Hub](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub).