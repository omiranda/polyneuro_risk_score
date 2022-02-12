<script type="text/javascript"
        src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS_CHTML"></script>
# Detailed description of input data and files

This package has 2 main functions: `run_CWAS` and `run_PBS` that are used to calculate the \\( \beta\\)-weights and to predict the corresponding brain-derived behavioral score (or PoyNeuro Risk scores, PNRS), respectively (To note, some of the inputs are compatible with the inputs used in the toolboxes [*fconn_regression*](https://fconn-regression.readthedocs.io/en/latest/) and [*fconn_anova*](https://fconn-anova.readthedocs.io/en/latest/)).

## `run_CWAS`
### Mandatory Inputs 
These are positional arguments and need to be provided in this order:

1. **path_imaging**. Path to neuroimaging data (brain features) for the reference sample. The function can take as brain features connectivity matrices or scalars (such as cortical thickness values) for each participant. This function can accept the imaging data on any of the following formats:
    1. path to a *dot mat* file where the last dimension corresponds to participant index. 
        1. Connectivity matrices: this would correspond to a 3D object of dimensions number of ROIs X number of ROIs x number of participants.
        1. Scalars (such as cortical thickness values): this would correspond to a 2D matrix of dimensions number of ROIs x number of participants.
    1. path to a csv file with imaging data where the number of columns  corresponds to participant index (**WIP**, still to be validated). No headers
    1. path to a txt file with paths to individual files with neuroimaging data. Each row is the path to the cifti file with imaging data for each participant. The cifti file can be a timeseries (parcellated or dense) or a connectivity matrix. This file should NOT include headers.
        
1. **path_demographics_Table**. Path to a [csv file](https://en.wikipedia.org/wiki/Comma-separated_values) containing the demographic and behavioral data to be used in the BWAS. 
It must have headers. 
Each row should correspond to the data for each unique participant. 
The order of each row should match the order of the imaging data (see advanced usage below to skip participants in the imaging data).
Each column corresponds to the associated data for each participant.
You can use a csv file with more columns than needed in the analyis. 
Youl will select with columns to include in the group design table (**group_Design_Table**).

Such headers will be defined as *between* or *within* factors in the group design table (**group_Design_Table**). Depending on the analysis, this table could also have the reserved headers *id* and *outcome*. You could include a column in this table to associate each participant with the its relative position in the neuroimaging data (**path_imaging** ). If provided, the column should be titled "*consecutive_number*". If not provided, it will be assumed that neuroimaging (**path_imaging**) and non-imaging data are presented in the same order.
1. **group_Design_Table** Path to group design table that indicates which elements of the **demographics_Table** will be used in this analysis. This table also will indicate if each included parameters is a *between* or *within* factor. This table must have the following headers in the presented order:
    - Variable: Column names from the **demographics_Table** that will be used in the analysis. Column names listed in the **demographics_Table** but not included here will be ignored
    - Design: Only option are *between* or *within*

### Optional inputs
        - **parcel**, Parcel object. If not provided it will use defaults
            - dot mat file with assignemnt of ROIs to functional networks
            - table that assigns ROIs to functional networks. The number of ROIs (column ix, see below) must equal the number of elements of the neuroimaging data (i.e. mandatory input **path_imaging**). This table must be saved as a csv file and must have the following headers in the presented order
                - index
                - Network name. Network name each ROI belong to.
                - Newortk short name. Two or three letters' acronym to describe the network this ROI belongs to.
                - R. Number from 0 to 1 to indicate the Red value for the RGB colormap.
                - G. Number from 0 to 1 to indicate the Green value for the RGB colormap.
                - B. Number from 0 to 1 to indicate the Blue value for the RGB colormap.
        - color group design table. If not provided, colors will be assigned 
        - options, an structure with the required fields or a dot m  file with the text to define this structure
        - output_folder. Path to output folder to save the results. If not provided, it will make in the current path a new folder named output_fconn_anovan


