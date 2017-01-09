## XAML
### Property Syntax
#### Attribute Syntax
```xml
<Button Height="50" Width="200" Content="Watch Now" />
```
#### Property Syntax
```xml
<Button Width="120" Height="50">
    <Button.Background>
        <SolidColorBrush Color="Black" />
    </Button.Background>
</Button>
```

## GUI-Entwurf
### Alignment
<img width=500 src="../img/mge/wpf_alignment.png" />

- Es gibt `HorizontalAligment` und `VerticalAlignment`

<div style="page-break-after: always" ></div>

### Dialog-Fenster
```csharp
private void OpenDialog_OnClick(object sender, RoutedEventArgs e) {
    var win = new DialogWindow();
    if (win.ShowDialog() != true)
    {
        Debug.WriteLine("Cancelled :-(");
        return;
    }
    Debug.WriteLine("OK :-)");
}
```

### Teststack.White Example
```csharp
[TestClass]
public class MyFirstUITest {
    public string BaseDir => 
        Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);
    public string SutPath => Path.Combine(BaseDir,  
        $"{nameof(MenusAndCommands)}.exe");
    [TestMethod]
    public void MyFirstUITestMethod() {
        var app = Application.Launch(SutPath);
        var window = app.GetWindow("Hauptfenster",
            InitializeOption.NoCache);
        var button = window.Get<Button>("SaveButton");
        Assert.AreEqual("Speichern", button.Text);
        button.Click();
        Assert.AreEqual("Gespeichert!", button.Text);
        app.Close();
    }
}
```

## GUI-Design
### Resource Dictionaries
- Externe Resource Dictionaries erlauben Kaskadierung

```xml
<ResourceDictionary xmlns=... xmlns:x=...>
    <!- include your base dictionaries, here -->
    <ResourceDictionary.MergedDictionaries>
        <ResourceDictionary Source="Colors.xaml"/>
    </ResourceDictionary.MergedDictionaries>
    <!– now, access the externally defined resources -->
    <SolidColorBrush x:Key="ButtonBgBrush" 
        Color="{StaticResource ThemeColor1}" />
</ResourceDictionary>
```
<div style="page-break-after: always" ></div>
### Externe Ressourcen
```xml
<Image Source="/BasePicLib;component/media/pix/open.png" />
```
Kurzform für:
```xml
<Image Source="pack://application:,,,/BasePicLib;component/media/pix/open.png" />
```
- `BasePicLib` ist der name der externen Assembly
- `component` ist immer fix
- `media/pix/open.png` ist der Pfad in der externen Assembly

### Styles
- **Explizite Styles** haben einen Key
```xml
<Style x:Key="MyButtonStyle">
    <Setter Property="Button.Foreground" Value="#2672EC" />
    <Setter Property="Button.Padding" Value="10 2 10 2" />
</Style>
```
- Nutzung:
```csharp
<Button Style="{StaticResource MyButtonStyle}" Content="Save" />
```
- **Typenspezifische Styles** gelten für einen bestimmten Typ von Control
```xml
<Style x:Key="MyButtonStyle3" TargetType="Button">
    <Setter Property="Background" Value="Transparent" />
    <Setter Property="Margin" Value="2" />
</Style>
```
- Wird der `x:Key` weggelassen, gilt der Style implizit für alle Controls dieses Typen!
#### Styles "vererben"
```xml
<Style x:Key="DangerButtonStyle"
    TargetType="Button"
    BasedOn="{StaticResource MyButtonStyle}">
  <Setter Property="Background" Value="Red" />
</Style>
```
### Control Templates
- Jedes Control hat ein `Template`-Property, das bestimmt, wie es aufgebaut ist
- Eigenes Template über Style definieren:
```xml
<Setter Property="Template">
    <Setter.Value>
        <ControlTemplate TargetType="Button" >
            <Border BorderBrush="{TemplateBinding BorderBrush}"
                BorderThickness="{TemplateBinding BorderThickness}">
                <StackPanel Orientation="Horizontal" 
                    Background="{TemplateBinding Background}">
                    <Grid Margin="4" >
                        <Ellipse Fill="{TemplateBinding Foreground}" 
                            Height="16" Width="16" />
                        <Label Foreground="{TemplateBinding Background}"
                            HorizontalContentAlignment="Center">!</Label>
                    </Grid>
                    <ContentPresenter Content="{TemplateBinding Content}" 
                        Margin="0 0 10 0" VerticalAlignment="Center" />
                </StackPanel>
            </Border>
        </ControlTemplate>
    </Setter.Value>
</Setter>
```
- `TemplateBinding` bindet den Wert an eine Eigenschaft im Control, z.B. `Foreground`
- `ContentPresenter` stellt den Inhalt des Controls dar

