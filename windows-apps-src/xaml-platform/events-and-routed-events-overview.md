---
description: Das Programmierkonzept von Ereignissen in einer Windows-Runtime-App bei Verwendung von C#-, Visual Basic- oder Visual C++-Komponentenerweiterungen (C++/CX) als Programmiersprache und XAML für die UI-Definition wird beschrieben
title: Übersicht über Ereignisse und Routingereignisse
ms.assetid: 34C219E8-3EFB-45BC-8BBD-6FD937698832
ms.date: 07/12/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 67ee1f31e1480e0aa805e161433976d5e789d00c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155094"
---
# <a name="events-and-routed-events-overview"></a>Übersicht über Ereignisse und Routingereignisse

**Wichtige APIs**
- [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)
- [**RoutedEventArgs**](/uwp/api/Windows.UI.Xaml.RoutedEventArgs)

Das Programmierkonzept von Ereignissen in einer Windows-Runtime-App bei Verwendung von C#-, Visual Basic- oder Visual C++-Komponentenerweiterungen (C++/CX) als Programmiersprache und XAML für die UI-Definition wird beschrieben Sie können im Rahmen der Deklarationen für UI-Elemente Handler für Ereignisse in XAML zuweisen. Alternativ können Sie Handler im Code hinzufügen. Die Windows-Runtime unterstützt *Routingereignisse*: Bestimmte Eingabeereignisse und Datenereignisse können von anderen Objekten behandelt werden als dem Objekt, von dem das Ereignis ausgelöst wurde. Routingereignisse sind hilfreich, wenn Sie Steuerelementvorlagen definieren oder Seiten oder Layoutcontainer verwenden.

## <a name="events-as-a-programming-concept"></a>Ereignisse als Programmierkonzept

Grundsätzlich sind Ereigniskonzepte bei der Programmierung einer Windows-Runtime-App mit dem Ereignismodell in den meisten gängigen Programmiersprachen vergleichbar. Wenn Sie bereits mit .NET- oder C++-Ereignissen vertraut sind, kommen Sie sofort zurecht. Für verschiedene allgemeine Aufgaben, wie das Hinzufügen von Handlern, müssen Sie jedoch nicht allzu viel über Ereignismodellkonzepte wissen.

Wenn Sie C#, Visual Basic oder C++/CX als Programmiersprache verwenden, wird die UI im Markup (XAML) definiert. In der XAML-Markupsyntax ähneln einige der Prinzipien, nach denen UI-Ereignisse zwischen Markupelementen und Laufzeitcodeentitäten verbunden werden, denen anderer Webtechnologien (z. B. ASP.NET oder HTML5).

**Hinweis**    Der Code, der die Lauf Zeit Logik für eine XAML-definierte Benutzeroberfläche bereitstellt, wird häufig als *Code Behind* oder die Code-Behind-Datei bezeichnet. In den Projektmappenansichten von Microsoft Visual Studio wird diese Beziehung grafisch dargestellt. Dabei ist die CodeBehind-Datei eine abhängige und geschachtelte Datei zu der XAML-Seite, auf die sie sich bezieht.

## <a name="buttonclick-an-introduction-to-events-and-xaml"></a>Button.Click: Einführung in Ereignisse und XAML

Zu den häufigsten Programmieraufgaben für eine Windows-Runtime-App gehört es, Benutzereingaben für die UI zu erfassen. So kann Ihre UI beispielsweise eine Schaltfläche enthalten, auf die der Benutzer klicken muss, um Informationen zu senden oder einen Zustand zu ändern.

Sie definieren die UI für Ihre Windows-Runtime-App mittels XAML-Generierung. Bei diesem XAML handelt es sich meist um die Ausgabe einer Designoberfläche in Visual Studio. Sie können das XAML auch in einem Nur-Text-Editor oder im XAML-Editor eines Drittanbieters schreiben. Beim Generieren dieses XAML können Sie Ereignishandler für einzelne UI-Elemente verknüpfen, während Sie gleichzeitig alle anderen XAML-Attribute definieren, die Eigenschaftswerte dieses UI-Elements einrichten.

