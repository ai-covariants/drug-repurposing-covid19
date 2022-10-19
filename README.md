# COVID-19 Drug Repurposing 

Machine learning framework to systematicall downselect drugs from a set of FDA/TGA approved candidates for COVID-19 drug repurposing. 

## Objective 

There are approximately 8000 FDA/TGA-approved drugs, and this data has been made available at a website,  www.covirx.org, which was developed by BITS Pilani. In this database, there are approximately 130 characteristics for each of these 8000 drugs. These are chemical and pharmaceutical characteristics like molecular weight, class of drug, solubility, etc along with COVID-19 assay data. In a previous study by [MacRaild, C.A et al](https://www.preprints.org/manuscript/202209.0310/v1), a set of filters were applied to systematically reduce these 7817 drugs to a set of 214 drugs which were then used for further down-selection. 

In the previous study, a formula was developed to calculate the activity rank of compounds from their assay data to rank these 214 compounds and select top 12 performing compounds. One disadvantage of this method is that the activity score is biased towards the compounds that are active in only one assay. Furthermore, this method only includes certain assays and leaves out the ones that focus on mechanisms of action like 3C like protease (3CLpro). In this study, we aim to develop a machine learning framework that can sugggest drugs for COVID-19 drug repurposing experiments based on their similarity with the drugs down-selected by sySTEMs initivative.  

## Data

Data consists of 214 drugs and 62 features for each of these drugs. Of these 62 features, 6 are textual features and encode important biological information like pathway, MOA, target, indication, interaction with other drugs and contraindication and the rest 56 features represent pharmaceutial/chemical properties of the drug and performance of the drug in 14 different COVID-19 assays. 

## Data encoding 

Since the features are of mixed datatypes and usual machine learning methods for preprocessing text data like one-hot-encoding will not effectively capture the important network information, seven different relationship matrices have been created as follows: 
1. Drug-drugInteraction (58 x 233) - This has data for 58 unique drugs and 233 unique drug interactions. 
2. Drug indication (212 x 113) - This has data for 212 unique drugs and 113 unique indications.
3. Drug-MOA (185 x 109) - This has data for 185 unique drugs and 109 unique MOA.
4. Drug pathway (157 x 75) - This has data for 157 unique drugs and 75 unique pathways.
5. Drug-target (157 x 231) - This has data for 157 unique drugs and 231 unique targets.
6. Drug-contraindication (139 x 182) - This has data for 139 unique drugs and 182 unique contraindication.
7. Drug-numericalFeatures (214 x 56) - This has data for 214 unique drugs and 56 pharmaceutical/chemical/assay data points.

## Framework 

This problem can be viewed as a multi-attribute type clustering problem. To solve this, we propose a two tier framework inspired by [Hameed, P., Verspoor, K., Kusljic, S. et al.](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-018-2123-4)

### Tier 1 

Out of the 7 relation matrices, clustering was performed on six (except drug-numericalFeatures) individual homogenous matrices using KMeans. The results from these six clustering experiments were combined to form a drug-drug-relation (DDR) matrix. 

### Tier 2

A GCN (without loss function) was used to extract node embeddings for the 214 drugs. GCN model recieved as an input an undirected weighted graph (DDR matrix) and a node feature matrix (drug-numericalFeatures). These node embeddings were then used to perform clustering using KMeans. 
