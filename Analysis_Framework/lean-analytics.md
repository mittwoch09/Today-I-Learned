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
- Picking the OMTM(One Metric That Matters) lets you run more controlled experiments quickly and compare the results more effectively.
- When you’re focused on acquiring users (and converting them into customers), your OMTM may be tied to which acquisition channels are working best or the conversion rate from signup to active user. When you’re focused on retention, you may be looking at churn, and experimenting with pricing, features, improving customer support, and so on.
- Let’s look at four reasons why you should use the One Metric That Matters.
    - Let’s look at four reasons why you should use the One Metric That Matters.
    - It forces you to draw a line in the sand and have clear goals.
    - It focuses the entire company.
    - It inspires a culture of experimentation.
- When everyone rallies around the OMTM and is given the opportunity to experiment independently to improve it, it’s a powerful force.
- CASE STUDY - Solare Focuses on a Few Key Metrics
    - Randy explained when staffing costs exceed 30% of gross revenues, that’s bad, because it means that you’re either spending too much on staff or not deriving enough revenue per customer.
    - Restaurants know from experience that demand is tied to reservations, and what the right ratio of staffing to revenue should be.
    - Good metrics help predict the future, giving you an opportunity to anticipate problems and correct them.
- You need to pick a number, set it as the target, and have enough confidence that if you hit it, you consider it success. And if you don’t hit the target, you need to go back to the drawing board and try again.
- If you know that you need 10% of your users to sign up for the paid version of your site in order to meet your business targets, then that’s your number.
- Knowing an industry baseline means you know what’s likely to happen, and you can compare yourself to it.
- Optimizing your OMTM not only squeezes that metric so you get the most out of it, but it also reveals the next place you need to focus your efforts, which often happens at an inflection point for your business

### CHAPTER 7 - What Business Are you in?
- Business growth comes from improving one of these five “knobs”:
    - **More stuff** means adding products or services, preferably those you know your customers want so you don’t waste time building things they won’t use or buy.
    - **More people** means adding users, ideally through virality or word of mouth, but also through paid advertising.
    - **More often** means stickiness (so people come back), reduced churn (so they don’t leave), and repeated use (so they use it more frequently).
    - **More money** means upselling and maximizing the price users will pay, or the revenue from ad clicks, or the amount of content they create, or the number of in-game purchases they make.
    - **More efficiently** means reducing the cost of delivering and supporting your service, but also lowering the cost of customer acquisition by doing less paid advertising and more word of mouth.
- You need to segment real, valuable users from drive-by, curious, or detrimental ones. Then you need to make changes that maximize the real users and weed out the bad ones.
- Predicting revenues accurately relies on an understanding of how its different user segments employ the product.
- Not all customers are good. Don’t fall victim to customer counting. Instead, optimize for *good* customers and segment your activities based on the kinds of customer those activities attract.
- You can build business models this way, but instead of heads, torsos, and feet, you have several aspects of a business: the acquisition channel, selling tactic, revenue source, product type, and delivery model.
    - The **acquisition channel**channel is how people find out about you.
    - The **selling tactic** is how you convince visitors to become users or users to become customers.
    - The **revenue source** is simply how you make money.
    - The **product type** is what value your business offers in return for the revenue.
    - The **delivery model** is how you get your product to the customer.
    <img width="500" alt="Screen Shot 2022-05-30 at 12 19 39 PM" src="https://user-images.githubusercontent.com/73784742/170916402-f8939536-ea91-4a1b-bdfc-43501d2eba3f.png">

### CHAPTER 8 - Model One:E-commerce
- Early e-commerce models consisted of a relatively simple “funnel”: a visitor arrived at the site, navigated through a series of pages to get to a particular item, clicked “buy,” provided some payment information, and completed a purchase.
- To understand this, he calculates the annual repurchase rate: what percentage of people who bought something from you last year will do so this year?
    - Acquisition mode
        If less than 40% of last year’s buyers will buy this year, then the focus of the business is on new customer acquisition. Loyalty pro- grams aren’t good long-term investments for this kind of business.
    - Hybrid mode
        If 40–60% of last year’s buyers will buy this year, then the com- pany will grow with a mix of new customers and returning cus- tomers. It needs to focus on acquisition as well as on increasing purchase frequency—the average customer will buy 2 to 2.5 times a year.
    - Loyalty mode
        If 60% or more of last year’s buyers will buy something this year, the company needs to focus on loyalty, encouraging loyal clients to buy more frequently.
