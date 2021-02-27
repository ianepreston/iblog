---
title: "How to work with conda environments in shell scripts and Makefiles"
description: "conda environments are a little tricky to incorporate into automation. This post outlines a bit of how they work, and how to integrate them in scripts and makefiles."
layout: post
toc: true
comments: true
hide: false
categories: [conda, python, bash]
---

I've struggled with automating working with the ```conda``` python environment manager for a while. It's a relatively small part of my work flow so I haven't made figuring it out a top priority, but it's really bugging me. In this post I'm going to document the problem and all the troubleshooting steps I went through to resolve it. I'm writing this post in parallel with actually resolving the issue, so it's going to be a bit stream of consciousness.

## Setting up the problem

I have conda installed. I'm using bash as my shell. I've run ```conda init bash``` and my version of conda is new enough that this works nicely in interactive mode.

Before I go further describing the problem, I should note which version of conda I'm running, since I know a lot has changed with how it sets itself up, and more may change in the future. This guide is built using Miniconda on Windows 10, running ```conda 4.8.3```.

When I open a new bash terminal I can see from my prompt that I'm in the ```base``` environment and I can run ```conda activate example_env``` to activate to activate an environment called ```eg_env```. I can get back to ```base``` with ```conda deactivate```. Now let's say I've written a python script that's intended to be run in that environment. I might want to write a shell script to run that file, or use a Makefile to use that script as part of a larger pipeline. To demo and work through this I'm going to make a small conda environment, and a small python script that will only work if I'm in that python environment.

Here's my ```env.yml```:

```yml
name: eg_env
dependencies:
        - numpy
```

