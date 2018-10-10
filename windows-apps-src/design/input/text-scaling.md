---
author: Karl-Bridge-Microsoft
Description: Build UWP apps and custom/templated controls that support platform text scaling.
title: Textskalierung von
label: Text scaling
template: detail.hbs
keywords: UWP, Text, Skalierung, Eingabehilfen, "erleichterte Bedienung", "Stellen Text größer", Benutzerinteraktion, Eingabe anzeigen
ms.author: kbridge
ms.date: 08/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 885ccc89fcbd4315eeed40c3546ef485c515294e
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2018
ms.locfileid: "4461865"
---
# <a name="text-scaling"></a>Textskalierung von

![Beispiel für die Skalierung von 100 % auf 225 % text](images/coretext/text-scaling-news-hero-small.png)  
*Beispiel für Text Skalierung in Windows 10 (100 % bis 225 %)*

## <a name="overview"></a>Übersicht

Lesen von Text auf einem Computerbildschirm (von mobilen Gerät, Laptop, desktop-Monitor auf dem großen Bildschirm von Surface Hub) kann für viele schwierig sein. Einige Benutzer suchen dagegen die Schriftgrade in apps und Websites verwendet, um größer als erforderlich sein.

Um sicherzustellen, dass Text lesbar ist wie für die IT. Benutzer möglich ist, bietet Windows die Möglichkeit für Benutzer über das Betriebssystem und die einzelnen Programmen relativen Schriftgrad ändern. Anstelle eine Bildschirmlupe-app (die in der Regel nur alles in einem Bereich des Bildschirms vergrößert und führt eine eigene Probleme hinsichtlich der Verwendbarkeit) verwenden, ändern Auflösung oder verlassen sich DPI-Skalierung (die alles basierend auf der Anzeige und normale Anzeige ändert Abstand), Benutzer können schnell zugreifen, eine Einstellung, um nur-Text, angefangen bei 100 % (die Standardgröße) Größe bis zu 225 %.

## <a name="support"></a>Unterstützung

Universelle Windows-Anwendungen (sowohl Standard und PWA), Text standardmäßig Skalierung zu unterstützen.

Wenn Ihre UWP-Anwendung benutzerdefinierte Steuerelemente, benutzerdefinierter Text Flächen, hartcodierten Steuerelement Höhen, älteren Frameworks oder 3rd Party-Frameworks enthält, müssen Sie wahrscheinlich einige Updates für eine konsistente und nützlich Erfahrung für Ihre Benutzer sicherzustellen.  

DirectWrite, GDI und XAML-SwapChainPanels unterstützen nativ Text zu skalieren, keine während Win32-Unterstützung auf Menüs, Symbole und Symbolleisten begrenzt ist.  

<!-- If you want to support text scaling in your application with these frameworks, you’ll need to support the text scaling change event outlined below and provide alternative sizes for your UI and content.   -->

## <a name="user-experience"></a>Benutzerfreundlichkeit

Benutzer können Textanzeige anpassen mit dem stellen Text größer Schieberegler in den Einstellungen -> -> erleichterte Bedienung Vision/Bildschirm.

![Beispiel für die Skalierung von 100 % auf 225 % text](images/coretext/text-scaling-settings-100-small.png)  
*Textanzeige Einstellung aus den Einstellungen -> erleichterte Bedienung Vision/Bildschirm ->*

## <a name="ux-guidance"></a>Erläuterungen zur Benutzeroberfläche

Wie Text geändert wird, Steuerelemente und Container müssen auch die Größe und umbrechen, um den Text und das neue Layout aufzunehmen. Wie bereits erwähnt abhängig von der app-Frameworks und -Plattform ist der Großteil der Arbeit für Sie erledigt. Die folgende UX-Richtlinien werden diese Fälle, in denen es nicht behandelt.

### <a name="use-the-platform-controls"></a>Verwenden Sie die Steuerelemente der Plattform

Sagten wir dies bereits? Dabei ist zu wiederholen: Wenn möglich, müssen Sie die integrierten Steuerelemente mit den verschiedenen Windows-app-Frameworks bereitgestellten immer verwenden, um die umfassendste Benutzeroberfläche für wenig Aufwand wie möglich abzurufen.

Alle UWP-Textsteuerelemente z. B. den vollständigen Text Skalierung Erfahrung ohne Anpassung oder Templating unterstützen.

Hier ist ein Codeausschnitt aus einer einfachen UWP-app, die eine Reihe von standard-Text-Steuerelemente enthält:

``` xaml
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <TextBlock Grid.Row="0" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test" 
                HorizontalTextAlignment="Center" />
    <Grid Grid.Row="1">
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto"/>
            <ColumnDefinition Width="*"/>
            <ColumnDefinition Width="Auto"/>
        </Grid.ColumnDefinitions>
        <Image Grid.Column="0" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
        <StackPanel Grid.Column="1" 
                    HorizontalAlignment="Center">
            <TextBlock TextWrapping="WrapWholeWords">
                The quick brown fox jumped over the lazy dog.
            </TextBlock>
            <TextBox PlaceholderText="Type something here" />
        </StackPanel>
        <Image Grid.Column="2" 
                Source="Assets/StoreLogo.png" 
                Width="450" Height="450"/>
    </Grid>
    <TextBlock Grid.Row="2" 
                Style="{ThemeResource TitleTextBlockStyle}"
                Text="Text scaling test footer" 
                HorizontalTextAlignment="Center" />
</Grid>
```

