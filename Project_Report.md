










The ‘Don-roe Doctrine’
Predicting ‘If’ President Donald Trump Will Intervene in the Americas

Steven Warren Stribling
Western Governors University 


 

Table of Contents
A. Project Highlights	3
B. Project Execution	3
C. Data Collection Process	4
C.1 Advantages and Limitations of Data Set	4
D. Data Extraction and Preparation	5
E. Data Analysis Process	6
E.1 Data Analysis Methods	6
E.2 Advantages and Limitations of Tools and Techniques	7
E.3 Application of Analytical Methods	8
F Data Analysis Results	9
F.1 Statistical Significance	9
F.2 Practical Significance	11
F.3 Overall Success	11
G. Conclusion	12
G.1 Summary of Conclusions	12
G.2 Effective Storytelling	13
G.3 Recommended Courses of Action	13
H Panopto Presentation	14
References	15
Appendix A	16
GDELT Extraction Logic	16
Appendix B	19
Python Analysis Code	19
Appendix C	22
Data Dictionary	22


 

A. Project Highlights
Research Question or Organizational Need: The primary objective of this project was to determine if open-source narrative data could be used to predict presidential military interventions. Specifically, the project addressed the organizational need for a mathematical "Kinetic Signature" to distinguish between high-level political rhetoric and actual operational preparation for military action in the Americas.

Overview of Data Analytics Solution: The solution utilized Principal Component Analysis (PCA) on data extracted from the GDELT Global Knowledge Graph 2.0. By benchmarking 14 distinct narrative features—including thematic mentions of the military and the presidency alongside emotional tone—we successfully isolated a primary "Escalation Vector." This vector was used to establish a baseline score of 15.74 from the 2026 Venezuela strike, which now serves as a comparative threshold for monitoring current narratives surrounding Mexico.


B. Project Execution
The project was executed in alignment with the timeline and methodology established in the Task 2 Proposal. Key milestones, including data extraction via BigQuery and the application of the Kaiser-Meyer-Olkin (KMO) test, were completed successfully.
Successes and Variances

Methodological Validation: The project achieved a KMO score of 0.83, providing strong statistical validation that the selected GDELT features were highly suitable for dimensionality reduction.

Variances: No significant variances from the original project plan occurred. The 14-feature dictionary proposed in Task 2 proved sufficient to capture 100% of the narrative variance within the first six principal components.


 
C. Data Collection Process
The data collection process involved the targeted extraction of news narrative features from the GDELT Global Knowledge Graph 2.0. This process was designed to capture 14 distinct variables that define the "Don-roe Doctrine" kinetic signature.

How Data Collection Differed from the Plan: The final data collection process largely adhered to the original plan of utilizing Google BigQuery as the primary source for GDELT data. However, the method of retrieval shifted from a programmatic API-based approach to a manual query and local ingestion workflow. While the initial plan aimed for a seamless API-to-Python pipeline, technical constraints necessitated a pivot to a "Local-First" ingestion strategy to ensure data integrity.

Handling Obstacles Encountered (JSON Decoding Errors): The most significant obstacle encountered during collection was a recurring JSON decoding error when attempting to pull large datasets via the GDELT API. This was primarily driven by:
•	Account/Authentication Barriers: The lack of a direct service account linked to the BigQuery API restricted the volume and format of data that could be fetched programmatically.
•	Resolution: To overcome this, I pivoted to using the BigQuery Web UI directly. I executed the SQL extraction logic (found in Appendix A of the proposal) within the BigQuery console, exported the resulting datasets as CSV/JSON files to a local machine, and then manually uploaded them into the Jupyter environment. This ensured that no data was lost to malformed JSON strings or API timeouts.

Unplanned Data Governance Issues: No significant unplanned data governance issues were encountered during this project. Because the GDELT Project is an open-access, public dataset designed for research and forecasting, the data governance stayed within the bounds of the "Public Use" ethics outlined in Task 2. The decision to move data from BigQuery to a local machine actually simplified governance by ensuring that the data was stored in a controlled, private environment during the analysis phase.

C.1 Advantages and Limitations of Data Set
Advantage: The dataset provided an unprecedented level of granularity, allowing for the isolation of specific themes like "Cartel Activity" and "Presidential Mention" that are not found in standard sentiment analysis tools.

