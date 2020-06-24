# 1. Abstract

This report is released in order to provide data-driven decision for a board game company
launching its campaign on KickStarter, a funding platform for creative projects. The board game
company sets its pledging minimum at **$15,000 USD** , the goal setting strategy and backers
(donators) requirement are provided to serve the analysis purpose in **Section 4**.

The provided dataset records 15000 KickStarter campaigns from 23 different countries, 57 main
categories and 159 different sub categories with duration between September 2009 and
January 2018.

This report analyzed Board Game on KickStarter from the total **649 datasets** of two
subcategories named _Tabletop Games_ and _Playing Cards_ according to the schema. And the
average successful Board Game campaign goal is **$10,039.9** which result in **$55,849.5** average
pledged fundings. The success ratio of all Board Game is **51%** (331 out of 649), and some
guidances on Section 3 and 4 can be evaluated to launch the designated campaign more
effectively.

In this report, MySQL and Tableau are used for data retrieving and visualization purpose,
original files can be found as attachments to this report.

This analysis report is individual effort by the author, Zhenjie Zhou.

# 2. Problems Response

### 2.1.Average Pledge between successful and unsuccessful campaigns

The average pledged amount of successful campaigns is significantly larger than unsuccessful
campaigns. It is noticed that the average of successful board games campaigns raised at
55,849.5 dollars with average goal setting at 10,039.9 dollars.

On the contrary, the failed campaigns only averaged at 2,956 dollars and cancelled pledged
averaged at 3,539.5 dollars. Most of cancelled and failed projects are ceased without meeting
its initial goal, which averages at $16,529 and $19,607 respectively.

However, the average duration of each campaign is around 30 days, which shows irrelative
tendency on the success of each campaign. It is likely result from the rule of KickStart, the
posted campaign is required to show in the time windows of 30-40 days, no matter if the
pledging process in completed or not.

Figure.2 Origins of Board Game Campaigns shows that U.S. has largest accumulative
fundraising among all those 23 countries, which is over $16.3 millions, supporting 484 Board
Games campaigns in past decade.


### 2.2.Categories with most and least backers and fundings

‘Tabletop Games’, ’Anthologies’, ’Video Games’, are the top 3 categories with most backers
'247120', ‘221931' and ‘141052' respectively.

‘Anthologies’, ’Tabletop Games’, ’Video Games’ are the top 3 categories with most pledged
money ‘$21,111,581.59’, ‘$18,827,697.39’ and ‘$7,811,750.92’ respectively.

‘Glass’,’Photo’, and ‘Latin’ are the bottom 3 categories with least backers ‘2’,’12’,’13’
coordinately.

‘Glass’,’Crochet’, and ‘Latin’ are the bottom 3 categories with least pledged money
‘$150’,’$210.99’,’$268’ coordinately.

The average pledge duration remains around 30 days for most categories.

### 2.3.Board Game campaign

Figure.3 Popularity of Board Game on KickStarter shows that **Gloomhaven (Second Printing)**
is the most popular Board Game, gathering 40,642 supporters with total funding of almost $
millions.

### 2.4. Countries with most and least backers and fundings

United States, United Kingdom, and Canada are the top 3 countries with most backers
'1435673', ‘102446' and ‘34462' respectively, and those three countries have same order in sum
of pledged fundings ranking.

Norway, Luxembourg, and Austria are the bottom 3 countries of 23 recorded countries with least
backers '0', ‘98' and ‘682' respectively

Luxembourg, Norway, and Ireland are the bottom 3 countries of 23 recorded countries with least
fundings ‘$6,046.15', ‘$101,022.29’ and ‘$104,459.49’ respectively


