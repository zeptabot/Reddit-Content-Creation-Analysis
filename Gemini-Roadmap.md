## **Refined Data Analysis Project Roadmap: GWA Male Performer Success Factors**

### **Project Objective:**

To analyze all of male-performed posts on r/gonewildaudio to identify the tags, content characteristics, and posting strategies that are most strongly correlated with audience engagement. The final deliverable will be a set of actionable recommendations for a new 20M male creator looking to enter the space.

### **Guiding Principles for this Roadmap:**

1. **Start Simple, Build Complexity:** We will begin with the most straightforward, highest-impact analysis (EDA) and progressively add more sophisticated layers. This ensures we get value early and don't get bogged down.  
2. **Focus on Interpretability:** The goal is not to build a perfect predictive model, but to understand *why* certain posts succeed. Every step will be geared towards generating clear, understandable insights.  
3. **Acknowledge Technical Constraints:** The plan will rely on well-documented Python libraries and avoid techniques that require massive computational power or deep theoretical knowledge. I will explain the "how" and the "why" for each step.

---

### **Phase 1: Data Acquisition & Initial Filtering**

This phase corresponds to your "Step-1" and is the foundation of the project.

* **Tool:** The Python PRAW (Python Reddit API Wrapper) library is the industry standard for this. It's well-documented and handles authentication and rate-limiting gracefully.  
* **Timeframe:** Last 24 months (from August 2023 to August 2025). This is a good scope—large enough for statistical significance, but recent enough to be relevant.  
* **Data Points to Scrape per Post:**  
  * id: Unique post identifier.  
  * title: The full post title.  
  * author: The username of the poster.  
  * created\_utc: The exact posting time (in Unix timestamp format, easily convertible).  
  * score: The net upvotes (upvotes \- downvotes).  
  * upvote\_ratio: The percentage of votes that are upvotes.  
  * num\_comments: The number of comments on the post.  
  * selftext: The full body of the post (the description, script link, etc.).  
  * url: The direct link submitted (usually to soundgasm.net, etc.).  
  * stickied: A boolean to identify and filter out moderator posts.  
* Filtering Logic:  
  Your proposed filter is good. We can make it more robust using a regular expression (regex) to catch variations. The initial filter will be on the post title to find any that contain patterns like:  
  * \[M4F\], \[M4M\], \[M4A\] (and variations like \[M4FM\])  
  * \[M\] (as a standalone tag, to not capture things like \[Script Fill\])  
  * This will be our initial raw dataset of "Male Performance Posts".  
* **Output:** A single CSV file named gwa\_male\_posts\_raw.csv. This ensures we only have to run this time-consuming scraping process once.

### **Phase 2: Data Cleaning & Feature Engineering**

This is the most critical phase and addresses your "Possible Processing approach 1 & 2". We'll use a hybrid approach to tackle the tag problem.

* **Step 2.1: Basic Cleaning:**  
  * Load gwa\_male\_posts\_raw.csv into a Pandas DataFrame.  
  * Convert created\_utc to a standard datetime format. From this, extract useful features like post\_hour, post\_day\_of\_week, and post\_month.  
  * Calculate post\_body\_length from the selftext column.  
  * Calculate title\_length.  
* **Step 2.2: Tag Extraction & Normalization (The Core Challenge):**  
  1. **Extraction:** Use a regular expression like re.findall(r'\\\[(.\*?)\\\]', title) to extract a list of all tags from each title into a new column called tags\_raw.  
  2. **Rule-Based Normalization (First Pass):** This handles 80% of the problem with 20% of the effort. Create a function that applies these rules to each tag:  
     * Convert to lowercase (\[DD/lg\] \-\> \[dd/lg\]).  
     * Remove punctuation and spaces (\[dd/lg\] \-\> ddlg, \[lots of kisses\] \-\> lotsofkisses).  
     * Standardize synonyms using a manually created dictionary (a "mapping"). For example, map {'kissing', 'kisses', 'lotsofkisses'} to a single standardized tag: kissing. Map {'ddlg', 'daddy dom'} to ddlg. You will build this dictionary iteratively as you explore the data.  
  3. **LLM-Powered Normalization (Second Pass):** Your concern about LLM filters is valid, but manageable for a *classification* task. We won't ask it to *generate* NSFW content, but to *categorize* it.  
     * **Workflow:** After rule-based normalization, get a list of unique, unmapped tags. For these remaining messy tags, use an LLM API (like Gemini).  
     * Prompting Strategy: "You are a data analyst assistant. Your task is to group semantically similar tags into a single, standardized category. Here is a list of existing categories: \[category1, category2, ...\]. For each tag in the list below, assign it to the most appropriate existing category, or suggest a new one if none fit. The context is romantic and erotic audio scripts.  
       Tag List: \['tag\_a', 'tag\_b', 'tag\_c'\]  
       Output format should be a JSON object: {'tag\_a': 'standardized\_category', 'tag\_b': 'standardized\_category'}"  
     * This approach minimizes the risk of triggering safety filters because it's framed as a technical categorization task. The feasibility is high. The output will be a new mapping dictionary we can use to finish standardizing our tags.  
* **Output:** A new CSV file gwa\_male\_posts\_processed.csv with new columns like tags\_normalized, post\_hour, post\_body\_length, etc. The tags\_normalized column will be a list of clean, standardized tags for each post.

### **Phase 3: Defining & Measuring "Success"**

This directly addresses your concern about the arbitrariness of success metrics ("Possible Analysis Approach 1"). The solution is to use a percentile-based approach, which is standard practice.

