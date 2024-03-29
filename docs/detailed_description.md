<script type="text/javascript"
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS_CHTML"></script>
# Detailed description of input data and files

This package has 2 main functions: `run_CWAS` and `run_PBS` that are used to calculate the \\( \beta\\)-weights and to predict the corresponding brain-derived behavioral scores (or PoyNeuro Risk scores, PNRS), respectively (To note, some of the inputs are compatible with the inputs used in the toolboxes [*fconn_regression*](https://fconn-regression.readthedocs.io/en/latest/) and [*fconn_anova*](https://fconn-anova.readthedocs.io/en/latest/)).

## `run_CWAS`
### Mandatory Inputs
These are positional arguments and need to be provided in this order:

#### 1. **path_imaging_reference**
Path to neuroimaging data (brain features) for the reference sample. This argument can take as brain features connectivity matrices or scalars (such as cortical thickness values) for each participant. This function can accept the imaging data on any of the following formats:

1. path to a *dot mat* file where the last dimension corresponds to participant index. 
    1. Connectivity matrices: this would correspond to a 3D object of dimensions number of ROIs X number of ROIs x number of participants.
    1. Scalars (such as cortical thickness values): this would correspond to a 2D matrix of dimensions number of participants x number of ROIs.
2. path to a text file with extension txt ("*txt file*") with paths to individual files with neuroimaging data. This file should NOT include headers. Each row is the path to a cifti file with imaging data for each participant. The cifti files can correspond to timeseries (parcellated or dense) or a connectivity matrix. 
3. path to a file with extension *csv* ("*csv file*") with imaging data, and corresponds to a 2D matrix of dimensions number of participants x number of ROIs, where the number of columns corresponds to the data from each participant. No headers.   

#### 2. **path_demographics_Table_reference**
Path to a [csv file](https://en.wikipedia.org/wiki/Comma-separated_values) containing the demographic and behavioral data to be used in the BWAS. 
It must have headers. 
The header should describe the data included in the demographics file.
Avoid using spaces and limit the number of '_' to 1 in the name of the headers, specially for the first column.
Each row must have the data for each unique participant. 
The order of each row should match the order of the imaging data (see advanced usage below to skip participants in the imaging data).
Each column corresponds to the associated data for each participant.
It is okay to use a csv file with additional columns/data since you will define which columns to include in the group design table (**group_Design_Table**, see below). This allows you to re-use demographic tables to test different cases.

- **Advanced usage**: You can include a column in this table to associate each participant with its relative position in the neuroimaging data (**path_imaging** ). If provided, the column should have as header the reserved word "*consecutive_number*". If the demographics table does not have a column titled "*consecutive_number*" the code will assume that neuroimaging (**path_imaging**) and non-imaging data are presented in the same order. 
- example of a [**demographics_file**](./detailed_specs/demographics_Table.csv)

#### 3. **path_dictionary_demographics_reference**
This is the path to a csv file where you define if the data included in the demographics file is numeric or alphanumeric. This file must have headers. The number of rows corresponds to the number of columns included in the demogrpahics file. 

- **headers**. This file must have the following headers:
    1. Variable_name, mandatory. The rows of this column must be the headers used in the demographcis file, in the same order. Make sure 1) there are no typos and 2) there are no empty spaces at the begining of the text.
    1. Variable_type, mandatory, only allowed values are alphanumeric or numeric.
    1. Description, optional. Here you can describe each variable.
    1. Range, optional. Here you can indicate the expected range in values
    1. Units, optional. Self-descriptive.
- Examples:
    - [**demographics_file**](./detailed_specs/demographics_Table.csv)
    - [**dictionary_demographics_file**](./detailed_specs/Dictionary_for_demographics_Table.csv)
    
#### 4. **group_Design_Table_reference**
Path to a csv file that defines which elements of the **demographics_Table_reference** will be used in this analysis. 
This table will also indicate if each included variable is a *between* or *within* factor (*within* factors are ignored in this version). 

- Headers. This table must have the following headers in the presented order:
    - **Variable**: Column names from the **demographics_Table** that will be used in the analysis. Column names listed in the **demographics_Table** but not included here will be ignored
    - **Design**: Only option are *between* or *within*
- Valid options for **Design**
    - **outcome**. The row containing the data to be used as outcome must be labeled with the reserved word **outcome**.
    - **id**. You can have the subject id in one of the columns of the demographics file. If you do this, you should use the reserved word *id* for this row. This way the some of the output tables generated using this code will report the subject id for each row/participant. 
    - **between**. Use this reserved word to include covariates in the model. 
- **Example**. This is a [**group_Design_Table** file](./detailed_specs/Group_Design_Table.csv)

### Optional inputs
#### **path_parcellation_table**
Path to a Parcel object that assigns each region of interest (ROI) to a given network. If provided, the code will make Manhattan plots colorcoded by network. 
It can be provided as a [dot mat](./detailed_specs/HCP_ColeAnticevic.mat) file ot as a table in [csv format](./detailed_specs/HCP_ColeAnticevic.csv).

- [dot mat](./detailed_specs/HCP_ColeAnticevic.mat) file with assignemnt of ROIs to functional networks
- [table](./detailed_specs/HCP_ColeAnticevic.csv) that assigns ROIs to functional networks. The number of ROIs (column ix, see below) must equal the number of elements of the neuroimaging data (i.e. mandatory input **path_imaging**). This table must be saved as a csv file and must have the following headers in the presented order
    - ix, index
    - name, Network name. Network name each ROI belong to.
    - shortname, Newortk short name. Two or three letters' acronym to describe the network this ROI belongs to.
    - R. Number from 0 to 1 to indicate the Red value for the RGB colormap.
    - G. Number from 0 to 1 to indicate the Green value for the RGB colormap.
    - B. Number from 0 to 1 to indicate the Blue value for the RGB colormap.

#### **output_folder_reference**
Define the path to save the results. If not provided, the function will make a folder named *BWAS* in the location where the function is called and it will save the results there.

#### **model**
This is an optional input but it is **highly recomended** to use it.
Using this argument, you can explicitly define the model to be used in the BWAS.
If provided, the function will ignore the model defined by the [*group_Design_Table*](#4-groupdesigntable).
The Model must be defined using [Wilkinson notation](https://www.mathworks.com/help/stats/wilkinson-notation.html). 
This input is text defining the model and needs to be formatted as follows: `outcome~brain_feature+...1`, where **outcome** needs to be replaces with the variable to be modeled and **brain_feature** is a reserved words to indicate dependance on the provided imaging data. You can add covariates, if they where provided in the *demographics_table*. Include "1" if you want to include the intercept in the model. 

- Example 1: `lutein_PCA1~brain_feature`. Here you are modeling `lutein_PCA1` as a function of the provided imaging data (`brain_feature`). Notice that `lutein_PCA1` should be data provided in the *demographics_table*.
- Example 2: `Delta_DTCgaitspeed~brain_feature+FoG+Age_at_session+MDS_UPDRSIII_score+1`. Here you are modeling `Delta_DTCgaitspeed` as a function of the provided imaging data (`brain_feature`). This model includes **intercept** and controls for `FoG`, `Age_at_session`, and `MDS_UPDRSIII_score`. 

## `run_PBS`
### Mandatory Inputs
These are positional arguments and need to be provided in this order:

#### 1. **path_imaging_target**
Same as defined for [**path_imaging_reference**](#1-pathimagingreference). 

#### 2. **path_betaweights**
Here you provide the path to the table containg the \\( \beta\\)-weights calculated by the function `run_CWAS`. The path to this table is `output_folder_reference/tables/brain_feature.csv'`
#### 3. **path_Rsquared**
Here you provide the path to the table containg the explained variance by each brain feature. This table is calculated by the function `run_CWAS`. The path to this table is `output_folder_reference/tables/Rsquared.csv'`
### Optional inputs

#### **path_Group_Color_Table**
This optional argument corresponds to a table with colors for categorical variables included as covariates. Those colors are used to colorcode subjects in scatter plots. If not provided, colors will be auto-assigned.
If provided, the table needs to be saved as a csv file and have 4 columns titled: *subgroup*, *R*, *G*, and *B*. To add color for a variable, list the name of the variable, and include the corresponding color in RGB scale (0-1), as indicated in this [example](./detailed_specs/Group_Color_Table.csv). 

#### **output_folder_target**
Define the path to save the results. If not provided, the function will make a folder named *PBS* in the location where the function is called and it will save the results there.

#### **path_demographics_Table_target**
Same as defined for [path_demographics_Table_reference](#2-pathdemographicstablereference)
#### **path_dictionary_demographics_Table_target**
Same as defined for [path_dictionary_demographics_reference](#3-pathdictionarydemographicsreference)
#### **path_group_Design_Table_target**
Same as defined for [group_Design_Table_reference](#4-groupdesigntablereference). This table is used to indicate which column from the *demographics_Table_target* will be used to compare with the predicted brain scores calculated by the function `run_PBS`.  This selection is done by labeling the column of interest with the reserved word *outcome*. 
#### **path_parcellation_table_target**
Same as defined for [path_parcellation_table](#pathparcellationtable)
#### **path_reference_table_by_networks**
This table is created by the function `run_CWAS` when a parcellation table is provided. This table reports how much variance is predicted using within-sample. When provided to the function `run_PBS` it will calculate and combined scores in the target sample following the order provided by this table. The path to this table is `output_folder_reference/tables/correlations_by_networks.csv'`