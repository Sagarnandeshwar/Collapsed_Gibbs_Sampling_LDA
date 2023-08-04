# Collapsed Gibbs Sampling LDA
Hierarchical guided-topic model for electronic health record data

## Collapsed Gibbs sampling
Collapsed Gibbs sampling is a inference algorithm used to estimate the latent topic structure in Latent Dirichlet Allocation (LDA), a probabilistic generative model for topic modeling. LDA is widely used for discovering the hidden thematic structure in a collection of documents. 

The key idea behind collapsed Gibbs sampling is to update the latent topic assignments for each word in a document while integrating out the topic variables that are not of interest. This integration, or collapsing, helps in simplifying the inference process and makes it computationally more efficient compared to other sampling methods like Metropolis-Hastings or variational inference. 

The process of "collapsing" refers to integrating out the topic variable from the conditional distribution, making the computation of the conditional probability for a word's topic assignment much simpler. By iteratively sampling topic assignments for each word, the algorithm converges towards a stable distribution over topic assignments, which represents the topic structure of the corpus. 

Collapsed Gibbs sampling is a fundamental method for performing posterior inference in LDA. 

## Dataset 

Downloaded the electronic health record (EHR) data containing ICD-9 codes for a subset of the  MIMIC-III ICU patients’ records and the meta information about the ICD codes.

The data file named MIMIC3 DIAGNOSES ICD subset.csv.gz contains two columns with the first column for the patient ID (SUBJECT ID) and the second column for the ICD-9 code (ICD9 CODE). 

For the purpose of this assignment, the patients were chosen to have at least one of 3 ICD-9 categories: 331, 332, 340, which correspond to Alzheimer’s disease, Parkinson disease, and Multiple Sclerosis, respectively. 

To make this assignment even more manageable, the codes for external injuries, supplementary classification code, and hypertension NOS (ICD-9 401.9) were removed. We will work with only 1000 ICD codes that were randomly sampled from the ICD codes of those pre-filtered patients. As a result, among the 1000 ICD codes from the data file, there are D = 689 unique patients based on the SUBJECT ID and M = 389 unique ICD-9 codes. 

The file named D ICD DIAGNOSES.csv.gz contain description of the ICD-9 codes in the SHORT TITLE and LONG TITLE columns. 

## Implementing collapsed Gibbs sampling LDA 

Set the number of topics K to 5. Set the hyperparameters α = 1 and β = 0.001 for the document topic Dirichlet prior and the ICD-9 topic Dirichlet prior, respectively. 

As shown in the equations below, your implementation involves 3 key updates: (1) update the K x 1 topic distribution of $z_{id}$ for each ICD code i from each patient d, (3) update $n_{.dk}$ for the patients-by-topics count matrix (i.e., the D x K count matrix), (4) update $n_{w.k}$ for the ICDs-bytopics counts (i.e., the M x K count matrix): 
 
Ran 100 iterations of your collapsed Gibbs sampling algorithm, which took one minute. Normalize the final ICDs-by-topics and the patients-by-topics matrix, respectively: 

## Visualizing the top ICD codes under each topic 

For each topic k, we choose the top 10 ICD-9 codes defined under the distribution $ϕ_k$. Concatenate the top 10 ICD codes per topic together, resulting in a 50 x 5 ICDs-by-topics matrix as 

Topic 1 and topic 5 have ICD codes prefix 331 as the top code, implying their connections with Alzheimer’s disease. Topic 2 has the top second code beginning with 332 (i.e., Paralysis agitans), which codes Parkinson’s disease although we see comorbidity code 4280 CHF for Congestive heart failure, unspecified (https://icdlist.com/icd-9/428.0), appearing as the top 1 code. Interestingly, topic 4 involves code 294 w/o behavioral disturbance and the target code 340 for multiple sclerosis (among others). Topic 3 does not have any of the target codes and contain codes for infection such as urinary tract infection (5990), pneumonia (486), etc. 

## Correlating topics with the target ICD codes 
To further make sense of the 5 topics, compute the normalized patient-by-topic mixture $θ_{dk}$ for each patient d and topic k using Eq (6). Then, correlate each topic from the N x 5 patient topic mixtures θ with each binary target ICD code (331,332,340) over the N patients. 

Indeed, we see that topic 1 and 5 are positively correlated with ICD 331, topic 2 correlates with ICD 332, topic 4 correlates with code 340, and topic 3 does not correlate with any of the target codes

## Visualizing patient topic mixtures 

We chose top 100 patients under each topic and display them in a heatmap as 

Reassuringly, we observe that the top patients with high probabilities under topic 1 and 5 are enriched for ICD codes 331, the top patients under topic 2 and 4 are enriched for ICD codes 332 and 340, respectively. In contrast, top patients under topic 3 do not have any of the 3 target ICD codes. Once again, your heatmap may differ from the one shown below but some of your topics should prioritize patients in one of the 3 disease groups. 

 

 

 

 

 

 

 

 

 

 

 

 

 
