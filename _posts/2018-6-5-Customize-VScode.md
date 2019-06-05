# Customizing VS Code for Python (Windows version)

I started using VScode very recently and have found it to be very easy to use and productive for what I do. 
I use it to write my python code but have also tried it with R and other language and have liked so far. 
So, I thought i'd write up the steps to customize the settings for future reference.
<p align = "left">
    <img alt= "VScode logo" src ="https://upload.wikimedia.org/wikipedia/commons/thumb/2/2d/Visual_Studio_Code_1.18_icon.svg/512px-Visual_Studio_Code_1.18_icon.svg.png"
        width = "100"/>
</p>




### Installation

- Get VS Code from [project website](https://code.visualstudio.com/Download)
- extract the zip and open the file
- Ta Dah! its is installed
- Opens VScode welcome page. Lots of shortcuts and tutorials to play around

### Extensions I have used

- _Ayu_: for folder icons
- _HTML Preview_: for previewing html, markdown etc
- _IntelliJ IDEA Keybinding_: for keybindings in vscode
- _Markdown + Math_: for typing math symbols in $\LaTeX$
- _Predawn Theme Kit_: for vscode theme
- **_Python_**: for python of course! details below
- _Rainbow CSV_: For csv preview, search etc

### Python Extension

- Need to have python installed in the system
- __**INSTALLATION**__
    - Go to Extension icon on the "Activity Bar" on the left
    - Search for Python. Click "install" and the python extension is ready
- It has nice features such as code highlighting, linting, formatting, intellisence
    - Usually as we write code and try to run, there will be notification asking to install. Otherwise, can install `pylint`, `black`, etc. from command line in global environment
- VS code also comes with integrated terminal
- Install Linter eg. `pylint` to use Linter
- Install code formatter eg. `black`, `ypaf`, `pep8`, etc. `shift+Alt+F`
- To see how certain python module was written - hover the mouse to the function, and right click then select `go to definition`
    

### Running Python in VS code
<!-- ![VScode Screenshot](https://user-images.githubusercontent.com/1487073/58344409-70473b80-7e0a-11e9-8570-b2efc6f8fa44.png) -->
<p align="center">
  <img alt="VS Code in action" src="https://user-images.githubusercontent.com/1487073/58344409-70473b80-7e0a-11e9-8570-b2efc6f8fa44.png"
  width = "800"

<figcaption>Screenshot of VScode Editor. Source: vscode github</figcaption>
</p>
Nice thing i like about VS code is that i can run python without leaving VScode. VScode recognizes the python script with the .py extension  
and run the global python install (usually in the `C:\\Program Files *\Python *\python.exe` location). Can switch to any python environment by 
choosing a different intrepreter. If running a python file in VScode, on the bottom left on the blue bar, ther eis a python intrepreter version. 
To select a different intrepreter, click there or press `F1` or `ctrl+shift+p` and it will start the commad pallete and type `python select intrepreter`

VScode creates .vscode folder in the location where the project is located and this is where all the local settings are stored. 
Python settings can be found in `settings.json` file
This settings could include which python intrepreter is selected, font size, color scheme, etc. They all can be customized.
There are times when I use the same Python virtual environment for some of the prototyping and install the libraries there instead of creating a new venv.
It is possible that the `Select Python Intrepreter` may not list that because it is in a path not recognized at the directory level of the script. 
To add python from that venv add the path to the settings.json `"python.pythonPath":"~\\..\\my_virtual_env\\Scripts\\activate"`

### Global User settings

Changing the global user settings to customize the VScode work environment gives the user the familiar workspace. Similar to the workspace setting above, 
there is a global user setting availe

- Color Theme - `command pallete > preferance: color theme` But if needed to install external color theme then `install`. It will open the extension marketplace.
Choose to install something like `predawn`
- File icon Theme - `command pallete > preferance: file icon theme` To install from the marketplace, go to `install` and search from the extension marketplace.
Selecting the icon Theme changed color theme but then went back to color theme and changed it back to the prefered color theme.
- Change global settings- click on the `gear` icon. then select the settings. It by default opens on UI version. 
If want a json version, then click on {} towards the top right

Open Default settings 
- `command palette > open default setting (json)`
    - To change setting view from UI to JSON- find `workbench.quickOpen.preserveInput` and change to JSON
    **To see what the options to change the values to**: Look at the pencil icon and right click to select the option
    - See default setting while opening user setting  `"workbench.settings.openDefaultSettings": true,`
    Font and other appearance setting
    - `code runner`- will allow run code with keyboard shortcuts without having to go to terminal. Install code runner form extension market place eg 


__References__:  
[VS Code Docs:](https://code.visualstudio.com/docs)  
[Corey M Schafer](https://coreyms.com)  
Corey's User [settings](https://github.com/CoreyMSchafer/dotfiles/blob/master/settings/VSCode-Settings.json)