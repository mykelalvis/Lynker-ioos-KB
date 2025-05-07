From an email from [[Patrick]] [[2024-07-11]] slightly edited for formatting

---

Every model is different and getting a new model working in the sandbox requires several steps, all of which require hands on work. Some of the steps to launch the model run can be made easier with additional scripting. The cloudflow workflow is designed to be extended to support new models being added.

Here are the steps to setup a new model.

1. The model binaries need to be built using the specific library module files on the sandbox. New libraries might need to be added. The build system is different for every model/application.
2. The models/applications usually have some existing script or method for launching the model run. I try to use that if possible, usually with some modifications. Each run also usually requires some sort of [[input namelist]] (e.g. ROMS uses [ocean.in](http://ocean.in/)) and [[other files]] (forcing data, etc.) to be placed in a specific folder for the run to work. Cloudflow does this for the models we are currently using.
3. Input data is going to be different for each model. Where is the data? How can we access it? Does it need to be pre-processed before the model runs. etc.
4. Model output, location, log files, etc. are going to be different for each model. e.g. LiveOcean uses an LO_roms folder, CORA uses ADCIRC/ERA5/.

Once you understand how the model runs and what it needs, you can integrate it with the cloudflow workflow. New flows and tasks can be created as needed. For example, adding multi-day hindcasts was done by copying and modifying existing code. It has proven to be fairly easy for me to conform new models to the cloudflow structure.

Here is how the cloud flow proceeds through the different workflow steps:

Start: `cloudflow/workflows/workflow_main.py` – this will launch a flow depending on the type of job submitted

(There is also a similar `lohindcast_main.py` that should be combined with the above script.)

 e.g. flows.fcst_flow(fcstconf, jobfile, sshuser)

 workflows/flows.py (e.g. fcst_flow) – runs several tasks

                …

                fcstjob = tasks.job_init(cluster, fcstjobfile)

                                 workflows/tasks.py job_init()

                                                 creates a Job object depending on the job type

                                                                 job/JobFactory

                                                                                 e.g. romsforecast ROMForecast.py

                                                                                                 ROMSForecast.py __init__()

                                                                                                                 make_oceanin().  – uses an existing template to fill in variable values

                tasks.forecast_run(cluster, fcstjob)

                                                 workflows/tasks.py forecast_run()

 workflows/fcst_launcher.sh (or cora_launcher.sh)

 model provided run script (modified as needed for our needs)

                …

And here is [a diagram](https://github.com/ioos/Cloud-Sandbox/blob/main/CLOUDFLOW.md) showing the relationship between the different Python modules.

Here is the [Prefect documentation](https://docs-v1.prefect.io/api/0.15.13/) for the version we are using:

