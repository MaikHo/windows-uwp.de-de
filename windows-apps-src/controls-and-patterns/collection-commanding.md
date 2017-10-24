---
author: mijacobs
Description: 
title: Kontextbefehle
ms.assetid: 
label: Contextual commanding in collections
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
pm-contact: chigy
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.openlocfilehash: 88a32b9f0bce252534fe7d726e4843f4790de586
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2017
---
# <a name="contextual-commanding-for-collections-and-lists"></a>Kontextbefehle für Sammlungen und Listen

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Viele Apps enthalten Sammlungen von Inhalten in Form von Listen, Rastern und Strukturen, auf die der Benutzer Aktionen anwenden kann. Beispielsweise kann er Elemente löschen, umbenennen, kennzeichnen oder aktualisieren. In diesem Artikel wird beschrieben, wie Sie mithilfe von Kontextbefehlen derartige Aktionen so implementieren können, dass bei allen Eingabearten die jeweils bestmögliche Benutzererfahrung gewährleistet ist.  

> **Wichtige APIs:** [Schnittstelle „ICommand“](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.ICommand), [Eigenschaft „UIElement.ContextFlyout“](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_ContextFlyout), [Schnittstelle „INotifyPropertyChanged“](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.data.inotifypropertychanged)

![Ausführen des Befehls „Als Favorit speichern“ mittels verschiedener Eingabearten](images/ContextualCommand_AddFavorites.png)

## <a name="creating-commands-for-all-input-types"></a>Erstellen von Befehlen für alle Eingabearten

Benutzer können zur Interaktion mit UWP-Apps eine [Vielzahl unterschiedlicher Geräte und Eingabearten](../input-and-devices/device-primer.md) verwenden. Daher sollte Ihre App Befehle sowohl über von der Eingabeart unabhängige Kontextmenüs als auch über eingabeartspezifische Beschleuniger verfügbar machen. Wenn Sie beide Optionen integrieren, können Benutzer Befehle schnell auf Inhalte anwenden, unabhängig von Eingabeart und Gerätetyp.

In der Tabelle unten sind einige typische Befehle für Sammlungen aufgeführt sowie Möglichkeiten, diese Befehle verfügbar zu machen. 

| Befehl          | Eingabeartunabhängig | Beschleuniger für die Mauseingabe | Beschleuniger für die Tastatureingabe | Beschleuniger für die Toucheingabe |
| ---------------- | -------------- | ----------------- | -------------------- | ----------------- |
| Element löschen      | Kontextmenü   | Hoverschaltfläche      | ENTF-Taste              | Löschen per Wischen   |
| Element kennzeichnen        | Kontextmenü   | Hoverschaltfläche      | STRG+UMSCHALT+G         | Kennzeichnen per Wischen     |
| Daten aktualisieren     | Kontextmenü   | –               | F5-Taste               | Aktualisieren durch Ziehen   |
| Element als Favorit speichern | Kontextmenü   | Hoverschaltfläche      | F-Taste, STRG+S            | Als Favorit speichern per Wischen |


* **Grundsätzlich sollten Sie sämtliche Befehle für ein Element im [Kontextmenü](menus.md) des Elements verfügbar machen.** Kontextmenüs sind für den Benutzer bei jeder Eingabeart verfügbar und sollten alle Kontextbefehle enthalten, die er ausführen darf.

* **Für häufig verwendete Befehle empfiehlt sich die Implementierung von Eingabebeschleunigern.** Eingabebeschleuniger basieren auf dem jeweiligen Eingabegerät und erlauben es dem Benutzer, Aktionen schnell auszuführen. Zu den Eingabebeschleunigern gehören:
    - Aktionsausführung per Wischen (Beschleuniger für die Toucheingabe)
    - Datenaktualisierung per Ziehen (Beschleuniger für die Toucheingabe)
    - Tastenkombinationen (Beschleuniger für die Tastatureingabe)
    - Zugriffstasten (Beschleuniger für die Tastatureingabe)
    - Hoverschaltflächen für Maus und Stift (Beschleuniger für die Eingabe per Zeigegerät)

