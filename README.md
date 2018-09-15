Notes to go along with my notebook and demo on hierarchical dirichlet process gaussian mixture models presented to the tellic DSE team on Wednesday, September 12, 2018. These notes include some key references for understanding both hierarchical dirichlet process models, and relevant automated flow cytometry literature.


Papers Referenced
1. “Hierarchical Modeling for Rare Event Detection…” Cron et al
    1. https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1003130 
2. “Critical Assessment of Automated Flow Cytometry Data Analysis Techniques” Aghaeepour et al. 
    1. https://www.nature.com/articles/nmeth.2365 
3. “Hierarchical Dirichlet Processes” Teh et al.
    1. https://people.eecs.berkeley.edu/~jordan/papers/hdp.pdf 
4. “BayesFlow: latent modeling of flow cytometry cell populations” Johnsoon et al.
    1. https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-015-0862-z 
5. “High-content flow cytometry and temporal data analysis for defining a cellular signature of graft-versus-host disease” Brinkman et al.

What is flow cytometry?

What is a flow cytometer?
“Flow cytometers provide high-dimensional quantitative measurement of light scatter and fluorescence emission properties of hundreds of thousands of individual cells in each analyzed sam- ple. FCM is used routinely both in research labs to study normal and abnormal cell structure and function and in clinical labs to diagnose and monitor human disease as well as response to ther- apy and vaccination. In a typical FCM analysis, cells are stained with fluorochrome-conjugated antibodies that bind to the cell surface and intracellular molecules. Within the flow cytometer, cells are passed sequentially through laser beams that excite the fluorochromes. The emitted light, which is proportional to the antigen density, is then measured. The latest flow cytometers can analyze 20 different characteristics for individual cells in complex mixtures1, and recently developed mass spectrophotometry– based cytometers could dramatically increase this number2–4.“ -Aghaeepour

What is the key goal of flow cytometry analysis?
Group cells (known as ‘events’ into discrete groups based on similarities in light scattering and fluorescence properties.
- This ‘clustering’ used to be done manually in a process that is known as manual gating

Why is it important to identify sub populations in and across cell samples?
Areas that use flow cytometry analysis are:
- Vaccine development (Cron)
    - Monitoring antigen specific T cell populations
- Monitoring HIV Infection (Johnsson)
- Diagnosing Blood Cancers (Johnsson)
- Diagnosing acute graft vs. host disease (Brinkman)

All of the above use cases rely upon the ability to identify subpopulations of cells within and across samples

Why can’t the subpopulation identification be done manually?
Up until recently, the identification was done manually, but at Johnsson cites, the progression of flow cytometry technology has increased the number of samples we can collect, and the dimensionality of those samples.

What are the problems in flow cytometry that statistical methods could solve?
The key objective then in analyzing flow cytometry data is to group cells into cluster by way of their light scatter and fluorescence measurements. But typical in clustering problems, given a cell sample, how do we know how many clusters to use? Furthermore, the problem in this application is augmented since we are trying to cluster cells in samples that we taken at different times and sometimes from different patients. 

We want a modeling technique that is able to cluster the data without a predefined number of clusters, while also gives us the flexibility to acknowledge that the data is composed of separate samples, each of which should come from the same underlying distribution.

What is an example of a statistical technique that has been used and how does it work?

HIERARCHICAL DIRICHLET PROCESS MIXTURE MODELS (we use gaussians)

Unfortunately, the sample data that came with the package that Cron made to implement this method did not even remotely resemble the data discussed in the paper. Furthermore, the package was written in 2012, so all of the dependencies were wonky.

Thus…we must settle to work with synthetic data!!

Distillation of the problem
Given J samples containing N events, we find K clusters to group the data. For our synthetic data, we only consider 1-dimensional data since I’m still working on how to implement DPGMMs in pymc3 with multi-dimensional data…