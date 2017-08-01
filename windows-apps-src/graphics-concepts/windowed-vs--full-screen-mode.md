---
title: Vergleich von Vollbild- und Fenstermodus
description: "Direct3D-Anwendungen können jeweils entweder im Fenstermodus oder im Vollbildmodus ausgeführt werden."
ms.assetid: EE8B9F87-822B-4576-A446-CA603E786862
keywords: Vergleich von Vollbild- und Fenstermodus
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.openlocfilehash: 91b488dce85ed173584fdb9873884f9f36689858
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="span-iddirect3dconceptswindowedvsfull-screenmodespanwindowed-vs-full-screen-mode"></a><span id="direct3dconcepts.windowed_vs__full-screen_mode"></span>Vergleich von Vollbild- und Fenstermodus


Direct3D-Anwendungen können jeweils entweder im Fenstermodus oder im Vollbildmodus ausgeführt werden. Im *Fenstermodus* teilt sich die Anwendung die verfügbare Bildschirmfläche auf dem Desktop mit allen anderen ausgeführten Programmen. Im *Vollbildmodus* bedeckt das Fenster der Anwendung den gesamten Desktop, alle anderen ausgeführten Programme (einschließlich der Entwicklungsumgebung) werden ausgeblendet. Normalerweise werden beispielsweise Spiele standardmäßig im Vollbildmodus ausgeführt, damit der Benutzer ganz in das Spiel eintauchen kann, ohne durch die anderen Anwendungen abgelenkt zu werden.

Die Codeunterschiede zwischen Vollbildmodus und Fenstermodus sind äußert gering.

Da eine Anwendung, die im Vollbildmodus ausgeführt wird, den gesamten Bildschirm ausfüllt, wird für das Debuggen der Anwendung entweder ein zweiter Bildschirm benötigt oder ein Remotedebugger. Ein Vorteil von Anwendungen im Fenstermodus ist, dass Sie den Code in einem Debugger schrittweise ausführen lassen können, ohne einen zweiten Bildschirm oder einen Remotedebugger verwenden zu müssen.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Verwandte Themen


[Geräte](devices.md)

 

 