# 3. Visualization
![Dashboard](https://github.com/0zz10/KickStarter/blob/master/Dashboard.png?raw=true)

# 4. Recommendation

By analyzing the ratio of amount pledged by amount of goal SQL#6, it is hard to conclude that
higher goal can lead to higher pledged money. The top 3 return of goal (ratio of pledged/goal) in
Board Game are ‘Endangered Orphans of Condyle Cove’, ‘THE TRAVELER DICE TOWER -
Custom Options + Storage!’ and ‘DUNGEONS of the MOUNTAIN KING’. It shows the goal sets
quite differently and backer numbers vary as well. It is advised that referring to its original
posting page to conclude its digital marketing. And this strategy can be implemented into the
designated project launch.

Figure.5 Board Game Campaigns in different outcomes and SQL#2 in Appendix shows that all
the successful Board Games campaigns launched their average goal sets at $10,039.9,
generating $55,849.5 in fund raising, so it is likely to set **initial goal to $10,000** and risk is
worthy to accomplish the minimum $15,000 requirement.

After executing SQL#7 in Appendix , the donation efforts per backer is highly related to the
project category, the **PB ratio** (average of backers to pledge ratio/ how many backers
contributing one dollar) for Tabletop Games and Playing Cards are **0.0376** and **0.**
respectively, an estimation of the total backers can be further made by $15,000 x BP ratio. For
average pledged amount close to $15,000, campaigns can expect **560-750 backers**.

# 5. Appendix

### 5.1. SQL source code

① Initial Digest
/*
campaign--> table a
category--> table b
category is not matched in campaign
sub_category--> table c
'Tabletop Games' id=14 and 'Playing Cards' id=70 are the sub category equivalent to 'Board
Games'
country--> table d
currency--> table e

Saved file as **board_games_649.csv**
*/

SELECT a.ID, a.name, c.name as sub_category, d.name as country_name, a.launched,
a.deadline, a.goal, a.pledged, e.name as currency, a.backers, a.outcome FROM campaign as a
JOIN sub_category as c ON a.sub_category_id=c.id AND (c.name='Tabletop Games' OR
c.name='Playing Cards')
JOIN country as d ON a.country_id=d.id
JOIN currency as e ON a.currency_id=e.id

ORDER by a.pledged DESC


② Section 2.
SELECT outcome,avg(goal) as target, avg(pledged) as raised, avg(datediff(deadline,launched))
as days
FROM campaign
WHERE sub_category_id=14 OR sub_category_id=
Group by outcome

③ Section 2.
SELECT d.name,avg(a.goal) as average_target, avg(a.pledged) as
average_raised,avg(datediff(a.deadline,a.launched)) as average_days
FROM campaign AS a
INNER JOIN country as d ON a.country_id=d.id
JOIN sub_category as c ON a.sub_category_id=c.id AND (c.name='Tabletop Games' OR
c.name='Playing Cards')
group by a.country_id
ORDER BY average_raised DESC

④ Section 2.
SELECT c.name,avg(a.backers) as average_backers, avg(a.goal) as average_target,
avg(a.pledged) as average_raised,avg(datediff(a.deadline,a.launched)) as average_days
FROM campaign AS a
JOIN sub_category as c ON a.sub_category_id=c.id AND (c.name='Tabletop Games' OR
c.name='Playing Cards')
group by a.sub_category_id


⑤ Section 2.
SELECT d.name,avg(a.backers) as average_backers, avg(a.goal) as average_target,
avg(a.pledged) as average_raised,avg(datediff(a.deadline,a.launched)) as average_days
FROM campaign AS a
INNER JOIN country as d ON a.country_id=d.id
JOIN sub_category as c ON a.sub_category_id=c.id AND (c.name='Tabletop Games' OR
c.name='Playing Cards')
group by a.country_id
ORDER BY average_backers DESC

⑥ Section 4
SELECT a.ID, a.name, c.name as sub_category, d.name as country_name, a.launched,
a.deadline, a.goal, a.pledged, e.name as currency, a.backers, a.outcome FROM campaign as a
JOIN sub_category as c ON a.sub_category_id=c.id AND (c.name='Tabletop Games' OR
c.name='Playing Cards')
JOIN country as d ON a.country_id=d.id
JOIN currency as e ON a.currency_id=e.id
WHERE a.pledged>

ORDER by a.pledged/a.goal DESC

⑦ Section 4
SELECT c.name,avg(a.backers) as average_backers, avg(a.goal) as average_target,
avg(a.pledged) as average_raised,avg(a.backers/a.pledged) as ratio_bp,
avg(datediff(a.deadline,a.launched)) as average_days


FROM campaign AS a
JOIN sub_category as c ON a.sub_category_id=c.id AND (c.name='Tabletop Games' OR
c.name='Playing Cards')
group by a.sub_category_id

### 5.2. References

[1] raw data: https://api.brainstation.io/content/link/1mKULpzSugp8pdursO0_NRs11r5CFKMXn
[2] BrainStation: https://brainstation.io/
[3] KickStarter: https://www.kickstarter.com/

### 5.3. Data Dictionary

Below is a list of variables in the dataset, and some contextual descriptions to get you started in
your analysis.

Schema of Table **campaign**

**- ID** : unique project ID
**- name** : project name
**- sub_category_id** : what industry/category was the project in?
**- country_id** : id number associated to country of origin
**- currency_id** : what currency funding was given in?
**- launched** : date fundraising began
**- deadline:** when target amount must be raised by
**- goal** : desired amount of funding
**- pledged** : how much was promised (whether or not the goal was reached)
**- backers** : how many individuals contributed to the campaign?
**- outcome** : was the project funded or not?
**-**
Schema of Table **sub_category
- ID** : **sub_category_id** in **campaign
- name** : sub_category^ corresponding

Schema of Table **country**

**- ID** : **country_id** in **campaign
- name** : country name^ corresponding

Schema of Table **currency**

**- ID** : **currency_id** in **campaign
- name** : currency^ corresponding
