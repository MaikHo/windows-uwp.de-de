---
description: Ein XAML-NameScope speichert Beziehungen zwischen den definierten XAML-Namen von Objekten und ihren entsprechenden Instanzen. Dies ist vergleichbar mit der weiteren Bedeutung des Begriffs NameScope in anderen Programmiersprachen und Technologien.
title: XAML-Namescopes
ms.assetid: EB060CBD-A589-475E-B83D-B24068B54C21
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e4fa430cdd6c2f7b47576478ec30f0f139b80363
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173754"
---
# <a name="xaml-namescopes"></a>XAML-Namescopes


Ein *XAML-NameScope* speichert Beziehungen zwischen den definierten XAML-Namen von Objekten und ihren entsprechenden Instanzen. Dieses Konzept ähnelt der umfassenderen Bedeutung des Begriffs " *Namescope* " in anderen Programmiersprachen und-Technologien.

## <a name="how-xaml-namescopes-are-defined"></a>Definieren von XAML-NameScopes

Die Namen in XAML-NameScopes aktivieren Benutzercode, der auf die Objekte verweist, die ursprünglich in XAML definiert wurden. Das interne Ergebnis der XAML-Analyse ist das Erstellen eines Satzes von Objekten, in dem einige oder alle Beziehungen dieser Objekte in den XAML-Deklarationen beibehalten werden. Diese Beziehungen werden als spezifische Objekteigenschaften der erstellten Objekte beibehalten oder für Hilfsmethoden in den Programmiermodell-APIs verfügbar gemacht.

Die typischste Verwendung eines Namens in einem XAML-NameScope ist der direkte Verweis auf eine Objektinstanz, die vom Markupkompilierungsschritt aktiviert wird, in Kombination mit einer generierten **InitializeComponent**-Methode in den partiellen Klassenvorlagen.

Sie können auch die [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname)-Hilfsmethode zur Laufzeit verwenden, um einen Verweis auf Objekte zurückzugeben, die mit einem Namen im XAML-Markup definiert wurden.

### <a name="more-about-build-actions-and-xaml"></a>Mehr zu Buildaktionen und XAML

Technisch gesehen wird für das XAML selbst ein Markupcompilerdurchlauf durchgeführt, während gleichzeitig das XAML und die darin für CodeBehind definierte partielle Klasse gemeinsam kompiliert werden. Jedes Objektelement, für das im Markup ein **Name**- oder [x:Name-Attribut](x-name-attribute.md) definiert wurde, generiert ein internes Feld mit einem Namen, der mit dem XAML-Namen übereinstimmt. Dieses Feld ist anfänglich leer. Die Klasse generiert dann eine **InitializeComponent**-Methode, die erst aufgerufen wird, wenn das gesamte XAML geladen wurde. Anschließend werden durch die **InitializeComponent**-Logik alle internen Felder mit dem [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname)-Rückgabewert für die entsprechende Namenszeichenfolge aufgefüllt. Sie können diese Infrastruktur selbst erkennen, indem Sie sich die Dateien mit der Erweiterung ".g" (generiert) ansehen, die für die einzelnen XAML-Seiten nach der Kompilierung im /obj-Unterordner eines Projekts für eine Windows-Runtime-App erstellt werden. Sie können die Felder und die **InitializeComponent**-Methode auch als Member der erstellten Assemblys anzeigen, wenn Sie eine Reflexion über diese ausführen oder in anderer Weise ihre MSIL-Inhalte untersuchen.

**Hinweis**    Speziell für Visual C++ Komponenten Erweiterungen (C++/CX)-apps wird ein dahinter liegendes Feld für einen **x:Namensverweis** nicht für das Stamm Element einer XAML-Datei erstellt. Wenn Sie aus CodeBehind für C++/CX auf das Stammobjekt verweisen müssen, verwenden Sie andere APIs oder eine Strukturausnahme. Sie können beispielsweise erst [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) für ein bekanntes benanntes Unterelement und anschließend [**Parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent) aufrufen.

## <a name="creating-objects-at-run-time-with-xamlreaderload"></a>Erstellen von Objekten zur Laufzeit mit XamlReader.Load

