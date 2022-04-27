<script type="text/javascript"
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS_CHTML"></script>
# Example using functional connectivity as brain features 
In this example we'll run a BWAS analysis using functional connectivity as brain features. Resulting \\( \beta\\)-weights will be used to predict the corresponding brain-derived behavioral scores. Here we assume you have succesfully downloaded the toolbox and associated data. Here we are using as brain features connectivity matrices obtained using macaque data that was parcellated using the [Bezgin ROI set](https://doi.org/10.1016/j.neuroimage.2012.04.013) with network assignment by [Grayson, *et. al.*, 2016](https://doi.org/10.1016/j.neuron.2016.06.005).

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
    - [Group_Color_Table_reference](./example1/Group_Color_Table.csv)

## BWAS: Estimating beta-weights
The first part of the analysis is to calculate the \\( \beta\\)-weights. This section describes how to do it using Matlab and also using the container. See [Detailed specs.](./detailed_description.md) for details about input arguments.

### Runing the code using Matlab
Once you have opened Matlab and added to your session the path to the repo, define the input arguments:

```Matlab
                        path_imaging_reference = '/home/example1/input_data/fconn.mat';
             path_demographics_Table_reference = '/home/example1/input_data/demographcis_Table.csv';
  path_dictionary_demographics_Table_reference = '/home/example1/input_data/Dictionary_for_demographics_Table.csv';
             path_group_Design_Table_reference = '/home/example1/input_data/Group_Design_Table.csv';
                            output_folder_BWAS = '/home/example1/BWAS';
                        path_parcellation_table= '/home/example1/input_data/parcel.mat';
               path_Group_Color_Table_reference= '/home/example1/input_data/Group_Color_Table.csv';
                        model='lutein_PCA1 ~ brain_feature-1';
```

Now, you can call the function as follows:

```Matlab
run_BWAS (path_imaging_reference,...
    path_demographics_Table_reference,...
    path_dictionary_demographics_Table_reference,...
    path_group_Design_Table_reference,...
    'output_folder',output_folder_BWAS,...
    'model',model,...
    'path_parcellation_table',path_parcellation_table) 
```



Once the run is completed, all the files will be saved in the folder you defined as output folder (in this example is `/home/example1/BWAS`). This folder will contain the following subfolders:

```markdown
├── /home/example1  
    ├── BWAS
        └── tables
        ├── figures
            └── weights_explainedvariance
            ├── scatter_plots
               └── by_networks
            └── relativecontributions_plots
            └── pvalues_explainedvariance
            └── manhattan_plots
        └── ciftis
```
The section [Exploring outputs](./exploring_outputs.md) describes all the outputs.


Bonus: Consider re-runing the code using the following model `model='lutein_PCA1 ~ brain_feature+Diet+betacarotene_PCA1+1`, *i.e.*, adding the intersect and controlling for the variables *Diet* and *betacarotene_PCA1*.

### Runing the code using the container


BWAS

To run the container 

```Container
singularity run -B $base_folder:$base_folder container_bwas.sif bwas \
-path_imaging_reference $path_imaging_reference \
-path_demographics_table_reference $path_demographics_Table_reference \
-path_dictionary_demographics_table_reference $path_dictionary_demographics_Table_reference \
-path_group_design_table_reference $path_group_Design_Table_reference \
-output_folder $output_folder_BWAS \
-path_parcellation_table $path_parcellation_table \
-model $model

```
The expression "-B $base_folder:$base_folder" in the container command, mounts the folder $base_folder as a virtual unit located in $base_folder. Its required
when the container does not have access to this path  in the local unit and part or all the data is located.



## PNRS: Estimating risk

Once the \\( \beta\\)-weights are calculated, you can use them to predict scores in an independent sample. Weights and explained variance files are saved in a subfolder named `tables/` within the folder that contains the outputs of the BWAS analyses. See [Detailed specs.](./detailed_description.md) for details about input arguments/
### Runing the code using Matlab
First, you need to define input arguments:

```Matlab

                       path_imaging_target = '/home/example1/input_data/fconn.mat';
            path_demographics_Table_target = '/home/example1/input_data/demographcis_Table.csv';
 path_dictionary_demographics_Table_target = '/home/example1/input_data/Dictionary_for_demographics_Table.csv';

                          path_betaweights = '/home/example1/BWAS/tables/brain_feature.csv';
                             path_Rsquared = '/home/example1/BWAS/tables/Rsquared.csv';
          path_reference_table_by_networks = '/home/example1/BWAS/tables/correlations_by_networks.csv';

                         output_folder_PNS = '/home/example1/PNRS';
            path_group_Design_Table_target = '/home/example1/input_data/Group_Design_Table.csv';
             path_Group_Color_Table_target = '/home/example1/input_data/Group_Color_Table.csv';

```
Now, you can call the function as follows:

```Matlab
PBScores=run_PNRS(path_imaging_target,...
    path_betaweights,...
    path_Rsquared,...
    'output_folder',output_folder_PNS,...
    'path_demographics_Table',path_demographics_Table_target,...
    'path_dictionary_demographics_Table',path_dictionary_demographics_Table_target,...
    'path_group_Design_Table',path_group_Design_Table_target,...
    'path_Group_Color_Table',path_Group_Color_Table_target,...
    'path_parcellation_table',path_parcellation_table,...
    'path_reference_table_by_networks',path_reference_table_by_networks);
```

Once the run is completed, all the files will be saved in the folder you defined as output folder (in this example is `/home/example1/PNRS`). This folder will contain the following subfolders:

```markdown
├── /home/example1/
    ├── PNRS
        └── tables
            └── weights_explainedvariance
        └── figures
            └── weights_explainedvariance
            └── scores
            └── scatter_plots
                └── by_top_connection
                └── by_networks
            └── pvalues_explainedvariance_2_samples
            └── pvalues_explainedvariance
            └── explainedvariance_and_null
```
The section [Exploring outputs](./exploring_outputs.md) describes all the outputs.


### Runing the code using the container

PNRS

```
singularity run -B $base_folder:$base_folder $container pnrs \
-path_imaging_target $path_imaging_target \
-path_betaweights $path_betaweights \
-path_Rsquared $path_Rsquared \
-path_reference_table_by_networks $path_reference_table_by_networks \
-path_demographics_Table $path_demographics_Table_reference \
-path_dictionary_demographics_Table $path_dictionary_demographics_Table_reference \
-path_group_Design_Table $path_group_Design_Table_reference \
-output_folder $output_folder_PNRS
```
