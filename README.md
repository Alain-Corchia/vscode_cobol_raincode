# Raincode COBOL Source editing for Visual Studio Code

[![Version](https://vsmarketplacebadges.dev/version/raincode.cobol.png)](https://marketplace.visualstudio.com/items?itemName=raincode.cobol) [![Installs](https://vsmarketplacebadges.dev/installs-short/raincode.cobol.png)](https://marketplace.visualstudio.com/items?itemName=raincode.cobol) [![Downloads](https://vsmarketplacebadges.dev/downloads-short/raincode.cobol.png)](https://marketplace.visualstudio.com/items?itemName=raincode.cobol) [![Rating](https://vsmarketplacebadges.dev/rating-star/raincode.cobol.png)](https://marketplace.visualstudio.com/items?itemName=raincode.cobol)

This unofficial extension provides syntax highlighting for `Raincode Stack` based COBOL languages, as well as syntax highlighting for other related languages/file formats such JCL, PL/I.

Some of the features this extension provides are:

- Colourisation and problem matchers for the following Raincode COBOL dialects:
  - [Raincode Stack and JCL](https://www.raincode.com/products/) 
- COBOL tab key support (configurable)
- COBOL source navigation support
  - Shortcuts/Commands for navigation to divisions
  - Fixed format margin support
  - Outline view/breadcrumb support
  - Text based find all references
  - Peek definition
  - Copybook navigation
- Intellisense support for keywords in lowercase, uppercase and camelcase
- Snippet support for various keywords
  - including callable COBOL library routines
  - and intrinisic functions
- Source code linter for in house/internal COBOL standards
- Compiler directive file colourisation
- Unit test report colourisation
- COBOL Source Utilities  
  - Remove column numbers
  - Remove program identification area
  - Remove all comments
  - Text based rename paragraphs/sections and variables
  - Make all keywords/fields/sections uppercased, lowercased or camelcased
  - Re-sequence column numbers
  - Optional xedit'ish key bindings
  - Align storage items
  - Text to hex literals & reverse
- Documentation for using development containers with Visual COBOL
- and more..

While also being able to use it with the official Raincode Cobol debugger (requiring instalaion of the extention (for debugging for example).

## Examples of features provided

### Code colorization for COBOL, PL/I and JCL

 ![sieve_jcl](https://raw.githubusercontent.com/spgennard/vscode_cobol/main/images/screenshot_three.png)

### IntelliSense example

![perform_add](https://raw.githubusercontent.com/spgennard/vscode_cobol/main/images/perform_add.gif)

### Breadcrumb support

![breadcrumbs](https://raw.githubusercontent.com/spgennard/vscode_cobol/main/images/breadcrumb.png)

### Outline support

![outline](https://raw.githubusercontent.com/spgennard/vscode_cobol/main/images/outline.png)

### Go to definition

![gotodef](https://raw.githubusercontent.com/spgennard/vscode_cobol/main/images/gotodef.gif)

### Peek definition

![peekdef](https://raw.githubusercontent.com/spgennard/vscode_cobol/main/images/peekdef.gif)

### COBOL specific coloured comments

![coloured_comments](https://raw.githubusercontent.com/spgennard/vscode_cobol/main/images/coloured_comments.png)

## Keybindings

| Keys              |                           Description                           |
| ----------------- | :-------------------------------------------------------------: |
| ctrl+alt+p        |                    Go to procedure division                     |
| ctrl+alt+w        |                  Go to working-storage section                  |
| ctrl+alt+d        | Go to data division (or working-storage section if not present) |
| ctrl+alt+,        |              Go backwards to next section/division              |
| ctrl+alt+.        |            Go forward to next next section/division             |
| f12 or ctrl+click |                       Go to copybook/file                       |
| ctrl+hover        |              Peek head of copybook or symbol/field              |
| right mouse/peek  |             Peek copybook without opening the file)             |
| ctrl+alt+a        |                 Adjust line to cursor position                  |
| ctrl+alt+l        |                 Left adjust line to left margin                 |
| alt+right         |                  Insert spaces to column 72                     |

## Keybindings - xedit'ish

Only active when `coboleditor.xedit_keymap` is set to true.

| Keys   |       Description       |
| ------ | :---------------------: |
| ctrl+a | cursor to start of line |
| ctrl+b |       cursor left       |
| ctrl+c |     clipboard paste     |
| ctrl+d | delete right character  |
| ctrl+e |     cursor line end     |
| ctrl+f |      cursor right       |
| ctrl+h |       delete left       |
| ctrl+j |    insert line after    |
| ctrl+k |     delete to right     |
| ctrl+m |   insert line before    |
| ctrl+t |        transpose        |
| ctrl+z |     scroll line up      |
| alt+z  |    scroll line down     |

## Settings

- COBOL tab stops can be changed by editing the ```coboleditor.tabstops``` setting.
- Extensions used for *Go to copybook*, can be changed by editing the ```coboleditor.copybookexts``` settings.
- Directories used for *Go to copybook*, can be changed by editing the ```coboleditor.copybookdirs``` settings.

## New File

New file creation support is provided for Raincode-COBOL

## Changing the default file associations

The command "Enforce extension via file.assocations" allows the default to be change from the "Raincode-COBOL".

## Tasks

Visual Studio code can be setup to build your COBOL source code.

### Task: Using MsBuild

MsBuild based projects can be consumed as build task, allowing navigation to error/warnings when they occur.

Below is an example of *build* task that uses *mycobolproject.sln*.

```json
{
    "version": "2.0.0",
    "tasks": [ {
            "label": "Compile: using msbuild (mycobolproject.sln)",
            "type": "shell",
            "command": "msbuild",
            "args": [
                "/property:GenerateFullPaths=true",
                "/t:build",
                "mycobolproject.sln"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "presentation": {
                "reveal": "always"
            },
        }
    ]
}
```

### Task: Single file compile using Micro Focus COBOL - ERRFORMAT(3)


The example below shows you how you can create a single task to compile one program using the `cobol` command.

For Net Express/Server Express compilers use the "$mfcobol-errformat3-netx-sx" problem matcher as although the directive ERRFORMAT"3" is used, the compiler output error format is slightly different.

```json
{
    "label": "Compile: using cobol (single file)",
    "command": "cobol",
    "args": [
        "${file}",
        "noint",
        "nognt",
        "noobj",
        "noquery",
        "errformat(3)",
        "COPYPATH($COBCPY;${workspaceFolder}\\CopyBooks;${workspaceFolder}\\CopyBooks\\Public)",
        ";"
    ],
    "group": {
        "kind": "build",
        "isDefault": true
    },
    "options": {
        "cwd": "${workspaceRoot}"
    },
    "presentation": {
        "echo": true,
        "reveal": "never",
        "focus": true,
        "panel": "dedicated"
    },
    "problemMatcher": "$mfcobol-errformat3"
}
```

### Task: Single file compile using Micro Focus COBOL - ERRFORMAT(2)

The example below shows you how you can create a single task to compile one program using the `cobol` command.

For Net Express/Server Express compilers use the "$mfcobol-errformat2-netx-sx" problem matcher as although the directive ERRFORMAT"2" is used, the compiler output error format is slightly different.

```json
{
    "label": "Compile: using cobol (single file)",
    "command": "cobol",
    "args": [
        "${file}",
        "noint",
        "nognt",
        "noobj",
        "noquery",
        "errformat(2)",
        "COPYPATH($COBCPY;${workspaceFolder}\\CopyBooks;${workspaceFolder}\\CopyBooks\\Public)",
        ";"
    ],
    "group": {
        "kind": "build",
        "isDefault": true
    },
    "options": {
        "cwd": "${workspaceRoot}"
    },
    "presentation": {
        "echo": true,
        "reveal": "never",
        "focus": true,
        "panel": "dedicated"
    },
    "problemMatcher": [ "$mfcobol-errformat2", "$mfcobol-errformat2-copybook" ]
}
```

### Task: Single file compile using COBOL-IT

The example below shows you how you can create a single task to compile one program using the `cobc` command.

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Compile: cobc (single file)",
            "type": "shell",
            "command": "cobc",
            "args": [
                "-fsyntax-only",
                "-I${workspaceFolder}\\CopyBooks",
                "-I${workspaceFolder}\\CopyBooks\\Public",
                "${file}"
            ],
            "problemMatcher" : "$cobolit-cobc"
        }
    ]
}
```

### Task: Single file compile using ACUCOBOL-GT

The example below shows you how you can create a single task to compile one program using the `cobrc` command.

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Compile: using ccbl32 (single file)",
            "type": "shell",
            "command": "%ACUCOBOL%\\bin\\ccbl32",
            "args": [
                "-Sp", "${workspaceFolder}\\CopyBooks",
                "-Sp", "${workspaceFolder}\\CopyBooks\\Public",
                "${file}"
            ],
            "windows": {
                "options": {
                    "env": {
                        "ACUCOBOL" : "C:\\extend10.1.1\\AcuGT"
                    }
                }
            },
            "problemMatcher" : [ "$acucobol-warning-ccbl", "$acucobol-ccbl" ]
        }
    ]
}
```


## Pre-Processor support for "hidden" source code

Some pre-processor reference copybooks that are inserted into the code without using standard COBOL syntax.

These source files are often difficult to edit due this extension not knowing anything about these files.

In order to help this extension, a special style of comment can be inserted into the code that allows the extension to locate these extra copybooks.

For example, if your preprocessor includes the files ```Shared/foo.cbl OldCopyBooks/Shared/bar.cbl```, then you can use the following comment line.

```cobol
*> @source-dependency Shared/foo.cbl OldCopyBooks/Shared/bar.cbl
````

The source-dependency names are separated by a space.

To enable this feature enable the ```scan_comments_for_hints``` setting, for example:

```json
"coboleditor.scan_comments_for_hints": true
```

The hint token can be configured by the ```coboleditor.scan_comment_copybook_token``` setting, which has the default value set to ```source-dependency```.

It is recommended that the token name remain consistent in your source, otherwise it will make it hard for observers of your source to understand the code.

## coboleditor.fileformat & coboleditor.fileformat_strategy

When ```coboleditor.fileformat_strategy``` is set to "normal", the source format will be determined heuristically but can be overriden by either embedded directives with each source file.

However, if you need to tell the extension which file(s) are which particular file format, this can be achieved with ```coboleditor.fileformat``` property.

For example, if you want all the files that match ```A*.cbl``` to be fixed and every other ```*.cbl``` is free format, you can then use:

```json
    "coboleditor.fileformat": [
        {
            "pattern": "**/A*.cbl",
            "sourceformat": "fixed"
        },
        {
            "pattern": "**/*.cbl",
            "sourceformat": "free"
        }
    ],
```

If always use ```fixed``` format you can set ```coboleditor.fileformat_strategy=always_fixed```.

## Handling code pages

The defaults embedded in the extension can be overwritten by either changing sessions at the user level in the settings json file or more efficiently, you change it just for the "COBOL" files.

For example, to ensure you use utf8 for all you files use:

```json
{
    "[COBOL]": {
        "files.encoding": "utf8",
        "files.autoGuessEncoding": false
    }
}
```

## Intellisense and case formatting

A overall intellisense style can be selected via the ```coboleditor.intellisense_style``` property.

If you find a keyword or snippet includes a extra space that is not required, you can amend the ```coboleditor.intellisense_no_space_keywords``` property to exclude it.

Custom formatting rules can be enabled for a specific item or prefixed item via the ```coboleditor.custom_intellisense_rules``` setting.

The format is an array of strings, that are in two parts, seperated by a ```:```.   The first part of item and the second is one of four characters, that denote the case style.  

    u = Uppercase
    l = Lowercase
    c = camelcase
    = = unchanged

If the end items a ```*```, a partial search is done:

For example, to ensure all items that start ```WS-``` should be uppercased.

```WS-*:u```

or a more specific item:

```WS-COUNTER:u```

The property ```coboleditor.format_on_return``` allows the intellisense rules to be applied to the previous line when the return key has been pressed.

## Tips for use

- If you find you are not getting any symbols in the outline view or the peek/goto definition functionality does not work, please check the ``Output->COBOL`` panel as it may give you a reason for this.

   For example the editor line limit has been surpassed or the file fails to be identified as a COBOL source file.

  - The colors in the editor can be changed on a per theme basis, for example:

```json
"editor.tokenColorCustomizations": {
        "[Monokai]": {
            "comments": "#229977"
        }
    }
```

Where *comments* is a token name, standard tokens can be found in the [textmate documentation](https://macromates.com/manual/en/language_grammars).

Useful tokens that are often changed are: comment.line.cobol.newpage, keyword.operator.

## Complementary extensions

### [COBOL Language Dictionary - Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=bitlang.code-spell-checker-cobol)

Spell checking code is helpful but without specific support for the COBOL language it can be a painful experience, so in order to make it easier. I have produced a spell checker extension that has a the standard COBOL reserved words and keywords from various dialects such a Micro Focus COBOL and IBM Enterprise COBOL.

Spell checking can be enabled/disabled in your source code by using:

```cobol
      * spell-checker: disable
      * spell-checker: enable
```

You can also ignore words in the code, for example:

```cobol
      * cSpell:ignoreWords TPCC, ridfld, dfhresp
```

Lastly, you use a regular expression, for example, to ignore words that contain a '-', you could use:

```cobol
      * cSpell:ignoreRegExp /\w*-\w*/
```

### [ToDo tree](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.todo-tree) by Gruntfuggly

Although this extension does not understand comments in COBOL source files, it can be made to by adding the following user setting:

```json
{
    "todo-tree.tree.flat": false,
    "todo-tree.tree.expanded": true,
    "todo-tree.regex.regex": "((//|#|<!--|;|/\\*|\\*>|^......\\*)\\s*($TAGS)|^\\s*- \\[ \\])",
    "todo-tree.general.tags": [
        "TODO",
        "FIXME",
        "!FIXME",
        "CHANGED",
        "BUG",
        "NOTE"
    ],
    "todo-tree.tree.filterCaseSensitive": true,
    "todo-tree.highlights.customHighlight": {
        "FIXME": {
            "icon": "flame",
            "iconColour": "#A188FF",
        },
        "NOTE": {
            "iconColour:" : "blue"
        },
        "TODO": {
            "iconColour:" : "cyan"
        },
        "CHANGED": {
            "iconColour:" : "yellow"
        },
        "BUG": {
            "icon": "bug"
        }
    }
}
```

## Visual Studio Code Workspace Trust security

When in limited functionality mode the extension disables all functionality that might be use for malicious purposes.

The extension only enables features that allow basic editing, making it ideal for browsing untrusted source.

## Online resources

- Online communities
  - [Facebook COBOL Group](https://www.facebook.com/groups/COBOLProgrammers/)
  - [Open Mainframe Project - COBOL Forum](https://community.openmainframeproject.org/c/cobol-technical-questions)
  - [Tek-Tips - COBOL General discussion](https://www.tek-tips.com/threadminder.cfm?pid=209)
- Stack Overflow topics/tags:
  - [COBOL](https://stackoverflow.com/questions/tagged/cobol)
  - [CICS](https://stackoverflow.com/questions/tagged/cics)
- [COBOL Programming Language Articles on Reddit](https://www.reddit.com/r/cobol/)
- [Linkedin Learning COBOL Resources](https://www.linkedin.com/learning/topics/cobol)
- wikipedia
  - [COBOL](https://en.wikipedia.org/wiki/COBOL)
  - [CICS](https://en.wikipedia.org/wiki/CICS)
- standards
  - [ICS > 35 > 35.060 / ISO/IEC 1989:2014](https://www.iso.org/standard/51416.html)

## Shortcuts

- [ALT] + [SHIFT] + [C]: Change to COBOL Syntax (default)
- [ALT] + [SHIFT] + [A]: Change to ACUCOBOL-GT Syntax
- [ALT] + [SHIFT] + [M]: Toggle margins (overrides user/workspace settings)

## Contributors

This Extention is base on the orignal [Bitlang project](https://github.com/spgennard/vscode_cobol) from Stephen Gennard 

I would like to thank the follow contributors for providing patches, fixes, kind words of wisdom and enhancements.

- Ted John of Berkshire, UK
- Kevin Abel of Lincoln, NE, USA
- Simon Sobisch of Germany

 NOTE: Some of the above contributions have now been moved into the GnuCOBOL extension.
