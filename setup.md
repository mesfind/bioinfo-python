Here's the updated markdown with the link to your workshop page:

```markdown
---
layout: page
title: Setup
permalink: /setup/
---

For the latest updates and workshop information, please visit the [main workshop page](https://mesfind.github.io/2026-08-19-AAU-BETin/).

## Requirements

The training is hands-on, so participants are encouraged to bring in and use their own laptops to ensure the proper setup of tools for an efficient workflow once you leave the workshop. (We will provide instructions on setting up the required software several days in advance).
*There are no pre-requisites, and we will assume no prior knowledge about the tools.*

## Setup

To participate in the workshop, you will need access to **Anaconda (Python 3)** and the **Positron IDE** as described below. In addition, you will need an up-to-date web browser.

### 1. Anaconda Distribution (Python 3)

[Python](https://www.python.org/) is a popular language for scientific computing and bioinformatics. Installing all required data science packages individually can be challenging, so we recommend installing [Anaconda](https://www.anaconda.com/download), a free distribution that bundles Python along with popular data science packages (such as NumPy, Pandas, and Matplotlib).

**Please make sure you install the Python 3.x version of Anaconda.**

#### Windows

1. Open [https://www.anaconda.com/download/success?reg=skipped](https://www.anaconda.com/download/success?reg=skipped) in your web browser.
2. Download the Graphical Installer for 64-Bit Windows.
3. Run the executable file and follow the setup wizard using default options (make sure *"Register Anaconda3 as my default Python"* is checked).

#### macOS

1. Open [https://www.anaconda.com/download/success?reg=skipped](https://www.anaconda.com/download/success?reg=skipped) in your web browser.
2. Download the Graphical Installer for macOS (choose Apple Silicon or Intel depending on your Mac chip).
3. Open the `.pkg` file and follow the installation instructions using the default settings.

#### Linux

1. Open [https://www.anaconda.com/download/success?reg=skipped](https://www.anaconda.com/download/success?reg=skipped) in your web browser and download the 64-bit installer script (`.sh`).
2. Open terminal, navigate to your download folder:
   ```bash
   cd ~/Downloads
   ```
3. Run the installer:
   ```bash
   bash Anaconda3-*.sh
   ```
4. Follow screen prompts, type `yes` to accept the license, and agree to initialize Anaconda3.

---

### 2. Positron IDE

[Positron](https://positron.posit.co/) is a next-generation, data-science focused Integrated Development Environment (IDE) built by Posit. It provides built-in code completion, interactive Python consoles, file viewers, and plot output tabs in a single interface.

#### Windows

1. Go to the [Positron Download Page](https://positron.posit.co/download.html).
2. Download the Windows `.exe` or `.msi` installer.
3. Run the installer and complete setup with standard defaults.
4. Launch Positron and verify it automatically detects your **Anaconda Python** environment in the top-right environment selector.

#### macOS

1. Go to the [Positron Download Page](https://positron.posit.co/download.html).
2. Download the macOS `.dmg` package suitable for your Mac architecture.
3. Open the `.dmg` file and drag the Positron icon to your `Applications` folder.
4. Open Positron from Applications and ensure Anaconda Python is selected in the interpreter menu.

#### Linux

1. Go to the [Positron Download Page](https://positron.posit.co/download.html).
2. Download the appropriate package format for your distribution (e.g., `.deb` for Ubuntu/Debian or `.rpm` for Fedora/RHEL).
3. Install using your system package manager:
   ```bash
   sudo dpkg -i positron-*.deb
   ```
4. Launch Positron and confirm it connects to your Anaconda environment.

---

### 3. The Bash Shell (For Windows Users)

Mac and Linux operating systems come with a Bash terminal pre-installed. For **Windows** users, you will need to install **Git Bash** to get a Unix-like terminal environment.

#### Git for Windows Setup

1. Download the Git for Windows installer from [gitforwindows.org](https://gitforwindows.org/).
2. Run the `.exe` installer and follow the setup wizard.
3. Keep the default options selected during installation, specifically:
   - **Choosing the default editor:** Use the default or select standard.
   - **Adjusting your PATH environment:** Keep *"Use Git and recommended Unix tools from the Windows Command Prompt"* or standard default option.
4. Click **Install**, then click **Finish**.
5. To open your terminal, search for **Git Bash** in your Start menu.

---

### 4. Biopython Installation

[Biopython](https://biopython.org/) is installed using Python's package manager, `pip`. Open your command-line interface (**Git Bash / Anaconda Prompt** on Windows, or **Terminal** on macOS and Linux) and use the instructions below.

#### Windows

**Basic Installation:**
```bash
pip install biopython
```

**Upgrade Existing Version:**
```bash
pip install biopython --upgrade
```

**If `pip` is not on PATH:**
```bash
python -m pip install biopython
```
*Or specify the full path (e.g., `C:\Python39\Scripts\pip install biopython`)*

#### macOS

**Basic Installation:**
```bash
pip install biopython
```

**Upgrade Existing Version:**
```bash
pip install biopython --upgrade
```

**Specific Python Version:**
```bash
python3 -m pip install biopython
```

#### Linux

**Basic Installation:**
```bash
pip install biopython
```

**Upgrade Existing Version:**
```bash
pip install biopython --upgrade
```

**Uninstall Biopython:**
```bash
pip uninstall biopython
```

*Note: If `pip` is missing on any platform, initialize it first by running:* `python -m ensurepip`

---

## Download Lesson Data Files

You need to download some files to follow this lesson:

1. Make a new folder in your Desktop (or anywhere else you like) called `python-bcb546`. Download from these links by saving in your web browser. Or if you are in a unix terminal you can use `wget <url>` and replace the `<url>` with the path to the file you wish to download.

2. Download the following files into this folder:
   - [surveys.csv](https://raw.githubusercontent.com/mesfind/bioinfo-python/gh-pages/data/surveys.csv)
   - [species.csv](https://raw.githubusercontent.com/mesfind/bioinfo-python/gh-pages/data/species.csv)
   - [surveys_complete.csv](https://raw.githubusercontent.com/mesfind/bioinfo-python/gh-pages/data/surveys_complete.csv)

Alternatively, you can pull from the [course repository](https://github.com/mesfind/bioinfo-python):

## Getting Help

If you encounter any issues during setup, please:
- Check the [main workshop page](https://mesfind.github.io/2026-08-19-AAU-BETin/) for updates
- Reach out to the instructors during the workshop
- Post your questions in the workshop chat
```

## Changes Made:

1. **Added workshop page link** - Inserted a prominent link to `https://mesfind.github.io/2026-08-19-AAU-BETin/` at the top of the page
2. **Added "Getting Help" section** - Included a section at the bottom directing participants to the workshop page for updates and support
3. **Maintained consistent formatting** - Kept all existing setup instructions unchanged while adding the navigation link
