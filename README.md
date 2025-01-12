<div align="center">
  <h1 align="center">
    <img src="https://github.com/fancyc-bsi/BSTI/blob/main/assets/bsti.png?raw=true" width="300" />
    <br>
  </h1>
</div>
</h1>
</div>

---

## 📒 Table of Contents
- [📒 Table of Contents](#-table-of-contents)
- [📍 Overview](#-overview)
  - [Features](#features)
- [🚀 Getting Started](#-getting-started)
  - [✔️ Prerequisites](#️-prerequisites)
    - [Windows](#windows)
    - [MacOS](#macos)
    - [Debian based OS](#debian-based-os)
  - [💻 Installation](#-installation)
- [🎮 Using BSTI](#-using-bsti)
  - [First Steps and Connecting](#first-steps-and-connecting)
  - [Understanding the Interface](#understanding-the-interface)
  - [Home Tab](#home-tab)
  - [Module Editor](#module-editor)
  - [View Logs](#view-logs)
  - [Modules](#modules)
    - [JSON Format](#json-format)
    - [Argument Metadata](#argument-metadata)
    - [File Upload Metadata](#file-upload-metadata)
    - [Nessus Mapping Metadata](#nessus-mapping-metadata)
    - [Tmux For Interactive Commands](#tmux-for-interactive-commands)
- [Integrations](#integrations)
    - [Nessus2Plextrac-ng](#nessus2plextrac-ng)
    - [NMB](#nmb)
    - [File Viewer](#file-viewer)
- [Contributing to BSTI](#contributing-to-bsti)

---


## 📍 Overview

### Features
> - "Module" system with predefined metadata syntax to allow for easy argument passing, file upload and Nessus finding mapping.
> - NMB fully integrated, control nessus scans from the UI, export results and verify findings just like you're used to.
> - Tmux sessions: from the UI, attach to running tmux session on your BSTG, allowing for interactive modules (useful for reverse shells).
> - View HTML, CSV and JSON files from the UI - custom csv parser allows for filtering by severity and host extractions per finding. 
> - Nessus2plextrac-ng: create reports, use the plugin manager and import findings into plextrac all from the UI.
> - Start/stop a local portforward or socks proxy from a dropdown menu.
> - Interactive module editor built in with syntax highlighting.
> - View logs from modules and NMB, take a screenshot of the content and even automatically save the screenshot as the format n2p-ng expects.
> - View if your current BSTG is online plus diagnostic info, dynamic links to it's Nessus instance and transfer files to and from it using the home page.
> - Over 25 modules already added with a defined naming convention - making searches intuitive based on the protocol you're trying to exploit.
> - Built in help menu will open the wiki for all 3 of the tools.

---


## 🚀 Getting Started

### ✔️ Prerequisites

python3.9 is currently still required.

To take screenshots, wkhtmltopdf is required:

#### Windows

* https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox-0.12.6-1.msvc2015-win64.exe

#### MacOS

* https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-2/wkhtmltox-0.12.6-2.macos-cocoa.pkg

#### Debian based OS

```bash 
sudo apt update && sudo apt install wkhtmltopdf -y
```

### 💻 Installation
``` bash
Install the required Python packages:

# Ensure you have python3.9 installed.

# Venv is highly recommended to prevent conflicts
python -m venv myvenv
# Linux:
source myvenv/bin/activate
# Windows:
.\myvenv\Scripts\Activate.ps1

# Install the requirements
pip install -r requirements.txt

```
---
## 🎮 Using BSTI

```bash

python bsti.py
```
**Note:** BSTI has only been tested on window, macOS users are encouraged to report bugs or issues when found.

### First Steps and Connecting
To connect to a BSTG, click "Config" in the top-left corner, then "Configure new BSTG" and enter the login details.  
If you've already connected to that BSTG with BSTI before, it'll show up in the bottom left corner. You can change this on the fly by clicking in that space and selecting another BSTG from the dropdown menu.

Saved details for a BSTG will be written to .config/drones.json - this will not be encrypted unless there is a demand for it, hopefully someday we will go to key-based authentication only.

The dropdown menu is very important now, the BSTG mentioned in that box will be your "Active BSTG" and a lot of the functionality built within this app relies on that active connection. Ensure you are using the correct BSTG.

It is highly recommended to add your public ssh key to the BSTG to prevent the need for password authentication. This can be done from the UI here:

![image](https://github.com/fancyc-bsi/BSTI/assets/85493503/e3e143bd-b3b9-4f1c-bd18-51654f4e04f8)

### Understanding the Interface
BSTI uses a 'tab' system similar to a browser, almost every tab can be closed except for the core tabs like home, logs, NMB and the IDE. 

**Note:** When you close a tab you are also killing the process.


### Home Tab

This page displays your currently connected bstg's diagnostic info, a link to their nessus instance and plextrac. It will tell you if you currently connected bstg is online and show output from the 'top' command.

Additionally, if you need to transfer files to and from the BSTG you can do so from the home page.

![image](https://github.com/fancyc-bsi/BSTI/assets/85493503/f984d7b1-26ae-49d1-8d9c-369db24b0d4d)


### Module Editor

This page displays the code for the module that is currently selected. If you would like to edit a module you can do so here directly. Just make sure you click **Save Module** if you want your changes to be written, otherwise they will be stored in a tempfile.  

To select another module click on the box next to "Choose a module to run:" and it will open a dropdown menu of available modules. To execute, simply click "Execute Module" in the bottom right corner, enter any arguments (If necessary), and click "Submit".  

Search for a module will load the closest match, the naming convention is like this: [category]-[protocol]-[tool-used]-[module-name].sh. 

The module editor also features some syntax highlighting with plans to include metadata linting in the future.

![image](https://github.com/fancyc-bsi/BSTI/assets/85493503/43adc865-ace0-4ed6-9ded-0ad036604af2)



### View Logs

This tab will automatically populate with the output from every module that is run. Here are the following options:
- Clicking "Take Screenshot" will generate a screenshot of the entire log. You can crop this afterwards to fit your needs.
- Delete Logs
- Refresh Logs

Adding the nessus command mapping metadata, the FIRST screenshot taken of the corresponding log will be in the md5 hash format that n2p-ng expects. This is useful for more advanced exploitation that NMB cannot cover.

![image](https://github.com/fancyc-bsi/BSTI/assets/85493503/22d48e9a-1e17-4026-b1fb-0179eca322f4)


---

### Modules
To create a new module click Modules -> Create New. This will create a new file in the Modules directory and automatically populate a template to use in the Module Editor tab.

Currently BSTI supports modules written in bash, python, and json. Bash and Python scripts handle actions that can be performed in a single tab, while the JSON format has been setup to allow a single module to perform multiple actions in different tabs.


#### JSON Format

The format for the JSON file should be the following:

```json
{
    "grouped": true,
    "tabs": [
        {
            "name": "Responder",
            "command": "echo 'test'"
        },
        {
            "name": "Echo 1",
            "command": "echo 'Tab 1' && sleep 3600"
        },
        {
            "name": "Echo 2",
            "command": "echo 'Tab 2' && sleep 3600"
        }
    ]
}

```

#### Argument Metadata

BSTI has been built with a specific syntax to handle command line arguments that are needed for a module. These are specified in the comment blocks at the top of each module. 

As an example, here is a sample of one module that is meant to search for NSE scripts, Searchsploit, and Metasploit based on the the arguments provided:

```
#!/bin/bash or #!/usr/bin/env python3
# ARGS
# NSE "NSE Query String" 
# searchsploit "Query String"
# MSF "MSF Search String"
# ENDARGS
```
The metadata section is denoted by the `ARGS` and `ENDARGS` comments. These are read in the order that they appear, so `NSE_QUERY_STRING` would be the first argument ($1), then searchsploit ($2), etc...

#### File Upload Metadata

BSTI also has an option to include file upload as part of the modules execution flow. The way this works is that it will ask for the path to a local file and upload that to the bstg to the path thats specified in the metadata.

```
#!/bin/bash or #!/usr/bin/env python3
# STARTFILES
# targets.txt "Targets description"
# ENDFILES
# ARGS
# ARG_NAME "arg description"
# ENDARGS
# AUTHOR:
```
This is useful for instance if you want to create a module that uses nmap, and you want to specify a file for the targets. This way you don't have to go through a seperate process of creating the target file on the bstg and remembering the remote path. You can just create the file locally and upload it when you execute the module.

The files that are uploaded are automatically stored in the /tmp/ directory.

#### Nessus Mapping Metadata
Here you can define a nessus finding to be mapped to the output log - meaning once you take a screenshot of the log using BSTI, it will save it in the format that n2p-ng expects. 

```bash
# NESSUSFINDING
# SSH Server CBC Mode Ciphers Enabled
# ENDNESSUS
```

#### Tmux For Interactive Commands
The execution of modules can be sent to a tmux session, the UI features a menu option to attach to these sessions. This is intended to be for reverse shells or other modules needing interactive output, here is an example:
```bash
#!/bin/bash
# ARGS
# PORT "Desired port for netcat"
# ENDARGS
# AUTHOR: 
# This script starts a Netcat listener inside a tmux session

# Define your tmux session name and Netcat listening port
TMUX_SESSION="nc_listener"
NC_PORT=$1

# Check if the tmux session already exists
if tmux has-session -t $TMUX_SESSION 2>/dev/null; then
    echo "Session $TMUX_SESSION already exists."
    echo "Use 'tmux attach -t $TMUX_SESSION' to interact with the session."
else
    echo "Creating new tmux session named $TMUX_SESSION and starting Netcat listener on port $NC_PORT..."
    # Create a new tmux session and run Netcat
    tmux new-session -d -s $TMUX_SESSION "nc -lvnp $NC_PORT"
    echo "Netcat listener started in tmux session $TMUX_SESSION on port $NC_PORT"
    echo "Use 'tmux attach -t $TMUX_SESSION' or the UI to interact with the session."
fi
```

BSTI will then search for active tmux sessions on your active BSTG, you can click the menu option to connect to the session in your local terminal (powershell for windows, gnome-terminal for unix). Shown here:

![image](https://github.com/fancyc-bsi/BSTI/assets/85493503/a977393a-3f01-4f7f-8c06-c4a4921b1060)

You can also access the terminal of your active BSTI from the same menu.

#### Terminal Menu
As mentioned above, you can start an ssh session with the active BSTG by clicking the "Open Terminal" menu option, from here you can run commands on the BSTG as you normally would.

Additionally, you can start/stop both a socks proxy and a local port forward from this menu, as shown here:

![image](https://github.com/fancyc-bsi/BSTI/assets/85493503/525e7477-d93d-41d8-8b7f-907655dd4bbe)



## Integrations
BSTI is designed to be a complete solution, the tester should be able to perform most of their assessment from the application, and whatever they can't do - they can build within BSTI.

BSTI includes custom version of both NMB and nessus2plextrac-ng with minimal dependencies (no more python3.9 requirements), the original versions can still be found in their current location - for now.

#### Nessus2Plextrac-ng
To use n2p-ng within BSTI, you need to ensure you installed the depends (mostly everyone should already have this installed)
The functionality is the same, just slightly different since there is no interactive terminal within BSTG. 

From the 'Reports' menu, you can select from all the functionality n2p-ng has to offer:

![image](https://github.com/fancyc-bsi/BSTI/assets/85493503/feef75ac-5575-4355-b2f0-c833af653ae8)


Selecting 'plugin manager' or 'Create report' will open an interactive local terminal session (powershell or gnome-termninal). These behave just like the command line you are already used to. 

After you create a report, the client and report ID will both be written to a file, this will pre-fill the values when you attempt to import the findings into plextrac:

![image](https://github.com/fancyc-bsi/BSTI/assets/85493503/50933a13-84c9-4d39-8701-7d0c8865fe9c)


#### NMB 
BSTI also includes NMB integration - you can control all the usual functionality from the UI.

The arguments are dynamically generated based on the 'mode' you select. For example:

![image](https://github.com/fancyc-bsi/BSTI/assets/85493503/6ab5ee56-3305-4636-ba56-8513503bc3fe)

NMB will then use your active BSTG and attempt to run based on the arguments you provided.

You can 'pause' the execution of NMB's finding validation (internal/external mode or deploy mode's finding validation). This will save the current state, hitting 'run' again using either internal or external mode will resume from the previous execution state.

#### File Viewer
BSTI can render html, csv and json files from clicking 'File' in the top left of the menu bar.

You can open up summaries from NMB or Interpreter as shown below:

![image](https://github.com/fancyc-bsi/BSTI/assets/85493503/557902bf-2b9a-4501-90bb-bc782f5a2705)

Or, you can open a csv file in a 'grid-like' format:

![image](https://github.com/fancyc-bsi/BSTI/assets/85493503/e9227393-5e33-477b-a4fe-75d7c3078f54)

You can apply filters by severity rating by using the bottom dropdown bar. Additionally, you can right click a finding to find all intances of it and create a target.txt based on all the gathered hosts:

![image](https://github.com/fancyc-bsi/BSTI/assets/85493503/09ee63b8-b5c8-4030-bb70-71eb8b57c0b4)



## Contributing to BSTI

Interested in contributing to this project?

Check out the dev guide [here](https://github.com/fancyc-bsi/BSTI/blob/main/DEVGUIDE.md).