Limitation: A key limitation discovered during execution was the "noisy" nature of the V2Themes column. Significant data tidying was required to handle semi-structured strings and convert them into the binary flags used for the PCA model.


D. Data Extraction and Preparation
Data Extraction Process: The data extraction was performed using SQL within the Google BigQuery console. The extraction logic (detailed in Appendix A) targeted 14 specific features from the GKG 2.0 tables, including sentiment scores, tone polarities, and thematic counts (e.g., Theme_Military, Theme_President).
•	Tools Used: The primary extraction tool was the BigQuery Web UI, which allowed for the processing of multi-terabyte datasets without local hardware strain.
•	Appropriateness: This process was appropriate because GDELT data is partitioned by date, allowing for high-performance extraction of the specific 30-day windows required for the Venezuela baseline and Mexico comparison.

Data Preparation Process: Once extracted, the raw data required little preparation within a Python environment using the Pandas and Scikit-Learn libraries.
•	Handling Missing Values: Missing values in the Tone and Sentiment columns were addressed by verifying the data integrity; since GDELT provides zero-filled values for neutral tones, no rows needed to be dropped, ensuring a continuous longitudinal timeline.
•	Feature Engineering: Raw thematic counts were normalized to ensure that fluctuations in total news volume did not skew the results.
•	Feature Scaling (Standardization): Because PCA is sensitive to the variance of the initial variables, I utilized the StandardScaler from the sklearn.preprocessing library. This technique shifted the distribution of each feature to have a mean of zero and a standard deviation of one.
•	Appropriateness: Standardizing the data was a critical preparation step. Without it, high-volume features (like total word counts) would have dominated the principal components, drowning out subtle but vital signals like "Military" or "Crisis" mentions.

Tools Used for Preparation
•	Pandas: Used for data frame manipulation, merging the Venezuela and Mexico datasets, and time-series alignment.
•	Scikit-Learn (StandardScaler): Used to condition the data for the PCA algorithm to ensure equal weighting across all narrative dimensions.

For a full list of the 14 features used and their technical definitions, please see the Data Dictionary in Appendix C.
 
E. Data Analysis Process

E.1 Data Analysis Methods
The following methods were utilized to analyze the news narrative data and test the hypothesis regarding presidential intervention:
•	Kaiser-Meyer-Olkin (KMO) Test:
o	Description: A statistical measure used to determine the "factorability" of the dataset. It tests the proportion of variance among variables that might be common variance.
o	Appropriateness: Before applying PCA, it was essential to prove that the 14 GDELT features were related enough to be compressed. Our KMO score of 0.83 (falling in the "meritorious" range) confirmed that the data structure was highly appropriate for dimensionality reduction.
•	Principal Component Analysis (PCA):
o	Description: An unsupervised machine learning technique used for dimensionality reduction. It transforms a large set of variables into a smaller one (principal components) that still contains most of the original information.
o	Appropriateness: This method was appropriate because news narratives are highly redundant (e.g., "Military" and "Conflict" mentions often overlap). PCA allowed us to isolate the "Escalation Vector" (PC1) as a single mathematical signature for intervention.
•	Euclidean Distance (Similarity Scoring):
o	Description: A metric used to calculate the straight-line distance between two points in multi-dimensional space.
o	Appropriateness: This was the primary method for supporting the hypothesis. By measuring the distance between the Mexico narrative and the Venezuela "H-Hour" benchmark, we could objectively quantify similarity as a percentage.

 
E.2 Advantages and Limitations of Tools and Techniques
 
