---
title: "Before the Workshop: Setup"
---

This page tells you everything you need to do before the workshop.
You will run the workshop on your own laptop, so you need to install two free programs and download the workshop files.

The whole process takes about 20 minutes. Please do it before the day — installing software is the part most likely to hit a snag, and it is much easier to fix before the workshop starts.

---

## Step 1: Install VS Code

**VS Code** (Visual Studio Code) is a free program for writing and running code. We use it to open and run the workshop notebooks. A **notebook** is a document made up of cells — blocks of text or code that you can run one at a time.

1. Go to [code.visualstudio.com](https://code.visualstudio.com)
2. Download and install the version for your computer (Windows or Mac)
3. Open VS Code

Next, add two extensions. An **extension** is an add-on that gives VS Code extra features.

1. In VS Code, click the **Extensions** icon in the far-left bar (it looks like four small squares)
2. Search for **Python** and install the one published by **Microsoft**
3. Search for **Jupyter** and install the one published by **Microsoft**

---

## Step 2: Install uv

**uv** is a free tool that sets up Python and all the packages the workshop needs. A **package** is a bundle of ready-made code that someone else has written. You do not need to install Python separately — uv downloads the right version for you.

You install uv by typing one command into a **terminal** — a window where you type instructions to your computer instead of clicking.

:::::::::::::::: spoiler

### Mac

1. Open the **Terminal** app (press **Cmd + Space**, type `Terminal`, and press Enter)
2. Copy this line, paste it into the Terminal, and press Enter:

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

::::::::::::::::::::::::

:::::::::::::::: spoiler

### Windows

1. Open **PowerShell** (click the Start menu, type `PowerShell`, and press Enter)
2. Copy this line, paste it into PowerShell, and press Enter:

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

::::::::::::::::::::::::

When it finishes, **close the Terminal or PowerShell window**. uv only works in windows opened after it was installed.

---

## Step 3: Download the workshop files

1. [Download the workshop as a ZIP file](https://github.com/TD-Johnson/embedding-llms-unstructured-data-workshop/archive/refs/heads/main.zip)
2. **Unzip it.** On Windows, right-click the ZIP file and choose **Extract All**. On a Mac, double-click it.
3. Move the unzipped folder somewhere easy to find, such as your **Documents** folder

The folder is called `embedding-llms-unstructured-data-workshop-main`.

:::::::::::::::::::::::::::::::::::::::: callout

### Unzip first

On Windows you can look inside a ZIP file without unzipping it. The workshop will not work from inside the ZIP file. Make sure you use the unzipped folder.

::::::::::::::::::::::::::::::::::::::::::::::::

---

## Step 4: Set up the workshop in VS Code

1. In VS Code, choose **File → Open Folder** and select the unzipped workshop folder
2. If VS Code asks **"Do you trust the authors of the files in this folder?"**, click **Yes, I trust the authors**
3. Open a terminal inside VS Code: choose **Terminal → New Terminal**. It appears at the bottom of the window.
4. Type this command and press Enter:

```bash
uv sync
```

uv downloads Python and the workshop packages. This takes a minute or two. When it finishes, a new folder called `.venv` appears in the file list on the left. This folder holds everything the notebooks need. You do not need to open it.

---

## Step 5: Check that it works

1. In the file list on the left, open the `notebooks` folder and click `01-environment-setup.ipynb`
2. Scroll down to **Exercise 1** and click the code cell that says `2 + 3`
3. Press **Shift + Enter** to run it
4. VS Code asks you to **select a kernel**. A kernel is the program that runs your Python code. Choose **Python Environments**, then the one called **.venv**.

If you see the number `5` below the cell, you are ready. You do not need to go any further in the notebook — we will do the rest together on the day.

:::::::::::::::::::::::::::::::::::::::: callout

### Something went wrong?

Check these things in order:

1. **`uv` is not recognised, or `command not found`.** Close VS Code completely and open it again, then retry Step 4. If that does not work, repeat Step 2.
2. **You cannot see `.venv` when selecting a kernel.** Check that you ran `uv sync` in Step 4 and that it finished without errors. Then close VS Code, open it again, and retry.
3. **VS Code opened the notebook as plain text.** Check that the Python and Jupyter extensions from Step 1 are installed.

If none of these help, bring the error message to the workshop and we will fix it together in the first 5 minutes.

::::::::::::::::::::::::::::::::::::::::::::::::

---

## Step 6: Check you can connect to the university VPN

The LLM we use runs on university computers. To reach them, your laptop must be connected to the university **VPN** (virtual private network) — a secure connection into the university network.

Before the workshop, check that you can connect to the university VPN from your laptop. On the day, connect to it before you start.

You do not need an API key before the workshop. Your instructor will give you one on the day, and you will set it up in notebook 01.

---

## On the day: the first 10 minutes

At the start of the workshop, before any content begins, the instructor will say:

> **"Here's the only things you need to know today."**

They will then live-demonstrate four things in the notebook — not to teach you Python,
but to show you how a notebook behaves. This takes about 8 minutes.

**1. Running a cell**
Click a cell, press Shift+Enter. It runs. That's it.

**2. Changing a value**
Change a word or a number in the cell, run it again. The output updates.
The notebook does exactly what you tell it.

**3. Triggering an error — on purpose**
The instructor will break something deliberately and read the error message aloud.
The last line of the error message tells you what went wrong.
They will then fix it.

This is the most important minute of the whole workshop. Errors are not failures.
They are information. Every programmer sees them constantly. Reading the last line
and fixing it is the whole skill.

**4. Seeing inside the notebook**
The instructor will show a `print()` statement — a way to look at the value of
any variable at any point. If you are ever unsure what your code is doing, print it.

After these four things, you know enough to follow every exercise in the workshop.
