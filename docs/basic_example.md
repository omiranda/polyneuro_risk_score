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
                        path_imaging_reference = '/panfs/roc/groups/8/faird/shared/code/stable/utilities/BWAS_PNRS_package/v2/codebase/polyneuro_risk_score/data/xsectional_1_outcome_fconn/fconn.mat';
             path_demographics_Table_reference = '/panfs/roc/groups/8/faird/shared/code/stable/utilities/BWAS_PNRS_package/v2/codebase/polyneuro_risk_score/data/xsectional_1_outcome_fconn/demographcis_Table.csv';
  path_dictionary_demographics_Table_reference = '/panfs/roc/groups/8/faird/shared/code/stable/utilities/BWAS_PNRS_package/v2/codebase/polyneuro_risk_score/data/xsectional_1_outcome_fconn/Dictionary_for_demographics_Table.csv';
             path_group_Design_Table_reference = '/panfs/roc/groups/8/faird/shared/code/stable/utilities/BWAS_PNRS_package/v2/codebase/polyneuro_risk_score/data/xsectional_1_outcome_fconn/Group_Design_Table.csv';
                            output_folder_BWAS = '/panfs/roc/groups/4/miran045/shared/projects/polyneuro_risk_score/experiments/toolbox_tutorial/example1/BWAS';
                        path_parcellation_table= '/panfs/roc/groups/8/faird/shared/code/stable/utilities/BWAS_PNRS_package/v2/codebase/polyneuro_risk_score/data/xsectional_1_outcome_fconn/parcel.mat';
               path_Group_Color_Table_reference= '/panfs/roc/groups/8/faird/shared/code/stable/utilities/BWAS_PNRS_package/v2/codebase/polyneuro_risk_score/data/xsectional_1_outcome_fconn/Group_Color_Table.csv';
                        model='lutein_PCA1 ~ brain_feature+Diet+betacarotene_PCA1+1';
                        model='lutein_PCA1 ~ brain_feature-1';
```

Now, you can call the function

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