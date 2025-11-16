# Assignment 2: Design a Logical Model and Advanced SQL

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

#### Submission Parameters:
* Submission Due Date: `November 12, 2025`
* Weight: 70% of total grade
* The branch name for your repo should be: `assignment-two`
* What to submit for this assignment:
    * This markdown (Assignment2.md) with written responses in Section 1 and 4
    * Two Entity-Relationship Diagrams (preferably in a pdf, jpeg, png format).
    * One .sql file 
* What the pull request link should look like for this assignment: `https://github.com/<your_github_username>/sql/pulls/<pr_id>`
    * Open a private window in your browser. Copy and paste the link to your pull request into the address bar. Make sure you can see your pull request properly. This helps the technical facilitator and learning support staff review your submission easily.

Checklist:
- [ ] Create a branch called `assignment-two`.
- [ ] Ensure that the repository is public.
- [ ] Review [the PR description guidelines](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md#guidelines-for-pull-request-descriptions) and adhere to them.
- [ ] Verify that the link is accessible in a private browser window.

If you encounter any difficulties or have questions, please don't hesitate to reach out to our team via our Slack. Our Technical Facilitators and Learning Support staff are here to help you navigate any challenges.

***

## Section 1:
You can start this section following *session 1*, but you may want to wait until you feel comfortable wtih basic SQL query writing. 

Steps to complete this part of the assignment:
- Design a logical data model
- Duplicate the logical data model and add another table to it following the instructions
- Write, within this markdown file, an answer to Prompt 3


###  Design a Logical Model

#### Prompt 1
Design a logical model for a small bookstore. 📚

At the minimum it should have employee, order, sales, customer, and book entities (tables). Determine sensible column and table design based on what you know about these concepts. Keep it simple, but work out sensible relationships to keep tables reasonably sized. 

Additionally, include a date table. 

There are several tools online you can use, I'd recommend [Draw.io](https://www.drawio.com/) or [LucidChart](https://www.lucidchart.com/pages/).

**HINT:** You do not need to create any data for this prompt. This is a conceptual model only. 

## ANSWER:

- <img src ="assignment2-prompt1.png" width="600">


#### Prompt 2
We want to create employee shifts, splitting up the day into morning and evening. Add this to the ERD.

## ANSWER:

- <img src ="assignment2-prompt2.png" width="600">


#### Prompt 3
The store wants to keep customer addresses. Propose two architectures for the CUSTOMER_ADDRESS table, one that will retain changes, and another that will overwrite. Which is type 1, which is type 2? 

**HINT:** search type 1 vs type 2 slowly changing dimensions. 

## ANSWER:

The first architecture (type 2) will retain address changes and thus keep a historical record of customer addresses. Here, each customer is allowed to have multiple records, where a new row will be added for an address change and will be tracked through a version column. For example, this customer has recently moved to Toronto:

| customer_id | address                | city      | province | version |
|-------------|------------------------|-----------|----------|---------|
| 539         | 123 SQL Street         | Vancouver | BC       | 0       |
| 539         | 21 Bubblegum Boulevard | Toronto   | ON       | 1       |
 
The second architecture (type 1) will overwrite the previous address and will not track previous addresses. So, the same customer’s entry in the `CUSTOMER_ADDRESS` table will simply look like this:

| customer_id | address                | city      | province |
|-------------|------------------------|-----------|----------|
| 539         | 21 Bubblegum Boulevard | Toronto   | ON       |


***

## Section 2:
You can start this section following *session 4*.

Steps to complete this part of the assignment:
- Open the assignment2.sql file in DB Browser for SQLite:
	- from [Github](./02_activities/assignments/assignment2.sql)
	- or, from your local forked repository  
- Complete each question


### Write SQL

#### COALESCE
1. Our favourite manager wants a detailed long list of products, but is afraid of tables! We tell them, no problem! We can produce a list with all of the appropriate details. 

Using the following syntax you create our super cool and not at all needy manager a list:
```
SELECT 
product_name || ', ' || product_size|| ' (' || product_qty_type || ')'
FROM product
```

But wait! The product table has some bad data (a few NULL values). 
Find the NULLs and then using COALESCE, replace the NULL with a blank for the first column with nulls, and 'unit' for the second column with nulls. 

**HINT**: keep the syntax the same, but edited the correct components with the string. The `||` values concatenate the columns into strings. Edit the appropriate columns -- you're making two edits -- and the NULL rows will be fixed. All the other rows will remain the same.

<div align="center">-</div>

#### Windowed Functions
1. Write a query that selects from the customer_purchases table and numbers each customer’s visits to the farmer’s market (labeling each market date with a different number). Each customer’s first visit is labeled 1, second visit is labeled 2, etc. 

You can either display all rows in the customer_purchases table, with the counter changing on each new market date for each customer, or select only the unique market dates per customer (without purchase details) and number those visits. 

**HINT**: One of these approaches uses ROW_NUMBER() and one uses DENSE_RANK().

2. Reverse the numbering of the query from a part so each customer’s most recent visit is labeled 1, then write another query that uses this one as a subquery (or temp table) and filters the results to only the customer’s most recent visit.

3. Using a COUNT() window function, include a value along with each row of the customer_purchases table that indicates how many different times that customer has purchased that product_id.

<div align="center">-</div>

#### String manipulations
1. Some product names in the product table have descriptions like "Jar" or "Organic". These are separated from the product name with a hyphen. Create a column using SUBSTR (and a couple of other commands) that captures these, but is otherwise NULL. Remove any trailing or leading whitespaces. Don't just use a case statement for each product! 

| product_name               | description |
|----------------------------|-------------|
| Habanero Peppers - Organic | Organic     |

**HINT**: you might need to use INSTR(product_name,'-') to find the hyphens. INSTR will help split the column. 

2. Filter the query to show any product_size value that contain a number with REGEXP. 

<div align="center">-</div>

#### UNION
1. Using a UNION, write a query that displays the market dates with the highest and lowest total sales.

**HINT**: There are a possibly a few ways to do this query, but if you're struggling, try the following: 1) Create a CTE/Temp Table to find sales values grouped dates; 2) Create another CTE/Temp table with a rank windowed function on the previous query to create "best day" and "worst day"; 3) Query the second temp table twice, once for the best day, once for the worst day, with a UNION binding them. 

***

## Section 3:
You can start this section following *session 5*.

Steps to complete this part of the assignment:
- Open the assignment2.sql file in DB Browser for SQLite:
	- from [Github](./02_activities/assignments/assignment2.sql)
	- or, from your local forked repository  
- Complete each question

### Write SQL

#### Cross Join
1. Suppose every vendor in the `vendor_inventory` table had 5 of each of their products to sell to **every** customer on record. How much money would each vendor make per product? Show this by vendor_name and product name, rather than using the IDs.

**HINT**: Be sure you select only relevant columns and rows. Remember, CROSS JOIN will explode your table rows, so CROSS JOIN should likely be a subquery. Think a bit about the row counts: how many distinct vendors, product names are there (x)? How many customers are there (y). Before your final group by you should have the product of those two queries (x\*y). 

<div align="center">-</div>

#### INSERT
1. Create a new table "product_units". This table will contain only products where the `product_qty_type = 'unit'`. It should use all of the columns from the product table, as well as a new column for the `CURRENT_TIMESTAMP`.  Name the timestamp column `snapshot_timestamp`.

2. Using `INSERT`, add a new row to the product_unit table (with an updated timestamp). This can be any product you desire (e.g. add another record for Apple Pie). 

<div align="center">-</div>

#### DELETE 
1. Delete the older record for the whatever product you added.

**HINT**: If you don't specify a WHERE clause, [you are going to have a bad time](https://imgflip.com/i/8iq872).

<div align="center">-</div>

#### UPDATE
1. We want to add the current_quantity to the product_units table. First, add a new column, `current_quantity` to the table using the following syntax.
```
ALTER TABLE product_units
ADD current_quantity INT;
```

Then, using `UPDATE`, change the current_quantity equal to the **last** `quantity` value from the vendor_inventory details. 

**HINT**: This one is pretty hard. First, determine how to get the "last" quantity per product. Second, coalesce null values to 0 (if you don't have null values, figure out how to rearrange your query so you do.) Third, `SET current_quantity = (...your select statement...)`, remembering that WHERE can only accommodate one column. Finally, make sure you have a WHERE statement to update the right row, you'll need to use `product_units.product_id` to refer to the correct row within the product_units table. When you have all of these components, you can run the update statement.
*** 

## Section 4:
You can start this section anytime.

Steps to complete this part of the assignment:
- Read the article
- Write, within this markdown file, <1000 words.

### Ethics

Read: Boykis, V. (2019, October 16). _Neural nets are just people all the way down._ Normcore Tech. <br>
    https://vicki.substack.com/p/neural-nets-are-just-people-all-the

**What are the ethical issues important to this story?**

Consider, for example, concepts of labour, bias, LLM proliferation, moderating content, intersection of technology and society, ect. 


## ANSWER:

This article presents several ethical issues with neural nets. Though this article was written in 2019, these issues persist or have become even more widespread. One example highlighted int he article is how large-scale annotation work is often severely underpaid:

> But the dataset was, really, created by hundreds of thousands of people manually identifying what the pictures were.
> To date, more than 14 million images have been labeled by ImageNet, aka by people from around the world looking at images and clicking on buttons for cents.

I still often see these types of details omitted from publications presenting large-scale annotation and corpus work.

The data bias discussion (around the ImageNet Roulette part) is something I think about very frequently in my own research. I am a language researcher, and my research highlights how human heuristics are embedded in the “sophisticated reasoning” patterns of LLMs like GPT-4o. I give LLMs my experimental tasks to supplement my human data. In these tasks, humans and LLMs are asked to judge the intended referent of an ambiguous pronoun in cases like “Susan asked Amanda if she likes cooking new dishes” (inspired by Winograd Schemas)[^1]. My findings show that human readers near-uniformly agree on the intended character: In the example above, 100% of readers resolved the “she” as co-referring with “Amanda”. My follow-up work shows that we arrive at this understanding by making inferences about perspective: Susan is likely asking for novel information, and it would generally be weird for Susan to ask someone about her own preferences. In contrast, LLMs are terrible at resolving these cases. Unsurprisingly, human experiences, learning, world knowledge, and situational reasoning are extremely difficult to capture in LLMs. However, simply changing the names and gender of the characters highlights pervasive data bias issues in LLMs. I ran a study with hundreds of combinations of names (using typical and atypical baby names from different decades) and found extremely inconsistent patterns, even though the rest of the sentence stayed the same. I can’t see exactly how these decisions are made, so I ask the LLM to give me its reasoning for the decision, which often doesn’t even line up with its answer! However, my takeaway from this project is that there are very strong name and gender biases (e.g., the same sentence about cooking dishes above appears to be 50/50 with typical male names, rather than obvious bias to one or another female character - coincidence, or gender-driven bias for cooking?).

These LLMs are presented as highly sophisticated for complex tasks, garnering a false illusion of trust. However, human annotators introduce biases, and training over unrestrained data can lead to hallucinations, misinformation, and varied performance on many tasks (i.e., not all language benchmarking tasks and evaluations are performed well, like Winograd Schemas). These decisions are not transparent, and the black box of typical LLMs make it very difficult to diagnose why a decision was made, where the biases come from (e.g., in my own work, why does using the combination “Fred asked John” yield something very different from “John asked Fred”?), or the training that led to these responses, and who is moderating these models/what the documentation is around this.

[^1]: Inspired by [Winograd Schemas](https://cdn.aaai.org/ocs/4492/4492-21843-1-PB.pdf)

