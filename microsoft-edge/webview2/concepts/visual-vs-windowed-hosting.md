---
title: Visual vs. windowed hosting of WebView2
description: Learn about hosting WebView2 in visual and windowed environments.
author: MSEdgeTeam
ms.author: msedgedevrel
ms.topic: conceptual
ms.prod: microsoft-edge
ms.technology: webview
ms.date: 08/23/2022
---
# Visual vs. windowed hosting of WebView2

This article describes the two options you have for hosting the Microsoft Edge WebView2 control in your application. Before you decide which hosting option to use in your app, it's important to understand visual and windowed hosting. In addition to having different requirements, each form of hosting has its own constraints and benefits. Hosting is about how to get WebView2 onto different platforms.

Both approaches implement a whiteboard or container approach for storing and presenting information. The hosting form determines how the application works and the key differences are as follows:

*  Visual: The host app takes spatial input (for example, mouse, touch) from the user. The app sends this input to the WebView2 control.
*  Windowed: The WebView2 control takes input from the operating system (OS).  The OS sends the input to the WebView2.


<!-- ====================================================================== -->
## Compatibility and constraints

Key compatibility limitations include the operating system and rendering in framework and non-framework applications.

#### Operating system

Windows 7 and Windows 8 can only do windowed hosting.
  
#### Rendering WebView2 in framework and non-framework applications

If you're using a UI framework for your application, you should use the corresponding WebView2 element for that UI framework. If you aren't using a UI framework for your application (for example, Win32, React Native, etc.) or your UI framework doesn't have a WebView2 element, you'll have to create `CoreWebView2Controller` and render it into the desired application. 

> [!NOTE]
> If your application's UI is built using `DirectComposition` or `Windows.UI.Composition`, then you should use `CoreWebView2CompositionController`, otherwise you should use `CoreWebView2Controller`. 

The `CoreWebView2Controller` properties and methods:

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.CoreWebView2 Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.corewebview2?view=webview2-dotnet-1.0.1293.44)
* [CoreWebView2Controller.Close Method](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.close?view=webview2-dotnet-1.0.1293.44)
* [CoreWebView2Environment.CreateCoreWebView2ControllerAsync Method](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2controllerasync)


##### [WinRT/C#](#tab/winrtcsharp)

