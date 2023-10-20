# 项目结构

* App.config   应用配置
* Form1.cs    源码文件（窗口）
  * Form1.Designer.cs	源码文件（界面设计）
  * Form1.resx       资源文件
* Program.cs      源码文件（Main方法）



Form1的两种打开方式：

* 双击Form1.cs，打开的是界面设计器
* 右键，选择查看代码，打开的是源码编辑器



From1.cs和Form1.Designer.cs两个文件中都定义了Form1类，使用了类的拆分

* From1.cs主要是业务的逻辑
* Form1.Designer.cs为visual studio根据界面设计器自动生成文件



## 手工创建窗口

右键添加类，然后继承于Form

```c#
using System.Windows.Forms;
class MyForm:Form
{
    public MyForm()
        {
            this.Text = "我的新建窗口";
        }
}
```



为了显式窗口，修改Program.cs，创建一个MyForm并运行

```c#
internal static class Program
    {
        [STAThread]
        static void Main()
        {
            MyForm form = new MyForm();
            Application.Run(form);
        }
    }
```



# 添加控件

将控件拖入后，在Form1.Designer.cs的InitializeComponent方法中自动生成控件代码，如显式位置、大小、文本等



Form1.Designer.cs为设计器自动生成，一般不要手工修改



代码调用关系：

在Form1.cs中有InitializeComponent方法，在Form1.Designer.cs中定义



## 手动添加控件

* 打开Form1.cs，创建button控件，并将其添加到窗口中

```c#
public partial class Form1 : Form
    {
        private Button button2 = new Button();
        public Form1()
        {
            this.Controls.Add(button2);
            button2.Text = "测试按钮";
            button2.Location = new Point(40, 40);
            button2.Size = new Size(100, 100);
        }
    }
```



窗口坐标左上角为（0，0），从左往右x坐标增大，从上往下y坐标增大

控件左上角位置使用Point来指定，宽度和高度使用Size类型来指定



# 事件处理

给按钮添加事件处理：

* 右键点击按钮，属性
* 点击闪电，切换到事件显式，Click事件
* 输入回调方法的名字，回车

完成后会自动生成一个用于事件处理的回调方法



双击控件按钮时，会添加默认的事件处理，如Click事件



事件处理回调方式是定义在Form1.cs中的



## 手工添加事件处理

* 在设计器里，给按钮起一个名字，如button1
* 在Form1.cs里，添加一个回调方法

```c#
public void Button1_function(object sender, EventArgs e)
{
    MessageBox.Show("手动添加事件");
}
```



* Form1.cs的Form1中添加事件处理

```c#
 public Form1()
 {
     button1.Click += new EventHandler(this.Button1_function);
 }
```



EventHandler是一个委托类型，该委托类型为：

```c#
delegate void EventHandler(object sender, EventArgs e)
```

* sender：事件发送者，即点击的控件
* e：事件的额外参数，比如鼠标点击的位置



# 控件的布局

当窗口中有多个控件时，如何决定每个控件的位置和大小



布局方式：

* 可视化布局：在设计器里使用鼠标进行拖放
* 手工布局：使用代码计算每个控件的位置
* 使用布局器：使用布局器自动布局



当窗口改变大小时，布局并不能够自动适应

因此这种布局只适用于窗口大小固定不变的情况



## 手工布局

使用代码计算每个控件的位置

需要重写OnLayout方法，当窗口大小改变时，会自动调用（系统框架调用）该方法重新布局

```c#
protected override void OnLayout(LayoutEventArgs levent)
{
    //调用父类的OnLayout，不是必须
    base.OnLayout(levent);

    //获取窗体大小，不包含标题栏
    int windowsWidth = this.ClientSize.Width;
    int windowsHeight = this.ClientSize.Height;
}
```



窗体大小：

* Size：窗口大小，含边框和标题栏
* ClientSize：仅窗口客户区的大小



注意：

* 将TextBox的AutoSize属性设为false，否则TextBox无法调整大小



## Anchor

锚定，将控件固定于某个位置

在属性中点击“Anchor”属性后，有上下左右四个方向，若希望固定于右上角，则选中右边和上边

当窗口大小改变时，该控件锚定于窗口右上角，即上边距和右边距保持不变



锚定上边缘，水平拉伸		则Anchor = Top | Left | Right

锚定上边缘，水平居中		则先居中，然后Anchor = Top

整体居中								则先水平居中和垂直居中，Anchor = None



## Dock

停靠，将控件停靠在一侧或中央



可选值：

* 上		Top
* 下		Bottom 
* 左        Left
* 右        Right
* 中        Full
* 无        None



 实际项目中，界面布局可能有多个层次

例如，一个Panel内部可能还会添加多个控件



当设置了Dock属性时，Anchor属性会失效



# 布局器

LayoutEngine：负责容器中子控件布局

默认地，一个Form或Panel都自带了一个布局器

窗口改变大小时，由窗口地布局器来负责调整布局



自定义布局器，创建一个SimpleLayoutPanel.cs

```c#
using System.Windows.Forms;
using System.Windows.Forms.Layout;

namespace FormApp
{
    //自定义面板
    class SimpleLayoutPanel : Panel
    {
        //布局器
        private LayoutEngine layoutEngine = new SimpleLayoutPanel();
        
        private override LayoutEngine LayoutEngine
        {
            get
            {
                return layoutEngine;
            }
        }
    }
    
    //自定义布局器
    class SimpleLayoutEngine : LayoutEngine
    {
        public override bool Layout(object container, 
                                   layoutEventArgs layoutEventArgs)
        {
            
        }
    }
}





```



使用步骤：

* 工具->选项->Windows窗体设计器->常规->自动填充工具箱，设为True
* 添加自定义类
* 重新生成解决方案
* 重新打开Form1，在工具箱中可以找到自定义控件



## FlowLayoutPanel

流式布局

子控件依次排列，一行排满后换行继续排



当多个控件叠加在一起时，使用右键进行选择



## TableLayoutPanel











