Zur Ereignisverknüpfung in XAML gehört, dass Sie den Namen der Handlermethode, die Sie bereits definiert haben oder später im CodeBehind definieren werden, als Zeichenfolge angeben. Diese XAML definiert beispielsweise ein [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button)-Objekt mit anderen Eigenschaften ([x:Name-Attribut](x-name-attribute.md), [**Content**](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)), die als Attribute zugewiesen sind, und verknüpft einen Handler für das [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignis der Schaltfläche durch einen Verweis auf eine Methode namens `ShowUpdatesButton_Click`:

```xaml
<Button x:Name="showUpdatesButton"
  Content="{Binding ShowUpdatesText}"
  Click="ShowUpdatesButton_Click"/>
```

**Tipp**  *Ereignisverknüpfung* ist ein Begriff der Programmiersprache. Er bezieht sich auf den Prozess oder Code, mit dem Sie angeben, dass Vorkommen eines Ereignisses eine benannte Handlermethode aufrufen sollen. In den meisten prozeduralen Codemodellen handelt es sich bei der Ereignisverknüpfung um impliziten oder expliziten „AddHandler“-Code, der sowohl das Ereignis als auch die Methode benennt und in der Regel eine Zielobjektinstanz verwendet. In XAML ist "AddHandler" implizit und die Ereignisverknüpfung umfasst nur das Benennen des Ereignisses als Attributnamen eines Objektelement und das Benennen des Handlers als Wert dieses Attributs.

Sie schreiben den eigentlichen Handler in der Programmiersprache, die Sie für Ihren gesamten App-Code oder CodeBehind verwenden. Mit dem Attribut `Click="ShowUpdatesButton_Click"` haben Sie einen Vertrag erstellt, mit dem beim Kompilieren des Markups und Analysieren des XAML sowohl der XAML-Markupkompilierschritt in der Erstellungsaktion Ihrer IDE als auch die mögliche XAML-Analyse beim Laden der App eine Methode mit dem Namen `ShowUpdatesButton_Click` finden können, die Teil des App-Codes ist. `ShowUpdatesButton_Click` muss eine Methode darstellen, die eine kompatible Methodensignatur (basierend auf einem Delegaten) für jeden Handler des [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignisses implementiert. Dieser Code definiert z. B. den `ShowUpdatesButton_Click`-Handler.

```csharp
private void ShowUpdatesButton_Click (object sender, RoutedEventArgs e) 
{
    Button b = sender as Button;
    //more logic to do here...
}
```

```vb
Private Sub ShowUpdatesButton_Click(ByVal sender As Object, ByVal e As RoutedEventArgs)
    Dim b As Button = CType(sender, Button)
    '  more logic to do here...
End Sub
```

```cppwinrt
void winrt::MyNamespace::implementation::BlankPage::ShowUpdatesButton_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e)
{
    auto b{ sender.as<Windows::UI::Xaml::Controls::Button>() };
    // More logic to do here.
}
```

```cpp
void MyNamespace::BlankPage::ShowUpdatesButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e) 
{
    Button^ b = (Button^) sender;
    //more logic to do here...
}
```

In diesem Beispiel basiert die `ShowUpdatesButton_Click`-Methode auf dem [**RoutedEventHandler**](/uwp/api/windows.ui.xaml.routedeventhandler)-Delegaten. Sie wissen, dass dies der zu verwendende Delegat ist, da Sie den Delegaten in der Syntax für die [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) -Methode sehen.

**Tipp**    Visual Studio bietet eine bequeme Möglichkeit, den Ereignishandler zu benennen und die Handlermethode zu definieren, während Sie XAML bearbeiten. Wenn Sie den Attributnamen des Ereignisses im XAML-Text-Editor bereitstellen, warten Sie einen Moment, bis eine Microsoft IntelliSense-Liste angezeigt wird. Wenn Sie in der Liste auf ** &lt; neuer Ereignis Handler &gt; ** klicken, wird Microsoft Visual Studio einen Methodennamen vorschlagen, der auf dem **x:Name** (oder Typnamen) des Elements, dem Ereignis Namen und einem numerischen Suffix basiert. Anschließend können Sie mit der rechten Maustaste auf den ausgewählten Ereignishandler und dann mit der linken Maustaste auf **Zum Ereignishandler navigieren** klicken. Dadurch navigieren Sie direkt zu der neu eingefügten Ereignishandlerdefinition, wie sie in der Code-Editoransicht Ihrer CodeBehind-Datei für die XAML-Seite angezeigt wird. Der Ereignishandler hat bereits die richtige Signatur, einschließlich des *sender*-Parameters und der Ereignisdatenklasse, die von dem Ereignis verwendet wird. Wenn eine Handlermethode mit der richtigen Signatur bereits im Code-Behind vorhanden ist, wird der Name der Methode in der Dropdown-Dropdown-Dropdown-Option zusammen mit der ** &lt; neuen Ereignishandler &gt; ** -Option angezeigt. Sie können auch die Tabulatortaste drücken, anstatt auf die IntelliSense-Listenelemente zu klicken.

## <a name="defining-an-event-handler"></a>Definieren eines Ereignishandlers

Für Objekte, die UI-Elemente darstellen und in XAML deklariert werden, wird Ereignishandlercode in der partiellen Klasse definiert, die als CodeBehind für eine XAML-Seite fungiert. Ereignishandler sind Methoden, die Sie als Teil der Ihrem XAML zugeordneten partiellen Klasse schreiben. Diese Ereignishandler basieren auf den Delegaten, die von einem bestimmten Ereignis verwendet werden. Die Ereignishandlermethoden können öffentlich oder privat sein. Der private Zugriff funktioniert, weil der vom XAML erstellte Handler und die Instanz letztendlich durch die Codegenerierung verbunden werden. Im Allgemeinen wird empfohlen, die Ereignishandlermethoden in der Klasse öffentlich zu machen.

**Hinweis**    Ereignishandler für C++ werden in partiellen Klassen nicht definiert. Sie werden im Header als privates Klassenmember deklariert. Bei den Erstellungsaktionen für ein C++-Projekt wird Code generiert, der das XAML-Typsystem und das CodeBehind-Modell für C++ unterstützt.

### <a name="the-sender-parameter-and-event-data"></a>Der *sender*-Parameter und Ereignisdaten

Der Handler, den Sie für das Ereignis schreiben, kann auf zwei Werte zugreifen. Diese Werte sind jedes Mal als Eingabe verfügbar, wenn der Handler aufgerufen wird. Der erste Wert ist *sender*. Dabei handelt es sich um einen Verweis auf das Objekt, dem der Handler angefügt ist. Der *sender*-Parameter ist als **Object**-Basistyp typisiert. Häufig wird *sender* in einen präziseren Typ umgewandelt. Diese Maßnahme ist nützlich, wenn Sie beabsichtigen, den Zustand direkt für das *sender*-Objekt zu überprüfen oder zu ändern. Abhängig davon, wo der Handler angefügt wird, und von anderen Designmerkmalen wissen Sie bei Ihrem eigenen App-Design in der Regel, dass es sich um einen Typ handelt, in den *sender* sicher umgewandelt werden kann.

Den zweiten Wert stellen Ereignisdaten dar. In der Regel sind diese in Syntaxdefinitionen als *e*-Parameter enthalten. Wenn Sie feststellen möchten, welche Eigenschaften für Ereignisdaten verfügbar sind, prüfen Sie den *e*-Parameter des Delegaten, der dem behandelten Ereignis zugewiesen ist. Verwenden Sie dann IntelliSense oder den Objektkatalog in Visual Studio. Sie können auch in der Referenzdokumentation von Windows Runtime nachschlagen.

Bei einigen Ereignissen ist die Kenntnis der speziellen Eigenschaftswerte der Ereignisdaten so wichtig wie die Kenntnis, dass das Ereignis eingetreten ist. Das gilt speziell bei Eingabeereignissen. Bei Zeigerereignissen kann die Position des Zeigers beim Auslösen des Ereignisses wichtig sein. Bei Tastaturereignissen wird durch alle möglichen Tastendrücke ein [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown)- und [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup)-Ereignis ausgelöst. Um zu bestimmen, welche Taste von einem Benutzer gedrückt wurde, müssen Sie auf die [**KeyRoutedEventArgs**](/uwp/api/Windows.UI.Xaml.Input.KeyRoutedEventArgs)-Klasse zugreifen, die für den Ereignishandler verfügbar ist. Weitere Informationen zur Behandlung von Eingabeereignissen finden Sie unter [Tastaturinteraktionen](../design/input/keyboard-interactions.md) und [Behandeln von Zeigereingaben](../design/input/handle-pointer-input.md). Bei Eingabeereignissen und Eingabeszenarien gibt es häufig weitere Überlegungen, die in diesem Thema nicht behandelt werden, wie das Erfassen des Zeigers bei Zeigerereignissen oder Zusatztasten und Plattformtastencodes für Tastaturereignisse.

### <a name="event-handlers-that-use-the-async-pattern"></a>Ereignishandler, die das **async**-Muster verwenden

In einigen Fällen wird die Verwendung von APIs empfohlen, die innerhalb eines Ereignishandlers ein **async**-Muster verwenden. Beispielsweise können Sie einen [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) auf einer [**AppBar**](/uwp/api/Windows.UI.Xaml.Controls.AppBar) verwenden, um eine Dateiauswahl anzuzeigen und mit dieser zu interagieren. Viele der Dateiauswahl-APIs sind jedoch asynchron. Sie müssen innerhalb eines **async**/awaitable-Bereichs aufgerufen werden. Dies wird vom Compiler erzwungen. Sie können dem Ereignishandler das **async**-Schlüsselwort hinzuzufügen, sodass der Handler nun **async** **void** ist. Nun kann der Ereignishandler **async**/awaitable-Aufrufe ausführen.

Ein Beispiel für die Benutzerinteraktions-Ereignisbehandlung mit dem **async**-Muster finden Sie unter [Dateizugriff und -auswahl](/previous-versions/windows/apps/jj655411(v=win.10)) (in der Reihe [Erstellen Ihrer ersten Windows-Runtime-App mit C# oder Visual Basic](/previous-versions/windows/apps/hh974581(v=win.10))). Siehe auch [Aufrufen asynchroner APIs in C].

## <a name="adding-event-handlers-in-code"></a>Hinzufügen von Ereignishandlern im Code

Es gibt noch andere Möglichkeiten als XAML, um einem Objekt einen Ereignishandler zuzuweisen. Sie können die spezielle Syntax zum Hinzufügen von Ereignishandlern einer Sprache nutzen, um bestimmten Objekten (auch Objekten, die in XAML nicht verwendet werden können) Ereignishandler im Code hinzufügen.

In C# wird für die Syntax der `+=`-Operator verwendet. Sie registrieren den Handler, indem Sie auf der rechten Seite des Operators auf den Namen der Ereignishandlermethode verweisen.

Wenn Objekten, die in der Laufzeit-UI angezeigt werden, Ereignishandler per Code hinzugefügt werden, ist es üblich, diese Handler als Reaktion auf ein Objektlebensdauer-Ereignis oder einen Rückruf hinzuzufügen, wie z. B. [**Loaded**](/uwp/api/windows.ui.xaml.frameworkelement.loaded) oder [**OnApplyTemplate**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate). Die Ereignishandler für das relevante Objekt stehen dann zur Laufzeit für Ereignisse bereit, die von Benutzern eingeleitet werden. Dieses Beispiel zeigt eine XAML-Gliederung der Seitenstruktur und führt anschließend die C#-Sprachsyntax für das Hinzufügen eines Ereignishandlers zu einem Objekt auf.

```xaml
<Grid x:Name="LayoutRoot" Loaded="LayoutRoot_Loaded">
  <StackPanel>
    <TextBlock Name="textBlock1">Put the pointer over this text</TextBlock>
...
  </StackPanel>
</Grid>
```

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += textBlock1_PointerEntered;
    textBlock1.PointerExited += textBlock1_PointerExited;
}
```

**Hinweis**    Es ist eine ausführlichere Syntax vorhanden. 2005 wurde in C# der Rückschluss auf Delegaten als neues Feature eingeführt. Damit kann der Compiler die neue Delegatinstanz ableiten, und die vorherige, einfachere Syntax kann verwendet werden. Die Funktion der ausführlichen Syntax entspricht dem vorherigen Beispiel. Vor der Registrierung wird jedoch explizit eine neue Delegatinstanz erstellt. Der Delegatrückschluss wird somit nicht genutzt. Diese explizite Syntax ist zwar nicht so üblich, aber dennoch in einigen Codebeispielen anzutreffen.

```csharp
void LayoutRoot_Loaded(object sender, RoutedEventArgs e)
{
    textBlock1.PointerEntered += new PointerEventHandler(textBlock1_PointerEntered);
    textBlock1.PointerExited += new MouseEventHandler(textBlock1_PointerExited);
}
```

Für Visual Basic-Syntax gibt es zwei Möglichkeiten Sie können eine parallele C#-Syntax verwenden und den Instanzen die Handler direkt anfügen. Dazu benötigen Sie das **AddHandler**-Schlüsselwort und den **AddressOf**-Operator, der den Handlermethodennamen dereferenziert.

Die zweite Möglichkeit für die Visual Basic-Syntax besteht darin, das **Handles**-Schlüsselwort für Ereignishandler zu verwenden. Das ist in Fällen angebracht, in denen beim Laden voraussichtlich Handler für Objekte vorhanden sind und für die Objektlebensdauer bestehen bleiben. Wenn Sie **Handles** für ein in XAML definiertes Objekt verwenden, müssen Sie einen **Name** / **x:Name** bereitstellen. Dieser Name wird zum Instanzenqualifizierer, der für den *Instance.Event*-Teil der **Handles**-Syntax benötigt wird. In diesem Fall benötigen Sie keinen Ereignishandler, der auf der Objektlebensdauer basiert, um das Anfügen der anderen Ereignishandler einzuleiten. Die **Handles**-Verbindungen werden beim Kompilieren der XAML-Seite erstellt.

```vb
Private Sub textBlock1_PointerEntered(ByVal sender As Object, ByVal e As PointerRoutedEventArgs) Handles textBlock1.PointerEntered
' ...
End Sub
```

**Hinweis**    Visual Studio und die zugehörige XAML-Entwurfs Oberfläche Stufen im Allgemeinen das instanzbehandlungsverfahren anstelle des **Handles** -Schlüssel Worts herauf. Das Erstellen der Ereignishandlerverknüpfung in XAML ist Teil eines typischen Designer-Entwickler-Workflows, und die **Handles**-Schlüsselworttechnik ist mit dem Verknüpfen der Ereignishandler in XAML nicht kompatibel.

In C++/CX verwenden Sie auch die **+=** Syntax, aber es gibt Unterschiede in der grundlegenden c#-Form:

- Es gibt keinen Rückschluss auf Delegaten. Sie müssen deshalb **ref new** für die Delegatinstanz verwenden.
- Der Delegatkonstruktor besitzt zwei Parameter und benötigt das Zielobjekt als ersten Parameter. Normalerweise geben Sie **this** an.
- Der Delegatkonstruktor erfordert die Methoden Adresse als zweiten Parameter, sodass der **&** Verweis Operator dem Methodennamen vorangestellt wird.

```cppwinrt
textBlock1().PointerEntered({this, &MainPage::TextBlock1_PointerEntered });
```

```cpp
textBlock1->PointerEntered += 
ref new PointerEventHandler(this, &BlankPage::textBlock1_PointerEntered);
```

### <a name="removing-event-handlers-in-code"></a>Entfernen von Ereignishandlern im Code

Das Entfernen von Ereignishandlern im Code ist in der Regel nicht erforderlich, auch wenn sie von Ihnen im Code hinzugefügt wurden. Das Verhalten der Objektlebensdauer für die meisten Windows-Runtime-Objekte, z. B. Seiten und Steuerelemente, vernichtet die Objekte, wenn die Verbindung mit dem Haupt-[**Fenster**](/uwp/api/Windows.UI.Xaml.Window) und der dazugehörigen visuellen Struktur getrennt wird. Vorhandene Delegatverweise werden ebenfalls vernichtet. In .NET erfolgt dies durch eine Garbage Collection, und in Windows-Runtime mit C++/CX werden standardmäßig schwache Verweise verwendet.

In einigen seltenen Fällen sollten Ereignishandler explizit entfernt werden. Dazu zählen unter anderem folgende Einstellungen:

- Für statische Ereignisse hinzugefügte Handler, für die keine konventionelle Garbage Collection durchgeführt werden kann. Die Ereignisse der Klassen [**CompositionTarget**](/uwp/api/Windows.UI.Xaml.Media.CompositionTarget) und [**Clipboard**](/uwp/api/Windows.ApplicationModel.DataTransfer.Clipboard) sind Beispiele für statische Ereignisse in der Windows-Runtime-API.
- Testen Sie Code, wenn Sie Handler sofort entfernen möchten, oder Code, in dem alte und neue Ereignishandler für ein Ereignis zur Laufzeit ausgetauscht werden sollen.
- Die Implementierung eines benutzerdefinierten **remove**-Accessors
- Benutzerdefinierte statische Ereignisse
- Handler für Seitennavigationen

[**FrameworkElement.Unloaded**](/uwp/api/windows.ui.xaml.frameworkelement.unloaded) oder [**Page.NavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom) sind mögliche Ereignisauslöser mit entsprechenden Positionen in der Zustandsverwaltung und der Objektlebensdauer, sodass sie zum Entfernen von Handlern für andere Ereignisse verwendet werden können.

Sie können z. b. einen Ereignishandler mit dem Namen **textBlock1 \_ pointereingegeben** aus dem Zielobjekt **textBlock1** entfernen, indem Sie diesen Code verwenden.

```csharp
textBlock1.PointerEntered -= textBlock1_PointerEntered;
```

```vb
RemoveHandler textBlock1.PointerEntered, AddressOf textBlock1_PointerEntered
```

Sie können Handler auch in solchen Fällen entfernen, in denen das Ereignis über ein XAML-Attribut hinzugefügt wurde, was bedeutet, dass der Handler in generiertem Code hinzugefügt wurde. Dies ist einfacher, wenn Sie einen **Name**-Wert für das Element bereitstellen, dem der Handler angehängt wurde, da hierdurch später ein Objektverweis für den Code bereitgestellt wird. Sie können jedoch auch in der Objektstruktur nach dem erforderlichen Objekt suchen, wenn kein **Name** für das Objekt vorhanden ist.

Wenn Sie einen Ereignishandler in C++/CX entfernen müssen, benötigen Sie ein Registrierungstoken, das Sie im Rückgabewert der `+=`-Ereignishandlerregistrierung erhalten haben sollten. Der Grund ist, dass es sich bei dem Wert, den Sie für die rechte Seite der Aufhebung der `-=`-Registrierung in C++/CX-Syntax verwenden, um das Token handelt, nicht beim Methodennamen. Für C++/CX können Sie als XAML-Attribut hinzugefügte Handler nicht entfernen, da der generierte C++/CX-Code kein Token speichert.

## <a name="routed-events"></a>Routingereignisse

Die Windows-Runtime mit C#, Microsoft Visual Basic oder C++/CX unterstützt das Konzept von Routingereignissen für eine Gruppe von Ereignissen, die für die meisten UI-Elementen verwendet werden. Diese Ereignisse werden für Eingabe- und Benutzerinteraktionsszenarien verwendet und in der [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)-Basisklasse implementiert. Die folgenden Eingabeereignisse sind Routingereignisse:

- [**Bringingeviewangeforderten**](/uwp/api/windows.ui.xaml.uielement.bringintoviewrequested)
- [**Charakteristik empfangen**](/uwp/api/windows.ui.xaml.uielement.characterreceived)
- [**Contextabgeb Rochen**](/uwp/api/windows.ui.xaml.uielement.contextcanceled)
- [**Contextrequessiert**](/uwp/api/windows.ui.xaml.uielement.contextrequested)
- [**DoubleTapped**](/uwp/api/windows.ui.xaml.uielement.doubletapped)
- [**DragEnter**](/uwp/api/windows.ui.xaml.uielement.dragenter)
- [**DragLeave**](/uwp/api/windows.ui.xaml.uielement.dragleave)
- [**DragOver**](/uwp/api/windows.ui.xaml.uielement.dragover)
- [**DragStart**](/uwp/api/windows.ui.xaml.uielement.dragstarting)
- [**Dropdown**](/uwp/api/windows.ui.xaml.uielement.drop)
- [**Dropdown**](/uwp/api/windows.ui.xaml.uielement.dropcompleted)
- [**Gettingfocus**](/uwp/api/windows.ui.xaml.uielement.gettingfocus)
- [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus)
- [**Holding**](/uwp/api/windows.ui.xaml.uielement.holding)
- [**KeyDown**](/uwp/api/windows.ui.xaml.uielement.keydown)
- [**KeyUp**](/uwp/api/windows.ui.xaml.uielement.keyup)
- [**Losingfocus**](/uwp/api/windows.ui.xaml.uielement.losingfocus)
- [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus)
- [**ManipulationCompleted**](/uwp/api/windows.ui.xaml.uielement.manipulationcompleted)
- [**ManipulationDelta**](/uwp/api/windows.ui.xaml.uielement.manipulationdelta)
- [**ManipulationInertiaStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationinertiastarting)
- [**ManipulationStarted**](/uwp/api/windows.ui.xaml.uielement.manipulationstarted)
- [**ManipulationStarting**](/uwp/api/windows.ui.xaml.uielement.manipulationstarting)
- [**Nofocus candidatefound**](/uwp/api/windows.ui.xaml.uielement.nofocuscandidatefoundeventargs)
- [**PointerCanceled**](/uwp/api/windows.ui.xaml.uielement.pointercanceled)
- [**PointerCaptureLost**](/uwp/api/windows.ui.xaml.uielement.pointercapturelost)
- [**PointerEntered**](/uwp/api/windows.ui.xaml.uielement.pointerentered)
- [**PointerExited**](/uwp/api/windows.ui.xaml.uielement.pointerexited)
- [**PointerMoved**](/uwp/api/windows.ui.xaml.uielement.pointermoved)
- [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed)
- [**PointerReleased**](/uwp/api/windows.ui.xaml.uielement.pointerreleased)
- [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**PreviewKeyDown**](/uwp/api/windows.ui.xaml.uielement.previewkeydown)
- [**PreviewKeyUp**](/uwp/api/windows.ui.xaml.uielement.previewkeyup)
- [**PointerWheelChanged**](/uwp/api/windows.ui.xaml.uielement.pointerwheelchanged)
- [**RightTapped**](/uwp/api/windows.ui.xaml.uielement.righttapped)
- [**Tapped**](/uwp/api/windows.ui.xaml.uielement.tapped)

Ein Routingereignis ist ein Ereignis, das möglicherweise von einem untergeordneten Objekt an jedes seiner übergeordneten Objekte in der Objektstruktur weitergeleitet (*geroutet*) wird. Die XAML-Struktur Ihrer UI kommt dieser Struktur nahe, wobei der Stamm der Struktur dem Stammelement im XAML entspricht. Die tatsächliche Objektstruktur kann sich von der XAML-Elementschachtelung unterscheiden, da die Objektstruktur keine XAML-Sprachfeatures wie Eigenschaftenelementtags enthält. Routingereignisse durchlaufen die Struktur nach dem *Bubblingkonzept* von untergeordneten XAML-Objektelementen, die das Ereignis auslösen, zu den übergeordneten Objekten, in denen sie enthalten sind. Das Ereignis und seine Ereignisdaten können für mehrere Objekte entlang der Ereignisroute behandelt werden. Wenn kein Element über Handler verfügt, wird die Route u. U. bis zum Stammelement fortgesetzt.

Wenn Sie Webtechnologien wie Dynamic HTML (DHTML) oder HTML5 kennen, sind Sie möglicherweise bereits mit dem *Bubbling*-Ereigniskonzept vertraut.

Wenn für ein Routingereignis ein Bubbling durch die zugehörige Ereignisroute erfolgt, greifen alle angefügten Ereignishandler auf eine freigegebene Instanz von Ereignisdaten zu. Deshalb werden Änderungen an Ereignisdaten an den nächsten Handler weitergegeben, wenn einer der Handler Schreibzugriff auf Ereignisdaten besitzt. Die Daten entsprechen dann möglicherweise nicht mehr den ursprünglichen Ereignisdaten. Wenn ein Ereignis ein Routingereignisverhalten besitzt, wird in der Referenzdokumentation entsprechend darauf hingewiesen.

### <a name="the-originalsource-property-of-routedeventargs"></a>Die Eigenschaft **OriginalSource** von **RoutedEventArgs**

Wenn ein Ereignis entlang einer Route ein Bubbling durchläuft, ist *sender* nicht mehr das gleiche Objekt, das das Ereignis ausgelöst hat. Stattdessen ist *sender* das Objekt, an das der aufgerufene Handler angefügt ist.

In einigen Fällen interessieren Sie sich nicht für das *sender*-Objekt, sondern eher dafür, auf welchem der möglichen untergeordneten Objekte sich der Mauszeiger beim Auslösen eines Zeigerereignisses befindet oder welches Objekt in einer größeren UI beim Drücken einer Taste den Fokus hatte. Für diese Fälle können Sie den Wert der [**OriginalSource**](/uwp/api/windows.ui.xaml.routedeventargs.originalsource)-Eigenschaft verwenden. An allen Punkten in der Route meldet **OriginalSource** das ursprüngliche Objekt, von dem das Ereignis ausgelöst wurde, und nicht das Objekt, dem der Handler angefügt ist. Bei [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)-Eingabeereignissen ist das ursprüngliche Objekt jedoch häufig ein Objekt, das nicht direkt im XAML für die UI-Definition auf Seitenebene sichtbar ist. Stattdessen kann das ursprüngliche Quellobjekt ein Vorlagenteil eines Steuerelements sein. Wenn der Benutzer beispielsweise mit dem Mauszeiger auf die äußere Kante eines [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) zeigt, ist **OriginalSource** für die meisten Zeigerereignissen ein [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border)-Vorlagenbestandteil in [**Template**](/uwp/api/windows.ui.xaml.controls.control.template), nicht **Button** selbst.

**Tipp**    Das Eingabe Ereignis Blasen ist besonders nützlich, wenn Sie ein Steuerelement mit Vorlagen erstellen. Auf jedes Steuerelement, das eine Vorlage besitzt, kann durch seinen Consumer eine neue Vorlage angewendet werden. Der Consumer, der eine Arbeitsvorlage erneut zu erstellen versucht, kann versehentlich in der Standardvorlage deklarierten Ereignisbehandlungscode entfernen. Sie können dennoch eine Ereignisbehandlung auf Steuerelementebene bereitstellen, indem Sie Handler als Teil der [**OnApplyTemplate**](/uwp/api/windows.ui.xaml.frameworkelement.onapplytemplate)-Überschreibung in der Klassendefinition anfügen. Anschließend können Sie die Eingabeereignisse abfangen, die bei der Instanziierung per Bubbling zum Stamm des Steuerelements weitergeleitet werden.

### <a name="the-handled-property"></a>Die Eigenschaft **Handled**

Einige Ereignisdatenklassen für bestimmte Routingereignisse enthalten eine Eigenschaft mit dem Namen **Handled**. Beispiele finden Sie unter [**PointerRoutedEventArgs.Handled**](/uwp/api/windows.ui.xaml.input.pointerroutedeventargs.handled), [**KeyRoutedEventArgs.Handled**](/uwp/api/windows.ui.xaml.input.keyroutedeventargs.handled) und [**DragEventArgs.Handled**](/uwp/api/windows.ui.xaml.drageventargs.handled). In allen Fällen ist **Handled** eine festlegbare boolesche Eigenschaft.

Wenn die Eigenschaft **Handled** auf **true** festgelegt wird, wirkt sich dies auf das Ereignissystemverhalten aus. Wenn **Handled** auf **true** festgelegt, wird das Routing für die meisten Ereignishandler beendet. Das Ereignis wird nicht über die Route weitergeleitet, um andere angefügte Handler über dieses spezielle Ereignis zu informieren. Was „behandelt“ im Kontext des Ereignisses bedeutet und wie Ihre App auf ein behandeltes Ereignis reagiert, liegt in Ihrem Ermessen. Im Grunde ist **Handled** ein einfaches Protokoll, mit dem im App-Code angegeben werden kann, dass das Auftreten eines Ereignisses nicht per Bubbling an Container weitergeleitet werden muss, da Ihre App-Logik sich um die notwendigen Schritte gekümmert hat. In der Regel müssen Sie jedoch darauf achten, dass Sie keine Ereignisse verarbeiten, die wahrscheinlich Blasen Weise sein sollten, damit das integrierte System oder Steuerelement Verhalten agieren kann. Beispielsweise kann das Verarbeiten von Ereignissen auf niedriger Ebene innerhalb der Teile oder Elemente eines Auswahl Steuer Elements schädlich sein. Das Auswahlsteuerelement sucht möglicherweise nach Eingabeereignissen, um festzustellen, ob die Auswahl geändert werden muss.

Nicht alle Routingereignisse können eine Route auf diese Weise abbrechen, da sie die Eigenschaft **Handled** nicht besitzen. Beispielsweise führen [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) und [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus) Bubbling aus. Sie führen dieses Bubbling jedoch stets bis zum Stamm aus, und ihre Ereignisdatenklassen besitzen die Eigenschaft **Handled** nicht, die dieses Verhalten beeinflussen kann.

##  <a name="input-event-handlers-in-controls"></a>Eingabeereignishandler in Steuerelementen

Bei bestimmten Windows-Runtime-Steuerelementen wird das **Handled**-Konzept manchmal intern für Eingabeereignisse verwendet. Dadurch kann der Anschein erweckt werden, dass niemals ein Eingabeereignis eintritt, weil es vom Benutzercode nicht verarbeitet werden kann. Beispielsweise enthält die Klasse [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) Logik, mit der gezielt das allgemeine Eingabeereignis [**PointerPressed**](/uwp/api/windows.ui.xaml.uielement.pointerpressed) behandelt wird. Der Grund hierfür ist, dass Schaltflächen ein [**Click**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignis auslösen, das durch Eingaben mit gedrücktem Zeiger sowie durch andere Eingabemodi initiiert wird (z. B. durch Tasten wie die EINGABETASTE, die die Schaltfläche aufrufen können, wenn sie den Fokus besitzt). Im Rahmen des Klassendesigns von **Button** wird das Rohdateneingabe-Ereignis konzeptionell behandelt. Klassenconsumer wie Ihr Benutzercode können stattdessen mit dem steuerungsrelevanten **Click**-Ereignis interagieren. In den entsprechenden Themen zu den speziellen Steuerelementklassen in der API-Referenz für die Windows-Runtime wird häufig das von der Klasse implementierte Ereignisbehandlungsverhalten erwähnt. In einigen Fällen können Sie das Verhalten ändern, indem Sie **On**_Event_-Methoden überschreiben. Sie können beispielsweise die Art ändern, wie Ihre von [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) abgeleitete Klasse auf Tasteneingaben reagiert, indem Sie [**Control.OnKeyDown**](/uwp/api/windows.ui.xaml.controls.control.onkeydown) überschreiben.

##  <a name="registering-handlers-for-already-handled-routed-events"></a>Registrieren von Handlern für bereits behandelte Routingereignisse

Weiter oben wurde erwähnt, dass die meisten Handler nicht aufgerufen werden, wenn **Handled** auf **true** festgelegt ist. Die Methode [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) bietet jedoch eine Technik, um einen Handler anzufügen, der für die Route immer aufgerufen wird. Das geschieht selbst dann, wenn für andere Handler an früherer Stelle in der Route **Handled** auf **true** in den geteilten Ereignisdaten festgelegt wurde. Diese Technik ist nützlich, wenn ein von Ihnen verwendetes Steuerelement das Ereignis in seiner inneren Zusammensetzung behandelt hat. Sie eignet sich auch für spezielle Steuerelementlogik. bei der Sie weiterhin mit einer Steuerelementinstanz oder Ihrer App-UI auf das Ereignis reagieren möchten. Diese Technik sollte jedoch mit Bedacht eingesetzt werden, da sie dem Zweck von **Handled** widersprechen und die beabsichtigten Interaktionen des Steuerelements verhindern kann.

Nur Routingereignisse mit entsprechenden Routingereignisbezeichnern können die Technik zur [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler)-Ereignisbehandlung nutzen, da der Bezeichner als Eingabe für die **AddHandler**-Methode erforderlich ist. In der Referenzdokumentation zu [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler) finden Sie eine Liste der Ereignisse, für die Routingereignisbezeichner verfügbar sind. Diese Liste stimmt größtenteils mit der bereits gezeigten Liste mit Routingereignissen überein. Die letzten beiden Ereignisse in der Liste, [**GotFocus**](/uwp/api/windows.ui.xaml.uielement.gotfocus) und [**LostFocus**](/uwp/api/windows.ui.xaml.uielement.lostfocus), stellen eine Ausnahme dar, da sie keinen Routingereignisbezeichner besitzen. Daher können Sie **AddHandler** nicht für diese Ereignisse verwenden.

## <a name="routed-events-outside-the-visual-tree"></a>Routingereignisse außerhalb der visuellen Struktur

Bestimmte Objekte sind Bestandteil einer Beziehung mit der primären visuellen Struktur. Diese ist konzeptionell, wie z. B. eine Überlagerung über die visuellen Hauptobjekte. Diese bestimmten Objekte gehören nicht zu den üblichen Beziehungen zwischen übergeordneten und untergeordneten Elementen, mit denen alle Strukturelemente mit dem visuellen Stamm verbunden sind. Das betrifft jedes angezeigte [**Popup**](/uwp/api/Windows.UI.Xaml.Controls.Primitives.Popup)- oder [**ToolTip**](/uwp/api/Windows.UI.Xaml.Controls.ToolTip)-Element. Wenn Sie Routingereignisse eines **Popup**- oder **ToolTip**-Elements behandeln möchten, platzieren Sie die Handler für bestimmte UI-Elemente innerhalb des **Popup**- oder **ToolTip**-Elements und nicht direkt für die **Popup**- oder **ToolTip**-Elemente. Verlassen Sie sich nicht auf das Routing innerhalb einer Zusammensetzung, die für **Popup**- **ToolTip**-Inhalt ausgeführt wird. Das Ereignisrouting für geroutete Ereignisse funktioniert nur entlang der visuellen Hauptstruktur. Ein **Popup**- oder **ToolTip**-Element wird nicht als übergeordnetes Element untergeordneter UI-Elemente betrachtet und empfängt das Routingereignis in keinem Fall, auch wenn es versucht, Elemente wie den **Popup**-Standardhintergrund als Erfassungsbereich für Eingabeereignisse zu verwenden.

## <a name="hit-testing-and-input-events"></a>Treffertests und Eingabeereignisse

Das Bestimmen, ob und wo auf der UI ein Element für die Maus-, Touch und Stifteingabe sichtbar ist, wird als *Treffertests* bezeichnet. Bei Toucheingabeaktionen und interaktionsspezifischen Ereignissen oder Manipulationsereignissen, die aus einer Toucheingabeaktion resultieren, muss ein Element bei Treffertests sichtbar sein, damit es der Ereignisquelle entsprechen und das der Aktion zugeordnete Ereignis auslösen kann. Andernfalls durchläuft die Aktion das Element bis zu zugrunde liegenden oder übergeordneten Elementen in der visuellen Struktur, die mit dieser Eingabe interagieren könnte. Treffertests werden von mehreren Faktoren beeinflusst. Sie können jedoch feststellen, ob ein bestimmtes Element Eingabeereignisse auslösen kann, indem Sie die zugehörige Eigenschaft [**IsHitTestVisible**](/uwp/api/windows.ui.xaml.uielement.ishittestvisible) überprüfen. Diese Eigenschaft gibt nur dann **true** zurück, wenn das Element die folgenden Kriterien erfüllt:

- Der [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility)-Eigenschaftenwert des Elements ist [**Visible**](/uwp/api/Windows.UI.Xaml.Visibility).
- Der Wert der Eigenschaft **Background** oder **Fill** ist nicht **null**. Ein Wert **null für ** [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) führt zu Transparenz und Unsichtbarkeit von Treffertests. (Wenn Sie ein Element transparent machen und zugleich Treffertests für das Element ermöglichen möchten, verwenden Sie einen [**Transparent**](/uwp/api/windows.ui.colors.transparent)-Pinsel anstelle von **null**.)

**Hinweis**  **Background** und **Fill** werden nicht durch [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement) definiert, sondern durch verschiedene abgeleitete Klassen wie [**Control**](/uwp/api/Windows.UI.Xaml.Controls.Control) und [**Form**](/uwp/api/Windows.UI.Xaml.Shapes.Shape). Die von Ihnen für Vorder- und Hintergrundeigenschaften verwendeten Implikationen von Pinseln sind jedoch für Treffertests und Eingabeereignisse identisch. Dabei ist es unerheblich, welche Unterklasse die Eigenschaften implementiert.

- Wenn das Element ein Steuerelement ist, muss dessen Wert für die Eigenschaft [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled)**true** sein.
- Das Element muss im Layout über reale Dimensionen verfügen. Ein Element, bei dem [**ActualHeight**](/uwp/api/windows.ui.xaml.frameworkelement.actualheight) und [**ActualWidth**](/uwp/api/windows.ui.xaml.frameworkelement.actualwidth) 0 sind, kann keine Eingabeereignisse auslösen.

Bei einigen Steuerelementen sind besondere Regeln bezüglich Treffertests zu beachten. Beispielsweise besitzt [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) die Eigenschaft **Background** nicht. Dennoch können innerhalb des gesamten Bereichs der zugehörigen Dimensionen Treffertests ausgeführt werden. [**Image**](/uwp/api/Windows.UI.Xaml.Controls.Image) und [**MediaElement**](/uwp/api/Windows.UI.Xaml.Controls.MediaElement)-Steuerelemente können in den zugehörigen definierten Rechtecksdimensionen auf Treffer getestet werden. Dabei ist es unerheblich, ob in der Medienquelldatei transparente Inhalte wie Alphakanäle angezeigt werden. [**WebView**](/uwp/api/Windows.UI.Xaml.Controls.WebView)-Steuerelemente weisen ein spezielles Treffertestverhalten auf, da die Eingabe durch das gehostete HTML behandelt werden kann und Skriptereignisse auslösen kann.

Für die meisten [**Panel**](/uwp/api/Windows.UI.Xaml.Controls.Panel)-Klassen und [**Border**](/uwp/api/Windows.UI.Xaml.Controls.Border)-Elemente können im eigenen Hintergrund zwar keine Treffertests ausgeführt werden, sie können jedoch dennoch Benutzereingabeereignisse verarbeiten, die von den in ihnen integrierten Elementen weitergeleitet werden.

Elemente, die sich an der gleichen Position wie ein Benutzereingabeereignis befinden, können unabhängig davon ermittelt werden, ob sie für Treffertests infrage kommen. Rufen Sie dazu die [**FindElementsInHostCoordinates**](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates)-Methode auf. Wie der Name nahe legt, findet diese Methode die Elemente an einer Position relativ zu einem angegebenen Hostelement. Transformationen und Layoutänderungen können sich jedoch auf das Koordinatensystem eines Elements auswirken und somit die an einer bestimmten Position gefundenen Elemente beeinflussen.

## <a name="commanding"></a>Befehle

Eine geringe Anzahl von UI-Elementen unterstützt die *Steuerung*. Die Steuerung verwendet eingabebezogene Routingereignisse in der zugrunde liegenden Implementierung und ermöglicht die Verarbeitung verwandter UI-Eingaben (eine bestimmte Zeigeraktion oder Zugriffstaste) durch das Aufrufen eines einzelnen Befehlshandlers. Wenn die Steuerung für ein UI-Element verfügbar ist, sollten Sie dessen Steuerungs-APIs anstelle einzelner Eingabeereignisse verwenden. Normalerweise verwenden Sie einen **Binding**-Verweis auf Eigenschaften einer Klasse, die das Ansichtsmodell für Daten definiert. Die Eigenschaften enthalten benannte Befehle, die sprachspezifische **ICommand**-Befehlsmuster implementieren. Weitere Informationen finden Sie unter [**ButtonBase.Command**](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command).

## <a name="custom-events-in-the-windows-runtime"></a>Benutzerdefinierte Ereignisse in der Windows-Runtime

Beim Definieren von benutzerdefinierten Ereignissen hängen die Vorgehensweise beim Hinzufügen des Ereignisses und die Bedeutung für Ihren Klassenentwurf erheblich von der verwendeten Programmiersprache ab.

- Für C# und Visual Basic definieren Sie ein CLR-Ereignis. Sie können das standardmäßige .NET-Ereignis Muster verwenden, sofern Sie keine benutzerdefinierten Accessoren verwenden (**Add** / **Remove**). Zusätzliche Tipps:
    - Für den Ereignishandler empfiehlt sich die Verwendung von [**System.EventHandler<TEventArgs>**](/dotnet/api/system.eventhandler-1), da die Übersetzung in den generischen Windows-Runtime-Ereignisdelegaten [**EventHandler<T>**](/uwp/api/windows.foundation.eventhandler)integriert ist.
    - Basieren Sie Ihre Ereignisdatenklasse nicht auf [**System.EventArgs**](/dotnet/api/system.eventargs), da dann keine Übersetzung in die Windows-Runtime erfolgt. Verwenden Sie eine vorhandene Ereignisdatenklasse oder gar keine Basisklasse.
    - Wenn Sie benutzerdefinierte Accessoren verwenden, finden Sie weitere Informationen [unter benutzerdefinierte Ereignisse und Ereignisaccessoren in Windows-Runtime-Komponenten](/previous-versions/windows/apps/hh972883(v=vs.140)).
    - Wenn Ihnen das .NET-Standardereignismuster nicht bekannt ist, lesen Sie unter [Definieren von Ereignissen für benutzerdefinierte Silverlight-Klassen](/previous-versions/windows/). Dieser Inhalt wurde zwar für Microsoft Silverlight verfasst, stellt aber dennoch einen hilfreichen Überblick über den Code und die Konzepte für das .NET-Standardereignismuster dar.
- Für C++/CX lesen Sie [Ereignisse (C++/CX)](/cpp/cppcx/events-c-cx).
    - Verwenden Sie auch für Ihre eigenen Verwendungen von benutzerdefinierten Ereignissen benannte Verweise. Verwenden Sie nicht Lambda für benutzerdefinierte Ereignisse, da dadurch u. U. ein Zirkelverweis erstellt wird.

Sie können ein benutzerdefiniertes Routingereignis nicht für die Windows-Runtime deklarieren; Routingereignisse sind auf den Satz aus der Windows-Runtime beschränkt.

Das Definieren eines benutzerdefinierten Ereignisses erfolgt in der Regel im Rahmen der Definition eines benutzerdefinierten Steuerelements. Eine Abhängigkeitseigenschaft mit einem Rückruf für Eigenschaftsänderungen ist ein gängiges Muster, genau wie das Definieren eines benutzerdefinierten Ereignisses, das in manchen oder in allen Fällen durch den Abhängigkeitseigenschaftsrückruf ausgelöst wird. Benutzer Ihres Steuerelements haben keinen Zugriff auf den von Ihnen definierten Rückruf für Eigenschaftsänderungen; die beste Methode ist das Bereitstellen eines Benachrichtigungsereignisses. Weitere Informationen finden Sie unter [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md).

## <a name="related-topics"></a>Zugehörige Themen

* [Übersicht über XAML](xaml-overview.md)
* [Schnellstart: Fingereingabe](/previous-versions/windows/apps/hh465387(v=win.10))
* [Tastaturinteraktionen](../design/input/keyboard-interactions.md)
* [.NET-Ereignisse und -Delegate](/previous-versions/17sde2xt(v=vs.110))
* [Erstellen von Windows-Runtime-Komponenten](/previous-versions/windows/apps/hh441572(v=vs.140))
* [**AddHandler**](/uwp/api/windows.ui.xaml.uielement.addhandler)