- Getting pricing right is critical—particularly if you’re an acquisition- mode e-commerce site that gets only one chance to extract revenue from a customer.
- The company cares about several key metrics:
    - Conversion Rate
        - Conversion rate is simply the percentage of visitors to your site who buy something.
        - Early on, conversion rate may even be more important than total revenue because your initial goal is to simply prove that someone will buy something.
    - Purchases Per year
        - If you look at the repurchase rate on a 90-day cycle, it becomes a very good leading indicator for what type of e-commerce site you have.
        - There’s no right or wrong answer, but it is important to know whether to focus more on loyalty or more on acquisition.
    - Shopping Cart Size
        - Not only do you want to know what percentage of people bought something, you also want to know how much they spent.
        - The key to successful e-commerce is in increasing shopping cart size; that’s really where the money is made.
    - Abandonment
        - The number of people who abandon a funnel at each of these stages is the abandonment rate.
        - It’s important to analyze it for each step in order to see which parts of the process are hurting you the most.
    - Cost of Customer Acquisition
        - Once you know you can extract money from visitors, you’ll want to drive traffic to the site.
        - E-commerce sites are simple math: make more from selling things than it costs you to find buyers and deliver the goods.
    - Revenue Per Customer
        - Even if your business doesn’t engender loyalty (because you’re selling something that’s infrequently purchased), you want to maximize revenue per customer.
        - Revenue per customer is really an aggregate metric of other key numbers, and represents a good, single measure of your e-commerce business’s health.
        - CASE STUDY : WineExpress Increases Revenue by 41% Per Visitor
            - WineExpress.com used A/B testing to find a better-converting page.
            - While conversion went up, the real gain was a 41% increase in revenue per visitor.
            - You want *high revenue per visitor*, or *high customer lifetime value* (CLV), because that’s what’s really driving your business model.
    - Keywords and Search terms
        - Most people find products by searching for them, whether that’s in a web browser, on a search engine, or within a site.
        - First, you want to be sure you have what people are after. If users are searching for something and not finding it—or searching, then pressing the back button—that’s a sign that you don’t have what they want.
        - Second, if a significant chunk of searches fall into a particular category, that’s a sign that you might want to alter your positioning, or add that category to the home page, to see if you can capture more of that market faster.
    - Recommendation Acceptance Rate
        - Some use what the buyer has purchased in the past; others try to predict purchases from visitor attributes like geography, referral, or what the visitor has clicked so far.
        - When you make adjustments to the recommendation engine, you’ll want to see if you moved the needle in the right direction.
    - Virality
        - It has the lowest cost of customer acquisition and the highest implied recommendation from someone the recipient trusts.
    - Mailing List Click-through Rates
        - More and more social applications are leveraging the power of email to drive repeat usage and retention.
        - You calculate the email click-through rate by dividing the number of visits you get from a campaign by the number of messages you’ve sent.
- Offline components of any e-commerce business need to be analyzed carefully.
    - E-commerce companies can most likely achieve significant operational efficiencies just by optimizing their fulfillment and shipping processes.
    - Improving your inventory management can make a big difference to your bottom line.
    - You can also hide these items from searches, or again, make sure they appear lower in the search results.

- The E-commerece Customer Lifecycle

<img width="400" alt="Screen Shot 2022-06-01 at 11 14 47 AM" src="https://user-images.githubusercontent.com/73784742/171320613-5c3aff04-764b-409e-a2ba-23b595a91d16.png">

- From an analytics perspective, this means tracking additional metrics for the rate of payment expiration, the effectiveness of renewal campaigns, and the factors that help (or hinder) renewal rates.
- Key takeaways
    - It’s vital to know if you’re focused on loyalty or acquisition. This drives your whole marketing strategy and many of the features you build.
    - Searches, both off- and on-site, are an increasingly common way of finding something for purchase.
    - While conversion rates, repeat purchases, and transaction sizes are important, the ultimate metric is the product of the three of them: revenue per customer.
    - Don’t overlook real-world considerations like shipping, warehouse logistics, and inventory.

