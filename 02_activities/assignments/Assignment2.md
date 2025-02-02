# Assignment 2: Design a Logical Model and Advanced SQL

🚨 **Please review our [Assignment Submission Guide](https://github.com/UofT-DSI/onboarding/blob/main/onboarding_documents/submissions.md)** 🚨 for detailed instructions on how to format, branch, and submit your work. Following these guidelines is crucial for your submissions to be evaluated correctly.

#### Submission Parameters:
* Submission Due Date: `February 1, 2025`
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

#### Prompt 2
We want to create employee shifts, splitting up the day into morning and evening. Add this to the ERD.

#### Prompt 3
The store wants to keep customer addresses. Propose two architectures for the CUSTOMER_ADDRESS table, one that will retain changes, and another that will overwrite. Which is type 1, which is type 2? 

**HINT:** search type 1 vs type 2 slowly changing dimensions. 

```
Retain changes is type 2, overwrite is type 1.
type1: customer id, addresses
type2: customer id, current address, previous address, start_date for the address, end_date for the address.
```

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
Find the NULLs and then using COALESCE, replace the NULL with a blank for the first problem, and 'unit' for the second problem. 

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


```
The increasing integration of artificial intelligence (AI) and machine learning models into daily life presents a myriad of ethical challenges that often go unnoticed by the general public. In her article “Neural Nets Are Just People All the Way Down”, Vicki Boykis explores how neural networks, which are often hailed as autonomous or self-sustaining entities, are fundamentally reliant on human labor. She highlights critical ethical issues such as exploitation, bias, accountability, and the potential for harm in the proliferation of large language models (LLMs). This paper critically examines these concerns, illustrating how these issues intersect with societal inequalities, corporate responsibility, and the potential dangers posed by the widespread use of AI technologies.

One of the most pressing ethical issues discussed by Boykis is the exploitation of labor in the development of neural networks. While AI models such as neural nets may seem autonomous, they depend heavily on human workers to curate, label, and moderate the massive datasets that train these systems. Boykis argues that the general public tends to overlook the invisible labor that underpins the development of these technologies, which is often outsourced to workers in low-wage countries. Many of these workers are tasked with annotating images, moderating offensive content, or providing other services necessary for the training of AI systems.

This labor is not only underpaid but also emotionally taxing. Moderators, for example, often work with disturbing or harmful content, which can have significant psychological effects. Despite the enormous role these workers play in creating highly functional AI systems, they are often rendered invisible in discussions about the ethics of AI. The failure to recognize and properly compensate this labor speaks to a broader issue of labor exploitation in the tech industry, which raises significant ethical questions about the fairness and human cost of technological innovation.

Another key ethical concern in Boykis’ article revolves around the issue of bias in AI systems. Neural networks and large language models (LLMs) are trained on data collected from human sources. These datasets often reflect the biases, prejudices, and historical inequalities present in society. For instance, biased training data may result in algorithms that disproportionately disadvantage marginalized groups, reinforcing harmful stereotypes about race, gender, or socioeconomic status. Boykis argues that neural nets are not neutral; rather, they are shaped by the values and assumptions embedded in the data used to train them.

These biases can have real-world consequences, particularly in systems that make critical decisions about hiring, lending, policing, and healthcare. A hiring algorithm, for example, may prefer male candidates over female candidates, or a loan application algorithm may discriminate against people of color based on biased historical data. These issues underscore the ethical imperative for developers to actively address and mitigate bias in AI systems to ensure they do not perpetuate or exacerbate existing societal inequalities.

As AI systems, particularly large language models (LLMs), become more ubiquitous, there are growing concerns about the potential risks they pose in terms of content moderation and the spread of misinformation. Boykis notes that these models, while capable of generating impressively coherent and human-like text, can also be misused to spread disinformation, create fake news, and impersonate individuals. The ability of LLMs to generate persuasive text makes them powerful tools for malicious actors seeking to manipulate public opinion or deceive the public.

The ethical implications of LLM proliferation extend beyond just the creation of false content; they also raise questions about the control of information. Who decides what information is disseminated by AI systems? Companies that develop these technologies, such as OpenAI, Google, and others, hold significant power over what content is generated and shared. Given the reach of AI-generated content, this concentration of power raises concerns about the potential censorship or biased dissemination of information, where certain voices and perspectives may be suppressed or amplified based on corporate interests.

A critical ethical issue in the deployment of neural networks and LLMs is the question of accountability. Boykis draws attention to the challenges associated with determining responsibility when AI systems make harmful or biased decisions. If an AI system recommends the wrong candidate for a job or denies a loan to an individual based on biased data, who is responsible? The developers of the system? The corporations deploying it? Or the AI itself? The lack of transparency in many AI systems makes it difficult to pinpoint the source of the problem, complicating efforts to hold anyone accountable.

The question of accountability becomes even more pressing as AI systems gain more autonomy and decision-making capabilities. If these systems are capable of making high-stakes decisions without human intervention, it becomes ethically problematic to absolve developers and companies of responsibility when things go wrong. Moreover, the lack of explainability in many neural networks — often described as the “black box” problem — makes it difficult for stakeholders to understand how these models arrive at their conclusions. This lack of transparency exacerbates concerns about accountability and calls for greater regulation and oversight of AI technologies.

The Intersection of Technology and Society
The ethical issues surrounding neural networks and AI are not only technological but also deeply societal. Boykis underscores the political nature of AI, highlighting how these technologies reflect the interests and values of the people and corporations that design them. The deployment of AI systems can have far-reaching consequences for societal structures, particularly in areas such as education, employment, and healthcare. The question of who benefits from AI advancements — and who is left behind — is critical to understanding the ethical ramifications of these technologies.

As AI systems become more embedded in society, they risk entrenching existing inequalities. For example, people without access to advanced AI tools may find themselves marginalized in an increasingly automated world. Moreover, AI systems that prioritize corporate profit over the public good may exacerbate social inequalities, raising important ethical concerns about the intersection of technology, capitalism, and social justice.

The ethical issues surrounding neural networks and large language models are complex and multifaceted. From labor exploitation and data bias to the risks of misinformation and accountability, these technologies present significant challenges that require careful consideration. As AI continues to evolve, it is crucial that developers, policymakers, and society at large address these ethical issues, ensuring that AI systems are developed and deployed in ways that benefit all people, not just those who control the technology. Only by confronting these challenges head-on can we create a future where AI serves the public good rather than exacerbating existing inequalities and injustices.
```