XAML kann auch als Zeichenfolgeneingabe für die [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load)-Methode verwendet werden, die analog zum ursprünglichen XAML-Quellanalysevorgang funktioniert. **XamlReader.Load** erstellt zur Laufzeit eine neue getrennte Struktur von Objekten. Die getrennte Struktur kann dann an die Hauptobjektstruktur angefügt werden. Sie müssen die erstellte Objektstruktur explizit verbinden, entweder durch Hinzufügen zu einer Inhaltseigenschaftenauflistung wie **Children** oder durch Festlegen einer anderen Eigenschaft, die einen Objektwert akzeptiert (z. B. Laden eines neuen [**ImageBrush**](/uwp/api/Windows.UI.Xaml.Media.ImageBrush) für einen [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill)-Eigenschaftswert).

### <a name="xaml-namescope-implications-of-xamlreaderload"></a>XAML-NameScope-Auswirkungen von XamlReader.Load

Der vorläufige XAML-NameScope, der durch die neue mit [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) erstellte Objektstruktur definiert wird, wertet alle definierten Namen im bereitgestellten XAML auf Eindeutigkeit hin aus. Wenn das bereitgestellte XAML zu diesem Zeitpunkt intern nicht eindeutige Namen enthält, wird durch **XamlReader.Load** eine Ausnahme ausgelöst. Beim Verbinden der getrennten Objektstruktur mit der Hauptobjektstruktur der Anwendung wird nicht versucht, den zugehörigen XAML-NameScope mit dem XAML-Haupt-NameScope der Anwendung zusammenzuführen. Nach dem Verbinden der Strukturen weist die App eine einheitliche Objektstruktur auf, die jedoch einzelne XAML-NameScopes enthält. Die Trennungen treten an den Verbindungspunkten zwischen Objekten auf, an denen Sie Eigenschaften auf den Wert festlegen, der von einem **XamlReader.Load**-Aufruf zurückgegeben wird.

Das Problem mit einzelnen und getrennten XAML-NameScopes besteht darin, dass Aufrufe der [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname)-Methode sowie direkte verwaltete Objektverweise nicht mehr für einen einheitlichen XAML-NameScope durchgeführt werden können. Stattdessen impliziert das bestimmte Objekt, für das **FindName** aufgerufen wird, den Bereich. Dabei ist der Bereich der XAML-NameScope, in dem sich das aufrufende Objekt befindet. Bei direkten verwalteten Objektverweisen wird der Bereich durch die Klasse impliziert, die den Code enthält. In der Regel ist der CodeBehind für Laufzeitinteraktionen einer "Seite" mit App-Inhalt in der partiellen Klasse vorhanden, auf der die "Seite" für den Stamm basiert. Aus diesem Grund fungiert der XAML-NameScope XAML-Stamm-NameScope.

Wenn Sie [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) aufrufen, um ein benanntes Objekt aus dem XAML-Stamm-NameScope abzurufen, findet die Methode keine Objekte aus einem mit [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) erstellten einzelnen XAML-NameScope. Umgekehrt findet die Methode keine benannten Objekte im XAML-Stamm-NameScope, wenn Sie **FindName** von einem Objekt aus aufrufen, das sich in einem einzelnen XAML-NameScope befindet.

Dieses Problem mit einzelnen XAML-NameScopes wirkt sich nur auf Objektsuchen nach Namen in XAML-NameScopes aus, die mithilfe des [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname)-Aufrufs durchgeführt werden.

Um Verweise auf Objekte abzurufen, die in einem anderen XAML-NameScope definiert werden, stehen mehrere Vorgehensweisen zur Verfügung:

-   Durchlaufen Sie die gesamte Struktur in separaten Schritten mit über [**geordneten**](/uwp/api/windows.ui.xaml.frameworkelement.parent) und/oder Auflistungs Eigenschaften, die bekanntermaßen in der Objektstruktur vorhanden sind (z. b. die Auflistung, die von [**Panel. Children**](/uwp/api/windows.ui.xaml.controls.panel.children)zurückgegeben wurde).
-   Wenn Sie den Aufruf von einem einzelnen XAML-NameScope aus durchführen und auf den XAML-Stamm-NameScope abzielen, bietet es sich an, einen Verweis auf das gegenwärtig angezeigte Hauptfenster abzurufen. Sie können den visuellen Stamm (das XAML-Stammelement, das auch als Inhaltsquelle bezeichnet wird) aus der aktuellen Anwendung in einer Codezeile mit dem `Window.Current.Content`-Aufruf abrufen. Sie können dann eine Umwandlung in [**FrameworkElement**](/uwp/api/Windows.UI.Xaml.FrameworkElement) durchführen und von diesem Bereich aus [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) aufrufen.
-   Wenn Sie den Aufruf vom XAML-Stamm-NameScope aus ausführen und auf ein Objekt in einem einzelnen XAML-NameScope abzielen, bietet es sich an, in Ihrem Code vorauszuplanen und einen Verweis auf das Objekt beizubehalten, das von [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) und anschließend der Hauptobjektstruktur hinzugefügt wurde. Dieses Objekt ist jetzt ein gültiges Objekt zum Aufrufen von [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) innerhalb des einzelnen XAML-NameScopes. Speichern Sie dieses Objekt als globale Variable, oder übergeben Sie es mit Methodenparametern.
-   Sie können Probleme hinsichtlich Namen und XAML-NameScope vermeiden, indem Sie die visuelle Struktur untersuchen. Die [**VisualTreeHelper**](/uwp/api/Windows.UI.Xaml.Media.VisualTreeHelper)-API ermöglicht es Ihnen, die visuelle Struktur im Hinblick auf übergeordnete Objekte und untergeordnete Auflistungen auf Grundlage von Position und Index zu durchlaufen.