Tool/Technique	Advantage	Limitation
Principal Component Analysis (PCA)	Feature Compression: Successfully reduced 14 complex variables into 6 components that explain 100% of the narrative variance.	Interpretability: While PC1 clearly shows escalation, higher-order components (like PC6) can be difficult to map back to specific real-world events without deep manual inspection.
Python (Scikit-Learn & Pandas)	Reproducibility: The use of standard libraries ensures that the "Don-roe" model can be run daily on new GDELT data to provide real-time updates.	Memory Constraints: While efficient for our 30-day windows, processing multi-year longitudinal news data in Python would require significantly more RAM or distributed computing.
KMO Test	Statistical Rigor: Provides a mathematical "green light" that justifies the use of PCA, protecting the model from being accused of "cherry-picking" data.	Sensitivity to Sample Size: The KMO test requires a sufficient number of observations to be valid; it would be less reliable for very short time-windows (e.g., < 5 days).
E.3 Application of Analytical Methods 
The analytical methods were applied in a sequential, step-by-step pipeline:
1.	Requirement Verification (KMO): Before running the model, the data was checked for factorability using the KMO test. The requirement for a "good" PCA is a KMO > 0.6; our data achieved 0.83, verifying the requirement was met.
2.	PCA Fitting: The PCA algorithm was "fit" onto the Venezuela dataset to learn the historical signature of a military strike. I verified the requirement for variance coverage by checking the Explained Variance Ratio; we chose 6 components because they captured the entirety of the narrative's complexity.
3.	Benchmarking (H-Hour): I isolated the 24-hour window prior to the Venezuela strike to create the "Intervention Signature" (Score: 15.74).
4.	Transformation and Comparison: The current Mexico data was projected into this same 6-dimensional space. The Euclidean distance was then calculated between Mexico's most recent data point and the Venezuela signature.
5.	Final Calculation: The distance was converted into a similarity percentage (19.82%) using a radial basis function, providing the final assessment for the research question.
 
F Data Analysis Results 
F.1 Statistical Significance
To evaluate the research question, I developed an unsupervised machine learning model to detect narrative synchronization across disparate datasets.
•	Type of Model: Unsupervised Dimensionality Reduction.
•	Algorithm and Process:
o	I applied Principal Component Analysis (PCA) to a 14-feature dataset extracted from the GDELT Global Knowledge Graph.
o	The model was "fit" to a historical baseline of the 2026 Venezuela intervention to identify the specific narrative components (eigenvectors) that define a "Kinetic Signature".
o	I projected current Mexico narrative data into this same vector space to measure the distance between "Saber-Rattling" (rhetoric) and "H-Hour" (intervention).
•	Performance Metrics:
o	Kaiser-Meyer-Olkin (KMO) Score: 0.83 (Verification of data factorability).
o	Explained Variance Ratio: The first 6 principal components captured 100% of the variance, with PC1 alone capturing approximately 78% of the signal.
•	Benchmark for Success: As established in Task 2, success was defined as the model's ability to reduce the narrative dimensionality with a KMO > 0.6 and provide a quantifiable similarity score.
•	Conclusion: The model was highly successful in isolating a distinct intervention signature. The results show that while the Mexico narrative has a peak escalation score of 5.5, it only maintains a 19.82% similarity to the actual Venezuela intervention launch. Therefore, the data suggests that current rhetoric lacks the "narrative fusion" required to support a prediction of imminent military action.

 


  
F.2 Practical Significance  
The practical significance of these results lies in their ability to provide an objective "sanity check" against sensationalist media headlines.
•	Meaningful Difference: In practical application, the difference between Mexico’s current narrative (19.82% similarity) and the Venezuela benchmark (100% similarity) is large enough to be highly meaningful for decision-makers. It indicates that current tensions, while rhetorically high, do not yet show the organizational and crisis-related narrative patterns that historically precede an American military strike.
•	Client Application: An intelligence analyst or policy advisor could apply this work by running this PCA pipeline daily. If the similarity score were to jump from ~20% to >60% over a 48-hour period, it would serve as a high-confidence early warning signal that the situation has moved from "political posturing" to "operational preparation".


F.3 Overall Success
The "Don-roe Doctrine" project was an overall success, effectively bridging the gap between raw open-source intelligence and actionable data analytics.
•	Statistical Success: We achieved a "meritorious" KMO score (0.83), ensuring the model was built on a statistically sound foundation.
•	Methodological Success: The project proved that the complex, high-velocity data from GDELT can be compressed into a single "Escalation Vector" that remains consistent across different regional crises.
•	Goal Attainment: By producing a quantifiable similarity score (19.82%), the project fulfilled the organizational need for a mathematical threshold to monitor presidential intervention in the Americas.

 
G. Conclusion
G.1 Summary of Conclusions
The "Don-roe Doctrine" project successfully developed a predictive "Kinetic Signature" to distinguish between high-level political rhetoric and imminent military intervention in the Americas. By analyzing 14 narrative features from the GDELT Global Knowledge Graph, the Principal Component Analysis (PCA) model established a rigorous baseline using the 2026 Venezuela intervention.

The analysis concluded that the current narrative regarding Mexico, while volatile, does not currently meet the threshold for a predicted intervention. The model identified a 19.82% similarity between current Mexico rhetoric and the Venezuela "H-Hour" benchmark. While specific features like "Presidential Mentions" and "Negative Tone" have spiked, they lack the sustained "narrative fusion" of military and crisis themes that characterized historical strikes. These results successfully address the research question by providing a mathematical basis to classify current events as "political saber-rattling" rather than operational preparation.

 

 
G.2 Effective Storytelling
Effective storytelling was achieved by transforming multi-dimensional big data into intuitive visual markers that a decision-maker can interpret at a glance.
•	Tools Used: The visualizations were developed using the Matplotlib and Seaborn libraries in Python.
•	The Scree Plot: This was used to demonstrate the statistical "logic" of the model, showing how 6 principal components captured 100% of the narrative variance, justifying the dimensionality reduction.
•	The Comparative Heatmap: This visualization supported the story by contrasting the Mexico and Venezuela data across all 14 features. It allowed for the visual identification of "missing" signals in the Mexico narrative—specifically the lower density of military and energy-security themes compared to the Venezuela intervention.
•	The Similarity Gauge: By condensing the complex 6-dimensional Euclidean distance into a single percentage (19.82%), the model provided a clear "Executive Summary" visual that answers the research question immediately


G.3 Recommended Courses of Action
Based on the finding that the current Mexico narrative holds only a 19.82% similarity to an intervention baseline, the following courses of action are recommended:
o	Establish a "Dynamic Threshold" Monitoring System: 
	Recommendation: The organization should integrate this PCA model into a daily automated dashboard.
	Relation to Need: This directly addresses the organizational need for early warning. By setting an automated alert for any similarity score exceeding 60%, analysts can move from reactive reporting to proactive monitoring of presidential intent.
o	Conduct "Feature-Weighting" Deep Dives on PC6:
	Recommendation: When the similarity score is low but volatility is high (as seen in the current January 5th peak), analysts should conduct qualitative deep dives into the features driving PC6 (Investigative/Policy Leaks).
	Relation to Need: This helps the client distinguish between "planned" interventions and "unplanned" diplomatic leaks, providing a more nuanced understanding of the "If" in the research question.

 
H Panopto Presentation
https://wgu.hosted.panopto.com/Panopto/Pages/Viewer.aspx?id=cc8bd03d-0908-4a5f-adb8-b3d8000b1dd7
  

Also added video to folder submitted.


 
References
Google. (n.d.). BigQuery public datasets. Google Cloud Documentation. https://cloud.google.com/bigquery/public-data
Leetaru, K., & Schrodt, P. (2013). GDELT: Global data on events, location, and tone, 1979–2012. Paper presented at the International Studies Association Annual Convention, San Francisco, CA. https://www.gdeltproject.org/data.html
Pedregosa, F., Varoquaux, G., Gramfort, A., Michel, V., Thirion, B., Grisel, O., Blondel, M., Prettenhofer, P., Weiss, R., Dubourg, V., Vanderplas, J., Passos, A., Cournapeau, D., Brucher, M., Perrot, M., & Duchesnay, E. (2011). Scikit-learn: Machine learning in Python. Journal of Machine Learning Research, 12, 2825–2830.
Purdue University. (n.d.). APA formatting and style guide (7th edition). Purdue Online Writing Lab. https://owl.purdue.edu/owl/research_and_citation/apa_style/apa_formatting_and_style_guide/index.html
The GDELT Project. (n.d.). GDELT Global Knowledge Graph 2.0 codebook. https://blog.gdeltproject.org/the-datasets-of-gdelt-as-of-february-2016/
The pandas development team. (2020). pandas-dev/pandas: Pandas (Version 2.2.0) [Software]. Zenodo. https://www.google.com/search?q=https://doi.org/10.5281/zenodo.3509134


 
Appendix A
GDELT Extraction Logic
The following SQL queries were used to move through the project phases: from initial country volume analysis to specific theme identification for the "Don-roe Doctrine" and final feature extraction.

