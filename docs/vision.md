# Vision - Supplement / Nootropics / Health product recommendation engine

The vision is to be able to have a recommendation engine for supplements, nootropics, medication, health products that is heavily research-backed. 

## Key flow, in short

1. Leverage AI to identify published research -> Identify a trust index -> Extract key points of the research
2. Categorize information by product category (nootropics, skin products, dietary supplements, workout supplements, longevity supplements, health supplements, etc)
3. Give a confidence score of the efficacy of an individual supplement or product. Identify key pieces of research. 

## Additional:

1. May need to store summaries of resaerch in a database. Potentially also research vectors if valuable for pulling research quickly 
2. May want to pull from additional sources[1;below]
3. May want to have a recommendation for individual uses that takes their health / preferences into account

## Leveraging Anecdotes[1]

- Some supplements may have efficacy but lack formal research
- Anecdotal evidence may be able to be used, in aggregate, to identify those that have efficacy. 
   - Also may be able to be used to identify side effects. 
- However, challenge in identifying true-vs-false anecdotes. 

Approaches for leveraging anecdotes:
- Try to find anecdotes from those, who lack financial incentives, who are recommending a product over many-years 
   - Why? People placebo-effect themselves into believing a supplement works. But likely do not continue using for years
- Find anecdotes from individuals identified to be more reliable. Use AI to identify sincerity in their messages. 
- Find anecdotes from individuals disclaiming a supplement. In aggregate, may be indicative of products more likely to work. 


### Financial Gains:

- Join affiliate marketing programs (amazon affiliates, etc)

### Risks

- Unsure of legality of 
- The placebo effect has a real positive benefit for some. I do not want to ruin the effect by overly-rejecting what some believe to be true. 

### Additional To-dos

- Additional research/analysis into the psychology of research. We need to understand how to best identify truth from chaotic information
- Additional research/analysis into how this product can provide value. There is going to be unknown-unknowns that have to be accounted for. 
- Additional research/analysis into how to improve the accuracy. What algorithms can be used to generate confidence indexes that can take multi-factors into account
- Additional research/analysis into how we can pull the most accurate information from research

### Assumed Tech Stack

- Would prefer a cheaper tech stack. May need cron intervals. AI analysis. A vector Db. Redis/caching. React/VueJs client. And more. 
- Research into some of the cheapest and most flexible tech stacks for this kind of product

### Sources

- Examine.com
- Scott Alexander / SlateStarCodex


### Personal assumptions

- Skeptical. I believe most supplements are junk and psuedo-science. 
- However, universally agreed-upon supplements and medication exist (e.g. creatine). A confidence interval may be possible, but my default assumption for most heavily-praised-online supplements is more like a 2/10 rather than a 5/10. 



