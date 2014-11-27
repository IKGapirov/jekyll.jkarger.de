---
layout:     post
title:      "MahApps.Metro Themes"
date:       2014-11-27 17:47:14 +0100
comments:   true
categories: [Modern UI,MahApps.Metro,WPF,C#]
---

[MahApps.Metro][] is a toolkit for creating metro-styled, modern UI WPF applications. A guide can be found in my [blog post](http://jkarger.de/archive/2014/11/18/modern-ui-with-mahapps-metro) or on [GitHub](https://github.com/MahApps/MahApps.Metro) or the official web page [http://mahapps.com](http://mahapps.com).

You can change the accent or theme of a `MahApps.Metro` application in different ways.

All of `MahApp.Metro's` accents and themes are contained within separate resource dictionaries (**Make sure that all file names are spelled correct. They are Case Sensitive!**).

You can choose between this available accents

> "Red", "Green", "Blue", "Purple", "Orange", "Lime", "Emerald", "Teal", "Cyan", "Cobalt", "Indigo", "Violet", "Pink", "Magenta", "Crimson", "Amber", "Yellow", "Brown", "Olive", "Steel", "Mauve", "Taupe", "Sienna"

and themes

> "BaseLight", "BaseDark"

### via App.xaml

The fastest way is to specify the accent and theme via the `App.xaml`.

```xml
<Application x:Class="MahAppsMetroThemesSample.App"
             xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             StartupUri="MainWindow.xaml">
    <Application.Resources>
        <ResourceDictionary>
            <ResourceDictionary.MergedDictionaries>
                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Controls.xaml" />
                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Fonts.xaml" />
                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Colors.xaml" />
                
                <!-- accent resource -->
                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Accents/Cobalt.xaml" />
                <!-- theme resource -->
                <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Accents/BaseLight.xaml" />
            </ResourceDictionary.MergedDictionaries>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

![](screenshots{{ site.url }}/images/mahapps_themes_01.png)

### via ThemeManager

`MahApps.Metro` has a `ThemeManager` to change accent and theme at code behind. You can easily changing accent or theme with a two liner.

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // get the theme from the current application
        var theme = ThemeManager.DetectAppStyle(Application.Current);
        
        // now set the Green accent and dark theme
        ThemeManager.ChangeAppStyle(Application.Current,
                                    ThemeManager.GetAccent("Green"),
                                    ThemeManager.GetAppTheme("BaseDark"));

        base.OnStartup(e);
    }
}
```

### A Window different to Application

With `MahApps.Metro` you can have a different accent and theme for a `MetroWindow`. The main window or any other `MetroWindow` ceep their specified accent and theme through the `App.xaml`.

```xml
<Controls:MetroWindow.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <!-- this window should be blue -->
            <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Accents/Blue.xaml" />
            <!-- and should use the light theme -->
            <ResourceDictionary Source="pack://application:,,,/MahApps.Metro;component/Styles/Accents/BaseLight.xaml" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Controls:MetroWindow.Resources>
```

Now you can change the accent and theme also by the `ThemeManager`.

```csharp
public partial class AccentStyleWindow : MetroWindow
{
    public void ChangeAppStyle()
    {
        // get the theme from the window
        var theme = ThemeManager.DetectAppStyle(this);
        
        // now set the Red accent and dark theme
        ThemeManager.ChangeAppStyle(this,
                                    ThemeManager.GetAccent("Red"),
                                    ThemeManager.GetAppTheme("BaseDark"));
    }
}
```

### Custom Accents and Themes

Another nice feature of `MahApps.Metro` `ThemeManager` is to use custom created accents and themes or use a dynamically created accent and theme.

```xml
<ResourceDictionary xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">

    <!--ACCENT COLORS-->
    <Color x:Key="HighlightColor">#FF9F0055</Color>
    
    <!--80%-->
    <Color x:Key="AccentColor">#CCD80073</Color>
    <!--60%-->
    <Color x:Key="AccentColor2">#99D80073</Color>
    <!--40%-->
    <Color x:Key="AccentColor3">#66D80073</Color>
    <!--20%-->
    <Color x:Key="AccentColor4">#33D80073</Color>

    <!-- re-set brushes too -->
    <SolidColorBrush x:Key="HighlightBrush" Color="{StaticResource HighlightColor}" />
    <SolidColorBrush x:Key="AccentColorBrush" Color="{StaticResource AccentColor}"/>
    <SolidColorBrush x:Key="AccentColorBrush2" Color="{StaticResource AccentColor2}"/>
    <SolidColorBrush x:Key="AccentColorBrush3" Color="{StaticResource AccentColor3}"/>
    <SolidColorBrush x:Key="AccentColorBrush4" Color="{StaticResource AccentColor4}"/>

    <SolidColorBrush x:Key="WindowTitleColorBrush" Color="{StaticResource AccentColor}" />

    <SolidColorBrush x:Key="AccentSelectedColorBrush" Color="White" />

    <LinearGradientBrush x:Key="ProgressBrush" EndPoint="0.001,0.5" StartPoint="1.002,0.5">
        <GradientStop Color="{StaticResource HighlightColor}" Offset="0" />
        <GradientStop Color="{StaticResource AccentColor3}" Offset="1" />
    </LinearGradientBrush>

    <SolidColorBrush x:Key="CheckmarkFill" Color="{StaticResource AccentColor}" />
    <SolidColorBrush x:Key="RightArrowFill" Color="{StaticResource AccentColor}" />

    <Color x:Key="IdealForegroundColor">White</Color>
    <SolidColorBrush x:Key="IdealForegroundColorBrush" Color="{StaticResource IdealForegroundColor}"/>

</ResourceDictionary>
```

To use this custom accent you must add it to the `ThemeManager` before you can use it.

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        // add custom accent and theme resource dictionaries
        ThemeManager.AddAccent("CustomAccent1", new Uri("pack://application:,,,/MahAppsMetroThemesSample;component/CustomAccents/CustomAccent1.xaml"));
        
        // get the theme from the current application
        var theme = ThemeManager.DetectAppStyle(Application.Current);
        
        // now use the custom accent
        ThemeManager.ChangeAppStyle(Application.Current,
                                    ThemeManager.GetAccent("CustomAccent1"),
                                    theme.Item1);

        base.OnStartup(e);
    }
}
```

You can also create an accent resource dictionary dynamically by using a specific color.

A complete example can be found on [GitHub](https://github.com/punker76/code-samples).

  [MahApps.Metro]: https://github.com/MahApps/MahApps.Metro