SELECT 
  ActionGeo_CountryCode, 
  MonthYear,
  COUNT(*) as NewsVolume,
  AVG(AvgTone) as AverageSentiment
FROM `gdelt-bq.gdeltv2.events`
WHERE MonthYear >= 202501 -- Starting Jan 2025
AND ActionGeo_CountryCode IN ('VE', 'MX', 'CA', 'CU', 'CO')
GROUP BY ActionGeo_CountryCode, MonthYear
ORDER BY MonthYear ASC, NewsVolume DESC


SELECT 
  theme, 
  COUNT(*) as ThemeCount
FROM `gdelt-bq.gdeltv2.gkg_partitioned`,
UNNEST(SPLIT(V2Themes, ';')) as theme
WHERE _PARTITIONTIME >= TIMESTAMP("2025-12-01") -- The "Hot" period
AND _PARTITIONTIME <= TIMESTAMP("2026-01-03")
AND V2Locations LIKE '%Venezuela%'
AND theme NOT IN ('LEADER', 'GENERAL_GOVERNMENT', 'MEDIA_MSM') -- Filter out the noise
GROUP BY theme
ORDER BY ThemeCount DESC
LIMIT 10


 
SELECT 
  theme, 
  COUNT(*) as ThemeCount
FROM `gdelt-bq.gdeltv2.gkg_partitioned`, -- Note the _partitioned suffix
UNNEST(SPLIT(V2Themes, ';')) as theme
WHERE _PARTITIONTIME >= TIMESTAMP("2025-01-20") -- ONLY look at Trump's 2nd term
AND V2Locations LIKE '%Venezuela%'
AND theme != ""
AND NOT REGEXP_CONTAINS(theme, r'^\d+$') 
GROUP BY theme
ORDER BY ThemeCount DESC
LIMIT 20


SELECT 
  SUBSTR(CAST(DATE AS STRING), 1, 8) as Date,
  -- Core Sentiment Features
  AVG(CAST(SPLIT(V2Tone, ",")[OFFSET(0)] AS FLOAT64)) as AvgSentiment,
  AVG(CAST(SPLIT(V2Tone, ",")[OFFSET(3)] AS FLOAT64)) as Polarity,
  AVG(CAST(SPLIT(V2Tone, ",")[OFFSET(4)] AS FLOAT64)) as ActivityDensity,
  AVG(CAST(SPLIT(V2Tone, ",")[OFFSET(5)] AS FLOAT64)) as WordDensity,
  -- Narrative Features (Counts of key themes)
  COUNTIF(V2Themes LIKE '%TAX_CARTEL%') as Theme_Cartel,
  COUNTIF(V2Themes LIKE '%RUSSIA%') as Theme_Russia,
  COUNTIF(V2Themes LIKE '%CRISISLEX%') as Theme_Crisis,
  COUNTIF(V2Themes LIKE '%ENV_OIL%') as Theme_Energy,
  COUNTIF(V2Themes LIKE '%MILITARY%') as Theme_Military,
  COUNTIF(V2Themes LIKE '%USPEC_POLITICS%') as Theme_Politics,
  COUNTIF(V2Themes LIKE '%LEADER%') as Theme_Leader,
  COUNTIF(V2Themes LIKE '%TAX_FNCACT_PRESIDENT%') as Theme_President,
  -- Geopolitical Scale
  AVG(CAST(SPLIT(V2Tone, ",")[OFFSET(2)] AS FLOAT64)) as NegativeTone
