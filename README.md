# Automate MPAS Simulation

### Author: Riley X. Brady
### Contact: riley.brady@colorado.edu

A set of scripts to automate the process of setting up, building, and submitting an MPAS-Ocean case with or without particles.

## Installation

1. Run `git clone git@github.com:bradyrx/automate_mpas_simulation.git` on an institutional computer (Grizzly and Wolf tested at LANL).

2. Add folder to your E3SM directory. Make sure that a version of MPAS with particles is checked out. E.g., check out the `particlePassiveFloatVerticalTreatmentFix` branch from https://github.com/pwolfram/MPAS-Model.

3. Load a python2.7 virtual environment and make sure `lxml` is installed.

4. Edit the header variables in `generate_E3SM_case.sh` to set resolution, machine, p-code, particle count, etc.

5. Run `bash generate_E3SM_case.sh`

## Shell Script Options

The user should only need to modify header variables in the main `automate_mpas_simulation.sh` script.

At a minimum, `E3SM_DIR` should be changed to the direct path to the base E3SM folder.

### Model Configuration

Options pertaining to the model setup.

| Option      |  Description                                     | Example Values                    |
|-------------|--------------------------------------------------|-----------------------------------|
| BGC         | Whether or not to run with ocean biogeochemistry | true/false                        |
| res         | MPAS-Ocean grid resolution                       | T62_oEC60to30v3, T62_oRRS30to10v3 |
| nproc_ocean | Number of processors for ocean                   | 512                               |
| nproc_ice   | Number of processors for ice                     | 128                               |
| mach        | IC machine name                                  | grizzly, wolf                     |
| pcode       | Project account                                  | w19_marinebgc                     |
| input_dir   | Input directory for initialization data          | filepath                          |

### Run Configuration

Options pertaining to job submission and run time.

| Option      |  Description                                                   | Example Values |
|-------------|----------------------------------------------------------------|----------------|
| WALLCLOCK   | Wallclock time for this run                                    | hh:mm:ss       |
| STOP_OPTION | Whether to run N days or N months                              | ndays, nmonths |
| STOP_N      | Number of days or months to run (dependent on previous option) | 5              |

### Particle Configuration

Options pertaining to particle setup.

| Option           |  Description                                                        | Example Values              |
|------------------|---------------------------------------------------------------------|-----------------------------|
| nvertlevels      | Number of vertical levels for particle seeding (if 0, no particles) | 10                          |
| output_frequency | Frequency to save out particle trajectories in days                 | 2                           |
| particletype     | Particle seeding strategies, space-separated and bounded by ()      | surface, passive, isopycnal |

### Particle Sensor Configuration

Options pertaining to sensors on board the e-floats.

**NOTE**: The only current way to turn on sensors is to directly modify the source Registry in the MPAS-O directory. It currently cannot be changed via `user_nl_mpaso`. Thus, check your LIGHT Registry when submitting jobs in the future if you aren't using this script.

| Option            |  Description                                                          | Example Values |
|-------------------|-----------------------------------------------------------------------|----------------|
| sampleTemperature | If true, save out temperature along float trajectory                  | true/false     |
| sampleSalinity    | If true, save out salinity along float trajectory                     | true/false     |
| sampleDIC         | If true, save out DIC along float trajectory (only runs if BGC is on) | true/false     |
| sampleALK         | If true, save out ALK along float trajectory (only runs if BGC is on) | true/false     |