## <a name="xaml-namescopes-in-templates"></a>XAML-NameScopes in Vorlagen

Mithilfe von Vorlagen in XAML können Sie Inhalt auf einfache Weise wiederverwenden und erneut anwenden. Vorlagen können jedoch auch Elemente mit Namen enthalten, die auf Vorlagenebene definiert wurden. Dieselbe Vorlage wird möglicherweise mehrmals auf einer Seite verwendet. Deshalb definieren Vorlagen ihre eigenen XAML-NameScopes, und zwar unabhängig von der Seite, auf die der Stil oder die Vorlage angewendet wird. Sehen Sie sich dieses Beispiel an:

```xml
<Page
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"  >
  <Page.Resources>
    <ControlTemplate x:Key="MyTemplate">
      ....
      <TextBlock x:Name="MyTextBlock" />
    </ControlTemplate>
  </Page.Resources>
  <StackPanel>
    <SomeControl Template="{StaticResource MyTemplate}" />
    <SomeControl Template="{StaticResource MyTemplate}" />
  </StackPanel>
</Page>
```

Hier wird dieselbe Vorlage auf zwei verschiedene Steuerelemente angewendet. Wenn die Vorlagen keine getrennten XAML-NameScopes enthalten würden, würde der in der Vorlage verwendete MyTextBlock-Name einen Namenskonflikt auslösen. Jede Vorlageninstanz hat ihren eigenen XAML-Namescope, weshalb in diesem Beispiel der Namescope jeder Instanz der Vorlage genau einen Namen enthält. Der XAML-Stamm-NameScope enthält jedoch keinen der Vorlagennamen.

Wegen der separaten XAML-NameScopes erfordert die Suche nach benannten Elementen innerhalb einer Vorlage vom Bereich der Seite, auf die die Vorlage angewendet wird, ein anderes Verfahren. Statt [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname) für ein Objekt in der Objektstruktur aufzurufen, rufen Sie zunächst das Objekt ab, auf das die Vorlage angewendet wurde. Rufen Sie dann [**GetTemplateChild**](/uwp/api/windows.ui.xaml.controls.control.gettemplatechild) auf. Wenn Sie Autor eines Steuerelements sind und eine Konvention erstellen, in der ein bestimmtes benanntes Element in einer angewendeten Vorlage das Ziel für ein Verhalten darstellt, das vom Steuerelement selbst definiert wird, können Sie die **GetTemplateChild**-Methode aus dem Code zur Implementierung des Steuerelements verwenden. Die **GetTemplateChild**-Methode ist geschützt, sodass nur der Steuerelementautor Zugriff darauf hat. Außerdem gibt es Konventionen, die Steuerelementautoren befolgen sollten, um Teile und Vorlagenteile zu benennen und diese als auf die Steuerelementklasse angewendete Attributwerte zu melden. Diese Vorgehensweise macht die Namen wichtiger Teile für die Benutzer von Steuerelementen erkennbar, die möglicherweise eine andere Vorlage anwenden möchten, die die benannten Teile ersetzen müsste, um die Funktionalität der Steuerelemente aufrechtzuerhalten.

## <a name="related-topics"></a>Zugehörige Themen

* [Übersicht über XAML](xaml-overview.md)
* [x:Name-Attribut](x-name-attribute.md)
* [Schnellstart: Steuerelement Vorlagen](/previous-versions/windows/apps/hh465374(v=win.10))
* [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load)
* [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname)
 