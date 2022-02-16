<script type="text/javascript"
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS_CHTML"></script>
# Basic example 
In this example we'll run a BWAS analysis using functional connectivity as brain features. Resulting \\( \beta\\)-weights will be used to predict the corresponding brain-derived behavioral scores. Here we assume you have succesfully downloaded the toolbox and associated data. Here we are using as brain features connectivity matrices obtained using macaque data that was parcellated using the Bezgin ROI set with network assignment by ???.

**Considerations**:

- This is a toy example that uses a reduced sample to ilustrate the functionality of the toolbox.
- The same dataset is used to calculate the models and to predict scores. You should not do this in a real project.
- In this example we will assume that you have downloaded the data in the folder `/home/example1/input_data/`
- Here is the input data you will need to run this example:
        - [imaging_reference](./example1/fconn.mat)
        - [demographics_Table_reference](./example1/demographcis_Table.csv)
        - [dictionary_demographics_Table_reference](./example1/Dictionary_for_demographics_Table.csv)
        - [group_Design_Table_reference](./example1/Group_Design_Table.csv)
        - [parcellation_table](./example1/parcel.mat)
        - [Group_Color_Table_reference](./example1/)

## BWAS: Estimating beta-weights
asas sasad ad ad 
### Runing the code using Matlab
Once you have opened Matlab and added to your session the path to the repo, 

```Matlab
                        path_imaging_reference = '/home/example1/input_data/fconn.mat';
             path_demographics_Table_reference = '/home/example1/input_data/demographcis_Table.csv';
  path_dictionary_demographics_Table_reference = '/home/example1/input_data/Dictionary_for_demographics_Table.csv';
             path_group_Design_Table_reference = '/home/example1/input_data/Group_Design_Table.csv';
                            output_folder_BWAS = '/home/example1/BWAS';
                        path_parcellation_table= '/home/example1/input_data/parcel.mat';
               path_Group_Color_Table_reference= '/home/example1/input_data/Group_Color_Table.csv';
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
### Runing the code using Matlab

```Matlab
path_imaging_target = '/home/example1/input_data/fconn.mat';
path_demographics_Table_target = '/home/example1/input_data/demographcis_Table.csv';
path_dictionary_demographics_Table_target = '/home/example1/input_data/Dictionary_for_demographics_Table.csv';


path_betaweights = '/panfs/roc/groups/4/miran045/shared/projects/polyneuro_risk_score/experiments/toolbox_tutorial/example1/BWAS/tables/brain_feature.csv';
path_Rsquared = '/panfs/roc/groups/4/miran045/shared/projects/polyneuro_risk_score/experiments/toolbox_tutorial/example1/BWAS/tables/Rsquared.csv';
path_reference_table_by_networks= '/panfs/roc/groups/4/miran045/shared/projects/polyneuro_risk_score/experiments/toolbox_tutorial/example1/BWAS/tables/correlations_by_networks.csv';

output_folder_PNS = '/panfs/roc/groups/4/miran045/shared/projects/polyneuro_risk_score/experiments/toolbox_tutorial/example1/PNRS';
path_group_Design_Table_target = '/home/example1/input_data/Group_Design_Table.csv';
path_Group_Color_Table_target = '/home/example1/input_data/Group_Color_Table.csv';
```

```Matlab
PBScores=run_PBS(path_imaging_target,...
    path_betaweights,...
    path_Rsquared,...
    'output_folder',output_folder_PNS,...
    'path_demographics_Table',path_demographics_Table_target,...
    'path_dictionary_demographics_Table',path_dictionary_demographics_Table_target,...
    'path_group_Design_Table',path_group_Design_Table_target,...
    'path_Group_Color_Table',path_Group_Color_Table_target,...
    'path_parcellation_table',path_parcellation_table);
```
### Runing the code using the container
- Exploring the outputs