# Summary of the book "Lean Analytics: Use Data to Build a Better Startup Faster"

## PART ONE: STOP LYING TO YOURSELF

### CHAPTER 1 - We're All Liars
- Entrepreneurs are particularly good at lying to themselves. Lying may even be a prerequisite for succeeding as an entrepreneur.
- Analytics is the necessary counterweight to lying.
- Morevoer, data-driven learning is the cornerstone of success in startups.
- *Instincts are experiments. Data is proof.*
- Lean Startup provides a framework by which you can more rigorously go about the business of creating something new.
- CASE STUDY : Airbnb Phothography - Growth Within Growth
    - Aribnb's team had a hunch that better photos would increase rentals.
    - They tested the idea with a Concierge MVP(Minimum Viable Product), putting the least effort possible into a test that would give them valid results.
    - When the experiment showed good results, they bulit the necessary components and rolled it out to all customers.
- When you think you've found a worthwhile idea, decide how to test it quickly, with minimal investment.
- Lean, analytical thinking is about asking the right questions, and focusing on the one key metric that will produce the change you're after.

### CHAPTER 2 - How to Keep Score
- What Makes a Good Metric?
    - A good metric is comparative.
    - A good metric is understandable.
    - A good metric is a ratio or a rate 
        - Ratios are easier to act on.
        - Ratios are inherently comparative.
        - Ratios also good for comparing factors that are somehow opposed, or for which there's an inherent tension.
    - A good metric changes the way you behave.
- Metrics often come in pairs.
    - *Conversion rate* (the percentage of people who buy something) is tied to *time-to-purchase*(how long it takes someone to buy something). Together, they tell you a lot about your cash flow.
    - *viral coefficient*(the number of people a user successfully invites to your service) and *viral cycle*(how long it takes them to invite others) drive your adoption rate.
- Analysts look at specific metrics that drive the business, called *Key Performance Indicators* (KPIs).
    - Qualitative vs Quantitative Metrics
        - Quantitiative data is easy to understnad. It's the numbers we track and measure-for example, sports scores and movie ratings.
        - Qualitative data is messy, subjective, and imprecise. It's the stuff of interviews and debates.
    - Vanity vs Real Metrics
        - Whenever you look at a metric, ask yourself, "What will I do differently based on this information?"
        - Consider, for example, "total signups." This is a vanity metric. It tells us nothing about what those users are doing or whether they're valuable to us.
        - The real metric of interes-the *actionable* one-is "percent of users who are active." This is a critical metric because it tells us about the level of engagement your users have with your product.
    - Exploratory vs Reporting Metrics
       - The "known unknowns" is a reporting posture-counting money, or users, of lines of code. We *know* we don't know the value of the metric, so we go find out.
       - The "unknown unknowns" are most relevant to startups: exploring to discover something new that will help you disrupt a market.
       - CASE STUDY : Circle of Moms Explores Its Way to Success
            - Circle of Friends was a social graph application in the right place at the right time-with the wrong market.
            - By analyzing patterns of engagement and desirable behavior, then finding out what those users had in common, the company found the right market for its offering.
            - Once the comapny had found itss target, it focused-all the way to changing its name. Pivot hard or go home, and he prepared to burn some bridegs.
    - Leading vs Lagging Metrics
        - A leading metric (sometimes called a *leading indicator*) tries to predict the future.
        - A lagging metric, such as *churn* (which is the number of customers who leave in a given time period) gives you an indication that there's a problem-but by the time you're able to collect the data and identify the problem, it's too late.
        - Lagging metrics are still useful and can provide a solid baseline of performance. For leading indicators to work, you need to be able to do cohort analysis and compare groups of customers over periods of time.
    - Correlated vs Casual Metrics
        - Correlations can help you predict what will happen. But finding the *cause* of something means you can change it. Usually, casuations are't simple one-to-one relationships. Many factors conspire to cause something.
        - You prove causality by finding a correlation, then running an experiment in which you control the other variables and measure the difference.
        - If you have a big enough sample of users, you can run a reliable test without controlling all the other variables, because eventually the impact of the other variables is relatively unimportant.
- Adjusting your goals and how you define your key metrics is acceptable, provided that you're being honest with yourself, recognizing the change this means for your business, and not just lowering expectations so that you can keep going in spite of the evidence.
- CASE STUDY : HighScore House Defines an "Active User"
    - HighScore House drew an early, audacious line in the sand-which it couldn't hit.
    - The team experimented quickly to improve the number of active users but couldn't move the needle enough.
    - They picked up the phone and spoke to customers, realizing that they were creating value for a segment of users with lower usage metrics.
