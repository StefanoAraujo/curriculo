# AltaCV, yet another LaTeX CV/Résumé class

v1.1.3 (30 April 2017), by LianTze Lim (liantze@gmail.com)

(Thanks to [Nur](https://github.com/nurh) for the name.)

It all started with this:

[<img src="tweet-that-started-this.png" width="500px">]
(https://twitter.com/Leonduck/status/764281546408923136)

Leonardo was talking about a [résumé of Marissa Mayer that Business Insider put together](http://www.businessinsider.my/a-sample-resume-for-marissa-mayer-2016-7/) using [enhancv.com](https://enhancv.com).
I _knew_ I had to do something about it. And so AltaCV was born.

## Samples

This is how the re-created résumé looks like ([view/open on Overleaf](https://www.overleaf.com/read/gtqfpbwncfvp)):

<img src="mmayer.png" alt="Marissa Mayer's résumé, re-created with AltaCV" width="600px">

Though if you're creating your own CV/résumé, you'd probably prefer using the basic template ([view/open on Overleaf](https://www.overleaf.com/read/trgqjpwnmtgv)):

<img src="sample.png" alt="sample barebones AltaCV template" width="600px">


## Requirements and Compilation

* AltaCV uses [`fontawesome`](http://www.ctan.org/pkg/fontawesome) and [`academicons`](http://www.ctan.org/pkg/academicons); they're included in both TeX Live 2016 and MikTeX 2.9.
* Loading `academicons` is optional: enable it by adding the `academicons` option to `\documentclass`.
* Can now be compiled with pdflatex, XeLaTeX and LuaLaTeX!
* However if you're using `academicons`, you _must_ use either XeLaTeX or LuaLaTeX. If the doc then compiles but the icons don't show up in the output PDF, try compiling with LuaLaTeX instead.
* The samples here use the [Lato](http://www.latofonts.com/lato-free-fonts/) font.

install Dependencies

https://pmateusz.github.io/latex/2018/01/30/vs-code-latex-editor.html

Latex January 30, 2018
Visual Studio Latex Editor Cover Image

This article provides instructions how to setup Visual Studio Code for writing and editing Latex documents. Our previous article on this subject became obsolete due to recent changes in the editor and its plugins. This post aims to provide the most accurate and up to date configuration. Repeating steps of the tutorial should deliver a robust working environment for Latex with syntax highlight, spell checking and live document preview. The post is concluded by feature comparison between Visual Studio Code and Overleaf.
Introduction
Visual Studio Code is a general purpose text editor. Although it can be used to edit any plain text file it shines when working with source code. Its feature set has been selected to offer fast navigation and responsive editing experience in large text files. The editor does not offer out of the box capabilities that one is unlikely to use. Features for a targeted audience are available through extensions. They often belong to one of categories: integration with third-party tools, linters and editor enhancements.

In this tutorial we cover the editor configuration and the most useful extensions for Latex. As of this writing each referenced extension was installed more than 300000 times. Thus they were comprehensively tested and proved their utility in practice.

Even though the article was written using Debian 9.3 (stretch), the instructions from the tutorial should work without any changes on other operating systems, except for the installation of the bare Visual Studio Code editor.

Prerequisites
We assume that the programs to build a Latex document and a bibliography from the .tex and .bib files respectively are installed on your machine.

pdftex --version
bibtex --version
Otherwise, install relevant software packages for your operating system. Debian/Ubuntu users can accomplish this by installing the texlive package.

sudo apt-get install texlive 
Tutorial
Download and install Visual Studio Code.

The commands below will download and install the latest stable version of the editor on Debian/Ubuntu. If you are using a different operating system or want to install an insider distribution see the project website for relevant instructions.

 wget -Ovscode.deb https://vscode-update.azurewebsites.net/latest/linux-deb-x64/stable
 sudo dpkg --install vscode.deb
 sudo apt-get install --force
The last command sudo apt-get install --force downloads and installs all required dependency packages that are currently not installed in the system. Although not strictly necessary in our case it is a good practice to run this command after manual installation of .deb packages. After successful installation the editor can be launched using the code command.

code --version
You do not need to worry about future updates of Visual Studio Code. During the installation the package repository was added to the list of repositories of Advanced Package Tool (apt).

cat /etc/apt/sources.list.d/vscode.list 
   
> deb https://packages.microsoft.com/repos/vscode stable main
Install extensions.

Execute the following commands.

 code --install-extension James-Yu.latex-workshop
 code --install-extension streetsidesoftware.code-spell-checker
The same can be accomplished using the Extensions toolbar in the editor. To open the toolbar click View in the application menu and then select Extensions.

The Latex Workshop extension offers comprehensive set of features for development of .tex documents. The most important ones are syntax highlight, error reporting and document preview. The full list of supported actions can be listed using the the Ctrl+Alt+L keystroke combination. For brevity we are not going to cover all of them here, but we strongly recommend you to try them yourself. For example, the citation browser enables adding references without the need to open and browse the bibliography. The word count is useful for writing abstracts with word limits. Finally, the cleanup of auxiliary files removes all intermediate files created during the Latex and Bibtex compilation.

The Code Spell Checker extension does no more than its title suggests.

Open the directory of your Latex project in Visual Studio Code.

mkdir latex_project && code latex_project
Create Visual Studio Code tasks for building .tex and .bib documents.

Make .vscode directory in the root directory of your Latex project.

Create tasks.json file inside the .vscode directory with the following content.

{
 "version": "2.0.0",
 "tasks": [
       {
         "label": "Run pdflatex",
         "type": "shell",
         "group": {
             "kind": "build",
             "isDefault": true
         },
         "command": "pdflatex",
         "args": [
             "-interaction=nonstopmode",
             "-file-line-error",
             "*.tex"
         ]
     },
     {
         "label": "Run bibtex",
         "type": "shell",
         "group": {
             "kind": "test",
             "isDefault": true
         },
         "command": "bibtex",
         "args": [
             "-terse",
             "*.aux"
         ]
     }      
 ]
}
The configuration file defines two tasks: Run pdflatex and Run bibtex. They correspond to the commands pdflatex -interaction=nonstopmode -file-line-error *.tex and bibtex -terse *.aux respectively. The first command is set as the default build task and can be run using the Ctrl+Shift+B keystroke combination. The other command is set as the default test. Unfortunately, no keystroke is bound to this action by default. If you compile a bibliography often we suggest to bind the task to the Ctrl+Shift+T keystroke combination or any other you may prefer.

In order to bind an action to a keystroke open the Keyboard Shortcuts tab using the Ctrl+K Ctrl+S keystroke combination. Alternatively, in the application menu click File, Preferences and Keyboard Shortcuts. The Keyboard Shortcuts tab should be opened. Search for the Run Test Task (workbench.action.tasks.test) label and set up the keystroke of your choice.

Besides a keystroke combination Visual Studio Code allows to execute a task using the Task button in the application menu or via the task prompt that can be opened using the Ctrl+Shift+P keystroke combination.

Set up a spell checker.

Create the cSpell.json file inside the .vscode directory with the following content.

{
 "version": "0.1",
 "language": "en",
 "dictionaryDefinitions": [
  { "name": "projectDictionary", "path": "./dictionary.txt"}
 ],
 "dictionaries": ["projectDictionary"],
 "languageSettings": [
  { "languageId": "*", "dictionaries": ["projectDictionary"] }
 ],
 "ignoreRegExpList": [
 "\\\\cite{[A-Za-z0-9, -]+}",
 "\\\\begin{\\w+}",
 "\\\\end{\\w+}",
 "\\\\usepackage{\\w+}",
 "\\\\bibliographystyle{\\w+}",
 "\\\\hyphenation{[A-Za-z0-9, -]+}",
 "\\w+{?"]
}
The configuration instructs the Code Spell Checker to use a default English dictionary extended by a custom list of words loaded from the dictionary.txt file. Spelling errors will not be reported for Latex commands and text in-between curly brackets of the shortlisted commands.

Create an empty dictionary.txt file in the .vscode directory. It should contain words that are not recognized by the spell checker. An example of the valid file format, a list of words separated by a newline character, is provided below.

aforementioned
combinatorial
decremental
forementioned
polytope
pseudocode
scalability
superset
subproblem
Alter editor settings for better work with text files.

Create the settings.json file with the following content.

{  
  "cSpell.enabled": true,
  "editor.cursorBlinking": "solid",
  "editor.wordWrap": "on",
  "editor.wordWrapColumn": 80,
  "editor.wrappingIndent": "same",
  "latex-workshop.debug.showUpdateMessage": false,
  "telemetry.enableCrashReporter": false,
  "telemetry.enableTelemetry": false
}
Most of configuration settings above, though difficult to know off the top of one’s head, are self explanatory. The reason for disabling a blinking cursor is the anecdotal evidence that a solid cursor is less distracting and causes less eye fatigue problems. Word wrapping is enabled to get rid of the horizontal scroll bar.

Optionally, hide the activity bar.

Click View in the application menu and then the Hide Activity Bar button. While working with Latex we do not switch between the Explorer and Debugger panels, so the space occupied by the Activity Bar can freed for the document viewport.

Other Environments for Latex
It is worth to compare and contrast the working environment developed in this post with competitors. This will make us knowledgeable of other possible options and help avoiding bias while deciding what works best for us.

In our view the most important alternative to the setup we presented is Overleaf. A web application for creating and editing Latex documents. For quick overview check the list of most important features advertised at the company website. Overall, we believe that comparing any web application and its desktop companion is like comparing apples to oranges. The first one does not require installation and enables real-time collaboration. On the other hand, the desktop variant can work offline providing simultaneously more responsive and less distracting environment. In a nutshell the same holds between Overleaf and Visual Studio Code.

In our opinion the Overleaf is superior when one works with colleagues and wants to enforce that everyone uses the most recent version of a document to avoid conflict resolution. Secondly, Overleaf has effortless setup. One does not need to install anything Latex specific on a personal computer, so may enjoy an easier start in this environment. However, the free version has restrictions that may hinder productivity. It constrains a project structure, limits the number of files, does not allow to track changes and restore previous revisions of the document. For any larger work that benefits from no distractions, such as writing theses, we strongly recommend Visual Studio Code in conjunction with git for tracking changes.

Conclusion
If you reached this point you should be set to work with Latex documents. We believe there is few more tips on the subject that would suit everybody. In case you spot an error or know something on the subject you would like to share, you are more than welcome to leave a comment or join the discussion below.


====================================
https://www.scivision.dev/adding-missing-fonts-to-latex-ubuntu/

Add missing LaTeX fonts
19 May, 2018
Missing LaTeX fonts are fixed by installing TeXlive fonts.

Linux
marvosym: apt install texlive-fonts-recommended
fontawesome (for popular emoji and website icons): apt install texlive-fonts-extra
Windows
Use MiKTeX to search for package name and Install:

Start → MiKTeX → Package Manager (admin)

=====================================
https://tex.stackexchange.com/questions/140555/how-to-fix-tikz-sty-problem


Questions
Tags
Users
Unanswered
How to fix tikz.sty problem [closed]

sudo apt-get install texlive-full