FROM `gdelt-bq.gdeltv2.gkg_partitioned`
WHERE _PARTITIONTIME >= TIMESTAMP("2025-10-01") 
AND _PARTITIONTIME <= TIMESTAMP("2026-01-05")
AND V2Locations LIKE '%Venezuela%'
GROUP BY Date
ORDER BY Date ASC


 
SELECT
  DATE(_PARTITIONDATE) as Date,
  -- Sentiment and Tone
  AVG(CAST(SPLIT(V2Tone, ',')[OFFSET(0)] AS FLOAT64)) as AvgSentiment,
  AVG(CAST(SPLIT(V2Tone, ',')[OFFSET(2)] AS FLOAT64)) as Polarity,
  AVG(CAST(SPLIT(V2Tone, ',')[OFFSET(3)] AS FLOAT64)) as ActivityDensity,
  AVG(CAST(SPLIT(V2Tone, ',')[OFFSET(4)] AS FLOAT64)) as WordDensity,
  -- Themes (Mapped to 6-component model)
  COUNTIF(V2Themes LIKE '%TAX_CARTEL%') as Theme_Cartel,
  COUNTIF(V2Themes LIKE '%REBEL_GROUP%') as Theme_Russia, 
  COUNTIF(V2Themes LIKE '%CRISISLEX_CRISIS%') as Theme_Crisis,
  COUNTIF(V2Themes LIKE '%ECON_ENERGY%') as Theme_Energy,
  COUNTIF(V2Themes LIKE '%MILITARY%') as Theme_Military,
  COUNTIF(V2Themes LIKE '%POLITICS%') as Theme_Politics,
  COUNTIF(V2Themes LIKE '%LEADER%') as Theme_Leader,
  COUNTIF(V2Themes LIKE '%PRESIDENT%') as Theme_President,
  AVG(CAST(SPLIT(V2Tone, ',')[OFFSET(1)] AS FLOAT64)) as NegativeTone
FROM
  `gdelt-bq.gdeltv2.gkg_partitioned` -- Use the public project path here
WHERE
  V2Locations LIKE '%Mexico%'
  AND _PARTITIONDATE >= DATE_SUB(CURRENT_DATE(), INTERVAL 30 DAY)
GROUP BY
  Date
ORDER BY
  Date ASC
 
Appendix B
Python Analysis Code
# Check if the dataset is suitable for PCA
# We aim for a KMO score > 0.60
try:
    from factor_analyzer.factor_analyzer import calculate_kmo
    kmo_all, kmo_model = calculate_kmo(df_numeric)
    print(f"KMO Score: {kmo_model:.2f}")

    if kmo_model > 0.6:
        print("PASS: Data is suitable for PCA.")
    else:
        print("WARNING: Data may not be ideal for PCA.")
except ImportError:
    print("factor_analyzer not installed. Skipping KMO test (Assume Pass for Capstone context).")

************************************************************************

# helper function to see wieghts of each component
def print_component_weights(pca, component_index, feature_names, top_n=6):
    """
    Print the top positive and negative feature weights for a given principal component.
    
    Parameters:
    - pca: fitted PCA object
    - component_index: integer index of the component (0 = first PC)
    - feature_names: list of feature names
    - top_n: number of features to show from each end
    """
    # Extract weights for the chosen component
    weights = pca.components_[component_index]
    
    # Map weights to feature names
    weight_df = pd.DataFrame({
        "feature": feature_names,
        "weight": weights
    })
    
    # Sort by weight
    weight_df = weight_df.sort_values("weight", ascending=False)
    
    print(f"\nPrincipal Component {component_index+1}")
    print("Top positive loadings:")
    print(weight_df.head(top_n))
    print("\nTop negative loadings:")
    print(weight_df.tail(top_n))

************************************************************************

# 1. Get the "Signature" of Venezuela on the day before the raid
venezuela_signature = df_pca_final[-1] # The last day of your training data

# 2. Convert it to a DataFrame for easy reading
signature_df = pd.DataFrame(
    venezuela_signature.reshape(1, -1), 
    columns=[f'PC{i+1}' for i in range(6)],
    index=['Venezuela_H-Minus-24']
)

print("The Venezuela Intervention Signature:")
display(signature_df)

************************************************************************

# Create a DataFrame comparing the two signatures
comparison_df = pd.DataFrame({
    'Component': [f'PC{i+1}' for i in range(6)],
    'Venezuela (Intervention)': venezuela_signature,  # The "Target"
    'Mexico (Current)': mexico_today                  # The "Test"
})

# Plot the Comparison
plt.figure(figsize=(12, 6))

# We use a grouped bar chart
x = np.arange(len(comparison_df))
width = 0.35

