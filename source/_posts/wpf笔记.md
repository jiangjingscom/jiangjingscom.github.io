---
title: WPF笔记
date: 2017-03-02 18:41:51
categories: Others
tags: WPF 
---

###  改写窗体：

**1.WPF中使用默认窗口框架的外观，可以更改属性：([参考](http://www.cnblogs.com/libaoheng/archive/2011/11/18/2253751.html))**
*Icon*: 指定窗口的图标；
*Title*: 指定窗口的标题；
*WindowStyle*: 指定窗口样式，有4个取值：
- None，无边框；（当ResizeMode属性为NoResize时，仅剩下窗口核心。）
- SingleBorderWindow，单边框【默认】；
- ThreeDBorderWindow，3D边框；
- ToolBorderWindow，工具箱窗口；
- ResizeMode 是指定大小调节样式，有4个取值：
- NoResize，不可调节，同时没有最大最小按钮；
- CanMinimize，不可调节。但可以最小化；（此时最大化按钮不可用）
- CanResize，可调节【默认】；
- CanResizeWithGrid，可根据网格调节；（窗口右下脚显示可调节网格）
-  WindowStartLocation 指定窗口初始位置，有3个取值：
-  Manual，手工指定位置，表示可以通过设置其Top、Left属性值来决定窗口的初始位置；
-  CenterScreen，屏幕中央；
-  CenterOwner，父窗体中央；
 >另外：
 - MaxWidth、MinWidth、MaxHeight、MinHeight ：表示窗口最大宽度、最小宽度、最大高度、最小高度。可以通过得到和更改这些属性值，来获取和改变窗口的大小和长宽范围。
 - TitlebarHeight="45"          //修改窗体titlebar高度；
 - AllowsTransparency：获取或设置一个值，该值指示窗口的工作区是否支持透明；
 
<!--more-->

 
**2.设置窗体无边框：**
     设置 WindowStyle="None"、  AllowsTransparency="True" 即可。
     *如下*：
``` xml
<Window
    x:Class="WpfApplication1.MainWindow" 
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" 
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
    Title="MainWindow" Height="350" Width="525"   
    WindowStyle="None" 
    AllowsTransparency="True"> 
    <Grid> 
    </Grid> 
</Window>  
```

**3.窗口拖放：**
``` cs
private void Window_MouseLeftButtonDown(object sender, MouseButtonEventArgs e){
    this.DragMove();
}
```

*调用：*

``` cs
this.MouseLeftButtonDown += delegate { DragMove(); };
```

或：
``` cs
MouseDown="Window_MouseDown"
private void Window_MouseDown(object sender, MouseButtonEventArgs e){
	if (e.LeftButton == MouseButtonState.Pressed){
        DragMove();
	}
}
```
     
**4.自定义窗体最大化、最小化、关闭**
如下： 
``` cs
private void btn_min_Click(object sender, RoutedEventArgs e){
    this.WindowState = WindowState.Minimized;     
}
private void btn_max_Click(object sender, RoutedEventArgs e){
    if(this.WindowState == WindowState.Maximized){
    	this.WindowState = WindowState.Normal; //还原 
    }else{
       this.WindowState = WindowState.Maximized;
    }
}
private void btn_close_Click(object sender, RoutedEventArgs e){
    this.Close();
}
```
	
**5.窗口阴影：**(WPF4.5)
``` xml
<Window x:Class="WPFTest.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    Title="MainWindow"
    Width="525"
    Height="350">
    <WindowChrome.WindowChrome>
        <WindowChrome CaptionHeight="30"
                      CornerRadius="0"
                      GlassFrameThickness="1"
                      NonClientFrameEdges="None"
                      ResizeBorderThickness="5"
                      UseAeroCaptionButtons="False" />
    	</WindowChrome>
    <Grid> 
    </Grid>
</Window>
```


### tooltip
给某个空间增加tooltip显示详细信息:

``` cs
private void Tip_MouseMove(object sender, MouseEventArgs e){
    ListViewItem listitem = (sender as Image).TryFindParent<ListViewItem>();
    FDObject item = (FDObject)listitem.DataContext;           
    if (item != null){
        (sender as Image).ToolTip = item.path;
    }
}
```
	

直接用control.Tooltip=text;   在鼠标移入事件中确定

### System.Drawing.Image对象
**System.Drawing.Image 和 System.Windows.Media.ImageSource 之间转换**
**例：**
``` cs
MemoryStream ms = new MemoryStream ();
var bitmap = new BitmapImage();
bitmap.BeginInit();
userImage.Save (ms,System.Drawing.Imaging. ImageFormat.Bmp);
ms.Seek(0, SeekOrigin.Begin);
bitmap.StreamSource = ms;
bitmap.EndInit();
emplImage.Source = bitmap;
return emplImage;
```
	
	
### 旋转动画：
**例：**
``` cs
 private void Button_Click(object sender, RoutedEventArgs e){
    RotateTransform a = new RotateTransform();
    Refresh.RenderTransform = a;
    Refresh.RenderTransformOrigin = new Point(0.5, 0.5);
    DoubleAnimation myDouble = new DoubleAnimation(0, 360, new Duration(TimeSpan.FromSeconds(1)));
    Storyboard story = new Storyboard();
    myDouble.RepeatBehavior = RepeatBehavior.Forever;
    story.Children.Add(myDouble);
    Storyboard.SetTarget(myDouble, Refresh);
    Storyboard.SetTargetProperty(myDouble, new PropertyPath("RenderTransform.Angle"));
    story.Begin();
}
```
``` xml
<Grid>
    <TextBlock x:Name="Refresh"  VerticalAlignment="Center" HorizontalAlignment="Center"    Text="&#xe712;"   />
    <Button VerticalAlignment="Bottom" HorizontalAlignment="Center" Width="100" Height="50" Click="Button_Click"/>
</Grid>
    
```