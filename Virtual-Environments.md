# Virtual environments

It is of vital importance that you never install libraries directly at the system level. Linux, for example, relies on Python for many different tasks and operations, and if you fiddle with the system installation of Python, you risk compromising the integrity of the whole system. So, take this as a rule, such as brushing your teeth before going to bed: always create a virtual environment when you start a new project.

When it comes to creating a virtual environment on your system, there are a few different methods to carry this out. As of Python 3.5, the suggested way to create a virtual environment is to use the ``venv`` module.

## Your first virtual environment

It is very easy to create a virtual environment, but according to how your system is configured and which Python version you want the virtual environment to run, you need to run the command properly. Another thing you will need to do, when you want to work with it, is to activate it. Activating virtual environments basically produces some path juggling behind the scenes so that when you call the Python interpreter from that shell, you're actually calling the active virtual environment one, instead of the system one.

We will.

1. Open a terminal and change into the folder (directory) we use as root for our projects (our folder is ``Python``). We are going to create a new folder called ``myProject`` and change into it.
2. Create a virtual environment called ``lpp3ed``.
3. After creating the virtual environment, we will activate it. The methods are slightly different between Linux, macOS, and Windows.
4. Then, we will make sure that we are running the desired Python version (3.11.X) by running the Python interactive shell.
5. Deactivate the virtual environment.

These steps are all you need to start a project.

```bash
    C:\Temp\Python>md myProject
    C:\Temp\Python>cd myProject
    where python
```

Returns:

> C:\Users\username\AppData\Local\Programs\Python\Python39\python.exe       
> C:\Users\username\AppData\Local\Microsoft\WindowsApps\python.exe

```bash
    python -m venv lpp3ed
```

if you do an ``ls`` you will see a folder.

> lpp3ed

To activate your virtual environment.

```bash
    lpp3ed\Scripts\activate
```

This activates the virtual environment, now check where Python is installed again.

```bash
    where python
```

You should get.

> C:\Users\alanr\AppData\Local\Programs\Python\Python39\python.exe      
> C:\Users\alanr\AppData\Local\Microsoft\WindowsApps\python.exe

Now you can run Python.

```bash
    (lpp3ed) C:\Temp\Python\myProject>python
```

this should produce.

> Python 3.11.0 (main, Oct 24 2022, 18:26:48) [MSC v.1933 64 bit (AMD64)] on win32      
> Type "help", "copyright", "credits" or "license" for more information.        
> \>>>

You are now in a working environment.

To close down a working environment.

```bash
    >>>quit()
```

Then.

```bash
    (lpp3ed) C:\Temp\Python\myProject>deactivate
```

you should then see the prompt.

> C:\Temp\Python\myProject>

The process for Linux is slightly different to this.

**Note:** This only becomes important to do when you are installing third-party libraries. When we are learning the basics of Python virtual environments won't be needed.

## Installing third-party libraries

**Note:** In order to install third-party libraries, we need to use the Python Package Installer, known as pip. Chances are that it is already available to you within your virtual environment, but if not, you can learn all about it on its documentation page: <https://pip.pypa.io>.

The following example shows how to create a virtual environment and install a couple of third-party libraries taken from a requirements file.

```bash
    md my-project
```

cd into your new project and run.

```bash
    python3.9 -m venv lpp3ed
```

Activate your virtual environment.

```bash
    lpp3ed\Scripts\activate
```

**Important:** you should see the following prompt that shows you are using a virtual environment.

> (lpp3ed) C:\Temp\Python\my-project>

We have a ``requirements.txt`` file that contains the following.

> Django==3.1.7     
> requests==2.25.1

The following instruction shows how to use ``pip`` to install requirements from a file.

```bash
    pip install -r requirements.txt
```

Once you have installed the third-party libraries you should be able to see what you have installed in the ``lpp3ed\libraries`` folder.

To finish working in your virtual environment.

```bash
    (lpp3ed) C:\Temp\Python\my-project>deactivate
```