### Trigger
- Styles anhand unterschiedlicher Zustände anpassen
```xml
<Style.Triggers>
    <Trigger Property="IsMouseOver" Value="True">
        <Setter Property="Foreground" Value="White" />
    </Trigger>
</Style.Triggers>
```
- Problem: Bei einigen Elementen (z.B. Button) werden diese Trigger vom VisualStateManager übersteuert
- Lösung: Eigenes Control Template für diese Controls verwenden

### 2D-Transformationen
- `LayoutTransform` wird vor der Layout-Phase berechnet, `RenderTransform` nachher
```xml
<ScaleTransform ScaleX="1.5" ScaleY="1.5" />
<RotateTransform Angle="45" CenterX="30" CenterY="12" />
<SkewTransform AngleX="-35" AngleY="9" />
<TranslateTransform X="40" Y="-10" />
```

<div style="page-break-after: always" ></div>

## Data Binding
### StringFormat
```xml
<TextBox Text="{Binding ScaleX, StringFormat={}{0:0.0}}" />
```
z.B. um für Multibinding einen String aus mehreren Properties zusammen zu setzen:
```xml
<MultiBinding StringFormat="{}{0} -- Now only {1:C}!">
    <Binding Path="Description"/>
    <Binding Path="Price"/>
</MultiBinding>
```
### Binding auf ElementName
- Binding auf ein XAML-Element
```xml
<TextBlock Name="MyText" Margin="10" … />
<TextBlock Name="OtherText"
    Margin="{Binding ElementName=MyText, Path=Margin}" … />
```

### ItemsControl
- Ist die Basisklasse von `ListBox`, `ComboBox` und `DataGrid`
- Mit `ItemsSource` die Quelle per Binding angeben
- `ItemTemplate`: Ein `DataTemplate` angeben, das ein Layout enthält, wie ein einzelner Listeneintrag aussehen soll

### CollectionsViewSource
- Ermöglicht Gruppieren, Sortieren, Filtern
- `SortDescriptions`: Sortierungsmerkmale
- `Source`: Quelle per Binding (meist eine `ObservableCollection<T>`)

## Event Handling
- Merke: Es ist nicht immer klar, wo das Event herkommt
- Daher in den EventArgs die Quelle prüfen, von wem das Event ausgelöst wurde!

<div style="page-break-after: always" ></div>

## Internationalization (i18n)
- Mit .NET Embedded Resources
    - `System.Globalization.CultureInfo` Objekte enthalten infos über Sprache, Formate, etc.
    - Im Projekt ein `Resources.resx` ablegen
    - Abruf mit `Properties.Resources.STRING_ID`
        - oder `Properties.Resources.ResourceManager.GetString("STRING_ID")`
        - `Resources` = Name des Resources.resx File
    - Neue Sprache: z.B. `Resources.en-US.rex` erstellen
- WPF-Spezifisch
    - In `csproj` die Default `UICulture` festlegen
    - In `AssemblyInfo.cs` Zeile auskommentieren und auf Default-Sprache setzen
    - Zugriff im XML:
    ```xml
    <Window xmlns:resx="clr-namesapce:I18n.Properties" ...>
        <TextBlock Test="{x: Static resx:Resouces.STRING_ID}" />
    </Window>
    ```
## MVVM
<img width=500 src="../img/mge/mvvm.png" />

<img width=500 src="../img/mge/wpf_architecture.png" />

<div style="page-break-after: always" ></div>

### Commands
- Direkt implementieren:

```csharp
public class SomeCommand : ICommand {
    public bool CanExecute(object parameter) {
        // ... irgendwie true oder false zurückgeben
    }
    public void Execute(object parameter) {
        // ... irgendwas ausführen
    }
    public event EventHandler CanExecuteChanged;
}
```
- Das ViewModel hält eine Instanz (Property) auf das Command, das von den Controls gebindet wird
- Bessere Variante: Generischen `RelayCommand<T>` implementieren, der zwei delegates `Predicate<T>` (für `CanExecute()`) und `Action<T>` (für `Execute()`) entgegen nimmt und diese ausführt
```csharp
public GadgetVm()
{
    SaveCommand = new RelayCommand(() => this.Save(), () => this.CanSave));
}
```
- Binding im XML (hier mit Parameter)
```xml
<Button Content="Open"
    Command="{Binding OpenGadgetViewCommand}"
    CommandParameter="{Binding SelectedGadget}" />
```
### Projektlayout
- Kleine Projekte: 1 Projekt, für jede MVVM-Rolle 1 Ordner + 1 Test-Projekt
- Mittlere Projekte: 1 Projket pro Layer (Model, Business-Logik, UI) + 1 Test-Projekt
- Grosse Projekte: Projekte der Schichten in weitere (testbare) Unterprojekte aufteilen