> [!NOTE]
> Benutzer sollten immer auf sämtliche Befehle zugreifen können, ganz gleich, welches Gerät sie verwenden. Wenn Sie die Befehle Ihrer App beispielsweise nur in Form von Hoverschaltflächen für eine beschleunigte Eingabe über Zeigegeräte verfügbar machen, haben Benutzer von Touchsystemen keinen Zugriff auf sie. Implementieren Sie zumindest ein Kontextmenü, in dem alle Befehle verfügbar sind.  

## <a name="example-the-podcastobject-data-model"></a>Beispiel: Datenmodell „PodcastObject“

Zur Verdeutlichung unserer Empfehlungen für die Befehlsimplementierung erstellen wir im Rahmen dieses Artikels eine Liste von Podcasts für eine Podcast-App. Der Beispielcode demonstriert, wie Sie es Benutzern ermöglichen können, bestimmte Podcasts aus der Liste als Favoriten zu speichern.

Hier die Definition des Podcast-Objekts, mit dem Sie in diesem Beispiel arbeiten: 

```csharp
public class PodcastObject : INotifyPropertyChanged
{
    // The title of the podcast
    public String Title { get; set; }

    // The podcast's description
    public String Description { get; set; }

    // Describes if the user has set this podcast as a favorite
    public bool IsFavorite
    {
        get
        {
            return _isFavorite;
        }
        set
        {
            _isFavorite = value;
            OnPropertyChanged("IsFavorite");
        }
    }
    private bool _isFavorite = false;

    public event PropertyChangedEventHandler PropertyChanged;

    private void OnPropertyChanged(String property)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(property));
    }
}
```

Beachten Sie: Das Objekt „PodcastObject“ implementiert die Schnittstelle [INotifyPropertyChanged](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Data.INotifyPropertyChanged), um reagieren zu können, sobald der Benutzer die Eigenschaft „IsFavorite“ ändert.

## <a name="defining-commands-with-the-icommand-interface"></a>Definieren von Befehlen mit der Schnittstelle „ICommand“

Über die [Schnittstelle „ICommand“](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.ICommand) können Sie Befehle definieren, die für mehrere Eingabearten verfügbar sind. Ein Beispiel: Statt den Code eines Löschbefehls in identischer Form in zwei unterschiedliche Ereignishandler zu schreiben (einmal in einen Handler für das Drücken der ENTF-Taste und einmal in einen Handler für das Rechtsklicken auf „Löschen“ in einem Kontextmenü), können Sie Ihre Löschlogik einmalig als Schnittstelle des Typs [ICommand](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.ICommand) implementieren und anschließend für verschiedene Eingabearten verfügbar machen.

