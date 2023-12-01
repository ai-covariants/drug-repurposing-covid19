# COVID-19 Drug Repurposing 

Machine learning framework to systematicall downselect drugs from a set of FDA/TGA approved candidates for COVID-19 drug repurposing. 

## Data 

Data for this study was taken from CoviRx database. (https://covirx.org/)

* [https://github.com/ai-covariants/covirx/tree/main/CoviRx](https://github.com/nepython/covirx/tree/main/CoviRx) (website source code)
* [https://github.com/nepython/covirx/tree/main/CoviRx](https://github.com/nepython/covirx/tree/main/CoviRx) (csv data files)


## Code

### Tier 1 clustering

1st level of clustering. Applied only on biological data (textual+network data) to capture the interrelationship among these features. 

### DDR computation 

In this step, clustering results for tier-1 are combined to form a drug-drug-relationship graph where connection between two drugs represent similarity in biological features. 

### Graph autoencoders 

In this step, graph eutoencoder was applied on the drug-drug-relationship graph computed in the previous step with numerical drug features as node feature vectors to get latent vector embeddings of lower dimension for each drug. These now represent all drug features. 

### Tier 2 clustering

Clustering algorithms are applied to latent vector embeddings to get final drug clusters which are then qualitatively analysed to suggest top-15 performing compounds for COVID-19 drug repurposing. 

## Results 

- Hierarchical clustering performed better than k-means and spectral for data under consideration. 
- Graph autoencoders out-performed variational graph autoencoder 
- Out of 485 drugs, 15 drugs were selected and ranked for COVID-19 drug repurposing experiments. 
