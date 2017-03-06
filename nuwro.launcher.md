% NuWro Launcher
% Tomasz Golan
% OPUS meeting, 08.03.2017

# What is NuWro Launcher?

* Python script to run NuWro automatically based on a set of configuration files
    
* generates a bash script to run a simulation and ROOT macros

* the bash script can be run locally or on grid

* easy way to define a new job

# NuWro Launcher idea

![NuWro Launcher workflow](img/nuwro.launcher.idea.pdf)

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

# Configuration file

```ini
[paths]
; paths to NuWro and ROOT are required
; (unless $ROOTSYS and $NUWRO env are defined)
nuwro = /doc/git/nuwro
root  = /home/goran/soft/root

; paths to configuration files [optional]
; $PWD/config/[cfg type] will be used if not given
jobs = /doc/git/nuwro.launcher/config/jobs
analyses = /doc/git/nuwro.launcher/config/analyses
histograms = /doc/git/nuwro.launcher/config/histograms

; path to top output folder where results will be saved 
; nuwro/launcher_output will be used if not given
output = /home/goran/output
```

# Example job config

```ini
[input]
; root file - if provided simulation will be skipped
; root = /path/out.root
; path to params.txt file [optional]
; nuwro/data/params.txt will be used if not given
params = /home/goran/nuwro/params.txt

[params]
; parameters modification wrt params file in use
qel_cc_axial_mass = 1200
number_of_events = 100
number_of_test_events = 100

[analyses]
; the list of analyses to process on the output
; of the simulation (comma separated)
list = example_analyses
```

# Example analysis config - [signal]

```ini
[signal]
; signal definition (required)
; number of particles in the final state:
; p - protons, n - neutrons, 
; pip - pi+, pim - pi-, pi0 - pi0
; use any for any number of given particle in FS
; use N+ for at least
p = 1+
n = any
pip = 1
pim = 2+
pi0 = 0
```

# Example analysis config - [dynamics]

```ini
[dynamics]
; dynamics to include (required):
; qel, res, dis, mec, coh, cc, nc (True or False)
cc = True
nc = False
qel = True
res = True
dis = True
mec = True
coh = True
```

# Example analysis config - [cuts]

```ini
[cuts]
; particle = what [operator] cut, what [operator] cut
; possible what: energy, kinetic_energy, momentum, angle
; possible particle: p, n, pip, pim, pi0, l
; possible operations: >, <, ==, >=, <=
; cut = value (degrees for angle or MeV for others)
l = angle < 20, momentum > 200
p = kinetic_energy > 100, angle < 10
pip = momentum > 150, angle < 50

; cuts are combined with signal settings, e.g.
; signal.p = 1+ and cuts.p = momentum > 200
; -> at least one proton with momentum > 200 MeV
; signal.p = 2 and cuts.p = momentum > 200
; -> exactly 2 protons with momentum > 200 MeV
```

# Example analysis config - [histograms]

```ini
[histograms]
; the list of histograms to create for the analysis
; comma separated
list = example_histogram
```

# Output

```bash
$ tree output
output
|-- nuwro_17.01.1
    |-- 2017-03-05
    |   |-- example_job
    |-- 2017-03-06
        |-- example_job
```

# Output bash script

```bash
# automatically generated using NuWro Launcher

# setting up environmental variables for ROOT
export ROOTSYS=/home/goran/soft/root
export PATH=$PATH:$ROOTSYS/bin
LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$ROOTSYS/lib

# running NuWro
/doc/git/nuwro/bin/nuwro -i /home/goran/nuwro/params.txt \
  -p 'qel_cc_axial_mass = 1200' \
  -p 'number_of_events = 100' \
  -p 'number_of_test_events = 100' \
  -o /home/goran/output/nuwro_17.01.1/2017-03-06/\
  example_job/nuwro_17.01.1_example_job_2017-03-06.root

# here ROOT macros will be called
```

# Output ROOT macro - signal

```c
// check if event fulfill signal definition
// return false if some condition is not filfilled
bool is_signal(event *e) {
	if (e->fof(2212) < 1) return 0;

	if (e->fof(211) != 1) return 0;

	if (e->fof(-211) < 2) return 0;

	if (e->fof(111) != 0) return 0;

	return 1;
}
```

# Output ROOT macro - channels

```c
// check if event is in selected dynamics list
// return false if flag.channel is false/False
bool is_channel(event *e) {
	if (e->flag.nc) return 0;

	return 1;
}
```

# Output ROOT macro - cuts

```c
// check if all cuts are passed
// return false if at least on is not fulfilled
bool cuts_passed(event *e) {
  
  ...

  // check if final lepton passed cuts
  if(!(acos(e->out[0].p().z / e->out[0].p())*180.0/M_PI < 20 
       && e->out[0].p() > 200)) return 0;

  ...
}
```

# Output ROOT macro - other

```c
// extract total cross section from .root.txt file
double get_sigma (string filename, int dyn = -1);

// normalize to cross section per nucleon
void normalize(TH1* h, const double factor);

// main function
void example_analyses();
```

# TODO

* finish histograms
* testing / running on grid

\mbox{}

* compare two set of results
* add data to plots
* ...