Dazu müssen Sie die Schnittstelle „ICommand“ definieren, die die Aktion „Als Favorit speichern“ darstellt. In diesem Beispiel verwenden Sie die Methode [Execute](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.ICommand#Windows_UI_Xaml_Input_ICommand_Execute_) des Befehls, um einen Podcast als Favorit zu speichern. Der betreffende Podcast wird über den Parameter des Befehls an die Methode „Execute“ übergeben. Dieser Parameter kann mithilfe der Eigenschaft „CommandParameter“ gebunden werden.

```csharp
public class FavoriteCommand: ICommand
{
    public event EventHandler CanExecuteChanged;

    public bool CanExecute(object parameter)
    {
        return true;
    }
    public void Execute(object parameter)
    {
        // Perform the logic to "favorite" an item.
        (parameter as PodcastObject).IsFavorite = true;
    }
}
```

Wenn Sie einen Befehl für mehrere Sammlungen und Elemente verwenden möchten, können Sie ihn als Ressource auf der Seite oder in der App speichern.

```xaml
<Application.Resources>
    <local:FavoriteCommand x:Key="favoriteCommand" />
</Application.Resources>
```

Zur Ausführung des Befehls rufen Sie seine Methode [Execute](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.ICommand#Windows_UI_Xaml_Input_ICommand_Execute_) auf.

```csharp
// Favorite the item using the defined command
var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
favoriteCommand.Execute(PodcastObject);
```


## <a name="creating-a-usercontrol-to-respond-to-a-variety-of-inputs"></a>Erstellen eines „UserControl“-Steuerelements zur Reaktion auf verschiedene Eingabearten

Wenn Sie eine Liste von Elementen implementieren und jedes dieser Elemente auf verschiedene Eingabearten reagieren können soll, können Sie zur Vereinfachung des Codes jeweils ein [UserControl](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.UserControl)-Steuerelement für die einzelnen Elemente definieren. In diesem Steuerelement wiederum definieren Sie das Kontextmenü und die Ereignishandler des jeweiligen Elements. 

So erstellen Sie ein „UserControl“-Steuerelement in Visual Studio:
1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt. Es wird ein Kontextmenü angezeigt.
2. Klicken Sie auf **Hinzufügen > Neues Element...**. <br />Das Dialogfeld **Neues Element hinzufügen** wird angezeigt. 
3. Wählen Sie aus der Liste der Elemente die Option „UserControl“ aus. Geben Sie dem Element einen Namen, und klicken Sie auf **Hinzufügen**. Visual Studio generiert nun einen „UserControl“-Stub. 

In unserem Podcast-Beispiel werden alle Podcasts in einer Liste angezeigt. In dieser Liste gibt es verschiedene Möglichkeiten, wie der Benutzer einen Podcast als Favorit speichern kann. Konkret kann der Benutzer folgende Aktionen ausführen, um einen Podcast als Favorit zu speichern:
- Aufrufen eines Kontextmenüs
- Drücken einer Tastenkombination
- Anzeigen einer Hoverschaltfläche
- Durchführen einer Wischgeste

Um diese Verhaltensweisen zu kapseln und den Befehl „FavoriteCommand“ anwenden zu können, erstellen Sie nun ein neues [UserControl](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Controls.UserControl)-Steuerelement namens „PodcastUserControl“, das einen Podcast in der Liste darstellt.

„PodcastUserControl“ zeigt die Felder des Objekts „PodcastObject“ als „TextBlock“-Objekte an und kann auf verschiedene Benutzerinteraktionen reagieren. Im weiteren Verlauf des Artikels wird das „PodcastUserControl“-Steuerelement referenziert und erweitert.

**PodcastUserControl.xaml**
```xaml
<UserControl
    x:Class="ContextCommanding.PodcastUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    IsTabStop="True" UseSystemFocusVisuals="True"
    >
    <Grid Margin="12,0,12,0">
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**PodcastUserControl.xaml.cs**
```csharp
public sealed partial class PodcastUserControl : UserControl
{
    public static readonly DependencyProperty PodcastObjectProperty =
        DependencyProperty.Register(
            "PodcastObject",
            typeof(PodcastObject),
            typeof(PodcastUserControl),
            new PropertyMetadata(null));

    public PodcastObject PodcastObject
    {
        get { return (PodcastObject)GetValue(PodcastObjectProperty); }
        set { SetValue(PodcastObjectProperty, value); }
    }

    public PodcastUserControl()
    {
        this.InitializeComponent();

        // TODO: We will add event handlers here.
    }
}
```

Beachten Sie: „PodcastUserControl“ enthält einen Verweis auf „PodcastObject“ in Gestalt von „DependencyProperty“. So lassen sich Objekte des Typs „PodcastObject“ an das „PodcastUserControl“-Steuerelement binden.

Nun erstellen Sie einige Objekte des Typs „PodcastObject“ und binden diese Objekte an ein „ListView“-Element, um eine Podcast-Liste zu erstellen. Die „PodcastUserControl“-Objekte beschreiben die Visualisierung der „PodcastObject“-Objekte und werden daher über die Eigenschaft „ItemTemplate“ von „ListView“ festgelegt.

**MainPage.xaml**
```xaml
<ListView x:Name="ListOfPodcasts"
            ItemsSource="{x:Bind podcasts}">
    <ListView.ItemTemplate>
        <DataTemplate x:DataType="local:PodcastObject">
            <local:PodcastUserControl PodcastObject="{x:Bind Mode=OneWay}" />
        </DataTemplate>
    </ListView.ItemTemplate>
    <ListView.ItemContainerStyle>
        <!-- The PodcastUserControl will entirely fill the ListView item and handle tabbing within itself. -->
        <Style TargetType="ListViewItem" BasedOn="{StaticResource ListViewItemRevealStyle}">
            <Setter Property="HorizontalContentAlignment" Value="Stretch" />
            <Setter Property="Padding" Value="0"/>
            <Setter Property="IsTabStop" Value="False"/>
        </Style>
    </ListView.ItemContainerStyle>
</ListView>
```

## <a name="creating-context-menus"></a>Erstellen von Kontextmenüs

Kontextmenüs zeigen bei Aufruf durch den Benutzer eine Liste von Befehlen oder Optionen an. Sie stellen Kontextbefehle für das ihnen zugeordnete Element bereit und sind in der Regel für sekundäre Aktionen reserviert, die nur für dieses Element verfügbar sind.

![Anzeigen eines Kontextmenüs für ein Element](images/ContextualCommand_RightClick.png)

Benutzer können Kontextmenüs über die folgenden „Kontextaktionen“ aufrufen:

| Eingabeart    | Kontextaktion                          |
| -------- | --------------------------------------- |
| Maus    | Rechtsklick                             |
| Tastatur | UMSCHALT+F10, Menütaste                  |
| Toucheingabe    | Langes Drücken auf das Element                      |
| Stift      | Drücken der Drucktaste, langes Drücken auf das Element |
| Gamepad  | Menütaste                             |

**Da Kontextmenüs über jede Eingabeart geöffnet werden können, sollten sie sämtliche Kontextbefehle enthalten, die für das jeweilige Listenelement verfügbar sind.**

### <a name="contextflyout"></a>ContextFlyout

Über die in der Klasse „UIElement“ definierte [Eigenschaft „ContextFlyout“](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_ContextFlyout) können Sie ganz einfach ein Kontextmenü erstellen, das bei allen Eingabearten funktioniert. Über die Klasse „MenuFlyout“ stellen Sie ein Flyout bereit, das Ihr Kontextmenü darstellt. Sobald der Benutzer eine der oben aufgeführten „Kontextaktionen“ durchführt, wird das dem Element zugeordnete „MenuFlyout“-Objekt angezeigt.

In diesem Beispiel fügen Sie „PodcastUserControl“ eine Eigenschaft „ContextFlyout“ hinzu. Das in der Eigenschaft „ContextFlyout“ definierte „MenuFlyout“-Objekt enthält ein einziges Element, über das sich Podcasts als Favorit speichern lassen. Beachten Sie: „MenuFlyoutItem“ verwendet den oben definierten Befehl „favoriteCommand“, wobei „CommandParameter“ an „PodcastObject“ gebunden ist.

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <MenuFlyout>
            <MenuFlyoutItem Text="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" />
        </MenuFlyout>
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <!-- ... -->
    </Grid>
</UserControl>

```

Sie können auch das [Ereignis „ContextRequested“](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_ContextRequested) verwenden, um auf Kontextaktionen zu reagieren. Das Ereignis „ContextRequested“ wird nicht ausgelöst, wenn die Eigenschaft „ContextFlyout“ definiert wurde.

## <a name="creating-input-accelerators"></a>Erstellen von Eingabebeschleunigern

Für jedes Element in einer Sammlung sollte ein Kontextmenü mit allen verfügbaren Kontextbefehlen implementiert sein. Vielleicht möchten Sie jedoch, dass Benutzer eine kleinere Gruppe häufig verwendeter Befehle schnell ausführen können. Beispiel: In einer E-Mail-App gibt es die sekundären Befehle „Antworten“, „Archivieren“, „In Ordner verschieben“, „Kennzeichnen“ und „Löschen“. Am häufigsten verwendet werden jedoch die Befehle „Löschen“ und „Kennzeichnen“. Sobald Sie wissen, welche Befehle am häufigsten verwendet werden, können Sie eingabeartbasierte Beschleuniger implementieren, damit Ihre Benutzer diese Befehle einfacher ausführen können.

In unserer Podcast-App ist der Befehl „Als Favorit speichern“ einer der am häufigsten ausgeführten Befehle.

### <a name="keyboard-accelerators"></a>Beschleuniger für die Tastatureingabe

#### <a name="shortcuts-and-direct-key-handling"></a>Behandeln von Tastenkombinationen und Einzeltasten

![Aktionsausführung per STRG oder F-Taste](images/ContextualCommand_Keyboard.png)

Je nach Art des Inhalts können Sie bestimmte Tastenkombinationen für die Ausführung einer Aktion festlegen. In einer E-Mail-App beispielsweise könnten sich ausgewählte E-Mails über die ENTF-Taste löschen lassen. In einer Podcast-App könnte die Tastenkombination STRG+S oder eine der F-Tasten einen Podcast als Favorit zur späteren Wiedergabe speichern. Obwohl einige Befehle allgemein bekannte Standardtasten bzw. Standardtastenkombinationen haben (z.B. ENTF zum Löschen), sind die Tasten und Tastenkombinationen anderer Befehle jeweils App-spezifisch oder domänenspezifisch. Verwenden Sie falls möglich allgemein bekannte Tasten und Tastenkombinationen, oder zeigen Sie für den Benutzer eine kurze QuickInfo mit der Taste oder Tastenkombination für den jeweiligen Befehl an.

Über das Ereignis [KeyDown](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_KeyDownEvent) kann Ihre App auf einen Tastendruck des Benutzers reagieren. In der Regel gehen die Benutzer davon aus, dass die App beim ersten Tastendruck reagiert, nicht erst, wenn sie die Taste wieder loslassen.

In diesem Beispiel zeigen wir Ihnen, wie Sie den Handler „KeyDown“ zu „PodcastUserControl“ hinzufügen können, damit ein Podcast als Favorit gespeichert wird, wenn der Benutzer STRG+S oder eine der F-Tasten drückt. Dabei verwenden Sie denselben Befehl wie zuvor.

**PodcastUserControl.xaml.cs**
```csharp
// Respond to the F and Ctrl+S keys to favorite the focused item.
protected override void OnKeyDown(KeyRoutedEventArgs e)
{
    var ctrlState = CoreWindow.GetForCurrentThread().GetKeyState(VirtualKey.Control);
    var isCtrlPressed = (ctrlState & CoreVirtualKeyStates.Down) == CoreVirtualKeyStates.Down || (ctrlState & CoreVirtualKeyStates.Locked) == CoreVirtualKeyStates.Locked;

    if (e.Key == Windows.System.VirtualKey.F || (e.Key == Windows.System.VirtualKey.S && isCtrlPressed))
    {
        // Favorite the item using the defined command
        var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
        favoriteCommand.Execute(PodcastObject);
    }
}
```

### <a name="mouse-accelerators"></a>Beschleuniger für die Mauseingabe

![Anzeigen einer Schaltfläche durch Platzieren des Mauszeigers auf einem Element](images/ContextualCommand_HovertoReveal.png)

Benutzer sind vertraut mit per Rechtsklick aufrufbaren Kontextmenüs. Möglicherweise möchten Sie jedoch, dass sie häufig verwendete Befehle mit nur einem einzigen Mausklick ausführen können. Zu diesem Zweck können Sie der Canvas Ihres Sammlungselements dedizierte Schaltflächen hinzufügen. Damit Ihre Benutzer schnell mithilfe der Maus Aktionen durchführen können, die Benutzeroberfläche aber dennoch übersichtlich bleibt, können Sie festlegen, dass diese Schaltflächen nur angezeigt werden, wenn der Benutzer den Zeiger auf einem bestimmten Listenelement platziert.

In unserem Beispiel wird der Befehl „Als Favorit speichern“ durch eine Schaltfläche dargestellt, die direkt in „PodcastUserControl“ definiert ist. Beachten Sie: Die Schaltfläche in diesem Beispiel verwendet denselben Befehl wie zuvor, „favoriteCommand“. Die Sichtbarkeit der Schaltfläche können Sie über „VisualStateManager“ steuern. Diese Klasse schaltet zwischen den verschiedenen visuellen Zuständen um, sobald der Zeiger auf dem Steuerelement platziert wird oder vom Steuerelement entfernt wird.

**PodcastUserControl.xaml**
```xaml
<UserControl>
    <UserControl.ContextFlyout>
        <!-- ... -->
    </UserControl.ContextFlyout>
    <Grid Margin="12,0,12,0">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup x:Name="HoveringStates">
                <VisualState x:Name="HoverButtonsShown">
                    <VisualState.Setters>
                        <Setter Target="hoverArea.Visibility" Value="Visible" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState x:Name="HoverButtonsHidden" />
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>
        <StackPanel>
            <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
            <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
        </StackPanel>
        <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
            <AppBarButton Icon="OutlineStar" Label="Favorite" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" VerticalAlignment="Stretch"  />
        </Grid>
    </Grid>
</UserControl>
```

Die Hoverschaltflächen sollten angezeigt bzw. ausgeblendet werden, sobald der Mauszeiger auf dem Element platziert oder von ihm entfernt wird. Zur Reaktion auf Mausereignisse können Sie die Ereignisse [PointerEntered](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_PointerEnteredEvent) und [PointerExited](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.UIElement#Windows_UI_Xaml_UIElement_PointerExitedEvent) in „PodcastUserControl“ verwenden.

**PodcastUserControl.xaml.cs**
```csharp
protected override void OnPointerEntered(PointerRoutedEventArgs e)
{
    base.OnPointerEntered(e);

    // Only show hover buttons when the user is using mouse or pen.
    if (e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Mouse || e.Pointer.PointerDeviceType == Windows.Devices.Input.PointerDeviceType.Pen)
    {
        VisualStateManager.GoToState(this, "HoverButtonsShown", true);
    }
}

protected override void OnPointerExited(PointerRoutedEventArgs e)
{
    base.OnPointerExited(e);

    VisualStateManager.GoToState(this, "HoverButtonsHidden", true);
}
```

Die im Hoverzustand angezeigten Schaltflächen sind nur bei der Eingabe über Zeigegeräte verfügbar. Da diese Schaltflächen ausschließlich für die Eingabe über Zeigegeräte zur Verfügung stehen, möchten Sie zur Optimierung der Zeigereingabe möglicherweise den Abstand um das jeweilige Schaltflächensymbol minimieren oder vollständig entfernen. Wenn Sie sich dazu entscheiden, muss die Schaltfläche mindestens 20×20px groß sein, damit sie mit Stift und Maus noch gut bedienbar ist.

### <a name="touch-accelerators"></a>Beschleuniger für die Toucheingabe

#### <a name="swipe"></a>Wischgesten

![Aufrufen eines Befehls per Wischgeste](images/ContextualCommand_Swipe.png)

Wischgestenbasierte Befehle sind ein Beschleuniger für die Toucheingabe. Sie ermöglichen es Benutzern von Touchgeräten, häufig verwendete sekundäre Aktionen per Touchgeste auszuführen. Mithilfe von Wischgesten können Benutzer auf Touchgeräten schnell und natürlich mit Inhalten interagieren. So können sie beispielsweise gängige Aktionen wie „Löschen per Wischen“ oder „Aufrufen per Wischen“ durchführen. Weitere Informationen finden Sie im Artikel zum Thema [Wischgestenbasierte Befehle](http://windowsstyleguide/controls-and-patterns/swipe/).

Zur Integration von Wischgesten in Ihre Sammlung sind zwei Komponenten erforderlich: „SwipeContent" als Host für die Befehle und „SwipeContainer“ als Element-Wrapper, der Interaktionen per Wischgeste möglich macht.

„SwipeContent“ kann als Ressource in „PodcastUserControl“ definiert werden. In diesem Beispiel enthält „SwipeContent" einen Befehl, über den sich ein Element als Favorit speichern lässt.

```xaml
<UserControl.Resources>
    <preview:SwipeContent x:Key="RevealOtherCommands" Mode="Reveal">
        <preview:SwipeContent.Items>
            <preview:SwipeItem Icon="&#xE1CE;" Text="Favorite" Background="Yellow" Invoked="SwipeItem_Invoked"/>
        </preview:SwipeContent.Items>
    </preview:SwipeContent>
</UserControl.Resources>
```

„SwipeContainer“ fungiert als Wrapper für das Element und erlaubt es dem Benutzer, per Wischgesten mit ihm zu interagieren. Beachten Sie, dass „SwipeContainer“ einen Verweis auf „SwipeContent“ in seiner Eigenschaft „RightContent“ enthält. Das Element „Als Favorit speichern“ wird angezeigt, wenn der Benutzer von rechts nach links wischt.

```xaml
<preview:SwipeContainer x:Name="swipeContainer" RightContent="{StaticResource RevealOtherCommands}">
   <!-- The visual state groups moved from the Grid to the SwipeContainer, since the SwipeContainer wraps the Grid. -->
   <VisualStateManager.VisualStateGroups>
       <VisualStateGroup x:Name="HoveringStates">
           <VisualState x:Name="HoverButtonsShown">
               <VisualState.Setters>
                   <Setter Target="hoverArea.Visibility" Value="Visible" />
               </VisualState.Setters>
           </VisualState>
           <VisualState x:Name="HoverButtonsHidden" />
       </VisualStateGroup>
   </VisualStateManager.VisualStateGroups>
   <Grid Margin="12,0,12,0">
       <Grid.ColumnDefinitions>
           <ColumnDefinition Width="*" />
           <ColumnDefinition Width="Auto" />
       </Grid.ColumnDefinitions>
       <StackPanel>
           <TextBlock Text="{x:Bind PodcastObject.Title, Mode=OneWay}" Style="{StaticResource TitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.Description, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}" />
           <TextBlock Text="{x:Bind PodcastObject.IsFavorite, Mode=OneWay}" Style="{StaticResource SubtitleTextBlockStyle}"/>
       </StackPanel>
       <Grid Grid.Column="1" x:Name="hoverArea" Visibility="Collapsed" VerticalAlignment="Stretch">
           <AppBarButton Icon="OutlineStar" Command="{StaticResource favoriteCommand}" CommandParameter="{x:Bind PodcastObject, Mode=OneWay}" IsTabStop="False" LabelPosition="Collapsed" VerticalAlignment="Stretch"  />
       </Grid>
   </Grid>
</preview:SwipeContainer>
```

Sobald der Benutzer über das Display wischt, um den Befehl „Als Favorit speichern“ aufzurufen, wird die Methode „Invoked“ aufgerufen.

```csharp
private void SwipeItem_Invoked(SwipeItem sender, SwipeItemInvokedEventArgs args)
{
    // Favorite the item using the defined command
    var favoriteCommand = Application.Current.Resources["favoriteCommand"] as ICommand;
    favoriteCommand.Execute(PodcastObject);
}
```

#### <a name="pull-to-refresh"></a>Aktualisieren durch Ziehen

Mithilfe der Aktion „Aktualisieren durch Ziehen“ können Benutzer eine Datensammlung per Touchgeste nach unten ziehen, um weitere Daten abzurufen. Weitere Informationen finden Sie im Artikel zum Thema [Aktualisieren durch Ziehen](http://windowsstyleguide/controls-and-patterns/pull-to-refresh-rs3/).

### <a name="pen-accelerators"></a>Beschleuniger für die Stifteingabe

Die Eingabe per Stift ist ebenso präzise wie die Eingabe per Zeigegerät. Mit stiftbasierten Beschleunigern können Benutzer gängige Aktionen wie beispielsweise das Öffnen von Kontextmenüs durchführen. Zum Öffnen eines Kontextmenüs kann der Benutzer mit gedrückter Drucktaste auf das Display tippen oder alternativ lange auf den Inhalt drücken. Er kann den Stift auch über Inhalten platzieren, um ähnlich wie mit der Maus Details zur Benutzeroberfläche (z.B. QuickInfos) aufzurufen oder sekundäre Hoveraktionen anzuzeigen.

Wie Sie Ihre App für die Stifteingabe optimieren können, erfahren Sie im Artikel zum Thema [Interaktion per Eingabestift](http://windowsstyleguide/input-and-devices/pen-and-stylus-interactions/).


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

* Stellen Sie sicher, dass Ihre Benutzer auf sämtliche Befehle zugreifen können, und zwar über alle Typen von UWP-Geräten.
* Integrieren Sie ein Kontextmenü, das alle für ein Sammlungselement verfügbaren Befehle bereitstellt. 
* Implementieren Sie Eingabebeschleuniger für häufig verwendete Befehle. 
* Verwenden Sie zur Implementierung von Befehlen die [Schnittstelle „ICommand“](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.ICommand). 

## <a name="related-topics"></a>Verwandte Themen
* [Schnittstelle „ICommand“](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Input.ICommand)
* [Menüs und Kontextmenüs](http://windowsstyleguide/controls-and-patterns/menus/)
* [Wischgesten](http://windowsstyleguide/controls-and-patterns/swipe/)
* [Aktualisieren durch Ziehen](http://windowsstyleguide/controls-and-patterns/pull-to-refresh)
* [Interaktion per Eingabestift](http://windowsstyleguide/input-and-devices/pen-and-stylus-interactions/)
* [App-Optimierung für Gamepads und Xbox](http://windowsstyleguide/input-and-devices/designing-for-tv)