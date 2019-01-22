# `p` - Python version management, simplified.

![introduction](https://user-images.githubusercontent.com/8281591/51531617-ed709d00-1e78-11e9-9da1-d897adc54a03.png)

`p` is powerful and feature-packed, yet simple, both in setup and use. There are no tricky settings, options, or crazy dependencies. `p` is just a helpful ~600 line Bash script that gets the job done, and was heavily inspired by [`n`, a version manager for Node.js](https://github.com/tj/n).

`p` is also great for getting started using Python development versions. Use `p latest` to get up and running with the latest development version of Python!

## Installation

Download and execute, simple!

```
curl -sSLo p https://raw.githubusercontent.com/Raphx/p/master/bin/p
chmod u+x p
./p
```

Python versions are installed to the `$P_PREFIX` directory. Example of installing to home directory and adding the python binaries to `$PATH`:

```
# In ~/.bash_profile, or the equivalent

export P_PREFIX="$HOME/.python"
export PATH="$P_PREFIX/p/versions/bin:$PATH"
```

## Usage

```
Usage: p [COMMAND] [args]

Commands:

p                              Output versions installed
p ls                           Output the versions of Python available
  p ls latest                  Output the latest Python version available
  p ls stable                  Output the latest stable Python version available
p <version>                    Activate to Python <version>
  p latest                     Activate to the latest Python release
  p stable                     Activate to the latest stable Python release
p status                       Output current status
p use <version> [args ...]     Execute Python <version> with [args ...]
p bin <version>                Output bin path for <version>
p rm <version ...>             Remove the given version(s)
p default                      Use default (system) Python installation

Options:

-V, --version   Output current version of p
-h, --help      Display help information
```

`p`

Executing `p` without any arguments displays a list of installed Python versions, with an indicator of the currently activated version.

```
$ p

    2.7.14
    3.3.4
  ο 3.6.5
```

`p ls [latest|stable]`

List all Python versions available for install. Use `latest` or `stable` to show the latest and stable version respectively.

```
$ p ls

# --snip--

    2.7.12
    2.7.13
    2.7.14
    3.0.1
    3.1.1
    3.1.2
    3.1.3
    3.1.4
    3.1.5
    3.2.1
    3.2.2
    3.2.3
    3.2.4
    3.2.5
    3.2.6
    3.3.0

# --snip--
```

`p [<version>|latest|stable]`

Install and activate the specified Python version. The `latest` or `stable` argument can be used to quickly install the latest or stable version respectively.

```
$ p 3.3.4

     install : Python-3.3.4
       fetch : https://www.python.org/ftp/python/3.3.4/Python-3.3.4.tgz
   configure : 3.3.4
     compile : 3.3.4

  Success: Installed Python 3.3.4!
```

`p status`

Show the version, bin path, and status of the currently activated version.

```
$ p status
     version : 3.6.5
         bin : /home/raphx/.python/p/versions/python/3.6.5/bin/python
      latest : no
      stable : yes
```

`p use`

Quickly use the specified version to execute a one-off command, even when the version is not activated.

```
$ p use 2.7.14 -c "import sys; print sys.version"
2.7.14 (default, Apr  5 2018, 22:47:52)
[GCC 7.3.1 20180312]
```

`p bin`

Output the bin path for the currently activated version.

```
$ p bin
/home/raphx/.python/p/versions/python/3.6.5/bin/python
```

`p rm`

Remove an installed version. Multiple versions can be supplied by using space character as the delimiter. The `rm` subcommand also accepts `stable` and `latest` identifier.

```
$ p rm 2.7.14
      remove : 2.7.14

  Success: Removed Python 2.7.14!
```

`p default`

Remove the symlinks created by `p`, falling back to default or system installed Python.

```
$ p default
    activate : default

  Success: Now using default system Python!
```

## How does `p` work?

`p` installs each Python version to the directory `$P_PREFIX/p/versions/python`. When a version is activated, symlinks to the `bin`, `include`, `lib`, and `share` directories of the activated Python version are created in `$P_PREFIX/p/versions`.

For example, with versions 2.7.15 and 3.6.8 installed, and the latter activated:

```
# tree .python/p/versions/ -L 3 -U

.python/p/versions/
├── include -> /home/raphx/.python/p/versions/python/3.6.8/include
├── share -> /home/raphx/.python/p/versions/python/3.6.8/share
├── lib -> /home/raphx/.python/p/versions/python/3.6.8/lib
├── bin -> /home/raphx/.python/p/versions/python/3.6.8/bin
└── python
    ├── 3.6.8
    │   ├── include
    │   ├── share
    │   ├── lib
    │   └── bin
    └── 2.7.15
        ├── include
        ├── share
        ├── lib
        └── bin
```

Hence, in order to use the Python versions that `p` installs and activates, simply prepend `$P_PREFIX/p/versions/bin` to `$PATH`:

```
export PATH="$P_PREFIX/p/versions/bin:$PATH"
```

`$P_PREFIX` allows customization on where Python versions are installed, and if not specified, defaults to `/usr/local`.

When installing Python 3, the symlink `python` and `pip` are also created for `python3` and `pip3` executables respectively, for the sake of convenience.

## FAQs

**What about `pip`?**

`pip` is installed by default for Python 3. For Python 2.7.9 or above, you can run the following command to install `pip`:

```
python -m ensurepip
```

For Python version less than 2.7.9, you will have to install `pip` manually.

**Can I use `p` to manage Python project dependencies?**

You could, though it is recommended to use [`pipenv`](https://pipenv.readthedocs.io/en/latest/) to do so. `p` can be used to install the Python version for your project, and `pipenv` can later be used to setup the project virtual environment using the Python version installed.

**How do I get this working on Windows?**

No Windows support planned at the moment, pull requests are welcomed.

## Attribution

This is a fork from the original [p](https://github.com/qw3rtman/p) by [Nimit Klara](https://github.com/qw3rtman).

## License

[MIT](LICENSE)
