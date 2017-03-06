% NuWro Launcher
% Tomasz Golan
% OPUS meeting, 08.03.2017

# What is NuWro Launcher?

* Python script to run NuWro automatically based on a set of configuration files
    
* generates a bash script to run a simulation and ROOT macros

* the bash script can be run locally or on grid

* easy way to define a new job configuration

# NuWro Launcher usage

```bash
$ nuwro.launcher --help
Usage: nuwro.launcher [OPTIONS]

  NuWro Launcher

Options:
  -c, --config TEXT  The path to the config file.
  -a, --all          Process all jobs defined 
                     in 'jobs' folder
  -s, --single TEXT  Define a job to process.
  --help             Show this message and exit.
```

# NuWro Launcher idea

![NuWro Launcher workflow](img/nuwro.launcher.idea.pdf)