- Testing is at the heart of Lean Analytics. Testing usually involves comparing two things against each other through segmentation, cohort analysis, or A/B testing. 
    - Segmentation
        - A segment is simply a group that shares some common characteristic.
        - On websites, you segment visitors according to a range of technical and demographic information, then compare one segment to another.
    - Cohort Analysis
        - A second kind of analysis, which compares similar groups over time, is cohort analysis.
        - Users who join you in the first week will have a different experience from those who join later on.
        - Imagine that you’re running an online retailer. Each month, you acquire a thousand new customers, and they spend some money.
        
            |                        |January|February|March|April|May |
            |------------------------|-------|--------|-----|-----|----|
            |Total customers         |1000   |2000    |3000 |4000 |5000|
            |Avg revenue per customer|5.00   |4.50    |4.33 |4.25 |4.50|

        - Since you aren’t comparing recent customers to older ones—and because you’re commingling the purchases of a customer who’s been around for five months with those of a brand new one—it’s hard to tell.

            |           |January|February|March|April|May |
            |-----------|-------|--------|-----|-----|----|
            |New users  |1000   |1000    |1000 |1000 |1000|
            |Total users|1000   |2000    |3000 |4000 |5000|
            |Month 1    |5.00   |3.00    |2.00 |1.00 |0.50|
            |Month 2    |       |6.00    |4.00 |2.00 |1.00|
            |Month 3    |       |        |7.00 |6.00 |5.00|
            |Month 4    |       |        |     |8.00 |7.00|
            |Month 5    |       |        |     |     |9.00|

        - Customers who arrived in month five are spending, on average, $9 in their first month—nearly double that of those who arrived in month one. That’s huge growth!

            |        |    |Month|of  |use |    |
            |--------|----|-----|----|----|----|
            |Cohort  |1   |2    |3   |4   |5   |
            |January |5.00|3.00 |2.00|1.00|0.50|
            |February|6.00|4.00 |2.00|1.00|    |
            |March   |7.00|6.00 |5.00|    |    |
            |April   |8.00|7.00 |    |    |    |
            |May     |9.00|     |    |    |    |
            |Averages|7.00|5.00 |3.00|1.00|0.50|
        
        - Another way to understand cohorts is to line up the data by the users’ experience. This shows another critical metric: how quickly revenue declines after the first month.
        - This kind of reporting allows you to see patterns clearly against the lifecycle of a customer, rather than slicing across all customers blindly without accounting for the natural cycle a customer undergoes.
    - A/B and Multivariate Tesing
        - Studies in which different groups of test subjects are given different experiences at the same time are called cross-sectional studies. 
        - When we’re comparing one attribute of a subject’s experience, such as link color, and assuming everything else is equal, we’re doing A/B testing.
        - Rather than running a series of separate tests one after the other—which will delay your learning cycle—you can analyze them all at once using a technique called *multivariate analysis*. This relies on statistical analysis of the results to see which of many factors correlates strongly with an improvement in a key metric.

### CHAPTER 3 - Deciding What to Do with Your Life
- The Lean Canvas is fantastic at identifying the areas of biggest risk and enforcing intellectual honesty.
    1. Problem
    2. Customer segements
    3. Unique value proposition
    4. Solution
    5. Channels
    6. Revenue streams
    7. Cost structure
    8. Metrics
    9. Unfair advantage

### CHAPTER 4 - Data-Driven Versus Data-Informed
- Math is good at optimizing a known system; humans are good at finding a new one. Put another way, *change favors local maxima; innovation favors global disruption*.
- If you’re optimizing for local maxima, you might be missing a bigger, more important opportunity. It’s your job to be the intelligent designer to data’s evolution.
- Ultimately, quantitative data is great for testing hypotheses, but it's lousy for generating new ones unless combined with human introspection.
-Lean Startup is focused on learning above everything else, and encourages broad thinking, exploration, and experimentation. It’s not about mindlessly going through the motions of build->measure->learn—it’s about really understanding what’s going on and being open to new possibilities.

## PART TWO: FINDING THE RIGHT METRIC FOR RIGHT NOW

