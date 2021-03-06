---
Description: Legen Sie fest, wie lange eine Spracherkennung Stille oder nicht erkennbare Geräusche (Störgeräusche) ignoriert und weiterhin auf Spracheingabe wartet.
title: Festlegen von Timeouts für die Spracherkennung
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: Sprache, Stimme, Spracherkennung, natürliche Sprache, diktieren, Eingabe, Benutzerinteraktion
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c68b8aeb71ed4269b3a7fc52c6b616a8b0760b0b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173324"
---
# <a name="set-speech-recognition-timeouts"></a>Festlegen von Timeouts für die Spracherkennung


Legen Sie fest, wie lange eine Spracherkennung Stille oder nicht erkennbare Geräusche (Störgeräusche) ignoriert und weiterhin auf Spracheingabe wartet.

> **Wichtige APIs**: [**Timeouts**](/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts), [**reden erkentungen**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognizerTimeouts)

## <a name="set-a-timeout"></a>Festlegen eines Timeouts


Hier geben wir verschiedene [**Timeouts**](/uwp/api/windows.media.speechrecognition.speechrecognizer.timeouts)-Werte an:

-   InitialSilenceTimeout – Die Zeitspanne, für die ein Spracherkennungsmodul Stille erkennt (vor Generierung etwaiger Erkennungsergebnisse) und davon ausgeht, dass keine Spracheingabe erfolgen wird.
-   BabbleTimeout – Die Zeitspanne, für die ein Spracherkennungsmodul weiterhin auf erkennbare Geräusche (Störgeräusche) wartet, bevor davon ausgegangen wird, dass die Spracheingabe beendet ist, und der Erkennungsvorgangs beendet wird.
-   EndSilenceTimeout – Die Zeitspanne, für die das Spracherkennungsmodul Stille erkennt (nach Generierung von Erkennungsergebnissen) und davon ausgeht, dass die Spracheingabe beendet ist.

**Hinweis**    Timeouts können pro Erkennung festgelegt werden.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>Verwandte Artikel

* [Sprachinteraktionen](speech-interactions.md)

**Beispiele**

* [Beispiel zu Spracherkennung und Sprachsynthese](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)