---
title: Zeichnen auf dem Bildschirm
description: Erfahren Sie, wie Sie mit gxdi und Direct3D-APIs OpenGL-Code in DirectX portieren und einen spinenden Cube auf den Bildschirm ziehen.
ms.assetid: cc681548-f694-f613-a19d-1525a184d4ab
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, DirectX, Grafiken
ms.localizationpriority: medium
ms.openlocfilehash: 6499f860dce0c3bb4f596b372a2de02b04b0d4ac
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175344"
---
# <a name="draw-to-the-screen"></a>Zeichnen auf dem Bildschirm




**Wichtige APIs**

-   [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)
-   [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)
-   [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)

Schließlich wird der Code portiert, der den sich drehenden Würfel auf den Bildschirm zeichnet.

In OpenGL ES 2.0 wird der Zeichnungskontext als EGLContext-Typ definiert. Er enthält die Fenster- und Oberflächenparameter sowie die erforderlichen Ressourcen zum Zeichnen in die Renderziele, mit denen das im Fenster angezeigte endgültige Bild erstellt wird. Mithilfe dieses Kontextes konfigurieren Sie die Grafikressourcen zur korrekten Anzeige der Ergebnisse Ihrer Shaderpipeline auf dem Bildschirm. Eine der primären Ressourcen ist der "Hintergrundpuffer" (auch als "Framepufferobjekt" bezeichnet), der die endgültigen, zusammengesetzten und für die Darstellung auf dem Bildschirm fertigen Renderziele enthält.

Bei Direct3D ist der Prozess zum Konfigurieren der Grafikressourcen für das Zeichnen auf den Bildschirm didaktischer und erfordert mehr APIs. (Eine Microsoft Visual Studio Direct3D-Vorlage kann diesen Vorgang jedoch erheblich vereinfachen.) Um einen Kontext (als Direct3D-Gerätekontext bezeichnet) zu erhalten, müssen Sie zunächst ein [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)-Objekt erhalten und dieses zur Erstellung und Konfigurierung eines [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)-Objekts verwenden. Diese beiden Objekte werden zusammen verwendet, um die spezifischen Ressourcen zu konfigurieren, die Sie zum Zeichnen auf den Bildschirm benötigen.

Kurz gesagt: Die DXGI-APIs enthalten in erster Linie APIs zum Verwalten von Ressourcen, die direkt den Grafikadapter betreffen, und Direct3D enthält die APIs, mit denen Sie die GPU und Ihr in der CPU ausgeführtes Hauptprogramm verknüpfen können.

Zum Vergleich werden in diesem Beispiel die folgenden relevanten Typen aus den APIs verwendet:

-   [**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1): Stellt eine virtuelle Darstellung des Grafikgeräts und seiner Ressourcen bereit.
-   [**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1): Stellt die Schnittstelle zum Konfigurieren von Puffern und Ausgeben von Renderbefehlen bereit.
-   [**IDXGISwapChain1**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1): Die Swapchain entspricht dem Hintergrundpuffer in OpenGL ES 2.0. Sie ist der Bereich im Speicher des Grafikadapters, der die endgültigen gerenderten Bilder für die Anzeige enthält. Dieser Bereich wird als „Swapchain“ bezeichnet, da er mehrere beschreibbare Puffer enthält, die ausgetauscht werden können, um das aktuelle Renderbild auf dem Bildschirm darzustellen.
-   [**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview): Enthält den 2D-Bitmappuffer, in den der Direct3D-Gerätekontext zeichnet und der von der Swapchain dargestellt wird. Wie bei OpenGL ES 2.0 können Sie mehrere Renderziele haben, von denen einige nicht an die Swapchain gebunden sind, die aber für Schattierungstechniken mit mehreren Durchgängen verwendet werden.

In der Vorlage enthält das Rendererobjekt die folgenden Felder:

Direct3D 11: Gerät- und Gerätekontextdeklarationen

``` syntax
Platform::Agile<Windows::UI::Core::CoreWindow>       m_window;

Microsoft::WRL::ComPtr<ID3D11Device1>                m_d3dDevice;
Microsoft::WRL::ComPtr<ID3D11DeviceContext1>          m_d3dContext;
Microsoft::WRL::ComPtr<IDXGISwapChain1>                      m_swapChainCoreWindow;
Microsoft::WRL::ComPtr<ID3D11RenderTargetView>          m_d3dRenderTargetViewWin;
```

