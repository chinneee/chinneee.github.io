---
title: "Three SQL Queries Asked in Interview for Business Analyst"
excerpt: "This document introduces and solves three key **SQL problems** often posed during **Business Analyst interviews**. Each query addresses real-world scenarios to demonstrate a strong understanding of SQL capabilities such as data *transformation, aggregation, and ranking*.   <br/><img src='https://artoftesting.com/wp-content/uploads/2019/12/SQL-query-interview-questions.jpg'>"
collection: portfolio
---

![](https://artoftesting.com/wp-content/uploads/2019/12/SQL-query-interview-questions.jpg)

This document introduces and solves three key **SQL problems** often posed during **Business Analyst interviews**. Each query addresses real-world scenarios to demonstrate a strong understanding of SQL capabilities such as data *transformation, aggregation, and ranking*.

---

## 1. Election Query

### Problem
Analyze election results to determine the number of seats won by each political party. The data includes:
- `candidates`: Details about candidates, including their associated parties.
- `results`: Voting outcomes, including votes received by candidates across constituencies.

### Target
Identify which party won the most seats by calculating the number of constituencies each party secured.

### Solution
- **Join Data**: Combine `results` and `candidates` tables to associate votes with parties.
- **Rank Votes**: Use the `ROW_NUMBER()` function to rank candidates in each constituency by votes.
- **Aggregate Results**: Count the number of seats won by each party using `SUM` with a conditional case.

```sql
-- Join table results and candidates to rank
with cte_grouped as (
    select 
        r.*,
        c.party
    from results r 
    join candidates c on r.candidate_id = c.id),
-- Ranking votes separated by consituency_id 
cte_ranked as (
    select 
        constituency_id,
        party,
        votes,
        row_number() over (partition by constituency_id order by votes desc) as rank
    from cte_grouped
)
-- Display result to detect how many seats won by each party
select
    party,
    sum(case when rank = 1 then 1 else 0 end) as seats_won
from cte_ranked
group by party;
```
## 2. Advertising System Deviations Report

### Problem
Identify campaigns with the highest success and failure counts for each customer. The data includes:
- `customers`: Information about customers.
- `campaigns`: Campaign details linked to customers.
- `events`: Events associated with campaigns, categorized by status (e.g., success, failure).

### Target
Generate a report showing:
- Campaign names and their outcomes for each customer.
- The campaigns with the most successes and failures.

### Solution
Create Common Table Expressions (CTEs) to :
- **Count Events by Status**: Aggregate `events` to determine the number of successes and failures for each campaign.
  ```sql
    with cte_count as (    
        select
            c.customer_id,
            c.name,
            e.[status],
            count(e.[status]) as [count]
        from campaigns c 
        join events e on c.id = e.campaign_id
        group by c.customer_id, c.name, e.[status]
    )
  ```
- **Summarize by Customer:** Use conditional aggregation to calculate the total success and failure counts for each customer.

    ```sql
    cte_category as (
    select
        c.customer_id,
        STRING_AGG(c.name, ', ') as campaign,
        status,
        sum(case when [status] = 'success' then [count] else 0 end) as success_count,
        sum(case when [status] = 'failure' then [count] else 0 end) as failure_count
    from cte_count c
    group by c.customer_id, status
    )
    ```
- **Identify largest values from each category** by using `MAX()` function.
    ```sql
    max_values as (
    select 
        max(success_count) as max_success_count,
        max(failure_count) as max_failure_count
    from cte_category
    )
    ```
- **Highlight Extremes:** Identify campaigns with the maximum success and failure counts for reporting. This focuses on key deviations.
    ```sql
    select
        cate.status,
        CONCAT(cust.first_name, ' ', cust.last_name) as customer_name,
        cate.campaign,
        cate.success_count + cate.failure_count as total
    from cte_category cate 
    join customers cust on cate.customer_id = cust.id
    join max_values mv on cate.success_count = mv.max_success_count 
        or cate.failure_count = mv.max_failure_count
    order by cate.status desc;
    ```

## 3. Election Exit Poll by State Reports

### Problem  
The query attempts to generate a report on election exit polls by state, showing the ranking of candidates in each state. It includes information on the candidate name, their rank per state, and the number of candidates per state. The data includes:    
- `candidates`: Candidate details, including their party.
- `results`: Display states where candidates occurred.

### Target  
 
- To generate an election exit poll report by state, displaying the candidate names along with their ranking for each state (1st, 2nd, 3rd).
- Ensure that no data is missing when there is no state rank for a particular candidate (handling the cases where candidates do not have 1st, 2nd, or 3rd places).

### Solution  

1. **Aggregate Votes**: Calculate the total votes received by each candidate within each state.  
2. **Rank Candidates**: Use `DENSE_RANK()` to assign ranks to candidates based on the total votes they received in each state. The rank will determine the positions (1st, 2nd, 3rd, etc.) of candidates within each state.  
3. **Extract Candidate Rankings:** Filter the data to extract the top three ranked candidates in each state.

---

```sql
-- Step 1: Aggregate votes by state and party
with cte_grouped as (   
    select 
        distinct
        concat(first_name, ' ', last_name) as candidate_name,
        rt.*,
        COUNT(state) over (partition by candidate_id, state) as total_candidates
    from candidates_tab ct 
    join results_tab rt on ct.id = rt.candidate_id
),

-- Step 2: Rank parties by total votes in each state
cte_ranked as (
    select
        candidate_name,
        total_candidates,
        CONCAT(state, ' (', total_candidates, ')') as state_with_count,
        dense_rank() over (partition by candidate_id order by total_candidates desc) as rank
    from cte_grouped
)

-- Step 3: Extract the candidate names along with their ranking for each state (1st, 2nd, 3rd)
select
    candidate_name,
    STRING_AGG(case when rank = 1 then state_with_count end, ', ') as '1st_place',
    STRING_AGG(case when rank = 2 then state_with_count end, ', ') as '2nd_place',
    STRING_AGG(case when rank = 3 then state_with_count end, ', ') as '3rd_place'
from cte_ranked
group by candidate_name;
```
## Conclusion
In conclusion, the three SQL queries demonstrate important skills for Business Analysts, including data aggregation, ranking, and transformation. These techniques are essential for analyzing and deriving insights from complex datasets. Mastery of such SQL operations helps business analysts efficiently solve real-world problems and support data-driven decisions.

<a href="https://github.com/chinneee/3-SQL-Queries-for-Business-Analyst" style="display: inline-block; padding: 10px 15px; background-color: #CCCCFF; color: Black; text-decoration: none; border-radius: 5px;">
  <img src="https://pngimg.com/uploads/github/github_PNG40.png" alt="GitHub logo" style="width: 30px; height: 30px; vertical-align: middle; margin-right: 8px;">
  Explore the full project on GitHub
</a>