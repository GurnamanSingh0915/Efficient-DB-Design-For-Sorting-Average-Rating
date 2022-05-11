# Efficient-DB-Design-For-Sorting-Average-Rating
Finding the most optimized algorithm used for sorting in popular social websites.

Efficient DB Design For Sorting Average Rating
 
Gurnaman Singh
University of Arkansas
Bentonville, USA
gs031@uark.edu






 
 
Abstract—Ever since popular websites implemented rating systems to analyze sentiments, tuning the database design algorithm to better represent a subset of population has become a priority. Sites like Twitter, YouTube, Reddit, and Yelp have their own ideal formula to rank user input values. However, each algorithm has its upsides and downsides depending on the use case.  Different use cases lead to uncertainty in best possible rating algorithm, but it can be concluded that the all-rounder formula turns out to be established by a website called SteamDB and other renowned research theories on different platforms, which has high accuracy and efficiency. (Abstract) 
I.	INTRODUCTION (HEADING 1)
Over the years, big data scientists have tried to come up with a revolutionary algorithm to optimize the sorted dynamic rendered data for various users across many platforms. From product ratings to video views and post likes, when users rate their sentiment on any given instance, there needs to be some sort of “score” to sort the highest-rated listings on the top and lowest-rated listings on the bottom. This paper will explore different kinds of solutions to the proposed problem of ability to effectively sort by average rating. The sections of this research can be categorized by: (1) attempts at wrong solution, (2) utilizing proven methodologies that work, and (3) finding more effective solutions that challenge old mathematical findings. Understanding the minute variances in each method is critical to pinpointing the formula that produce the best results on a system with bottlenecked performance. Due to the sheer complexity of the mathematics utilized in deriving the final application of the implementation, only the snippets of the mathematical functions will be provided without comprising the material overall. 
II.	ATTEMPTS AT THE WRONG SOLUTION
A.	Common Misconception 
Contrary to the popular belief, a lot of websites made major algorithmic mistakes in the beginning stages of their company. A particular website known by the name of Urban Dictionary assumed that to find the score, positive ratings must be subtracted with the negative ratings to attain the net as shown below: 
Score = (Positive ratings) − (Negative ratings)

To provide an analysis of why this proposed solution is wrong can be explained by a simple analogy. If assumed that a word definition has 600 positive ratings and 400 negative ratings, the result would be 60 percent positive. In comparison, if another word definition has 5,500 positive ratings and 4,500 negative ratings, the result would be 55 percent positive. Urban Dictionary would utilize the same algorithm and consider the second word definition of score (1000) and 55 percent positive to be sorted on higher priority than first word definition of score (200) and 60 percent positive [1]. This is inaccurate on a massive scale and misinforming to the users. 
 
Miller, Evan. “How Not to Sort by Average Rating.” – Evan Miller, https://www.evanmiller.org/how-not-to-sort-by-average-rating.html. Depiction of inaccuracy in Urban Dictionary’s rating algorithm.
	Another unusual algorithm was spotted in Amazon.com, which has since corrected its algorithm thanks to machine learning. Billions of customers base their daily purchase decisions on rating systems and to see even the biggest franchise make this common mistake is often astonishing. Amazon ranked its products based on the simple score ratio of positive ratings and total ratings as shown below:
	Score = Average rating = (Positive ratings) / (Total ratings)
	While this formula works on a bigger scale of input, it most definitely fails if a product does not have enough ratings. Consider a scenario where product 1 has 2 positive ratings and 0 negative ratings. Even if another product has 100 positive ratings and 1 negative rating, this algorithm puts product 2 below product 1 when sorted in terms of priority recommendation. Prioritizing a product with fewer number of positive ratings above a product with greater amount of positive ratings does not make sense and is demonstrated in the Amazon vintage image [1].
 
B.	Adding Complexity To The Mix
In the early stages of every website, it was critical to get the minimum viable product (MVP) up and running as a business to start racking in revenue fast for investors funding. Keeping things simple in the early stages is critical to scaling later and adding more expert employees to help improve the platform. Therefore, as companies like Urban Dictionary and Amazon got bigger in terms of market capital, complicated tasks were delegated to experts, who utilized complicated mathematics in database schematics to reach a better result.
III.	UTILIZING PROVEN METHODOLGIES THAT WORK
A.	Wilson Lower Bound Score
Sometimes, sticking to old and boring mathematics got the job done and it was the American mathematical statistician Edwin Bidwell Wilson in 1927, who was able to derive the lower bound of Wilson score confidence interval for a Bernoulli parameter [2]. An ideal algorithm is the one where the score is a statistical sampling of a hypothetical set of customer input. It can be said that an existing product with a sample community has a 95% confidence in the rating. Therefore, the function considers how the user community reacts to that 95% confidence. The mathematical literature to explain this is given by:
 
