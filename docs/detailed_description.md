<script type="text/javascript"
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS_CHTML"></script>
# Detailed description of input data and files

This package has 2 main functions: `run_CWAS` and `run_PBS` that are used to calculate the \\( \beta\\)-weights and to predict the corresponding brain-derived behavioral scores (or PoyNeuro Risk scores, PNRS), respectively (To note, some of the inputs are compatible with the inputs used in the toolboxes [*fconn_regression*](https://fconn-regression.readthedocs.io/en/latest/) and [*fconn_anova*](https://fconn-anova.readthedocs.io/en/latest/)).

## `run_CWAS`
### Mandatory Inputs 
These are positional arguments and need to be provided in this order:

#### 1. **path_imaging**. 
Path to neuroimaging data (brain features) for the reference sample. This argument can take as brain features connectivity matrices or scalars (such as cortical thickness values) for each participant. This function can accept the imaging data on any of the following formats:

1. path to a *dot mat* file where the last dimension corresponds to participant index. 
    1. Connectivity matrices: this would correspond to a 3D object of dimensions number of ROIs X number of ROIs x number of participants.
    1. Scalars (such as cortical thickness values): this would correspond to a 2D matrix of dimensions number of ROIs x number of participants.
2. path to a text file with extension txt ("*txt file*") with paths to individual files with neuroimaging data. This file should NOT include headers. Each row is the path to a cifti file with imaging data for each participant. The cifti files can correspond to timeseries (parcellated or dense) or a connectivity matrix. 
3. path to a file with extension *csv* ("*csv file*") with imaging data, where the number of columns corresponds to the data from each participant (**WIP**, still to be validated). No headers.   

#### 2. **path_demographics_Table**. 
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

#### 3. **path_dictionary_demographics**. 
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
    
#### 4. **group_Design_Table** 
Path to a csv file that defines which elements of the **demographics_Table** will be used in this analysis. 
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

#### **path_Group_Color_Table**
If not provided, colors will be assigned 

#### **output_folder**. 
Define the path to save the results. If not provided, the function will make a folder named *BWAS* in the location where the function is called and it will save the results there.

#### **model**
This is an optional input but it is highly recomended to use it.
(If used, it will ignore the data provided in the [*group_Design_Table*](#4-groupdesigntable)).
Here you can explicitly define the model to be used in the BWAS.
Model must be defined using [Wilkinson notation](https://www.mathworks.com/help/stats/wilkinson-notation.html). 
This input is text defined the model formatted as follows: `outcome~brain_feature+...1`, where **outcome** and **brain_feature** are reserved words that are used to define the behavioral outcome to be predicted as a function of the imaging data, respectively. Include "1" if you want to include the intercept in the model. You can also add covariates that are defined in the *demographics_table*.

- Example 1: `outcome~brain_feature`. Here you are only predicting the variable defined as outcome as a function of the imaging data.
- Example 2: `outcome~brain_feature+FoG+Age_at_session+MDS_UPDRSIII_score+1`. Same as before but controling for `FoG`, `Age_at_session`, and `MDS_UPDRSIII_score`. The model also includes intercept.
#### **options**
an structure with the required fields or a dot m  file with the text to define this structure
## `run_PBS`