* **Step 3.1: Calculate Percentiles:** For the key engagement metrics (score and num\_comments), calculate their distribution (e.g., 25th, 50th, 75th, 90th, 95th percentiles). This will show us how skewed the data is (most posts get little engagement, a few get a lot).  
* **Step 3.2: Create Success Tiers:**  
  * **Tier 1: "Viral Hit" (is\_viral):** A post is a 1 if its score is in the top 5% AND its num\_comments is in the top 5%. 0 otherwise.  
  * **Tier 2: "Successful Post" (is\_successful):** A post is a 1 if its score is in the top 20% OR its num\_comments is in the top 20%. 0 otherwise.  
  * **Tier 3: "Engagement Score" (engagement\_score):** Create a composite score to capture overall engagement. A simple way is to standardize the score and comments (so they're on the same scale) and add them together: engagement\_score \= (score \- mean(score))/std(score) \+ (num\_comments \- mean(num\_comments))/std(num\_comments).  
* **Output:** The processed DataFrame will now have these new columns, allowing us to easily filter for and compare successful vs. unsuccessful posts.

### **Phase 4: Exploratory Data Analysis (EDA)**

This is your "Possible Analysis Approach 1", but now supercharged with our clean data and clear success metrics.

* **Objective:** Find the "low-hanging fruit"—the most obvious patterns.  
* **Analysis & Visualizations:**  
  1. **Top Tags Overall:** Bar chart of the top 25 most frequent tags\_normalized. This shows what's most common.  
  2. **Top Tags for Successful Posts:** Filter the DataFrame for is\_successful \== 1\. Create the same bar chart. Compare it to the overall chart. *This is a core analysis.* Are tags like \[Comfort\] or \[Friends to Lovers\] more prevalent in successful posts?  
  3. **Engagement by Post Time:** Create box plots showing the engagement\_score distribution for each post\_day\_of\_week and post\_hour. Is there a "golden hour" to post?  
  4. **Post Body Length vs. Success:** A box plot showing the distribution of post\_body\_length for is\_successful \== 0 and is\_successful \== 1\. Do longer, more detailed descriptions correlate with success?  
  5. **Tag Combinations:** Create a heatmap or Sankey diagram showing which tags most frequently appear together. Is \[Boyfriend Experience\] often paired with \[Reassurance\]?

### **Phase 5: Focused Modeling for Deeper Insights**

This is a gentle, practical introduction to your "Possible Analysis Approach 2". We will use a simple, highly interpretable model.

* **Objective:** To statistically quantify how much each feature (like a specific tag) contributes to the likelihood of a post being successful, while controlling for other factors.  
* **Model Choice:** **Logistic Regression**. This model is perfect for your skill level and our goal. It predicts a binary outcome (0 or 1\) and its output coefficients are easy to interpret. The goal is *inference*, not prediction.  
* **Setup:**  
  * **Target Variable (Y):** is\_successful (the 0/1 flag we created in Phase 3).  
  * **Features (X):**  
    * We need to convert our list of tags into a format the model can use. This is called **One-Hot Encoding**. We'll create a new column for each of the top 30-40 most popular tags (e.g., a column tag\_comfort, tag\_ddlg, etc.). The value will be 1 if the post includes that tag, and 0 if it doesn't.  
    * Other numerical features: post\_body\_length, post\_hour.  
* **Interpretation of Results:** The model's output will give us a coefficient for each feature. For a tag like tag\_comfort, a positive coefficient means that, holding all other factors constant, the presence of the \[Comfort\] tag *increases the odds* of the post being successful. We can literally rank tags by how much they "boost" a post's chances of success. This is an extremely powerful and actionable insight that simple EDA can't provide.

### **Phase 6: Creator-Level Analysis**

This incorporates your "Possible Processing approach 3". It moves from a post-centric to a creator-centric view.

* **Objective:** To identify the characteristics of consistently successful creators.  
* **Method:**  
  1. Group the processed data by author.  
  2. For each author, calculate: post\_count, avg\_score, avg\_num\_comments, hit\_rate (percentage of their posts that are is\_successful), and a list of their most frequently used tags.  
  3. Identify the top 10-20 creators based on a combination of hit\_rate and post\_count (to avoid one-hit wonders).  
  4. Analyze the commonalities among these top creators. What are their "signature" tags? Do they have a consistent persona? This provides a qualitative "case study" to complement the quantitative findings.

### **Phase 7: Synthesis and Final Recommendations**

This is the final step where we translate all our findings into a practical guide for the client.

* **The Playbook:** The final output should be a clear report or presentation that answers:  
  1. **Content Strategy:** What are the top 5-10 "high-impact" tags that a new creator should focus on? (From Phase 5). What popular tag combinations work well together? (From Phase 4).  
  2. **Posting Strategy:** When is the best time of day/week to post? (From Phase 4). How long and detailed should the post description be? (From Phase 4).  
  3. **Long-Term Growth:** What do the most consistent, successful creators do differently? (From Phase 6). What are the common paths to building a following?  
  4. **Niche Identification:** Based on the data, are there underserved niches? (e.g., high engagement for a tag that isn't used very often).

This roadmap provides a comprehensive, step-by-step plan that is technically feasible, directly aligned with your goals, and sensitive to your current skill level. It breaks down complex problems like tag normalization into manageable steps and introduces a powerful but interpretable modeling technique. You can now take each phase and begin working on it, knowing how it fits into the larger picture. This is your "long prompt" for generating the code and analysis. Good luck\!
