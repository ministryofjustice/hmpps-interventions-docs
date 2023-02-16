# Analytical Platform notebooks

To execute the notebooks:

1. Ensure you [have access](https://ministryofjustice.github.io/hmpps-interventions-docs/get-started/analytical-platform-access.html) to the Analytical Platform.
1. Log in to the [control panel](https://controlpanel.services.analytical-platform.service.justice.gov.uk/)
1. Under "Jupyter Lab Data Science", start or resume a `jupyter-lab-datascience-notebook`
1. Use the menu to select "Git" and "Clone a repository" and clone `https://github.com/ministryofjustice/hmpps-interventions-docs.git`
1. Open a terminal ("File" menu, "New launcher", select "Terminal")
1. `cd hmpps-interventions-docs/analytical-platform`
1. `pip install -r requirements.txt`
1. In the file browser, open the Jupyter notebooks (`.ipynb` files)


If the data is missing:

1. Log in to the [control panel](https://controlpanel.services.analytical-platform.service.justice.gov.uk/)
1. Under "RStudio", start or resume an `rstudio`
1. Setup `create-a-derived-table` project with [these instructions](https://user-guidance.services.alpha.mojanalytics.xyz/tools/create-a-derived-table/#setting-up-your-working-environment)
1. Follow each step in the "Setting up a Python virtual environment" section in the above instructions
1. Run `dbt run --select models/interventions/probation_referrals/`