![Skalierung von 100 % auf 225 % animierten text](images/coretext/text-scaling.gif)  
*Animierte Text skalieren*

### <a name="use-auto-sizing"></a>Verwenden Sie die automatische Anpassung

Geben Sie keine absolute Größen für Ihre Steuerelemente. Wann immer möglich, können Sie die Größe der Steuerelemente, die automatisch basierend auf Benutzer- und geräteeinstellungen Plattform.  

In diesem Codeausschnitt aus dem vorherigen Beispiel verwenden wir die `Auto` und `*` Breitenwerte für eine Gruppe von Spalten des Rasters und ermöglichen Sie die Plattform passen Sie das app-Layout basierend auf der Größe der Elemente innerhalb des Rasters.

``` xaml
<Grid.ColumnDefinitions>
    <ColumnDefinition Width="Auto"/>
    <ColumnDefinition Width="*"/>
    <ColumnDefinition Width="Auto"/>
</Grid.ColumnDefinitions>
```

### <a name="use-text-wrapping"></a>Verwenden Sie den Textumbruch

Um sicherzustellen, dass das Layout Ihrer App als flexibel und anpassbar wie möglich ist, aktivieren Sie Textumbruch in jedes Steuerelement, das Text enthält (viele Steuerelemente nicht Textumbruch standardmäßig unterstützt).

Wenn Sie den Textumbruch nicht angeben, verwendet die Plattform andere Methoden zum Anpassen des Layouts, einschließlich Zuschneiden (siehe vorherigen Beispiel).

Hier verwenden wir die `AcceptsReturn` und `TextWrapping` TextBox-Eigenschaften, um sicherzustellen, dass unsere Layout flexibel wie möglich ist.

``` xaml
<TextBox PlaceholderText="Type something here" 
          AcceptsReturn="True" TextWrapping="Wrap" />
```

![Animierter Text, die Skalierung von 100 % auf 225 % mit Textumbruch](images/coretext/text-scaling-textwrap.gif)  
*Animierte Text Skalieren mit Textumbruch*

### <a name="specify-text-trimming-behavior"></a>Geben Sie Text Zuschneiden Verhalten

Wenn Textumbruch nicht das gewünschte Verhalten ist, können die meisten Textsteuerelemente entweder Ihr Text zuschneiden, oder geben Sie Ellipsen für den Text Zuschneiden Verhalten. Zuschneiden wird bevorzugt, um Ellipsen, als Ellipsen selbst Speicherplatz belegen.

> [!NOTE]
> Wenn Sie Sie den Text zu beschneiden müssen, clip am Ende der Zeichenfolge, die nicht am Anfang.

In diesem Beispiel zeigen wir, wie Sie Text in einem TextBlock zu beschneiden mithilfe der [TextTrimming](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.texttrimming) -Eigenschaft.

``` xaml
<TextBlock TextTrimming="Clip">
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

![Skalierung von 100 % auf 225 % mit Text Zuschneiden Text](images/coretext/text-scaling-clipping-small.png)  
*Text skalieren mit Text Zuschneiden*

### <a name="use-a-tooltip"></a>Verwenden Sie eine QuickInfo

Wenn Sie Text zuschneiden, verwenden Sie QuickInfos, um den vollständigen Text für Ihre Benutzer bereitzustellen.

Hier fügen wir eine QuickInfo in ein TextBlock-Element, das Textumbruch unterstützt:

``` xaml
<TextBlock TextTrimming="Clip">
    <ToolTipService.ToolTip>
        <ToolTip Content="The quick brown fox jumped over the lazy dog."/>
    </ToolTipService.ToolTip>
    The quick brown fox jumped over the lazy dog.
</TextBlock>
```

### <a name="dont-scale-font-based-icons-or-symbols"></a>Skalieren Sie nicht Schriftart-basierte Symbole oder Symbole

Wenn Sie Symbole Schriftart-basierte zur Betonung oder als Ergänzung zu verwenden, deaktivieren Sie die Skalierung auf diese Zeichen.

Legen Sie die Eigenschaft [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled) auf `false` für die meisten XAML-Steuerelemente.

### <a name="support-text-scaling-natively"></a>Unterstützung für Text nativ Skalierung

Behandeln Sie das [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged) UISettings System-Ereignis in Ihren benutzerdefinierten Framework Steuerelemente. Dieses Ereignis wird jedes Mal ausgelöst, wenn der Benutzer den Skalierungsfaktor Text auf ihrem System festlegt.

## <a name="summary"></a>Zusammenfassung

Dieses Thema enthält eine Übersicht über Text-Unterstützung in Windows Skalierung und enthält (UX) und Entwickler Richtlinien zum Anpassen der Benutzeroberfläche.

## <a name="related-articles"></a>Verwandte Artikel

### <a name="api-reference"></a>API-Referenz

- [IsTextScaleFactorEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istextscalefactorenabled)
- [TextScaleFactorChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.textscalefactorchanged)