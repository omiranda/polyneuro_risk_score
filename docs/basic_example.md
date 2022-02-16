<script type="text/javascript"
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS_CHTML"></script>
# Basic example 
In this example we'll run a BWAS analysis using functional connectivity as brain features. Resulting \\( \beta\\)-weights will be used to predict the corresponding brain-derived behavioral scores. Here we assume you have succesfully downloaded the toolbox and associated data. 

**Considerations**:

- This is a toy example that uses a reduced sample to ilustrate the functionality of the toolbox
- The same dataset is used to calculate the models and to predict scores. You should not do this in a real project.


## BWAS: Estimating beta-weights
asas sasad ad ad 
### Runing the code using Matlab
Once you have opened Matlab and added to your session the path to the repo, 

```Matlab
run_CWAS (path_imaging_reference,...
    path_demographics_Table_reference,...
    path_dictionary_demographics_Table_reference,...
    path_group_Design_Table_reference,...
    'output_folder',output_folder_BWAS,...
    'model',model,...
    'path_parcellation_table',path_parcellation_table) 
```


### Runing the code using the container


## PNRS: Estimating risk
- Runing the code using Matlab
- Runing the code using the container
- Exploring the outputs