---
author: QuinnRadich
Description: Learn how to use page transitions in your UWP apps.
title: Seitenübergänge in UWP-Apps
template: detail.hbs
ms.author: quradic
ms.date: 04/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: stmoy
ms.localizationpriority: medium
ms.openlocfilehash: 0afc2c55ab0d0bdd2bee0206f986b2724d331eaf
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "3958588"
---
# <a name="page-transitions"></a>Seitenübergänge

Seitenübergänge sind Animationen, die abgespielt werden, wenn Benutzer zwischen Seiten in einer App navigieren und Feedback als Beziehung zwischen Seiten liefern. Seitenübergänge zeigen dem Benutzer, ob er an der Spitze einer Navigationshierarchie steht, zwischen Geschwisterseiten wechselt oder tiefer in die Seitenhierarchie navigiert.

Für die Navigation zwischen Seiten in einer App stehen zwei verschiedene Animationen zur Verfügung: *Seitenaktualisierung* und *Drill*. Sie werden durch Unterklassen von [**NavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo) dargestellt.

## <a name="page-refresh"></a>Seite aktualisieren

Seitenaktualisierung ist eine Kombination aus einer Slide-Up-Animation und einer Einblendungsanimation für den eingehenden Inhalt. Verwenden Sie die Seitenaktualisierung, wenn der Benutzer an den Anfang eines Navigationsstapels gebracht wird, z. B. beim Navigieren zwischen Registerkarten oder Navigationselementen auf der linken Navigationsleiste.

Der gewünschte Effekt ist ein Neubeginn für den Benutzer.

![Seitenaktualisierungsanimation](images/page-refresh.gif)

Die Seitenaktualisierungsanimation wird durch die Klasse [**EntranceNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo) dargestellt.

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**Hinweis:**: Ein [**Frame**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.frame) verwendet automatisch [**NavigationThemeTransition**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) zur Animation der Navigation zwischen zwei Seiten. Standardmäßig ist die Animation eine Seitenaktualisierung.

## <a name="drill"></a>Drill

Verwenden Sie Drill, wenn Benutzer tiefer in eine Anwendung navigieren, z. B. um nach der Auswahl eines Elements weitere Informationen anzuzeigen.

Der gewünschte Effekt ist, dass der Benutzer tiefer in die App vorgedrungen ist.

![Drill-Animation](images/drill.gif)

Die Drill-Animation wird durch die Klasse [**DrillInNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) dargestellt.

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="suppress"></a>Unterdrückung

Um die Wiedergabe von Animationen während der Navigation zu vermeiden, verwenden Sie [**SuppressNavigationTransitionInfo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) anstelle anderer **NavigationTransitionInfo**-Subtypen.

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

Das Unterdrücken der Animation ist hilfreich, wenn Sie Ihre eigenen Übergang mit [verbundenen Animationen](connected-animation.md) erstellen oder das implizite Einblenden/Ausblenden von Animationen nutzen.

## <a name="backwards-navigation"></a>Rückwärtsnavigation

Um einen bestimmten Übergang bei der Rückwärtsnavigation darzustellen, verwenden Sie `Frame.GoBack(NavigationTransitionInfo)`.

Dies kann nützlich sein, wenn Sie das Navigationsverhalten dynamisch an die Bildschirmgröße anpassen – z. B. in einem dynamischen Master/Detail-Szenario.

## <a name="related-topics"></a>Verwandte Themen

- [Navigieren zwischen zwei Seiten](../basics/navigate-between-two-pages.md)
- [Bewegung in UWP-Apps](index.md)