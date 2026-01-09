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
Whisperx utilizes different “models” - precomputed datasets that inform what sounds correspond to what words. In short, they’re the data that informs the computer on how to transcribe. If you don’t care about running these models offline, you may skip this step.

First, visit https://huggingface.co/collections/Systran/faster-whisper and pick a model to download. For best results, I'd suggest faster-whisper-large-v3. On a weaker computer, I'd instead reccomend faster-whisper-small. On an extremely weak system, such as a personal laptop, go with faster-whisper tiny. However, note that the smaller the model you choose is, the less accurate the results are. The trade-off is, of course, that larger models demand far more processing power to utilize.

Once you have selected the model you want to use, note its full name. (Eg, "Systran/faster-whisper-large-v3" rather than "faster-whisper-large-v3""). Then, in a terminal, run the following command.

  `python -c "import huggingface_hub; huggingface_hub.hf_hub_download(repo_id='<FULL_MODEL_NAME>',filename='model.bin')`
  
Note that in the above example, you are expected to replace `<FULL_MODEL_NAME>` with the name mentioned above. The resulting command should not have `<>` brackets; those are only to denote what you're supposed to replace.

Once this command completes successfully, the model is downloaded for good. You will not need to connect to the internet to utilize the model you downloaded - any transcriptions using it can be preformed entirely offline.

## Step 3: Running Whisperx
Open a terminal from the folder containing a file that you want to transcribe. From that terminal, run `whisperx “<FILENAME>”`, where <FILENAME> is the name of the video (or audio) that you wish to transcribe. For example, if you have a video named `Interview.mp4`, you would run `whisperx “Interview.mp4”`.

Whisperx comes with several means of adjusting how transcription is performed. These come in the form of optional arguments. Put simply, these are keywords, prefaced by `--`, that allow you to specify details of the transcription. For example, if you wish to have the outputted transcription highlight words when they are spoken, you might run `whisperx “Interview.mp4” --highlight_words True`. You can append any number of these arguments to the original command - just ensure that each is separated by a space. Below is a table of potentially relevant arguments.

| Argument           | Description                                                                                                                                                                                                                                       | Options                                                                                         |
|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| --model            | Which model to use. Note that this should be the model you downloaded in the above step. Names here do not include the "Systran/faster-whisper-" prefix. Thus, "Systran/faster-whisper-large-v3" would turn into "large-v3", for example.         | "large-v3", "small", "tiny", "base", etc...                                                     |
| --model_cache_only | Whether or not to automatically download missing models from online. False by default. If you want to run whisperx offline, set it to True.                                                                                                       | True/False                                                                                      |
| --output_dir       | Determines where to save transcriptions to.                                                                                                                                                                                                       | A file path. Eg, "./Transcriptions"                                                             |
| --output_format    | What format to save transcriptions in. srt is most common for video captions.                                                                                                                                                                     | "all", "srt", "vtt", "txt", "tsv", "json", or "aud". I'd suggest either "all", "txt", or "srt". |
| --language         | What language you're transcribing. Though not strictly necessary, specifying English will make whisperx run faster.                                                                                                                               | "en","es","et","English", "Spanish", "Estonian"... The list goes on.                            |
| --best_of          | How many times to transcribe the audio. Increasing this number will dramatically slow whisperx down, but will make transcriptions more accurate.                                                                                                  | any number 1 or greater. I'd suggest no more than 3.                                            |
| --threads          | How many CPU threads to use on transcription. The default is 4. Increasing this number will make whisperx faster, but will cause problems if you increase it above how many threads your CPU has. (If you don't know, just ignore this argument.) | any number 1 or greater.                                                                        |
| --verbose          | Whether or not to print extraneous details while it's transcribing audio. True by default, but can be False if you want to see less visual clutter.                                                                                               | True/False                                                                                      |
| --highlight_words  | Whether or not to highlight individual words as audio is being transcribed. False by default. Setting it to True will make resulting files look prettier when used as subtitles, but far more difficult for humans to read.                       | True/False                                                                                      |
| --print_progress   | Whether or not to log how close to completing a full transcription Whisper is. False by default. If you want to be able to eyeball progress, there's no harm in setting this to True.                                                             | True/False                                                                                      |

## Step 4: Running Diarization
Diarization is a formal term for telling speakers apart. While this is an incredibly helpful feature for transcriptions (as it means you can identify who is talking and when), it requires a seperate model that is somewhat more diffiuclt to obtain. The first step to obtaining it is setting up a huggingface account. This can be done by following this https://hf.co/join link.

Next, you need to create an access token. This can be done via the following guide: https://huggingface.co/docs/hub/en/security-tokens. It is very important that when given the option, you check the check box that reads "Read access to contents of all public gated repos you can access".

Finally, you'll need to visit https://huggingface.co/pyannote/speaker-diarization-3.1, and accept the given terms in order to be able to access the repository.

Once all of this is done, you can finally download the model. This can be done by typing, in a terminal,
```
python -c "import pyannote.audio; import torch; pyannote.audio.Pipeline.from_pretrained('pyannote/speaker-diarization-3.1', token='<YOUR_HUGGINGFACE_TOKEN>').to(torch.device('cuda' if torch.cuda.is_available() else 'cpu'))"
```
The reason this command is so verbose is because there exists different diarization models for different hardware. The two most common means of running these models is either via cuda (which is far faster, but requires an NVIDIA GPU), or via cpu (which is far slower, but works on any device at all). The command above, which can be copy-pasted, downloads a cuda model if cuda is available, and a cpu model otherwise.

Now that everything is downloaded, to actually diarize the results of your transcription, include the following arguments in your command: `--diarize --min_speakers <NUMBER> --max_speakers <NUMBER>`, replacing <NUMBER> with the respective minimum/maximum number of speakers in a clip. These should be somewhat self-explanatory - min_speakers is the fewest number of people who might speak in the audio, and max_speakers is the max (e.g. if you know at least two different people speak, set min_speakers to 2). If you know exactly how many people will speak in the audio, you can set min_speakers and max_speakers to be the same number.

## Putting it All Together
This is a simpler segment to provide some example commands.

Say you wish to transcribe a meeting to a simple text document so that you can send the textual transcription to a friend. The meeting has 10 people in it, but you're not certain that everyone speaks at least once. A good command to transcribe this could be...

```whisperx "Meeting.mp4" --output_format "txt" --diarize --min_speakers 4 --max_speakers 10```

Say you're transcribing an interview between two people, and you intend to use the transcription to caption the interview. Since these will be professional captions, accuracy is important. A good command to transcribe this could be...

```whisperx "Interview.mp4" --output_format "srt" --highlight_words True --best_of 3 --model "large-v3"```

Say you're transcribing an extremely long segment of audio, and want to check in regularly to see how much progess has been made on the transcription. You're running it on a good CPU, so you know you have 16+ threads. A good command to transcribe this could be...

```whisperx "LongAudio.mp3" --output_format "all" --language "en" --threads 12 --verbose False --print_progress True```

As you can see, there's no limit to how many arguments you include in a whisperx command. As long as you match the format as demonstrated above, you could specify as many arguments as you want. (And in any order, too. Don't worry about whether `--threads` comes before `--verbose` or anything like that.) In fact, for the purpose of customizing a transcription to best suit your needs, I'd strongly encourage you to mix and match arguments!

## Common problems: 
> UnpicklingError: Weights only load failed. This file can still be loaded, to do so you have two options, do those steps only if you trust the source of the checkpoint

Set the `TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD` environment variable to `”true”`.
How specifically you do this varies by terminal - but most commonly, this would be…
- On PowerShell, typing `$env:TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD=”true”`
- On Linux/Mac, typing `export TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD=true`
- On Command Prompt/Cmd, typing `set TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD=true`
You can confirm that the above worked by typing `echo $TORCH_FORCE_NO_WEIGHTS_ONLY_LOAD`. If this prints “true”, you’re good. If nothing happens, the environment variable likely wasn’t set - figure out what terminal you’re running, and google how to set environment variables from it.

> Requested float16 compute type, but the target device or backend do not support efficient float16 computation.

This can be fixed by appending --compute_type int8 to the end of your `whisperx` call. (Eg, `whisperx --model large-v3 MYVIDEO.MP4` can be changed to `whisperx --model large-v3 MYVIDEO.MP4 --compute_type int8`)
