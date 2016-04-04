## Installing the Agave CLI

The Agave CLI is a collection of about 100 bash scripts.
The purpose of these scripts is to facilitate building cURL commands for interaction with the web API.
A version of the Agave CLI is distributed with this tutorial.
To begin, open up a terminal window and navigate to a directory where you would like to organize this work.
Then, clone this tutorial:

```git clone https://github.com/wjallen/using-agave```

Navigate into the `using-agave/` directory, then into the `src/` directory:

```cd using-agave/src/```

Unpack the file containing the Agave CLI:

```tar -xzf agave-cli.tar.gz```

Add the Agave CLI to your PATH, so that you may access the commands from anywhere on the current machine:

```
cd agave-cli/bin/
export PATH=$PWD:$PATH
```

You may also consider adding the whole path to your `~/.bashrc` so that each new terminal session knows the location:

```echo "PATH=/complete/path/to/using-agave/src/agave-cli/bin:\$PATH" >> ~/.bashrc```

(First replace `/complete/path/to` with the actual path to the `using-agave/` directory.)
Note that if you move or rename this project directory, the Agave commands will no longer be in your PATH, and the previous steps will need to be repeated.
Finally, verify that the Agave CLI has been added to the PATH by executing:

```which tenants-init```

The path to the Agave CLI should appear, e.g.:

```/home/username/using-agave/src/agave-cli/bin/tenants-init ```

Please note that the version of the Agave CLI is 2.1.4 (2015-09-14).
The Agave CLI is ever evolving, so please consider downloading the current source [here](https://bitbucket.org/taccaci/foundation-cli).

[Back to: README](../README.md) | [Next: Initializing with CyVerse](initializing.md)