Hier sehen Sie, wie der Hintergrundpuffer als Renderziel konfiguriert und für die Swapchain bereitgestellt wird.

``` syntax
ComPtr<ID3D11Texture2D> backBuffer;
m_swapChainCoreWindow->GetBuffer(0, IID_PPV_ARGS(backBuffer));
m_d3dDevice->CreateRenderTargetView(
  backBuffer.Get(),
  nullptr,
  &m_d3dRenderTargetViewWin);
```

Die Direct3D-Laufzeit erstellt implizit eine [**IDXGISurface1**](/windows/desktop/api/dxgi/nn-dxgi-idxgisurface1) für die [**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d). Diese stellt die Textur als „Hintergrundpuffer“ dar, der von der Swapchain zur Anzeige verwendet werden kann.

Für die Initialisierung und Konfiguration des Direct3D-Geräts und -Gerätekontexts sowie der Renderziele dienen die benutzerdefinierten Methoden **CreateDeviceResources** und **CreateWindowSizeDependentResources** in der Direct3D-Vorlage.

Weitere Informationen zum Direct3D-Gerätekontext, der sich auf die EGL und den eglcontext-Typ bezieht, finden Sie unter [Port-EGL-Code zu DXGI und Direct3D](moving-from-egl-to-dxgi.md).

## <a name="instructions"></a>Instructions

### <a name="step-1-rendering-the-scene-and-displaying-it"></a>Schritt 1: Rendern und Anzeigen der Szene

Nach dem Aktualisieren der Würfeldaten (in diesem Fall durch eine leichte Drehung des Würfels um die y-Achse) legt die Rendermethode den Viewport auf die Dimensionen des Zeichnungskontextes (EGLContext) fest. Dieser Kontext enthält den Farbpuffer, der unter Verwendung der konfigurierten Anzeige (EGLDisplay) auf der Fensteroberfläche (EGLSurface) angezeigt wird. Dabei werden folgende Schritte ausgeführt: Die Scheitelpunktdaten-Attribute werden aktualisiert, der Indexpuffer wird erneut gebunden, der Würfel wird gezeichnet, und der von der Schattierungspipeline gezeichnete Farbpuffer wird ausgetauscht und auf der Anzeigeoberfläche dargestellt.

OpenGL ES 2.0: Rendern eines Frames für die Anzeige

``` syntax
void Render(GraphicsContext *drawContext)
{
  Renderer *renderer = drawContext->renderer;

  int loc;
   
  // Set the viewport
  glViewport ( 0, 0, drawContext->width, drawContext->height );
   
   
  // Clear the color buffer
  glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
  glEnable(GL_DEPTH_TEST);


  // Use the program object
  glUseProgram (renderer->programObject);

  // Load the a_position attribute with the vertex position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_position");
  glVertexAttribPointer(loc, 3, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), 0);
  glEnableVertexAttribArray(loc);

  // Load the a_color attribute with the color position portion of a vertex buffer element
  loc = glGetAttribLocation(renderer->programObject, "a_color");
  glVertexAttribPointer(loc, 4, GL_FLOAT, GL_FALSE, 
      sizeof(Vertex), (GLvoid*) (sizeof(float) * 3));
  glEnableVertexAttribArray(loc);

  // Bind the index buffer
  glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, renderer->indexBuffer);

  // Load the MVP matrix
  glUniformMatrix4fv(renderer->mvpLoc, 1, GL_FALSE, (GLfloat*) &renderer->mvpMatrix.m[0][0]);

  // Draw the cube
  glDrawElements(GL_TRIANGLES, renderer->numIndices, GL_UNSIGNED_INT, 0);

  eglSwapBuffers(drawContext->eglDisplay, drawContext->eglSurface);
}
```

Der Prozess in Direct3D 11 ist sehr ähnlich. (Es wird vorausgesetzt, dass Sie die Viewport- und Renderzielkonfiguration der Direct3D-Vorlage verwenden.)