https://www.evanmiller.org/how-not-to-sort-by-average- rating.html
where p̂ = (# of positive ratings)/(Total ratings)
      n = Total ratings
      Z_(α/2)=  Quantile of the standard normal distribution
The following are examples to correct the Amazon and Urban Dictionary sorted rating output in accordance with the above formula: 
[10, 10, 10, 10, 10] =>
10 votes for rating {1–5}, then wilson_lower_bound(20,50,.95)
avg_rating([10, 10, 10, 10, 10]) => (0.2760838973025655, 3.0)
			        Another Example
	In an instance where one item receives 1 positive and 1 negative rating, while another item receives 10 positive and 2 negative ratings, it is evident that product with more positive ratings should be ranked higher as per Wilson formula.
wilson_lower_bound(1,1,0.95)< wilson_lower_bound(10,12,0.95) , which is true.
Another Example
	(209 up and 50 down votes) and B: (118 up and 25 down) wilson_lower_bound(209,259,0.95)< wilson_lower_bound(118,143,0.95)
Another Example
Product One => [5, 10, 20, 0, 0] ratings, then wilson_lower_bound(0,35,0.95) = 0. This is accurate because a product with zero ratings should have this result.
B.	Bayesian Approximation for K scale rating
Compared to the Wilson Lower Bound Score, Bayesian is preferred by many due to its flexibility in crucial variables such as time, prior beliefs, and attitude towards risks. These are variables that many websites value and model to explicitly account for in a rating system.
Time is an important variable to consider as certain applications find that old ratings should have less priority than new ratings. For prior beliefs, it could be important to have unrated item from bestselling author appear as higher priority in comparison to unrated item from unknown author. In terms of attitude towards risk, it makes sense to specify the scale at which items with few ratings should be promoted while avoiding skewing the results.
The Bayesian framework takes these factors and can be derived as a mathematical figure shown below: 
 
 
https://www.evanmiller.org/bayesian-average-ratings.html

	 
Bayesian average constitutes of a basic idea of the products and the average scores those products should attain. Essentially, each review impacts the overall figure by a little margin instead of immediately banishing the product rating to the bottom of the ranking based on one review or immediately ranking it right at the top.
IV.	FINDING MORE EFFECTIVE SOLUTIONS THAT CHALLENGE OLD MATHEMATICAL FINDINGS
    As always, researchers come up with more efficient models that reflect the data in a manner that leads to better estimate of the ranking scores and a website known as SteamDB succeeds in doing that with its simplistic formula. SteamDB prioritizes the following:
•  Games with more reviews should have an adjusted rating that is closer to their real rating, because the more reviews we have, the more certain we are that the score they give us is correct.

There was no approach utilized in deriving the formula and it was a eureka moment in its discovery. However, the formula works better than both Bayesian and Wilson based on a rating and the sample size (the number of reviews) and outputting a confidence interval. SteamDB’s formula is as follows:
( Total Reviews = Positive Reviews + Negative Reviews )
( Review Score = frac{Positive Reviews}{Total Reviews} )
( Rating = Review Score - (Review Score - 0.5)*2^{-log_{10}(Total Reviews + 1)} )

•  All ratings should be biased towards the average rating — 50%.

     Considering this, SteamDB wanted to figure out a formula that is shorter and less complicated than Wilson, yet accurate. A major issue with Wilson’s formula is the assumption that there must be a percentage certainty of a score between x and y, which leads to direct proportionality in increasing the percentage and distance between x and y, which increases as well. Simply put, Wilson’s method goes against the idea that all ratings should be biased towards the average. 

   The general psychology is also a principle, left ignored by the old methodologies. If a majority subset of population likes a product or feature, then another individual is more likely to also like that same product or feature. The herd mentality is the main reason behind how viral instances occur across the world. When trying to represent an entire entity with one number, it is important to account for this emotional behavior that lot of mathematics fails to account in previous formulas. 

   At the end of the day, websites have their own algorithm that they prefer to use, and these algorithms usually get the satisfaction from customers, hence their popularity. 

   While the sorting algorithms are represented mathematically, websites have an internal database design sql implementation. Here are some sql examples of the statistical formulas:

Edwin B. Wilson Lower Bound Score SQL implementation : 
SELECT widget_id, ((positive + 1.9208) / (positive + negative) - 
                   1.96 * SQRT((positive * negative) / (positive + negative) + 0.9604) / 
                          (positive + negative)) / (1 + 3.8416 / (positive + negative)) 
       AS ci_lower_bound FROM widgets WHERE positive + negative > 0 
       ORDER BY ci_lower_bound DESC;
	https://www.evanmiller.org/how-not-to-sort-by-average-rating.html

Bayesian SQL sorting implementation:
 
https://www.analyticsvidhya.com/blog/2019/07/introduction-online-rating-systems-bayesian-adjusted-rating/


SteamDB code implementation:
function GetRating( positiveVotes, negativeVotes ) {
    const totalVotes = positiveVotes + negativeVotes;
    const average = positiveVotes / totalVotes;
    const score = average - ( average - 0.5 ) * Math.pow( 2, -Math.log10( totalVotes + 1 ) );

    return score * 100;
}

REFERENCES

[1]	Miller, Evan. “How Not to Sort by Average Rating.” – Evan Miller, https://www.evanmiller.org/how-not-to-sort-by-average-rating.html. 
[2]	E. B. Wilson, “Probable inference, the law of succession, and statistical inference,” J. Amer. Stat. Assoc., vol. 22, no. 158, pp. 209–212, 1927.
[3]	“A Bayesian Recommender Model for User Rating and Review Profiling.” IEEE Xplore, https://ieeexplore.ieee.org/document/7350016. 
[4]	SteamDB. https://steamdb.info/. 
 


 

