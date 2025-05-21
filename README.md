# Global layoffs from 2019-2023

![First-ETL-EDA-Project](https://github.com/jdmavod/First-ETL-EDA-Project/blob/main/images/layoffs-work.png)

My first guided project that used a dataset wich included information on mass layoffs across a wide variety of companies, from large to small. The dates covered in this project were those during which the world was affected by COVID-19 in order to show how this impacted industries.

## Contents

- [Dataset](#dataset)
- [Goals](#goals)
- [Working with MySQL](#mysql)
- [EDA after cleaning the data](#eda)
- [Dashboard in Power BI](#powerbi)
- [Contribution](contribution)
- [License](#license)

## Dataset <a id="dataset"></a>
The dataset used was one from the youtuber Alex The Analyst, heres the link to the dataset (https://github.com/AlexTheAnalyst/MySQL-YouTube-Series/blob/main/layoffs.csv). From here I followed along starting from understanding the queries to applying them. This was my first introduccion to MySQL and it felt really smooth. I learned all the basics to some intermidiate queries like using CTE's.

## Goals <a id="goals"></a>
The main objective of this guided project was to learn the basics of SQL, from SELECT and FROM statements to advanced queries such as using CTEs and different types of JOINS. As a secondary objective, an exploratory data analysis was performed using the already cleaned data from the dataset to draw some conclusions and answer various questions.

## Working with MySQL <a id="mysql"></a>
![First-ETL-EDA-Project](https://github.com/jdmavod/First-ETL-EDA-Project/blob/main/images/sql-work.png)

Here I meticulously clean and prepare a dataset of global layoffs spanning late 2019 to early 2023 for subsequent Exploratory Data Analysis. The primary goal was to transform raw data into a reliable, standardized, and analysis-ready format. This report outlines the critical stages involved in the data cleaning and preparation process.<br>

<details>
  <summary>I. Initial Setup and Data Integrity</summary><br>
	Objective: To ensure the original dataset remained untouched and to provide a safe working environment for data manipulation.<br><br>
	Action: A duplicate of the original layoffs table, named layoffs_duplicate, was created. This common practice safeguards the source data, which is often live and utilized by multiple users in a business environment.<br><br>
</details>
<details>
  <summary>II. Duplicate Record Identification and Removal</summary><br>
	Objective: To enhance data accuracy by identifying and eliminating redundant records<br><br>
	Action:<br>
	1.- A Common Table Expression (CTE) was employed, utilizing the ROW_NUMBER() window function. This function partitioned data by key columns to assign a sequential integer to each row within its partition.<br><br>
	2.- Analysis of the CTE output revealed duplicate entries.<br><br>
	3.- To manage the cleaning process effectively, a third table, layoffs_duplicate2, was created. This table included the row_num column, facilitating targeted deletion of duplicates.<br><br>
	4.- Duplicate records (where row_num > 1) were then systematically removed from layoffs_duplicate2.<br><br>
</details>
<details>
	<summary>III. Data Standardization</summary><br>
	Objective: To ensure consistency in data representation across various fields, making the data more reliable and easier to analyze.<br><br>
	Action:<br>
	1.- Textual Data Normalization: Various columns underwent standardization. This involved TRIMming leading/trailing whitespace and correcting inconsistencies.<br><br>
	2.- Date Column Transformation: The date column, initially of a TEXT (string) data type, was converted to a DATE data type. This is crucial for accurate chronological analysis and time-series operations.<br><br>
</details>
<details>
	<summary>IV. Handling Missing and Null Values</summary><br>
	Objective: To address empty or null values systematically, thereby improving the dataset's completeness and analytical value.<br><br>
	Action:<br>
	1.- Empty string values in relevant columns were first converted to NULL to ensure uniform handling of missing data.<br><br>
	2.- A SELF JOIN was performed on the layoffs_duplicate2 table, using the company field as the join key. This allowed for the imputation of missing industry information by leveraging non-null industry data from other records pertaining to the same company.<br><br>
</details>
<details>
	<summary>V. Final Dataset Refinement</summary><br>
	Objective: To streamline the dataset for the upcoming EDA phase by removing columns deemed unnecessary or unreliable.<br><br>
	Action:<br>
	1.- total_laid_off and percentage_laid_off: These columns were removed due to a significant number of null values. Retaining them could lead to unreliable insights or misleading visualizations in a potential dashboard, thus they were considered redundant for the planned analysis.<br><br>
	2.- row_number: This column was auxiliary, created specifically for the duplicate removal process. Having served its purpose, it was no longer needed for the subsequent EDA.<br><br>
</details>

## Exploratory Data Analysis with clean data<a id="eda"></a>
![First-ETL-EDA-Project](https://github.com/jdmavod/First-ETL-EDA-Project/blob/main/images/eda-results.png)

To uncover initial patterns, trends, and insights from the cleaned global layoffs dataset. This EDA aimed to answer key questions regarding the scope, nature, and contributing factors of workforce reductions during the period of late 2019 to early 2023.<br>

<details>
	<summary>I. Overall Impact of Layoffs</summary><br>
	1.- Extreme Cases: A notable finding was the occurrence of companies laying off their entire workforce (100% of staff), including some that had previously secured significant funding.<br><br>
	2.- Critical Period: Layoffs were predominantly concentrated between 2020 and 2023, coinciding with the COVID-19 pandemic and its subsequent economic repercussions.<br><br>
	3.- Vulnerability Despite Funding: The data suggests that even well-funded businesses were not immune to the severe economic impact of the COVID-19 crisis, with some ceasing operations entirely.<br><br>
</details>
<details>
	<summary>II. Most Affected Industries</summary><br>
	1.- High-Impact Sectors: Consumer, Retail, and Transportation industries experienced the most substantial impact from layoffs. These sectors often rely on direct consumer interaction, which was heavily disrupted.<br><br>
	2.- Recurring Layoffs: Certain companies appeared repeatedly in top layoff figures across different years, potentially indicating deeper structural issues within those businesses or persistent crises in their respective sectors.<br><br>
</details>
<details>
	<summary>III. Geographic Impact</summary><br>
	1.- Leading Countries: The United States registered the highest total number of layoffs. European nations, including Germany and the United Kingdom, also featured prominently.<br><br>
	2.- Company Stage as a Factor: Post-IPO (publicly traded) companies accounted for a larger share of layoffs compared to early-stage startups. This suggests that larger, more established companies in competitive economies undertook significant cost-cutting measures.<br><br>
</details>
<details>
	<summary>IV. Temporal Trends</summary><br>
	1.- Initial Peak: The highest number of layoffs occurred in March 2020, at the onset of the COVID-19 pandemic.<br><br>
	2.- Subsequent Waves: While the initial shock was significant, smaller waves of layoffs were observed in 2022 and 2023, indicating a prolonged "ripple effect" of the initial crisis.<br><br>
	3.- Progressive Accumulation: The cumulative sum of layoffs demonstrated a substantial increase (48%) between 2020 and 2023, underscoring the sustained impact on the workforce.<br><br>
</details>
<details>
	<summary>V. Relationship Between Funding and Layoffs</summary><br>
	1.- Funding Paradox: A counterintuitive finding was that some companies laying off 100% of their staff had previously raised substantial capital (e.g., over $500 million).<br><br>
	2.- No Direct Correlation: The analysis did not reveal a clear, direct correlation between the amount of funding a company had raised and the extent of its layoffs. Companies with less funding were also observed to have survived.<br><br>
	3.- Insight: This suggests that capital injection alone is not a definitive safeguard against business failure or large-scale layoffs, and adaptability may play a more crucial role.<br><br>
</details>

This exploratory analysis highlighted significant trends and characteristics within the global layoffs dataset from late 2019 to early 2023. The findings provide a foundational understanding of the pandemic's impact on various industries, regions, and company types, and how factors like funding interacted with these events. These insights can serve as a basis for more in-depth investigation.

## Dashboard in Power BI <a id="powerbi"></a>
![First-ETL-EDA-Project](https://github.com/jdmavod/First-ETL-EDA-Project/blob/main/images/bi-dashboard.png)

This report highlights key insights from a survey conducted among 630 data professionals. The objective was to gain a deeper understanding of crucial aspects such as salary satisfaction, preferred programming languages, the perceived difficulty of entering and working in the data field, and overall job contentment. The following points detail the principal findings from this research.

<details>
	<summary>I. Average Salary by Job Role</summary>
	Data Scientists command the highest average salaries, closely followed by Data Engineers and Data Architects. This reflects the significant market demand for professionals skilled in extracting valuable insights and developing predictive models from complex datasets. Their advanced specialization, often encompassing deep knowledge of statistics, machine learning, and programming, typically justifies this higher compensation.<br><br>
	Conversely, roles such as Database Developer and positions held by students or those actively seeking employment show the lowest average salaries. This difference often mirrors the more operational nature of some roles or, in the case of students, a lack of consolidated work experience. While Database Developers are vital for data management, their specialization curve is often perceived as less steep compared to data science-focused roles.
</details>
<details>
	<summary>II. Geographic Distribution of Respondents</summary>
	The majority of the 630 survey participants reside in the United States. This demographic concentration is important because it suggests that the observed findings, particularly regarding salary benchmarks and job role hierarchies, are heavily influenced by US labor market conditions and cultural factors, and may not be directly generalizable to a global scale.<br><br>

Therefore, when interpreting conclusions, such as the salary leadership of Data Scientists, it's crucial to acknowledge this potential US-centric bias. In other countries with different costs of living or varying demand for data professionals, salary differences between roles might be less pronounced, or the ranking of roles by compensation could differ.
</details>
<details>
	<summary>III. Preferred Programming Languages</summary>
	Python stands out as the dominant programming language among the surveyed data professionals. This strong preference underscores Python's critical role as an essential tool in data analysis and data science, aligning with the high demand and compensation for Data Scientists and Data Engineers, particularly within the US market where most respondents are based.<br><br>

While other languages such as R, C/C++, JavaScript, and Java have a smaller footprint in the survey responses, a foundational or even specialized understanding of these languages can still offer competitive advantages. Their relevance may vary depending on the specific job role, industry, or the labor market conditions in different geographical regions.
</details>
<details>
	<summary>IV. Demographic Profile and Satisfaction Insights</summary>
	The average age of respondents is 30 years. Notably, while work-life balance satisfaction is moderate (5.74 out of 10), overall salary satisfaction is relatively low (4.27 out of 10). This discrepancy, even with roles like Data Scientists being well-compensated, suggests that compensation may not be meeting expectations across the board or that dissatisfaction is concentrated in lower-paying roles, posing potential talent retention challenges.<br><br>

Furthermore, the perceived difficulty of entering the data field or performing effectively within it varies, with a significant number of professionals finding it "Neither easy nor difficult" or "Difficult." This highlights a potential need for more effective training programs and mentorship opportunities. Addressing this could help bridge the gap between the high demand for data professionals and the current supply, especially for those in the earlier stages of their careers who form a large part of the respondent base.
</details>

In summary, while the data field highly values specialized roles like Data Scientists, the survey uncovers important considerations regarding overall salary satisfaction and the challenges of navigating a career in this domain. These findings emphasize the need for focused strategies on compensation, robust professional development, and accessible career pathways to ensure a thriving and sustainable talent pool.

## Contribution <a id="contribution"></a>
This project was my first in the field of data analysis. The work reflected here was my first personal project, where I learned a lot about the basics of SQL and Power BI. If you think anything you see here might help you, feel free to fork it for your own use.

## License <a id="license"></a>
The portfolio base site was created and edited using a template by HTML5UP.NET and the CCA 3.0 license, you can find the specifics below:

/*
	Dimension by HTML5 UP
	html5up.net | @ajlkn
	Free for personal and commercial use under the CCA 3.0 license (html5up.net/license)
*/


This is Dimension, a fun little one-pager with modal-ized (is that a word?) "pages" and a cool depth effect (click on a menu item to see what I mean). Simple, fully
responsive, and kitted out with all the usual pre-styled elements you'd expect. Hope you dig it :)

Demo images* courtesy of Unsplash, a radtastic collection of CC0 (public domain) images you can use for pretty much whatever.

(* = not included)

AJ
aj@lkn.io | @ajlkn

Credits:
	Demo Images:
		Unsplash (unsplash.com)
	Icons:
		Font Awesome (fontawesome.io)
	Other:
		jQuery (jquery.com)
		Responsive Tools (github.com/ajlkn/responsive-tools)