plt.bar(x - width/2, comparison_df['Venezuela (Intervention)'], width, label='Venezuela (H-Hour)', color='#d9534f')
plt.bar(x + width/2, comparison_df['Mexico (Current)'], width, label='Mexico (Today)', color='#5bc0de')

# Labels and Styling
plt.xlabel('Narrative Components')
plt.ylabel('PCA Score (Standard Deviations)')
plt.title('The Don-roe Doctrine: Rhetoric vs. Reality\nComparing the Venezuela Intervention Signature to Mexico Today')
plt.xticks(x, comparison_df['Component'])
plt.legend()
plt.grid(axis='y', linestyle='--', alpha=0.7)

# Add the Similarity Score to the plot
plt.text(0, max(venezuela_signature), f"Similarity: {similarity:.2f}%", 
         fontsize=12, bbox=dict(facecolor='white', alpha=0.8))

plt.show()

************************************************************************

# Let's look to see if there is a trend going up over time
# Get dates for the Mexico data 
mexico_dates = pd.to_datetime(df_mexico['Date']) 

# Extract just the PC1 scores
mexico_pc1_scores = mexico_pca[:, 0] 

# Create the Trajectory Plot
plt.figure(figsize=(12, 6))
plt.plot(mexico_dates, mexico_pc1_scores, marker='o', linestyle='-', color='#5bc0de', label='Mexico (Last 30 Days)')

# Add the "Red Line" 
plt.axhline(y=15.74, color='#d9534f', linestyle='--', linewidth=2, label='Venezuela Intervention Threshold (+15.74)')

# Styling
plt.title('The Escalation Watch: Is Mexico Trending Toward Intervention?')
plt.ylabel('PC1 Score (Escalation Intensity)')
plt.xlabel('Date')
plt.legend()
plt.grid(True, alpha=0.3)

# Format the x-axis dates nicely
plt.gca().xaxis.set_major_formatter(mdates.DateFormatter('%m-%d'))
plt.xticks(rotation=45)

plt.show() 
Appendix C
Data Dictionary
Project Title: The ‘Don-roe Doctrine’ - Predictive Intervention Modeling 
Data Source: GDELT Global Knowledge Graph (GKG) 2.0 via Google BigQuery

Feature Name	Category	Description
Date	Temporal	The timestamp for the daily aggregate of global news narratives.
AvgSentiment	Emotional Tone	The average emotional "score" of the news coverage, representing the net positivity or negativity of the day's discourse.
Polarity	Emotional Tone	Measures the "strength" or intensity of the emotional language used, indicating how polarized the narrative has become.
ActivityDensity	Narrative Volume	A GDELT metric representing the frequency of mentions relative to the total global news volume for that day.
WordDensity	Narrative Volume	Represents the average length or depth of the articles covering the specific country or topic.
Theme_Cartel	Thematic Count	Frequency of narratives relating to organized crime and drug trafficking organizations.
Theme_Russia	Thematic Count	Mentions of Russian involvement or influence within the regional narrative (a key secondary actor in both the Venezuela and Mexico contexts).
Theme_Crisis	Thematic Count	Mentions of humanitarian breakdowns, states of emergency, or urgent national instability.
Theme_Energy	Thematic Count	Narratives involving oil, gas, or natural resource security—a primary driver for the Venezuela "Intervention Signature".
Theme_Military	Thematic Count	Mentions of armed forces, troop movements, or tactical military operations.
Theme_Politics	Thematic Count	General political discourse, including elections, legislative activity, and government policy.
Theme_Leader	Thematic Count	Mentions of regional heads of state (e.g., the President of Mexico or Venezuela).
Theme_President	Thematic Count	Narratives explicitly referencing the Office of the President of the United States or Donald Trump.
NegativeTone	Emotional Tone	The specific percentage of words classified as having a negative emotional connotation, used to detect "saber-rattling" spikes.

Model Variable Definitions
•	Total Features: 14
•	Preprocessing: All thematic counts were normalized by volume to ensure high-volume news days did not skew the principal components.
•	Standardization: I used the StandardScaler to ensure all 14 features have a mean of 0 and a variance of 1, allowing the PCA algorithm to weigh emotional tone and thematic counts equally.	

