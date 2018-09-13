---
author: andrewleader
Description: Notifications Visualizer is a new Universal Windows Platform (UWP) app in the Store that helps developers design adaptive live tiles for Windows 10.
title: Notifications Visualizer
ms.assetid: FCBB7BB1-2C79-484B-8FFC-26FE1934EC1C
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: af8b2489346e1ef81c5cae304802814b79b8b950
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "3960384"
---
# <a name="notifications-visualizer"></a>Notifications Visualizer

 


Notifications Visualizer ist eine neue App für die Universelle Windows-Plattform (UWP) im [Store](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1), die Entwickler dabei unterstützt, adaptive Livekacheln und interaktive Popupbenachrichtigungen für Windows 10 zu entwerfen.


## <a name="overview"></a>Übersicht

Die App „Notifications Visualizer” bietet beim Bearbeiten der XML-Nutzlast eine sofortige visuelle Vorschau Ihrer Kachel und Popupbenachrichtigung, vergleichbar mit dem XAML-Editor/der Designansicht von Visual Studio. Die App prüft auch auf Fehler. So wird sichergestellt, dass Sie eine gültige Kachel- oder Popupbenachrichtigungsnutzlast erstellen.

Dieser Screenshot der App zeigt die XML-Nutzlast und die Art und Weise, wie Kachelgrößen auf einem bestimmten Gerät angezeigt werden:

![Screenshot des App-Editors von Notifications Visualizer mit Code und Kacheln](images/notif-visualizer-001.png)

 

Mit Notifications Visualizer können Sie adaptive Kachel-oder Popupnutzlasten erstellen und testen, ohne Ihre App selbst bearbeiten und bereitstellen zu müssen. Nachdem Sie eine Nutzlast mit idealen visuellen Ergebnisse erstellt haben, können Sie sie in Ihre App integrieren. Informationen finden Sie unter [Senden einer lokalen Kachelbenachrichtigung](sending-a-local-tile-notification.md) und [Senden einer lokalen Popupbenachrichtigung](send-local-toast.md).

**Hinweis**   Die Simulation des Windows-Startmenüs und der Popupbenachrichtigung mit Notifications Visualizer ist nicht immer hundertprozentig genau, und bestimmte erweiterte Nutzlasteigenschaften werden nicht unterstützt. Wenn Sie das gewünschte Kachel- oder Popupdesign fertig haben, testen Sie es, indem Sie die Kachel oder Popupbenachrichtigung an das tatsächliche Startmenü anheften, um sicherzustellen, dass es wie gewünscht angezeigt wird.

 

## <a name="features"></a>Features

Notifications Visualizer enthält eine Reihe von Beispielnutzlasten, um zu zeigen, was mit adaptiven Live-Kacheln und interativen Popups möglich ist und um Sie bei den ersten Schritten zu unterstützen. Sie können mit den verschiedenen Textoptionen, Gruppen/Untergruppen, Hintergrundbildern experimentieren und sehen, wie sich die Kachel an verschiedene Geräte und Bildschirme anpasst. Wenn Sie Änderungen vornehmen, können Sie Ihre aktualisierte Nutzlast in einer Datei zur späteren Verwendung speichern.

Der Editor stellt Fehler und Warnungen in Echtzeit bereit. Wenn Ihre App-Nutzlast beispielsweise größer als 5KB (eine Plattformbeschränkung) ist, warnt Notifications Visualizer Sie, falls Ihre Nutzlast diese Grenze überschreitet. Sie erhalten Warnungen aufgrund falscher Attributnamen oder Werte, wodurch das Debuggen visueller Probleme erleichtert wird.

Sie können Kacheleigenschaften, wie Anzeigename, Farbe, Logos, ShowName und Signalwert, steuern. Anhand dieser Optionen verstehen Sie sofort, wie Ihre Kacheleigenschaften und Kachelbenachrichtigungsnutzlasten interagieren und welche Ergebnisse sie produzieren.

Dieser Screenshot der App zeigt den Kachel-Editor:

![Screenshot des Notifications Visualizer-Editors mit Kacheln](images/notif-visualizer-004.png)

 

## <a name="related-topics"></a>Verwandte Themen

* [Notifications Visualizer aus dem Store herunterladen](https://www.microsoft.com/store/apps/notifications-visualizer/9nblggh5xsl1)
* [Erstellen adaptiver Kacheln](create-adaptive-tiles.md)
* [Interaktive Popupbenachrichtigungen](adaptive-interactive-toasts.md)