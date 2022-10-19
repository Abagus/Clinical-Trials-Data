# Clinical-Trials-Data#

# In this age of technology, connecting multiple sources of information and unlocking insights on new data sets becomes crucial to the future of healthcare. Therefore, we must excel in accessing, cleaning, and aggregating this data in order to develop hypotheses on potential insights, and generating the analysis to make it actionable.#

# Using the publicly available available Aggregated Analysis of CLinicalTrials.gov (AACT), I was able to upload a data dump file to pgadmin and use PostgreSQL to complete the following tasks:

1. Create a view of all prospective cancer-related that are completed:

{create view trial_details as
select s.nct_id,c.name,e.criteria,f.city || ', ' || f.country as location,i.intervention_type,count(p.id) as total_participants
from studies s join conditions c on s.nct_id=c.nct_id
join eligibilities e on s.nct_id=e.nct_id
join facilities f on  s.nct_id=f.nct_id
join interventions i on s.nct_id=i.nct_id
join participant_flows p  on s.nct_id=p.nct_id
where s.overall_status='Completed' 
group by s.nct_id,c.name,e.criteria,location,i.intervention_type;}

Explanation: 
This query makes a view with name trial_details while joining multiple tables to get desired columns we concat columns city and country of facilities table to give a meaningful location column. We use the participants_flow table to get no of participants per trail in this approach. To access this view you will use another query:

{Select * from trial_details;}

2. Create a view of all observed adverse events and outcomes recorded for each trial:

{Create view trial_effects as 
select s.nct_id,o.outcome_type,r.adverse_event_term from studies s
join outcomes o on  s.nct_id=o.nct_id
join reported_events r on s.nct_id=r.nct_id
where r.adverse_event_term is not null;}

Explanation:

This query makes a view with name trial_effects while joining multiple tables to get desired. To access this view you will use another query:

{Select * from trial_effects;}

3. Find the trial that had the most patients with a complete response to the intervention of study (using outcomes_measurements table):

{select s.nct_id,count(om.id) as patient_count
from studies s join outcomes o on s.nct_id=o.nct_id
join outcome_measures e on o.id=e.outcome_id
join outcome_measurements om on  om.outcome_measure_id=o.id
where s.overall_status='Completed' 
group by s.nct_id
order by patient_count desc limit 1;}

Explanation:

This query displays the trial with the most patient_counts (taken as count from outcome_measurements table. Order by orders the first row to be that with largest patient_count and limit 1 makes the result limited to that first row only giving us our required results.

4. Find the number of trials that started after 2005 and prior to 2010:

{select count(nct_id) from studies where 
 TO_DATE(start_month_year,'Month YYYY dd')>='2006-01-01'
and  TO_DATE(completion_month_year,'Month YYYY dd')<='2009-12-31';}

Explanation:

In this query we use columns start_month_year and completion month year and firstly convert them to date format using to_date function. Then we apply necessary filter (after 2005- before 2010) to get required results.

5. Distribution of trials by state for all countries available in dataset:

{select count(s.nct_id),f.city,f.country from studies s
join facilities f on  s.nct_id=f.nct_id
group by f.city,f.country
order by f.country,f.city;}

6. Distribution of trials by states of USA:

{select count(s.nct_id),f.city,f.country from studies s
join facilities f on  s.nct_id=f.nct_id
where f.country='United States'
group by f.city,f.country
order by f.country,f.city;}

#With this code, I am able to aggregate all the data regarding clinical trials between the years of 2005 and 2010. With that, I was able to find the response to each clinical trial to get a detailed report of results of medications. From here, medical professionals will be able to make recommendations on cancer medications using an aggregate analysis to come to better conclusions for patient care.#
