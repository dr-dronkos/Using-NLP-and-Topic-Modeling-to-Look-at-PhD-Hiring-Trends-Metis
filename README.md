# The PhD Candidate: Using NLP and Topic Modeling to Look at PhD Hiring Trends

## Abstract
Since 2020, an increasing number of people with PhDs -- both new graduates and tenured professors -- have been choosing to leave academia and pivot into industry. While Academic Twitter is full of useful anecdotes from PhDs who have successfully pivoted and advice from career counselors targeting former academics, there is a lack of comprehensive data on employers: Which companies, if any, are explicitly targeting PhDs as potential hires? What kind of skills are they looking for in their ideal candidates? This project attempted to answer some of these questions using NLP and a corpus of ~ 5,200 job descriptions scraped from Tapwage.com. After trying several combinations of document vectorizers and topic modeling algorithms, the best performing combination modeled the lemmas of nouns and proper nouns (as assigned by spaCy) into 18 semantically coherent topics. I then used these topics as features in a Logistic Regression model to successfully predict whether a job description was for an Academic job or an Industry job, achieving 91 percent accuracy. I also plotted the top 2,000 most frequent words (as calculated using TFIDF) from the corpus in an interactive ScatterText vizualization, a still image of which is visible in the Communication section below.

## Data
Using BeautifulSoup and Requests, I scraped 6,875 job description documents matching the search term PhD that were live on Tapwage on May 7, 2022 and May 8, 2022. I collected the following features:

- Job Title
- Company
- Location
- Unique ID (as assigned by Tapwage)
- Tapwage Tags
- Job Description text
- Headers (specifically tagged in website HTML)
- Bullet Points (specifically tagged in website HTML)

I used the PhD Tag to filter out some job descriptions that were erroneously collected (likely posts that expired so the site showed a random “related” job that ended up being scraped), leaving me with 6,815 relevant job descriptions. This number dropped to 5,179 once I eliminated duplicates created by extra spaces in some Unique IDs, and limited the focus of the project to job descriptions containing bulleted text. By focusing on the bulleted lists in the job descriptions -- typical for the structure of the responsibilities and qualifications sections that were of interest to me -- I hoped to preemptively limit noise caused by sections about the company or the role that tend to be organized into paragraphs of text without bullet points.

## Algorithms
I used a spaCy pipeline to tokenize the text data and assign part of speech tags. I used the part of speech tags to limit the vocabulary included in topic modeling to nouns and proper nouns after trying to include verbs and realizing they just added noise to the topics I was trying to create. I also used spaCy's lemma attribute to try to eliminate extraneous morphology with generally good results.

I used Sklearn's CountVectorizer and TFIDF algorithms to vectorize each document and then tried each one in combination with Latent Semantic Analysis (LSA) and Non-Negative Matrix Factorization (NMF) to try to model topics. The most coherent topics were generated by TFIDF with NMF, and I identified 18 distinct topics.

I used these topics as features in the Linear Regression model, built with Sklearn's Logistic Regression algorithm and GridSearchCV, to see if they could be used to predict whether a job description was for a job in Academia or Industry. "Academia" here is defined as jobs with either "University" or "College" in the Company name (n=763, compared to Industry jobs n=4,416). By using GridSearchCV to perform 5-fold cross validation and find the optimal value for C, I was able to achieve an accuracy of 0.91 on the held out test data for the best performing model. This suggests that it is indeed possible to predict whether a job is in Academia or Industry using the 18 Topics discovered by my NMF model. 

Looking at the Regression coefficients suggests that the topics that best predict that jobs are in Academia are "Application Materials", "Teaching", and "Academic Research", which intuitively makes sense. The coefficients also suggest that the topics that best predict that jobs are in Industry are "Product Development", "Business or Marketing", "Engineering" and "Machine Learning". My recommendation to PhDs looking to pivot into Industry is to focus on building their skills in these domains, since they are widespread in Industry job descriptions, but not strongly associated with job descriptions for positions in Academia.

## Tools
The project was created using the following tools:
- Requests and BeautifulSoup to scrape Tapwage.com
- Pandas to clean, manipulate, and store the data
- SpaCy to tokenize and tag the data, and select lemmatized nouns and proper nouns
- Scikit-Learn to turn tokenized documents into vectors using TFIDF and NMF, and build a Logistic Regression model
- Matplotlib and ScatterText to visualize the data

## Communication
(https://github.com/dr-dronkos/Using-NLP-and-Topic-Modeling-to-Look-at-PhD-Hiring-Trends-Metis/files/8713173/Metis.NLP.Danielle.Ronkos.pdf)<img width="1163" alt="Scattertext" src="https://user-images.githubusercontent.com/95940978/168955727-3c767c64-8801-4988-99db-8b013b60dcc6.png">
[Metis NLP Danielle Ronkos.pdf](https://github.com/dr-dronkos/Using-NLP-and-Topic-Modeling-to-Look-at-PhD-Hiring-Trends-Metis/files/8713226/Metis.NLP.Danielle.Ronkos.pdf)