* [CoreWebView2Controller.CoreWebView2 Property](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#get_corewebview2)
* [CoreWebView2Controller.Close Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#close)
* [CoreWebView2Environment.CreateCoreWebView2ControllerAsync Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2environment)

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller::CoreWebView2 Property](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#get_corewebview2),<!-- no put-->
* [ICoreWebView2Controller::Close Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#close)
* [ICoreWebView2Environment::CreateCoreWebView2Controller Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2environment?view=webview2-1.0.1293.44#createcorewebview2controller)

---

<!-- ====================================================================== -->
## Windowed hosting

Windowed hosting can be described as a whiteboard or container that stores information. You can have multiple whiteboards in your application that will each be used as a WebView component to access web content. Some of the Output/Input commands are handled for you by the framework you choose, however you will still need to handle window management. 

Some of the most notable benefits for Windowed hosting including the following:

* Zooming and rasterization scales (for example, menus, context menus, and so on) will also automatically scale to the application's parent `HWND`. It also handles how the WebView manages being focused and tabbing in/out of itself when it reaches the final element. 
* The application handles keyboard accelerators and keyboard shortcuts when in the WebView. For example, Ctrl + C in a WebView will know that you are trying to copy content in the WebView, and not pressing Ctrl and C on the keyboard. 
* If you don't have to manage the various composition-based rendering (for example,  Inputs, Outputs, and Accessibility controls) if you don't want to.

#### Window management

The following aspects of window management are handled in a windowed hosting environment.

**Sizing, positioning, and visibility** 

CoreWebView2Controller takes a parent `HWND`. The Bounds property sizes and positions the WebView2 relative to the parent `HWND`. The visibility of WebView2 can be toggled using 'IsVisible`. 

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.Bounds](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.bounds?view=webview2-dotnet-1.0.1293.44#microsoft-web-webview2-core-corewebview2controller-bounds)
* [CoreWebView2Controller.IsVisible Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.isvisible?view=webview2-dotnet-1.0.1293.44)

##### [WinRT/C#](#tab/winrtcsharp)

* [CoreWebView2Controller.Bounds](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#bounds)
* [CoreWebView2Controller.IsVisible Property](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#isvisible)

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller::Bounds Property(get](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#get_bounds), [put)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#put_bounds)
* [ICoreWebView2Controller::IsVisible Property(get](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#get_isvisible), [put)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#put_isvisible)

---

**Zooming**

WebView2 ZoomFactor is used to scale just the web content. This is also update??? when the user zooms the content through Ctrl+Mouse Wheel. 

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.ZoomFactor Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.zoomfactor?view=webview2-dotnet-1.0.1293.44#microsoft-web-webview2-core-corewebview2controller-zoomfactor) 
* [CoreWebView2Controller.ZoomFactorChanged Event](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.zoomfactorchanged?view=webview2-dotnet-1.0.1293.44) 
* [CoreWebView2Controller.SetBoundsAndZoomFactor Method](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.setboundsandzoomfactor?view=webview2-dotnet-1.0.1293.)

##### [WinRT/C#](#tab/winrtcsharp)

* [CoreWebView2Controller.ZoomFactor Property](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#zoomfactor) 
* [CoreWebView2Controller.ZoomFactorChanged Event](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#zoomfactorchanged)
* [CoreWebView2Controller.SetBoundsAndZoomFactor Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#setboundsandzoomfactor)

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller::ZoomFactor Property (get](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#get_zoomfactor),[put)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#put_zoomfactor)
* [ICoreWebView2Controller::ZoomFactorChanged Event (add](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#add_zoomfactorchanged), [remove)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#remove_zoomfactorchanged)
* [ICoreWebView2Controller::SetBoundsAndZoomFactor Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#setboundsandzoomfactor)

---

**Rasterization scale**

The RasterizationScale API scales all WebView2 UI including context menus, tooltip, and popups. The app can set whether the WebView2 should detect monitor scale changes and automatically update the RasterizationScale. `BoundsMode` is used to configure whether the `Bounds` property is interpreted as raw pixels, or DIPs (which need to be scaled by `RasterizationScale`). 


##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.BoundsMode Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.boundsmode?view=webview2-dotnet-1.0.1293.44#microsoft-web-webview2-core-corewebview2controller-boundsmode)
  * [CoreWebView2BoundsMode Enum](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2boundsmode?view=webview2-dotnet-1.0.1293.44)
* [CoreWebView2Controller.RasterizationScale Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.rasterizationscale?view=webview2-dotnet-1.0.1293.44)
* [CoreWebView2Controller.RasterizationScaleChanged Event](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.rasterizationscalechanged?view=webview2-dotnet-1.0.1293.44)
* [CoreWebView2Controller.ShouldDetectMonitorScaleChanges Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.shoulddetectmonitorscalechanges?view=webview2-dotnet-1.0.1293.44)

##### [WinRT/C#](#tab/winrtcsharp)

* [CoreWebView2Controller.BoundsMode Property](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#boundsmode)
  * [CoreWebView2BoundsMode Enum](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2boundsmode?view=webview2-winrt-1.0.1293.44)
* [CoreWebView2Controller.RasterizationScale Property](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#rasterizationscale)
* [CoreWebView2Controller.RasterizationScaleChanged Event](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#rasterizationscalechanged)
* [CoreWebView2Controller.ShouldDetectMonitorScaleChanges Property](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#shoulddetectmonitorscalechanges)

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller::BoundsMode Property (get](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#get_bounds), [put)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#put_bounds)
  * [ICoreWebView2BoundsMode Enum]() 
* [ICoreWebView2Controller::RasterizationScale Property (get](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller3?view=webview2-1.0.1293.44#get_rasterizationscale), [put)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller3?view=webview2-1.0.1293.44#put_rasterizationscale)
* [ICoreWebView2Controller::RasterizationScaleChanged Event (add](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller3?view=webview2-1.0.1293.44#add_rasterizationscalechanged), [remove)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller3?view=webview2-1.0.1293.44#remove_rasterizationscalechanged)
* [ICoreWebView2Controller::ShouldDetectMonitorScaleChanges Property (get](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller3?view=webview2-1.0.1293.44#get_shoulddetectmonitorscalechanges), [put)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller3?view=webview2-1.0.1293.44#put_rasterizationscale)

---

**Focus and tabbing**

WebView2 raises events to let the application know when it gains or loses focus. For tabbing, there's an API to move focus into WebView2 and an event for WebView2 to request the application to take focus back. 

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.MoveFocus Method](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.movefocus?view=webview2-dotnet-1.0.1293.44)
  * [CoreWebView2MoveFocusReason Enum](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2movefocusreason?view=webview2-dotnet-1.0.1293.44) 
* [CoreWebView2Controller.MoveFocusRequested Event](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.movefocusrequested?view=webview2-dotnet-1.0.1293.44) 
  * [CoreWebView2MoveFocusRequestedEventArgs Class](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2movefocusrequestedeventargs?view=webview2-dotnet-1.0.1293.44) 
* [CoreWebView2Controller.GotFocus Event](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.gotfocus?view=webview2-dotnet-1.0.1293.44) 
* [CoreWebView2Controller.LostFocus Event](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.lostfocus?view=webview2-dotnet-1.0.1293.44) 

##### [WinRT/C#](#tab/winrtcsharp)

* [CoreWebView2Controller.MoveFocus Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#movefocus) 
  * [CoreWebView2MoveFocusReason Enum](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2movefocusreason?view=webview2-winrt-1.0.1293.44)
* [CoreWebView2Controller.MoveFocusRequested Event](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#movefocusrequested) 
  * [CoreWebView2MoveFocusRequestedEventArgs Class](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2movefocusrequestedeventargs?view=webview2-winrt-1.0.1293.44)
* [CoreWebView2Controller.GotFocus Event](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#gotfocus) 
* [CoreWebView2Controller.LostFocus Event](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#lostfocus) 

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller::MoveFocus Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#movefocus)
  * [ICoreWebView2MoveFocusReason Enum]() 
* [ICoreWebView2Controller::MoveFocusRequested Event- (add](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#add_movefocusrequested), [remove)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#remove_movefocusrequested) 
  * [ICoreWebView2MoveFocusRequestedEventArgs Class](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2movefocusrequestedeventargs?view=webview2-1.0.1293.44) 
* [ICoreWebView2Controller::GotFocus Event (add](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#add_gotfocus), [remove)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#remove_gotfocus) 
* [ICoreWebView2Controller::LostFocus Event (add](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#add_lostfocus), [remove)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#remove_lostfocus) 

---

**Parent window**

WebView2 can be re-parented to a different parent `HWND`. WebView2 also needs to be notified when the application’s position on the screen changes. 


##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.NotifyParentWindowPositionChanged Method](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.notifyparentwindowpositionchanged?view=webview2-dotnet-1.0.1293.44)
* [CoreWebView2Controller.ParentWindow Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.notifyparentwindowpositionchanged?view=webview2-dotnet-1.0.1293.44)

##### [WinRT/C#](#tab/winrtcsharp)

* [CoreWebView2Controller.NotifyParentWindowPositionChanged Method](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.notifyparentwindowpositionchanged?view=webview2-dotnet-1.0.1293.44) 
* [CoreWebView2Controller.ParentWindow Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.parentwindow?view=webview2-dotnet-1.0.1293.44) 

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller::NotifyParentWindowPositionChanged Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#notifyparentwindowpositionchanged) 
* [ICoreWebView2Controller::ParentWindow Property (get)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2controller?view=webview2-winrt-1.0.1293.44#parentwindow), <!--— no put -->

---

**Keyboard accelerators** 

When WebView2 has focus, it receives input directly from the user. An application may want to intercept and handle certain accelerator key combinations or disable the normal browser accelerator key behaviors. 

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Settings.AreBrowserAcceleratorKeysEnabled Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2settings.arebrowseracceleratorkeysenabled?view=webview2-dotnet-1.0.1293.44)
* [CoreWebView2Controller.AcceleratorKeyPressed Event](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.acceleratorkeypressed?view=webview2-dotnet-1.0.1293.44) 
  * [CoreWebView2AcceleratorKeyPressedEventArgs Class](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2acceleratorkeypressedeventargs?view=webview2-dotnet-1.0.1293.44) 
  * [CoreWebView2KeyEventKind Enum](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2keyeventkind?view=webview2-dotnet-1.0.1293.44) 
  * [CoreWebView2PhysicalKeyStatus Struct](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2physicalkeystatus?view=webview2-dotnet-1.0.1293.44)

##### [WinRT/C#](#tab/winrtcsharp)

* [CoreWebView2Settings.AreBrowserAcceleratorKeysEnabled Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2settings.arebrowseracceleratorkeysenabled?view=webview2-dotnet-1.0.1293.44) 
* [CoreWebView2Controller.AcceleratorKeyPressed Event](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.acceleratorkeypressed?view=webview2-dotnet-1.0.1293.44) 
  * [CoreWebView2AcceleratorKeyPressedEventArgs Class](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2acceleratorkeypressedeventargs?view=webview2-dotnet-1.0.1293.44) 
  * [CoreWebView2KeyEventKind Enum](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2keyeventkind?view=webview2-dotnet-1.0.1293.44)
  * [CoreWebView2PhysicalKeyStatus Struct](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2physicalkeystatus?view=webview2-dotnet-1.0.1293.44)

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Settings::AreBrowserAcceleratorKeysEnabled Property (get](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2settings3?view=webview2-1.0.1293.44#get_arebrowseracceleratorkeysenabled), [put)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2settings3?view=webview2-1.0.1293.44#put_arebrowseracceleratorkeysenabled)
* [ICoreWebView2Controller::AcceleratorKeyPressed Event (add](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#add_acceleratorkeypressed), [remove)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller?view=webview2-1.0.1293.44#remove_acceleratorkeypressed)
  * [ICoreWebView2AcceleratorKeyPressedEventArgs Class](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2acceleratorkeypressedeventargs?view=webview2-winrt-1.0.1293.44)
  * [ICoreWebView2KeyEventKind Enum](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2keyeventkind?view=webview2-winrt-1.0.1293.44) 
  * [ICoreWebView2PhysicalKeyStatus Struct](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2physicalkeystatus?view=webview2-winrt-1.0.1293.44)

---

**Default background color** 

WebView2 can specify a default background color. This can be any opaque color or transparent color. This color will be used if the html page doesn't set its own background color. 

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2Controller.DefaultBackgroundColor Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.defaultbackgroundcolor?view=webview2-dotnet-1.0.1293.44)
  * [CoreWebView2Color Struct](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2color?view=webview2-dotnet-1.0.1293.44)

##### [WinRT/C#](#tab/winrtcsharp)

* [CoreWebView2Controller.DefaultBackgroundColor Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2controller.defaultbackgroundcolor?view=webview2-dotnet-1.0.1293.44#microsoft-web-webview2-core-corewebview2controller-defaultbackgroundcolor)
  * [CoreWebView2Color Struct](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2color?view=webview2-dotnet-1.0.1293.44)

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2Controller::DefaultBackgroundColor Property (get](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller2?view=webview2-1.0.1293.44#get_defaultbackgroundcolor), [put)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2controller2?view=webview2-1.0.1293.44#put_defaultbackgroundcolor)
  * [ICoreWebView2Color Struct]()

---

<!-- ====================================================================== -->
## Visual hosting

Visual Hosting can be described as a single whiteboard in which the information is written directly to the board. By doing so, content is embedded to a location on the whiteboard. This location must handle how it will scale and behave in an application when there's interaction with the application. In addition to the window management described  for windowed hosting, visual hosting will need the application to manage the composition-based rendering, when it receives any and all user interaction(s).  

The following facts apply to a WebView2 application that uses visual hosting: 

* The controls and/or configurations to DPI scaling must be set by the application. 
* Inputs are routed to the application's `HWND` and must be configured to send the spatial input (for example, mouse, touch, and pen) based on positions, NOT what currently has focus like a keyboard.  

The following APIs can be used when configuring WebView2 in a visual hosting environment.

#### Composition based rendering 

For composition based WebView2 rendering, use the `CoreWebView2Environment` to create a `CoreWebView2CompositionController`. The `CoreWebView2CompositionController` implements all the APIs as `CoreWebView2Controller`, but includes additional APIs specific to composition-based rendering. 

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2CompositionController Class](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller?view=webview2-dotnet-1.0.1293.44)
* [CoreWebView2Environment.CreateCoreWebView2CompositionControllerAsync Method](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2compositioncontrollerasync?view=webview2-dotnet-1.0.1293.44)

##### [WinRT/C#](#tab/winrtcsharp)

* [CoreWebView2CompositionController Class](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller?view=webview2-dotnet-1.0.1293.44)
* [CoreWebView2Environment.CreateCoreWebView2CompositionControllerAsync Method](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2compositioncontrollerasync?view=webview2-dotnet-1.0.1293.44)

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2CompositionController Class](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller?view=webview2-1.0.1293.44)
* [ICoreWebView2Environment::CreateCoreWebView2CompositionController Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2environment3?view=webview2-1.0.1293.44#createcorewebview2compositioncontroller)

---

#### Output

WebView2 can connect its composition tree to `IDCompositionVisual`, `IDCompositionTarget`, or `Windows::UI::Composition::ContainerVisual`. 

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2CompositionController.RootVisualTarget Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.rootvisualtarget?view=webview2-dotnet-1.0.1293.44)

##### [WinRT/C#](#tab/winrtcsharp)

* [CoreWebView2CompositionController.RootVisualTarget Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.rootvisualtarget?view=webview2-dotnet-1.0.1293.44)

##### [Win32/C++](#tab/win32cpp) 

* [ICoreWebView2CompositionController::RootVisualTarget Property (get](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller?view=webview2-1.0.1293.44#get_rootvisualtarget), [put)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller?view=webview2-1.0.1293.44#put_rootvisualtarget)

---

#### Input

Spatial input such as mouse, touch, or pen is received by the application and must be sent to WebView2. WebView2 notifies the application when the cursor should be updated based on the input device's position. 

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2CompositionController.Cursor Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.cursor?view=webview2-dotnet-1.0.1293.44)
* [CoreWebView2CompositionController.CursorChanged Event](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.cursorchanged?view=webview2-dotnet-1.0.1293.44) 
* [CoreWebView2CompositionController.SystemCursorId Property](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.systemcursorid?view=webview2-dotnet-1.0.1293.44) 
* [CoreWebView2CompositionController.SendMouseInput Method](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.sendmouseinput?view=webview2-dotnet-1.0.1293.44)
  * [CoreWebView2MouseEventKind Enum](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2mouseeventkind) 
  * [CoreWebView2MouseEventVirtualKeys Enum](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2mouseeventvirtualkeys?view=webview2-dotnet-1.0.1293.44)
* [CoreWebView2CompositionController.SendPointerInput Method](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2compositioncontroller.sendpointerinput?view=webview2-dotnet-1.0.1293.44) 
  * [CoreWebView2PointerEventKind Enum](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2pointereventkind?view=webview2-dotnet-1.0.1293.44) 
* [CoreWebView2Environment.CreateCoreWebView2PointerInfo Method](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2environment.createcorewebview2pointerinfo?view=webview2-dotnet-1.0.1293.44) 
  * [CoreWebView2PointerInfo Class](https://docs.microsoft.com/en-us/dotnet/api/microsoft.web.webview2.core.corewebview2pointerinfo?view=webview2-dotnet-1.0.1293.44) 

##### [WinRT/C#](#tab/winrtcsharp)

* [CoreWebView2CompositionController.Cursor Property](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2compositioncontroller?view=webview2-winrt-1.0.1293.44#cursor)
* [CoreWebView2CompositionController.CursorChanged Event](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2compositioncontroller?view=webview2-winrt-1.0.1293.44#cursorchanged) 
* [CoreWebView2CompositionController.SystemCursorId Property]()
* [CoreWebView2CompositionController.SendMouseInput Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2compositioncontroller?view=webview2-winrt-1.0.1293.44#sendmouseinput)
  * [CoreWebView2MouseEventKind Enum](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2mouseeventkind) 
  * [CoreWebView2MouseEventVirtualKeys Enum](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2mouseeventvirtualkeys?view=webview2-winrt-1.0.1293.44)
* [CoreWebView2CompositionController.SendPointerInput Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2compositioncontroller?view=webview2-winrt-1.0.1293.44#sendpointerinput) 
  * [CoreWebView2PointerEventKind Enum](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2mouseeventkind?view=webview2-winrt-1.0.1293.44) 
* [CoreWebView2Environment.CreateCoreWebView2PointerInfo Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2environment?view=webview2-winrt-1.0.1293.44#createcorewebview2pointerinfo) 
  * [CoreWebView2PointerInfo Class](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/winrt/microsoft_web_webview2_core/corewebview2pointerinfo?view=webview2-winrt-1.0.1293.44) 

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2CompositionControllerC::Cursor Property (get)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller?view=webview2-1.0.1293.44#get_cursor),<!---no put------> 
* [ICoreWebView2CompositionController::CursorChanged Event (add ](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller?view=webview2-1.0.1293.44#add_cursorchanged), [remove)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller?view=webview2-1.0.1293.44#remove_cursorchanged)
* [ICoreWebView2CompositionController::SystemCursorId Property (get)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller?view=webview2-1.0.1293.44#get_systemcursorid),<!---no put------> 
* [ICoreWebView2CompositionController::SendMouseInput Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller?view=webview2-1.0.1293.44#sendmouseinput)
  * [ICoreWebView2MouseEventKind Enum]
  * [ICoreWebView2MouseEventVirtualKeys Enum]
* [ICoreWebView2CompositionController::SendPointerInput Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller?view=webview2-1.0.1293.44#sendpointerinput) 
  * [ICoreWebView2PointerEventKind Enum] 
* [ICoreWebView2Environment::CreateCoreWebView2PointerInfo Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2environment3?view=webview2-1.0.1293.44#createcorewebview2pointerinfo) 
  * [ICoreWebView2PointerInfo Class](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2pointerinfo?view=webview2-1.0.1293.44) 

---

#### Accessibility

By default, WebView2 will show up in the accessibility tree as a child of the parent `HWND`. WebView2 provides an API to better position the WebView2 content relative to other elements in the application.

##### [.NET/C#](#tab/dotnetcsharp)

* [CoreWebView2CompositionController.AutomationProvider Property]() 
* [CoreWebView2Environment.GetAutomationProviderForWindow Method]() 

##### [WinRT/C#](#tab/winrtcsharp)

* [CoreWebView2CompositionController.AutomationProvider Property]() 
* [CoreWebView2Environment.GetAutomationProviderForWindow Method]()

##### [Win32/C++](#tab/win32cpp)

* [ICoreWebView2CompositionController::AutomationProvider Property (get)](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2compositioncontroller2?view=webview2-1.0.1293.44#get_automationprovider), <!--- no put -->
* [ICoreWebView2Environment::GetAutomationProviderForWindow Method](https://docs.microsoft.com/en-us/microsoft-edge/webview2/reference/win32/icorewebview2environment4?view=webview2-1.0.1293.44#getautomationproviderforwindow)

--- 

<!-- ====================================================================== -->
## Scenarios

There are instances where a developer might want to focus on displaying web content as quickly and easily as possible in their application. In this case, windowed hosting allows for a solution that quickly displays web content without having to include features for inputs, outputs, and accessibility.

Alternatively, visual based hosting allows for (and requires) more granular controls. The application needs specific handling of  window management and rendering API’s. 

Both hosting approaches are similar in functionality but suit different needs depending on the application requirements. 

<!-- ====================================================================== -->
## Conclusion

Windowed and visual hosting achieve the same output, but in different ways. The choice between the two options isn't a matter of convenience, but is based on what works best for the application. 

<!-- ====================================================================== -->
## See also

*  [link text]<!--(relative path)-->