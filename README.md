# Important Notices

1. To reproduce the project using the notebook, you must work within a Google Colab environment and have your own Reddit PRAW API developer app credentials, as the notebook utilized my own ones as well as Google Colab's unique function of storing the secrets in Google user secrets.

2. The project and report contain heavily explicit and NSFW language that could potentially be triggering. Please double-check your environment before scrolling down.

# Reddit Content Creation Analysis: Insights for Erotica Audio Creators

## Project Overview

As a freelance data analyst running an Online Influencer Agency, I recently completed a project for a client pivoting into the erotica space, specifically focusing on audio content creation for platforms like Reddit's r/gonewildaudio subreddit. This analysis examines high-performing male-performed audio posts to uncover patterns in themes, tags, and engagement metrics. The goal is to provide actionable recommendations for new creators, such as a 20-something male performer, to optimize content strategy and maximize audience reach.

The full workflow is documented in the Jupyter notebook [Workflow.ipynb](Workflow.ipynb), which I've made open-source on GitHub for transparency and community benefit. This README serves dual purposes:
- **Introduction to the Project**: A technical overview for developers or analysts interested in reproducing or extending the work.
- **Delivery-Oriented Report**: A high-level summary tailored for non-technical stakeholders, such as content creators, marketers, or clients, emphasizing key insights without deep dives into code.

The analysis leverages Reddit's API to fetch data, applies machine learning for theme clustering, and visualizes trends to highlight opportunities in the NSFW audio erotica niche.

## Objectives

The primary objectives of this project were:
- To acquire and analyze a dataset of top-performing posts from r/gonewildaudio, focusing exclusively on male-performed content (e.g., tags like [M4F], [M4M], [M4A], [M]).
- To identify common themes and tags associated with high engagement (upvotes, comments).
- To cluster content into thematic groups and evaluate their performance metrics (e.g., median upvotes, variance in scores).
- To derive actionable insights for new entrants in the erotica audio space, helping them prioritize content types that balance reliability and high-reward potential.

This work is based on the top 3000 posts (all with at least 2500 upvotes), filtered down to 291 male-performed entries, ensuring the insights are drawn from proven successes.

## Methodology (High-Level Summary)

For non-technical audiences: We used Reddit's official API (via the PRAW library) to collect post data, including titles, tags, scores, and comments. The data was cleaned and processed to extract standardized tags. Then, we applied clustering techniques (K-Means on TF-IDF vectors) to group similar themes into 10 clusters. Engagement metrics were analyzed per cluster to spot trends.

Key steps:
1. **Data Acquisition**: Fetched top posts from r/gonewildaudio and filtered for male-performed content.
2. **Data Cleaning**: Converted timestamps, extracted tags, and calculated features like title length.
3. **Theme Clustering**: Used machine learning to categorize posts into 10 thematic groups based on tags (e.g., gentle dominance, rough play).
4. **Engagement Analysis**: Calculated metrics like median upvotes and score variance for each cluster.
5. **Visualization**: Created charts and word clouds to illustrate distributions and popular tags.
6. **Insight Generation**: Interpreted results to provide recommendations.

Libraries used (for technical users): PRAW, pandas, scikit-learn (for TF-IDF and K-Means), matplotlib, seaborn, wordcloud.

## Key Findings

The analysis revealed distinct thematic clusters within male-performed erotica audio posts. Here's a summary of the clusters, their characteristics, and performance:

- **Cluster Distribution**: Posts were unevenly distributed across 10 clusters, with some themes (e.g., rough dominance) dominating in volume.
  
  ![Cluster Distribution Bar Chart](images/cluster_distribution.png)

- **Top Tags per Cluster**: Each cluster has unique tag profiles, highlighting niche preferences.
  
  ![Top Tags per Cluster Subplots](images/top_tags_per_cluster.png)

- **Word Clouds**: Visual representations of frequent tags (excluding common ones like [M4F]) for each cluster.
  
  ![Word Cloud for Cluster 0](images/wordcloud_cluster_0.png)
  ![Word Cloud for Cluster 1](images/wordcloud_cluster_1.png)
  ![Word Cloud for Cluster 2](images/wordcloud_cluster_2.png)
  ![Word Cloud for Cluster 3](images/wordcloud_cluster_3.png)
  ![Word Cloud for Cluster 4](images/wordcloud_cluster_4.png)
  ![Word Cloud for Cluster 5](images/wordcloud_cluster_5.png)
  ![Word Cloud for Cluster 6](images/wordcloud_cluster_6.png)
  ![Word Cloud for Cluster 7](images/wordcloud_cluster_7.png)
  ![Word Cloud for Cluster 8](images/wordcloud_cluster_8.png)
  ![Word Cloud for Cluster 9](images/wordcloud_cluster_9.png)

- **Engagement Metrics**:
  - Median upvotes and variance were calculated per cluster.
  - High-variance clusters indicate "boom or bust" potential, while low-variance ones suggest consistent performance.
  
  ![Variance Bar Chart](images/variance_per_cluster.png)
  ![Box Plot of Upvote Counts](images/boxplot_upvotes.png)

Notable trends:
- **Gentle & Sensual Themes** (Clusters 0 and 4): Tags like "praise," "missionary," "gentlemdom." High median scores (e.g., 5442), low variance—reliable "safe bets" with less competition.
- **Rough Dominance** (Clusters 7 and 2): Tags like "mdom," "degradation," "rough." Highest potential rewards but high variance and competition.
- **Male Submissive** (Cluster 6): Tags like "msub," "begging." Lower median engagement, mixed results—potentially suboptimal for broad popularity.

Overall, 1807 distinct tags were identified, with [M4F] being ubiquitous.

## Actionable Insights for Stakeholders

For a new 20-something male performer entering the erotica audio space:
- **Start Safe**: Focus on gentle and sensual content (Clusters 0 and 4) for consistent engagement and lower risk. Incorporate tags like "gentlemdom," "praise," and "kissing" to build an audience.
- **Pursue High Rewards**: Experiment with rough dominance themes (Clusters 7 and 2) for viral potential, using tags like "mdom," "choking," and "degradation." Be prepared for variability.
- **Approach with Caution**: Male submissive content (Cluster 6) shows lower average performance; use it sparingly or blend with other themes.
- **General Tips**: All analyzed posts are top-performers (min 2500 upvotes), so emulate their structure. Post during peak times, use descriptive titles, and monitor trends. Continuously adapt based on audience feedback—Reddit's NSFW space evolves quickly.

These insights can guide content calendars, tag strategies, and A/B testing to accelerate growth in the erotica influencer niche.

## How to Reproduce or Extend the Project

Refer to the notice at the top for setup requirements. Open [Workflow.ipynb](Workflow.ipynb) in Google Colab, input your Reddit API credentials, and run the cells sequentially. The notebook includes all code for data fetching, analysis, and visualization.

For technical users: Extend by adjusting cluster count, adding more subreddits, or incorporating sentiment analysis on comments.

## Conclusion

This project demonstrates how data-driven analysis can inform content strategy in niche markets like erotica audio. By open-sourcing it, I aim to help other creators and analysts. For the client: These findings provide a roadmap for your pivot—focus on high-engagement themes to build momentum. If you'd like further customization or additional analyses (e.g., competitor benchmarking), contact me at the Online Influencer Agency.

Thank you for reviewing this report. Feedback welcome via GitHub issues!
