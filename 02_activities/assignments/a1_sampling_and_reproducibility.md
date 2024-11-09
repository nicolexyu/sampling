# ASSIGNMENT: Sampling and Reproducibility in Python

Read the blog post [Contact tracing can give a biased sample of COVID-19 cases](https://andrewwhitby.com/2020/11/24/contact-tracing-biased/) by Andrew Whitby to understand the context and motivation behind the simulation model we will be examining.

Examine the code in `whitby_covid_tracing.py`. Identify all stages at which sampling is occurring in the model. Describe in words the sampling procedure, referencing the functions used, sample size, sampling frame, any underlying distributions involved, and how these relate to the procedure outlined in the blog post.

Run the Python script file called whitby_covid_tracing.py as is and compare the results to the graphs in the original blog post. Does this code appear to reproduce the graphs from the original blog post?

Modify the number of repetitions in the simulation to 1000 (from the original 50000). Run the script multiple times and observe the outputted graphs. Comment on the reproducibility of the results.

Alter the code so that it is reproducible. Describe the changes you made to the code and how they affected the reproducibility of the script file. The output does not need to match Whitby’s original blogpost/graphs, it just needs to produce the same output when run multiple times

# Author: Nicole Xiaoye Yu

```
Please write your explanation here...

```
Question 1: Identify all stages at which sampling is occurring in the model. 

Stage 1: Event Assignment and Initial Setup
- Procedure: A DataFrame ppl is created with 1,000 attendees (200 at a wedding and 800 at a brunch). Each attendee initially has an infected status set to False and a traced status set to NaN.
- Functions: pd.DataFrame() creates the initial dataset and astype(pd.BooleanDtype()) sets traced as a nullable Boolean.
- Sampling Frame: The sampling frame is the total population of simulated attendees (1,000).
- Sample Size: The total sample size in this stage is 1,000 individuals, with 200 assigned to weddings and 800 assigned to brunches. 
- Underlying Distributions: The distribution of event attendance is fixed and reflects the assumed popularity or size difference between the two event types.
- Relation to Blog Post: Similar to the blog post, I assigned individuals to easily traceable events (weddings) and harder-to-trace ones (brunches).

Stage 2: Random Infection Process
- Procedure:  A subset of attendees is randomly infected to simulate the initial spread of an infectious disease.
- Functions: The size of this infected group is determined by the constant ATTACK_RATE. np.random.choice() randomly selects individuals to be infected.
- Sampling Frame: The sampling frame includes all 1,000 attendees, regardless of event type.
- Sample Size: The number of infected individuals is determined as 0.10 * 1000 = 100, based on the ATTACK_RATE of 10%.
- Underlying Distributions: Infection is modeled as a binomial process where each attendee has a 10% chance of being infected.
- Relation to Blog Post: Infected individuals are randomly chosen, reflecting how infections can occur at any event, but some events are easier to trace than others.

Stage 3: Primary Contact Tracing Sampling
- Procedure: This stage simulates the first layer of contact tracing by randomly determining which infected individuals are successfully traced. Only infected individuals are eligible for tracing.
- Functions: np.random.rand() generates a random number for each infected person to decide whether tracing is successful, based on TRACE_SUCCESS.
- Sampling Frame: Only the 100 infected individuals are in the sampling frame for tracing.
- Sample Size: Of the 100 infected individuals, approximately 20 are traced, based on a tracing success rate (TRACE_SUCCESS) of 20%.
- Underlying Distributions: For each infected individual, there is a 20% probability of successful tracing.
- Relation to Blog Post: The 20% success rate in my model mirrors the blog post’s mention of imperfect primary contact tracing.

Stage 4: Secondary Contact Tracing Sampling
- Procedure: Secondary tracing expands the tracing effort to additional infected individuals based on event-level tracing counts. If an event has at least SECONDARY_TRACE_THRESHOLD traced infections, all infected individuals at that event are considered traced.
- Functions: value_counts() tallies the number of traced individuals per event. ppl.loc[] updates traced status for all infected individuals in high-traced events.
- Sampling Frame: Infected individuals at highly traced events form the sampling frame for secondary tracing.
- Sample Size: The sample size for secondary tracing varies, depending on the initial results of primary tracing. Only infected attendees from events with at least two traced individuals (e.g., weddings or brunches) will be included in secondary tracing.
- Underlying Distributions: The decision to trace additional attendees is conditional on the event-level count of primary traces. If an event meets the threshold, all infected individuals there are traced. This conditional sampling step introduces clustering, as certain events yield higher tracing outcomes than others.
- Relation to Blog Post: My model's secondary tracing (100% success if multiple infections are traced to an event) reflects the blog's point that events with multiple infections get more tracing attention.

Question 2:  Does this code appear to reproduce the graphs from the original blog post?

The code does not reproduce the graphs from the original blog post.

Blog post chart:
- shows a broader range of proportions for the "Observed proportion," suggesting a wider spread of potential values for observed cases attributed to weddings.
- the "True proportion" has a narrow, high peak, while the "Observed proportion" is spread out more uniformly across a range of values. The point of bias: True cases are concentrated in a small range, while observed cases vary widely due to contact tracing bias.
- X-axis goes from 0.0 to 1.0

My chart:
- has a narrower range.
- both the "Infections from Weddings" and "Traced to Weddings" distributions overlap closely and do not exhibit the same wide spread, which may not fully capture the intended bias effect.
- X-axis is limited to around 0.0 to 0.35. This difference in axis scaling can significantly impact the visualization, possibly missing the wider spread effect.

Question 3: Modify the number of repetitions in the simulation to 1000 (from the original 50000) and 
Run the script multiple time

I notice variability due to the decreased number of repetitions, which leads to less stable estimates of infection and tracing proportions.

Question 4:  Alter the code so that it is reproducible.

To make results consistent on each run, I set a random seed. This ensures that any random sampling in the code produces the same results every time it’s run.

At the beginning of the script, add np.random.seed(42) to fix the random number generator’s state, making results reproducible.





## Criteria

|Criteria|Complete|Incomplete|
|--------|----|----|
|Altercation of the code|The code changes made, made it reproducible.|The code is still not reproducible.|
|Description of changes|The author explained the reasonings for the changes made well.|The author did not explain the reasonings for the changes made well.|

## Submission Information

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

### Submission Parameters:
* Submission Due Date: `HH:MM AM/PM - DD/MM/YYYY`
* The branch name for your repo should be: `sampling-and-reproducibility`
* What to submit for this assignment:
    * This markdown file (sampling_and_reproducibility.md) should be populated.
    * The `whitby_covid_tracing.py` should be changed.
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sampling/pull/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `sampling-and-reproducibility`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack at `#cohort-3-help`. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.
