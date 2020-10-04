# Linux script and configuration to run OpenSim

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

