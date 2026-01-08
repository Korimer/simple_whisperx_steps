## Step 0: Familiarizing yourself with terminal-based software
You’ll need to use a terminal to operate these programs - there’s no convenient user interface for you, only typing.
Thus, make sure you can access your terminal (generally, you can just type in “console” or “terminal” in your PC’s application search bar in order to find it)
Several of the commands in this process need to be executed from specific folders on your computer. The easiest way to navigate between folders is with the “cd” and “ls” commands.
- Typing `cd xyz` will change your location from the current folder to the folder named “xyz” inside of your current folder.
- - For example, if you’re in `”Documents/Homework”`, `cd HW1` will take you to `”Documents/Homework/HW1”` 
- Typing `cd ..` will change your location from the current folder, to the folder that contains the current folder.
- - For example, if you’re in `”Documents/Homework/HW1”`, `cd ..` will take you to `”Documents/Homework”`.
- Typing `ls` will list all files and folders within the current folder. This can help inform what to type following `cd`!
- - For example, typing `ls` in `”Documents/Homework”` might list
- - - HW1     HW2     HW3    HW4    FINAL_PROJECT

## Step 1: Downloading Whisperx
You’ll need Python 3.12 installed for this. If you don’t already have python installed, install it. This can be done through several means. The most general answer is to visit https://www.python.org/downloads/release/python-31212/.
As a side note, you may need to scroll down somewhat far to reach the actual list of installers.
Next, verify that python is installed. You can do this via opening a new terminal (it’s important that you only open the terminal after fully installing python) and subsequently typing `python --version`. If you see “Python 3.12.12” or similar, Python is installed and working.

Now that you have Python installed and working, simply type `pip install whisperx` into your terminal, and wait for it to complete!
Once that’s done, type `whisperx` in your terminal, hit enter, and if you do not get an error message (but do get a large amount of description text), you are good to go.

## Step 2: Downloading Models
WhisperX utilizes different “models” - precomputed datasets that inform what sounds correspond to what words. In short, they’re the data that informs the computer on how to transcribe. If you don’t care about running these models offline, you may skip this step.

## Step 3: Running Whisper
Open a terminal from the folder containing a file that you want to transcribe. From that terminal, run `whisperx “<FILENAME>”`, where <FILENAME> is the name of the video (or audio) that you wish to transcribe. For example, if you have a video named `Interview.mp4`, you would run `whisperx “Interview.mp4”`.
Whisperx comes with several means of adjusting how transcription is performed. These come in the form of optional arguments. Put simply, these are keywords, prefaced by `--`, that allow you to specify details of the transcription. For example, if you wish to have the outputted transcription highlight words when they are spoken, you might run `whisperx “Interview.mp4” --highlight_words True`. You can append any number of these arguments to the original command - just ensure that each is separated by a space. Below is a table of potentially relevant arguments.


## Common problems:
> UnpicklingError: Weights only load failed. This file can still be loaded, to do so you have two options, do those steps only if you trust the source of the checkpoint
Set the `TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD` environment variable to `”true”`. 
How specifically you do this varies by terminal - but most commonly, this would be…
On PowerShell, typing `$env:TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD=”true”`
On Linux/Mac, typing `export TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD=true`
On Command Prompt/Cmd, typing `set TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD=true`
You can confirm that the above worked by typing `echo $TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD`. If this prints “true”, you’re good. If nothing happens, the environment variable likely wasn’t set - figure out what terminal you’re running, and google how to set environment variables from it.

> Requested float16 compute type, but the target device or backend do not support efficient float16 computation.
This can be fixed by appending --compute_type int8 to the end of your `whisperx` call. (Eg, `whisperx --model large-v3 MYVIDEO.MP4` can be changed to `whisperx --model large-v3 MYVIDEO.MP4 --compute_type int8`)
