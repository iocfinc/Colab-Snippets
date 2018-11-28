# Common Commands for Colab

I am creating this for easy reference on different commands when using Google Colab for notebooks.

## File Operations

### Linking to Google Drive

The code block below would mount a Google Drive on the location `/content/gdrive`. This is helpful when we need to access files from our Drive (i.e. Datasets or helper files). Running this code block will ask for the user to authenticate a Google account and allow access to files between drive and colab. I think the Google Drive that will be selected is the one linked to the account.

```python
from google.Colab import drive
drive.mount('/content/gdrive')
```

### Doing commands inside Colab

Adding `!` to a command like `dir` or `cd` will allow you to run that command while inside the notebook. This would be useful when we have to check or do some operations in the directory. The `!` is, from what I understand, used when doing commands in the current virtual env. On the other hand, `%` is more powerful in a sense that it will do a command in the actual terminal of the session.

```python
! # Before any command
# - or -
% # Before any command
```

Use case would be `!` for commands like `ls`, `wget`. Once we have to change the location inside the virtual session, we have to use `%`. An example would be `% cd content/gdrive/<sub-directory>`. Simply using `! cd` will not change the actual working directory. This would be useful when you want to point to a directory where the helper files are located.

### Installing additional modules

Similar to using anaconda prompt or the terminal, we can also use pip to install additional modules to our session. I have yet to find a list of default available modules in Colab. From what I have done before, it looks like the basics are already in the default Colab environment with the addition of TensorFlow.

```python
! pip install <module>
```

Should there be any issues using the simple `! pip install` command, I suggest looking at StackOverflow for possible issues or on how to install a module.

To add to this, every time the session in Colab is ended all the temporary files that were linked to that session would be removed. So Google Drive would be un-mounted and modules would return to default. **TLDR: Make sure to Change RunType Instance to GPU at the very start :grinning:**

### Cloning a GitHub Repo

If you want to clone a repo from GitHub to Drive, it can be done with the code block below.

```python
% cd <Desired location/Pref. in Drive>
!pwd
% git clone <GitHub Repo URL>
```

I have not yet tried using other git commands while in Colab. I have not tried to do `git push` via Colab-Drive (I did try but it did not work. :joy:)

### Downloading files

We can download files to our Drive (assuming that it is mounted) via `wget` command. This will automatically get download the file from the link given to our present working directory. *Make sure that the pwd is correct before installing the file*

```python
! wget <url>
```

We can also download files via `! curl` command. This has more options than `wget` and quite versatile. Do note that like normal commands spaces are considered delimiters so if you need to have spaces in the file name add the `\`. For example, `file name` should be `file\ name`.

```python
!curl -o <file name> <url>
```

These files should still be visible after you restart a session provided that they were downloaded to Google Drive and not in the virtual directory.

Also in terms of downloading individual files from GitHub (like helper files), it is important that we select the `Raw` option first before copying the link. Simply copying the link will try to pull the webpage instead of the actual file contents.

## Miscellaneous Commands

### Checking CUDA

The code below should output a `True` or `False` response based on the availability of CUDA compute in the session. This is useful in checking that we have enabled the GPU option in Colab.

```python
use_cuda = True
if use_cuda and torch.cuda.is_available():
  net.cuda()
print(use_cuda and torch.cuda.is_available())
```

Try to put this after importing torch, save yourself the hassle of reinstalling every module because the GPU instance was OFF in the first place.