-   Aktualisieren Sie die Konstanten Puffer (in diesem Fall die Matrix Model-View-Projection) mit Aufrufen von [**ID3D11DeviceContext1:: updatesubresource**](/windows/desktop/api/d3d11_1/nf-d3d11_1-id3d11devicecontext1-updatesubresource1).
-   Legen Sie den Scheitelpunktpuffer mit [**ID3D11DeviceContext1::IASetVertexBuffers**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetvertexbuffers) fest.
-   Legen Sie den Index Puffer mit [**ID3D11DeviceContext1:: iasetindexbuffer**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetindexbuffer)fest.
-   Legen Sie die spezifische Dreieckstopologie (eine Dreiecksliste) mit [**ID3D11DeviceContext1::IASetPrimitiveTopology**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetprimitivetopology) fest.
-   Legen Sie das Eingabelayout des Scheitelpunktpuffers mit [**ID3D11DeviceContext1::IASetInputLayout**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-iasetinputlayout) fest.
-   Binden Sie den Vertex-Shader mit [**ID3D11DeviceContext1:: vssetshader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-vssetshader).
-   Binden Sie den Fragment-Shader mit [**ID3D11DeviceContext1::PSSetShader**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-pssetshader).
-   Senden Sie die indizierten Vertices durch die Shader, und geben Sie die Farbergebnisse an den renderzielpuffer mit [**ID3D11DeviceContext1::D rawindexed**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-drawindexed)aus.
-   Zeigen Sie den Renderzielpuffer mit [**IDXGISwapChain1::Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) an.

Direct3D 11: Rendern eines Frames zur Anzeige

``` syntax
void RenderObject::Render()
{
  // ...

  // Only update shader resources that have changed since the last frame.
  m_d3dContext->UpdateSubresource(
    m_constantBuffer.Get(),
    0,
    NULL,
    &m_constantBufferData,
    0,
    0);

  // Set up the IA stage corresponding to the current draw operation.
  UINT stride = sizeof(VertexPositionColor);
  UINT offset = 0;
  m_d3dContext->IASetVertexBuffers(
    0,
    1,
    m_vertexBuffer.GetAddressOf(),
    &stride,
    &offset);

  m_d3dContext->IASetIndexBuffer(
    m_indexBuffer.Get(),
    DXGI_FORMAT_R16_UINT,
    0);

  m_d3dContext->IASetPrimitiveTopology(D3D11_PRIMITIVE_TOPOLOGY_TRIANGLELIST);
  m_d3dContext->IASetInputLayout(m_inputLayout.Get());

  // Set up the vertex shader corresponding to the current draw operation.
  m_d3dContext->VSSetShader(
    m_vertexShader.Get(),
    nullptr,
    0);

  m_d3dContext->VSSetConstantBuffers(
    0,
    1,
    m_constantBuffer.GetAddressOf());

  // Set up the pixel shader corresponding to the current draw operation.
  m_d3dContext->PSSetShader(
    m_pixelShader.Get(),
    nullptr,
    0);

  m_d3dContext->DrawIndexed(
    m_indexCount,
    0,
    0);

    // ...

  m_swapChainCoreWindow->Present1(1, 0, &parameters);
}

```

Nachdem [**IDXGISwapChain1::Present1**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) aufgerufen wurde, wird der Frame an die konfigurierte Anzeige ausgegeben.

## <a name="previous-step"></a>Vorheriger Schritt


[Portieren des GLSL-Codes](port-the-glsl.md)

## <a name="remarks"></a>Bemerkungen

In diesem Beispiel wurden viele der komplexen Schritte beim Konfigurieren von Geräteressourcen ausgelassen, insbesondere für UWP-DirectX-Apps (Universelle Windows-Plattform). Wir empfehlen, den vollständigen Vorlagencode durchzugehen, vor allem die Teile, die die Einrichtung und Verwaltung der Fenster- und Geräteressourcen betreffen. UWP-Apps müssen sowohl Drehungsereignisse als auch Anhalte-/Fortsetzungsereignisse unterstützen. Die Vorlage zeigt die empfohlenen Methoden zum Behandeln des Verlusts einer Schnittstelle oder einer Änderung der Anzeigeparameter.

## <a name="related-topics"></a>Zugehörige Themen


* [Gewusst wie: portieren eines einfachen OpenGL es 2,0-Renderers zu Direct3D 11](port-a-simple-opengl-es-2-0-renderer-to-directx-11-1.md)
* [Portieren der Shaderobjekte](port-the-shader-config.md)
* [Portieren des GLSL-Codes](port-the-glsl.md)
* [Zeichnen auf den Bildschirm](draw-to-the-screen.md)

 

 