### CHAPTER 9 - Model Two:Software as a Service(Saas)
- Most SaaS providers generate revenue from a monthly (or yearly) subscription that users pay.
- Finding the best mix of these tiers and prices is a constant challenge, and SaaS companies invest considerable effort in finding ways to upsell a user to higher, more lucrative tiers.
- The company cares about the following key metrics:
    - Attention : How effectively the business attracts visitors.
    - Enrollment : How many visitors become free or trial users, if you’re relying on one of these models to market the service.
    - Stickiness : How much the customers use the product.
    - Conversion : How many of the users become paying customers, and how many of those switch to a higher-paying tier.
    - Revenue per customer : How much money a customer brings in within a given time period.
    - Customer acquisition cost : How much it costs to get a paying user.
    - Virality : How likely customers are to invite others and spread the word, and how long it takes them to do so.
    - Upselling : What makes customers increase their spending, and how often that happens.
    - Uptime and reliability : How many complaints, problem escalations, or outages the company has.
    - Churn : How many users and customers leave in a given time period.
    - Lifetime value : How much customers are worth from cradle to grave.
- CASE STUDY : Backupify’s Customer Lifecycle Learning
    - Before focusing on sophisticated financial metrics, start with revenue. But don’t ignore costs, because profitability is the real key to growth.
    - You know it’s time to scale when your paid engine is humming along nicely, which happens when the CAC is a small fraction of the CLV—a sure sign you’re getting a good return on your investment.
    - Most SaaS businesses thrive on monthly recurring revenue— customers continue to pay month after month—which is a great foundation on which to build a business.
- If there’s a subsection of users who are hooked on your product—your early adopters—figure out what’s common to them, refocus on their needs, and grow from there.
- When measuring engagement, don’t just look at a coarse metric like visit frequency. Look for usage patterns throughout your application.
- Finding these engagement patterns means analyzing data in two ways:
    - If you find a concentration of desirable behavior in one segment, you can then target it.
    - To decide whether a change worked, test the change on a subset of your users and compare that subset’s results to others.
- A data-driven approach to measuring engagement should show you not only *how sticky* your product or service is, but also *who stuck and whether your efforts are paying off*.
- Churn is the percentage of people who abandon your service over time.
    (Number of churns during period) / (# customers at beginning of period)
- Churn isn’t normalized for behavior and size—you can get different churn rates for the same kind of user behavior if you’re not careful.
    (Number of churns during period) / ([(# customers at beginning of period)+(# customers at end of period)] / 2)
- The first is to measure churn by cohort, so you’re comparing new to churned users based on when they first became users.
- The second way is really, really simple, which is why we like it: measure churn each day. The shorter the time period you measure, the less that changes during that specific period will distort things.

<img width="400" alt="Screen Shot 2022-06-04 at 5 11 02 PM" src="https://user-images.githubusercontent.com/73784742/171992714-2cca41d1-377b-4154-aa01-4d241c9e27db.png">

- CASE STUDY : ClearFit Abandons Monthly Subscriptions for 10x growth
    - ClearFit initially focused on a subscription model for revenue, but customers misinterpreted its low pricing as a sign of a weak offering.
    - The company switched to a paid listing model, and tripled sales while improving revenue tenfold
    - The problem wasn’t the business model—it was the pricing and the messages it sent to prospects.
- In a SaaS model, most of the complexity comes from two things: the promotional approach you choose, and pricing tiers.
- Key takeaways
    - While freemium gets a lot of visibility, it’s actually a sales tactic, and one you need to use carefully.
    - In SaaS, churn is everything. If you can build a group of loyal users faster than they erode, you’ll thrive.
    - You need to measure user engagement long before the users become customers, and measure customer activity long before they vanish, to stay ahead of the game.
    - Many people equate SaaS models with subscription, but you can monetize on-demand software in many other ways, sometimes to great effect.

### CHAPTER 10 - Model Three:Free Mobile App