Now I create a simple python script, which I should be able to run without issue from that environment, but not from base (I'm using Miniconda so my base env is quite sparse).

Here's the ```eg.py``` file:

```python
import numpy as np
print(np.__version__)
```

Just to be sure I'll try running it from base. I get the following error:

```bash
$ python eg.py
Traceback (most recent call last):
  File "eg.py", line 1, in <module>
    import numpy as np
ModuleNotFoundError: No module named 'numpy'
```

Running it from my active environment is no problem:

```bash
$ python eg.py
1.18.1
```

Excellent, environment management works! But what if I want to call this from another scripts? This is clearly contrived in this example but there are certainly situations where I might want to do this, and if nothing else it will demonstrate a bit more about how conda works.

So first off, here's a bash script that just runs the python program:

```bash
#!/bin/bash

echo "This is my test bash script"
python eg.py
```

If I run this script from my base environment, it behaves basically the same as before:

```bash
$ ./eg.sh
This is my test bash script
Traceback (most recent call last):
  File "eg.py", line 1, in <module>
    import numpy as np
ModuleNotFoundError: No module named 'numpy'
```

Similarly, if I run it from my example environment it runs just fine:

```bash
$ ./eg.sh
This is my test bash script
1.18.1
```

Cool. OK, but say I want my script to handle activating the environment for me? Let's modify it and see what happens:

```bash
#!/bin/bash

echo "This is my test bash script"
echo "Activating conda environment"
conda activate eg_env
echo "Running python script"
python eg.py
```

Running this from my base environment I get:

```bash
$ ./eg.sh
This is my test bash script
Activating conda environment

CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
.
.
.
Running python script
Traceback (most recent call last):
  File "eg.py", line 1, in <module>
    import numpy as np
ModuleNotFoundError: No module named 'numpy'

```

## What's going on?

There's actually some guidance in the full error message from the attempt above. The relevant section is this:

```bash
CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.

To initialize your shell, run

    $ conda init <SHELL_NAME>
```

I've already done that for my shell, but apparently it doesn't apply to subshells launched from that shell. I can't just put ```conda init bash``` in the script, because you need to restart your shell for it to be applied.

As far as I know, all running ```conda init bash``` did for me was add a line to my ```.bash_profile``` file:

```bash
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
eval "$('/C/ProgramData/Miniconda3/Scripts/conda.exe' 'shell.bash' 'hook')"
# <<< conda initialize <<<
```

## Possible solution

What happens if I just put that at the top of the script?

Bash script updated:

```bash
#!/bin/bash

echo "This is my test bash script"
echo "Activating conda environment"
eval "$('/C/ProgramData/Miniconda3/Scripts/conda.exe' 'shell.bash' 'hook')"
conda activate eg_env
echo "Running python script"
python eg.py
```

Output:

```bash
$ ./eg.sh
This is my test bash script
Activating conda environment
Running python script
1.18.1
```

It worked! Notably, I'm still in my base environment when the script exits. The activation only happens in the subshell. I also tested running this script from a prompt that was already in that environment and it ran fine as well.

## Possible problem and solution

Ok, this works as long as I'm either the only person trying to use this script, or everyone else is also running Miniconda on Windows from a system level install. That seems pretty fragile. Can we improve this?

The ```which conda``` command returns the path to your conda install, so I should be able to replace the absolute path with what that returns to get the same result:

```bash
#!/bin/bash

echo "This is my test bash script"
echo "Activating conda environment"
eval "$($(which conda) 'shell.bash' 'hook')"
conda activate eg_env
echo "Running python script"
python eg.py
```

This works too! Getting pretty close to a nice solution.

## Ok, how about MakeFiles?

Because I am fancy, I like to have a MakeFile for my projects, which I can then use to run scripts or series of commands with nice convenient shortcuts. Most of what I do could definitely be accomplished with pure shell scripting, but it will be a little nicer if I can do it with Make, so let's try.

To make this a little more realistic I'm going to say there are two things I might want to do using ```make``` in this project. One might be to format the python file with ```black``` and the other might be to run the file. I'll add ```black``` to my example environment, but not my base, which means I'll have to have the environment activated to format or run the script. I'd like to be able to run the python script directly from the makefile, or run it through a shell script.

In the same folder as my example python and shell scripts I'll add a ```Makefile```, based off the top answer on [This stackoverflow question](https://stackoverflow.com/questions/53382383/makefile-cant-use-conda-activate):

```bash
# Oneshell means I can run multiple lines in a recipe in the same shell, so I don't have to
# chain commands together with semicolon
.ONESHELL:
# Need to specify bash in order for conda activate to work.
SHELL=/bin/bash
# Note that the extra activate is needed to ensure that the activate floats env to the front of PATH
CONDA_ACTIVATE=source $$(conda info --base)/etc/profile.d/conda.sh ; conda activate ; conda activate

test:
    $(CONDA_ACTIVATE) eg_env
    echo $$(which python)

# format the file with black
lint:
    $(CONDA_ACTIVATE) eg_env
    black eg.py

# Run the file directly
run_py:
    $(CONDA_ACTIVATE) eg_env
    python eg.py

# Run the file from a shell script
run_sh:
    $(CONDA_ACTIVATE) eg_env
    bash eg.sh
```

I'm also going to update ```eg.sh``` to remove the environment activation I set up earlier, because the idea is that ```make``` should be handling this for me now.

All of these work! I'm a little bummed I have to put that ```$(CONDA_ACTIVATE) eg_env``` at the top of each recipe, but that's still pretty solid. I tried adding an ```active``` recipe and setting it as a requirement for the other recipes like so:

```bash
active:
    $(CONDA_ACTIVATE) eg_env

test: active
    echo $$(which python)
```

But the activation from ```active``` didn't persist into running ```test```. Maybe there's a way to get that working but I'm going to call this close enough for my needs.


## Conclusion

Getting conda activation to work from within bash and Makefiles is a finicky process. Or at least I don't have a strong enough understanding of subshells, environment variables conda and probably some other things to make it seem otherwise. That said, the steps outlined in this guide should allow you to automate processes involving conda environments without too much hassle.

## The final scripts

For reference, here's the final form of what I used to setup and test this demo:

### Makefile

```bash
# Oneshell means I can run multiple lines in a recipe in the same shell, so I don't have to
# chain commands together with semicolon
.ONESHELL:
# Need to specify bash in order for conda activate to work.
SHELL=/bin/bash
# Note that the extra activate is needed to ensure that the activate floats env to the front of PATH
CONDA_ACTIVATE=source $$(conda info --base)/etc/profile.d/conda.sh ; conda activate ; conda activate

test:
    $(CONDA_ACTIVATE) eg_env
    echo $$(which python)

# format the file with black
lint:
    $(CONDA_ACTIVATE) eg_env
    black eg.py

# Run the file directly
run_py:
    $(CONDA_ACTIVATE) eg_env
    python eg.py

# Run the file from a shell script
run_sh:
    $(CONDA_ACTIVATE) eg_env
    bash eg.sh
```

### env.yml

```yml
name: eg_env
dependencies:
    - numpy
    - black
```

### eg.py

```python
import numpy as np

print(np.__version__)
```

### eg.sh (for makefile)

```bash
#!/bin/bash

echo "This is my test bash script"
echo "Running python script"
python eg.py
```

### eg.sh (standalone)

```bash
#!/bin/bash

echo "This is my test bash script"
echo "Activating conda environment"
eval "$($(which conda) 'shell.bash' 'hook')"
conda activate eg_env
echo "Running python script"
python eg.py
```
