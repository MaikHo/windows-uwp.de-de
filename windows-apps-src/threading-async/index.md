---
ms.assetid: beac6333-655a-4bcf-9caf-bba15f715ea5
title: Threading und asynchrone Programmierung
description: Mithilfe von Threading und der asynchronen Programmierung kann Ihre App Aufgaben asynchron in parallelen Threads ausführen.
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP, asynchron, Threads, Threading
ms.localizationpriority: medium
ms.openlocfilehash: 22c151b90be30b39da7decd9a0ce3109e29b7fb7
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "63813162"
---
# <a name="threading-and-async-programming"></a>Threading und asynchrone Programmierung
Mithilfe von Threading und der asynchronen Programmierung kann Ihre App Aufgaben asynchron in parallelen Threads ausführen.

Eine App kann den Threadpool verwenden, um Aufgaben asynchron in parallelen Threads auszuführen. Der Threadpool verwaltet eine Reihe von Threads und verwendet eine Warteschlange, um den einzelnen Threads, sobald sie verfügbar werden, Arbeitselemente zuzuweisen. Der Threadpool ähnelt den in der Windows-Runtime verfügbaren asynchronen Programmierungsmustern, da er zur Verarbeitung umfangreicher Aufgaben verwendet werden kann, ohne die Benutzeroberfläche zu blockieren. Der Threadpool bietet jedoch mehr Kontrolle als die asynchronen Programmierungsmuster, und mit dem Threadpool können Sie mehrere Arbeitselemente parallel bearbeiten. Mit dem Threadpool können Sie die folgenden Aktionen ausführen:

-   Arbeitselemente übermitteln, deren Prioritäten steuern und Arbeitselemente abbrechen

-   Arbeitselemente mit Timern und regelmäßigen Timern planen

-   Ressourcen für wichtige Arbeitselemente reservieren

-   Arbeitselemente als Reaktion auf benannte Ereignisse und Semaphoren ausführen

Der Threadpool bietet eine höhere Effizienz beim Verwalten von Threads, da der Mehraufwand zum Erstellen und Verwerfen von Threads reduziert wird. Das bedeutet, er verfügt über Zugriffsmöglichkeiten zum Optimieren von Threads über mehrere CPU-Kerne hinweg und kann Threadressourcen zwischen Apps und Hintergrundaufgaben ausgleichen. Die Verwendung des integrierten Threadpools erleichtert Ihnen die Arbeit, da Sie sich auf das Schreiben von Code für die Ausführung einer Aufgabe konzentrieren können, anstatt sich um die Abläufe der Threadverwaltung kümmern zu müssen.

| Thema                                                                                                          | Beschreibung                         |
|----------------------------------------------------------------------------------------------------------------|-------------------------------------|
| [Asynchrone Programmierung (UWP-Apps)](asynchronous-programming-universal-windows-platform-apps.md)              | In diesem Thema werden die asynchrone Programmierung auf der universellen Windows-Plattform (UWP) und ihre Darstellung in C#, Microsoft Visual Basic .NET, Visual C++-Komponentenerweiterungen (C++/CX) und JavaScript erläutert. |
| [Asynchrone Programmierung in C++/CX (UWP-Apps)](asynchronous-programming-in-cpp-universal-windows-platform-apps.md)| In diesem Artikel wird die empfohlene Vorgehensweise zur Verwendung asynchroner Methoden in C++/CX mithilfe der <code>task</code><code>concurrency</code>-Klasse beschrieben, die im -Namespace in „ppltasks.h“ definiert wird. |
| [Bewährte Methoden zum Verwenden des Threadpools](best-practices-for-using-the-thread-pool.md)                         | In diesem Thema werden bewährte Methoden für die Verwendung des Threadpools beschrieben. |
| [Aufrufen asynchroner APIs in C# oder Visual Basic](call-asynchronous-apis-in-csharp-or-visual-basic.md)             | Die Universelle Windows-Plattform (UWP) enthält viele asynchrone APIs. Diese sorgen dafür, dass Ihre App reaktionsfähig bleibt, wenn sie über einen längeren Zeitraum mit einer Aufgabe beschäftigt ist. In diesem Thema wird die Verwendung asynchroner Methoden aus der Universellen Windows-Plattform (UWP) in C# oder Microsoft Visual Basic erläutert. |
| [Erstellen einer regelmäßigen Arbeitsaufgabe](create-a-periodic-work-item.md)                                                   | Hier erfahren Sie, wie Sie ein Arbeitselement erstellen, die regelmäßig wiederholt wird. |
| [Senden einer Arbeitsaufgabe an den Threadpool](submit-a-work-item-to-the-thread-pool.md)                               | Hier erfahren Sie, wie Sie Arbeit in einem separaten Thread erledigen können, indem Sie ein Arbeitselement an den Threadpool übermitteln. |
| [Senden einer Arbeitsaufgabe mithilfe eines Timers](use-a-timer-to-submit-a-work-item.md)                                       | Hier erfahren Sie, wie Sie ein Arbeitselement erstellen, die nach dem Ablaufen eines Timers ausgeführt wird. |
