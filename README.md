The assignment:

Using the publicly available Aggregated Analysis of ClinicalTrails.gov dataset (​AACT​) please download this data and host it in a local postgres database to complete the following. Please complete each task using SQL.

1. Create a view of all ​prospective ​cancer related clinical trials that are completed (no longer actively recruiting and not prematurely terminated)
 
  a. This view should include an nct_id, the cancer condition, inclusion/exclusion criteria for the trial, location of the trial, and the       intervention of study, total participants in the study
  
  b. Use this view to subset/answer all below requests

2. Create a view for all observed adverse events and outcomes recorded for each trial

3. Find the trial that had the most patients with a complete response to the intervention of study (using
outcome_measurements table)

4. Find the number of trials that started after 2005 and ended before 2010

5. Distribution of trials by state


Deliverables:

1. Code used to load data to postgres database

2. SQL code used to write the views and questions specified above


Please view the "Assignment" file and download the PDF to view the query.
