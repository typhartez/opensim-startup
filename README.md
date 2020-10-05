# Linux script and configuration to run OpenSim

WORK IN PROGRESS - Use at your own risk

## Usage

Extract the OpenSim tarball somewhere (let's name it `opensim-test`) and go inside the directory. Copy the content of this repository in the OpenSim directory (not in the `bin` directory, `bin` mut be a subdirectory).

Then go in the `conf` directory, copy `opensim.conf.in` to `opensim.conf` and edit `opensim.conf`. This file defines a few environment variables that will be used by OpenSim. Most are ok as is if not customized, but it's better to understand what is used and why.

Note that the configuration is made to connect to an existing grid, so if you run in standalone, the default `conf/Grid.ini` wont be suited and you will have to adapt it to standalone.

- **SIM_LSN** is the HTTP port used by OpenSim (not the region port). If you run just one instance of OpenSim, it's safe to keep the default (`9000`)
- **SIM_HOST** 
- **GRID_NAME**
- **GRID_SCHEME**
- **GRID_HOST**
- **GRID_PUBLIC**
- **GRID_PRIVATE**

In the default configuration provided, there are also two lines to adapt the environment for running OpenSim through Mono. You can change them and/or add others.

Make the `startup` script executable and run it
```sh
chmod +x startup
./startup
```

## Generated files

The only file that needs to be created manually is `conf/opensim.conf` (from the template `conf/opensim.conf.in`). Other files are not provided from the repository, but are automatically generated if absent when the script runs:
- `conf/OpenSim.exe.config` generated from `conf/OpenSim.exe.config.in`. If you need the change things inside, it wont be overwritten on an update of the scripts
- `conf/Grid.ini` generated from `conf/Grid.ini.in`, used in a grid mode (**STANDADONE=false** in `conf/opensim.conf`). The default should be ok for simple grid implementations (it works with [Sacrarium](http://sacrarium24.ru)). If your grid uses different server names for the different services, using a customized one would be necessary, hence the template
- `conf/Local.ini` generated empty on startup if not present. This is the last configuration file read by OpenSim in either mode (standalone or grid). You can put anything you want to customize in it

## Provided files

The `startup` script reads a different file depending on the mode. All provided files should **not** be modified as they would be overwritten on update. If a feature is missing or incompatible with some installations, it's better to handle them in the project (pull requests welcome).
- Grid mode: `conf/provided/OpenSimGrid.ini`
- Standalone mone: `conf/provided/OpenSimStandalone.ini` and `conf/provided/Standalone.ini`

In both modes, a few others are included (just for the sake of following some code splitting OpenSim developers used):
- `conf/provided/FlotsamCache.ini` Flotsam asset caching settings
- `conf/provided/osslEnable.ini` OSSL features and permissions
- `conf/provided/SQLite.ini` SQLite database automatic configuration

## Examples

### A simple standalone

Let's have a computer whose public IP address is `88.77.66.55` and we have configured a free DNS `myopensim.freedns.me`. We have opened the incoming port `9000` on our router to accept connections on TCP and UDP. Let's name our OpenSim `Standalone`. Here is the needed `conf/opensim.conf`:
```sh
export STANDALONE=true
export SIM_LSN=9000
export SIM_HOST='myopensim.freedns.me'
export GRID_NAME='Standalone'
export GRID_HOST=''
export GRID_PUBLIC=
export GRID_PRIVATE=8003

export MONO_THREADS_PER_CPU=120
ulimit -s 1048576
```

For a default setup, nothing more is needed.

### Connecting to Sacrarium grid

We have the same hardware and network configuration than the standalone above:
```sh
export STANDALONE=false
export SIM_LSN=9000
export SIM_HOST='myopensim.freedns.org'
export GRID_NAME='Sacrarium'
export GRID_HOST='sacrarium24.ru'
export GRID_PUBLIC=8002
export GRID_PRIVATE=8003

export MONO_THREADS_PER_CPU=120
ulimit -s 1048576
```

For a default setup, nothing more is needed.
