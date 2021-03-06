---
description: Konfiguriert die XAML-Kompilierung, um partielle Klassen zwischen Markup und CodeBehind zu verknüpfen. Die partielle Codeklasse wird in einer separaten Codedatei definiert, die partielle Markupklasse wird von der Codegenerierung während der XAML-Kompilierung erstellt.
title: xClass-Attribut
ms.assetid: 40A7C036-133A-44DF-9D11-0D39232C948F
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: be238be3414fb17ff64a5c6d5da713f614c297be
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169084"
---
# <a name="xclass-attribute"></a>x:Class-Attribut


Konfiguriert die XAML-Kompilierung, um partielle Klassen zwischen Markup und CodeBehind zu verknüpfen. Die partielle Codeklasse wird in einer separaten Codedatei definiert, die partielle Markupklasse wird von der Codegenerierung während der XAML-Kompilierung erstellt.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax


``` syntax
<object x:Class="namespace.classname"...>
  ...
</object>
```

## <a name="xaml-values"></a>XAML-Werte

| Begriff | BESCHREIBUNG |
|------|-------------|
| Namespace | Dies ist optional. Gibt einen Namespace an, der die partielle Klasse gemäß Angabe durch _classname_. Wenn _Namespace_ angegeben ist, werden _namespace_ und _classname_ durch einen Punkt getrennt. Ist kein _namespace_ angegeben, wird davon ausgegangen, dass _classname_ keinen Namespace besitzt. |
| classname | Erforderlich. Gibt den Namen der partiellen Klasse an, die das geladene XAML und Ihr CodeBehind für dieses XAML verbindet. | 

## <a name="remarks"></a>Bemerkungen

**x:Class** kann als Attribut für ein beliebiges Element, das als Stamm einer XAML-Datei-/-Objektstruktur fungiert und durch Erstellungsaktionen kompiliert wird, oder für den [**Application**](/uwp/api/Windows.UI.Xaml.Application)-Stamm in der Anwendungsdefinition einer kompilierten Anwendung deklariert werden. Wenn Sie **x:Class** für ein Element, bei dem es sich nicht um einen Stammknoten handelt, oder unter beliebigen Umständen für eine nicht mit der Buildaktion **Seite** kompilierte XAML-Datei deklarieren, tritt zur Kompilierzeit ein Fehler auf.

Die als **x:Class** verwendete Klasse darf keine geschachtelte Klasse sein.

Der Wert des **x:Class**-Attributs muss eine Zeichenfolge sein, die den vollqualifizierten Namen einer Klasse angibt. Bei entsprechender CodeBehind-Struktur (Klassendefinition beginnt auf Klassenebene) können Sie die Namespaceinformationen auch weglassen. Die CodeBehind-Datei für eine Seiten- oder Anwendungsdefinition muss sich in einer Codedatei befinden, die Teil des Projekts ist. Die CodeBehind-Klasse muss öffentlich sein. Die CodeBehind-Klasse muss partiell sein.

## <a name="clr-language-rules"></a>CLR-Sprachregeln

Ihre CodeBehind-Datei kann zwar eine C++-Datei sein, bestimmte Konventionen richten sich aber trotzdem nach der CLR-Sprachform, damit keine Abweichungen bei der XAML-Syntax auftreten. Genauer: Das Trennzeichen zwischen Namespace- und Klassennamenkomponenten eines beliebigen **x:Class**-Werts ist immer ein Punkt („.“), auch wenn das Trennzeichen zwischen Namespace und Klassenname in der zum XAML-Code gehörigen C++-Codedatei „::“ ist. Wenn Sie geschachtelte Namespaces in C++ deklarieren, sollte das Trennzeichen zwischen den aufeinander folgenden geschachtelten Namespacezeichenfolgen auch „.“ statt „::“ sein, wenn Sie den *namespace*-Teil des **x:Class**-Werts angeben.