### CHAPTER 5 - Analytics Frameworks
- Dave McClure’s Pirate Metrics
    - Acquisition : Generate attention through a variety of means, both organic and inorganic(traffic, mentions, cost per click, search results, cost of acquisition, open rate)
    - Activations : Turn the resulting drive-by visitors into users who are somehow enrolled(enrollments, signups, com- pleted onboarding process, used the service at least once, subscriptions)
    - Retention : Convince users to come back repeatedly, exhibiting sticky behavior(engagement, time since last visit, daily and monthly active use, churns)
    - Revenue : Business outcomes (which vary by your business model: purchases, ad clicks, content creation, subscriptions, etc.)(customer lifetime value, conversion rate, shopping cart size, click-through revenue)
    - Referral : Viral and word-of-mouth invitations to other potential users(invites sent, viral coefficient, viral cycle time)
- Eric Ries’s Engines of growth
    - Sticky Engine
        - The sticky engine focuses on getting users to return, and to keep using your product.
        - The fundamental KPI for stickiness is customer retention.
        - Stickiness isn’t only about retention, it’s also about frequency, which is why you also need to track metrics like time since last visit.
    - Virality Engine
        - Virality is all about getting the word out.
        - The key metric for this engine is the *viral coefficient*—the number of new users that each user brings on.
        - Those distinct stages all contribute to virality, so measuring actions is how you tweak the viral engine—by changing the message, simplifying the signup process, and so on.
    - Paid Engine
        - The third engine of growth is payment.
        - If you make more money from customers than it costs you to acquire them—and you do so consistently—you’re sustainable.
    - Ash Maurya’s Lean Canvas
        - Once you’ve filled out the Lean Canvas (or most of it), you start running experiments to validate or invalidate what you’ve hypothesized.
            1. Problem : Respondents who have this need, respondents who are aware of having the need
            2. Solution : Respondents who try the MVP, engagement, churn, most-used/least-used features, people willing to pay
            3. Unique value proposition : Feedback scores, independent ratings, sentiment analysis, customer-worded descriptions, surveys, search, and competitive analysis
            4. Customer segments : How easy it is to find groups of prospects, unique keyword segments, targeted funnel traffic from a particular source
            5. Channels : Leads and customers per channel, viral coefficient and cycle, net promoter score, open rate, affiliate margins, click-through rate, Pagerank, message reach
            6. Unfair advantage : Respondents’ understanding of the UVP (Unique Value Proposition), patents, brand equity, barriers to entry, number of new entrants, exclusivity of relationships
            7. Revenue streams : Lifetime customer value, average revenue per user, conversion rate, shopping cart size, click-through rate
            8. Cost structure : Fixed costs, cost of customer acquisition, cost of servicing the nth customer, support costs, keyword costs
    - Sean Ellis’s Startup growth Pyramid
        - PRODUCT/MARKET FIT : Decide what you sell to whom, then prove it.
        - STACK THE ODDS : Find a defensible unfair advantage and tweak it.
        - SCALE GROWTH : Step on the gas ing new markets, products, and channels.
    - The Long Funnel
        - It’s a way of understanding how you first come to someone’s attention, and the journey she takes from that initial awareness through to a goal you want her to complete (such as making a purchase, creating content, or sharing a message).
        - Often, measuring a long funnel involves injecting some kind of tracking into the initial signal, so you can follow the user as she winds up on your site, which many analytics packages can now report.

    <img width="460" alt="Screen Shot 2022-05-28 at 2 43 40 PM" src="https://user-images.githubusercontent.com/73784742/170813881-9c035544-5d0a-4f10-8b21-c4ef26b44056.png">

- Some, like Pirate Metrics and the Long Funnel, focus on the act of acquiring and converting customers.
- Others, like the Engines of Growth and the Startup Growth Pyramid, offer strategies for knowing when or how to grow.
- Some, like the Lean Canvas, help you map out the components of your business model so you can evaluate them independent of one another.

### CHAPTER 6 : The Discipline of One Metric That Matters
- You might make your product sticky for Its core users, then use that to grow virally, and then use the user base to grow revenue. That’s focus.
- Don’t let your ability to track so many things distract you. Capture everything, but focus on what’s important.
- CASE STUDY : Moz Tracks Fewer KPIs to Increase Focus
    - Moz is metrics-driven but that doesn't mean it's swimming in data. It relies on one metric above all others:Net Adds.
    - One of its investors actually suggested *reducing* the number of metrics the company tracks to stay focused on the big picture.
    - While it’s great to track many metrics, it’s also a sure way to lose focus. Picking a minimal set of KPIs on which your business assumptions rely is the best way to get the entire organization moving in the same direction.

