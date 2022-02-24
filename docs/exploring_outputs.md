# Exploring outputs

This section explores the various outputs of the code.

While only .png files are referenced below, the code will output three versions of each figure:

1. .fig - this version is for ease of editability within Matlab
2. .png - this version is made for sharing
3. .tif - this is a high resolution version for presentations and publications

Of note, there are slight differences in the outputs based on your choice of input (functional connectivity or cortical thickness).

- The beta-weights for functional connectivity data are a summation of the absolute values. For cortical thickness, the beta-weights are not summed.
- Given that cortical thickness data does not entail network-network interactions, all of the network figures (and related subfolders) discussed below do not apply.

## *Output File Tree*

```markdown
├── /home/example1  
    ├── BWAS
        └── tables
        └── figures
            └── Weights_ExplainedVariance
            └── RelativeContributions_plots
            └── pValues_ExplainedVariance
            └── Manhattan_plots
        └── ciftis
            └── brain_feature
    ├── PNRS
        └── tables
            └── Weights_ExplainedVariance
        └── figures
            └── Weights_ExplainedVariance
            └── scores
            └── scatter_plots
                └── by_top_connection
                └── by_networks
            └── pValues_ExplainedVariance_2_samples
            └── pValues_ExplainedVariance
            └── ExplainedVariance_and_Null
```

## BWAS

___

### ***Tables***

- Rsquared.csv
- brain_feature.csv
- correlations_by_networks.csv
- mapping_brain_feature_index_2_ROIs.csv
- scores_by_networks.csv

### ***Figures***

#### **Weights by Explained Variance**

- AbsWeights_explainedVariance_by_Weight.png

![AbsWeights by Weight ](./example1/BWAS/figures/Weights_ExplainedVariance/AbsWeights_explainedVariance_by_Weight.png)

- AbsWeights_explainedVariance_by_brainFeature.png
- AbsWeights_explainedVariance_by_explainedvariance.png
- Weights_explainedVariance_by_Weight.png
- Weights_explainedVariance_by_brainFeature.png

![Weights by Explained Variance](./example1/BWAS/figures/Weights_ExplainedVariance/Weights_explainedVariance_by_brainFeature.png)

- Weights_explainedVariance_by_explainedvariance.png

#### **Relative Contribution Plots**

- Relative_networks_contribution.png

![Relative networks](./example1/BWAS/figures/RelativeContribution_plots/Relative_networks_contribution.png)

- Relative_networks_contribution_truncated.png

#### **P-values by Explained Variance**

- pValue_Variance_by_brainFeature.png

![pValue](./example1/BWAS/figures/pValues_ExplainedVariance/pValue_Variance_by_brainFeature.png)

- logpValue_Variance_by_brainFeature.png

#### **Manhattan Plots**

- BWAS_manhattan_plot_like.png

![Manhattan plot](./example1/BWAS/figures/Manhattan_plots/BWAS_manhattan_plot_like.png)

- BWAS_manhattan_plot_covering_less_than_1_percent.png
- BWAS_manhattan_plot_covering_more_than_1_percent.png
- BWAS_manhattan_plot_truncated.png

### ***Ciftis***

- explained_variance.pconn.nii
- normalized_rank_by_explaining_variance.pconn.nii
- rois_sorted_by_explaining_variance.pconn.nii

#### **Brain Features**

- by_networks
- by_networks_cummulative
- by_networks_cummulative_reversed
- sum_betas_by_th
- brain_feature_Estimate.pconn.nii
- brain_feature_pValue.pconn.nii
- brain_feature_tStat.pconn.nii

## PNRS

___

### ***Tables***

#### **Weights by Explained Variance**

- Rsquared.csv

#### **Correlations**

- correlations.csv
- correlations_by_networks.csv
- correlations_by_networks_cummulative.csv
- correlations_by_networks_cummulative_reversed.csv

#### **Scores**

- scores.csv
- scores_by_networks.csv
- scores_by_networks_cummulative.csv
- scores_by_networks_cummulative_reserved.csv

### ***Figures***

#### **Explained Variance and Null**

- Correlations_N_11.png

![Correlations](./example1/PNRS/figures/ExplainedVariance_and_Null/Correlations_N_11.png)

- Correlationsbynetworks_N_15.png
- Correlationsbynetworks_N_28.png
- Correlationsbynetworks_Negcorr__N_15.png
- Correlationsbynetworks_Negcorr__N_28.png
- ExplainedVariance_N_11.png
- ExplainedVariancebynetworks_N_15.png
- ExplainedVariancebynetworks_N_28.png
- ExplainedVariancebynetworks_Negcorr__N_15.png
- ExplainedVariancebynetworks_Negcorr__N_28.png

![Explained Variance NegCorr](./example1/PNRS/figures/ExplainedVariance_and_Null/ExplainedVariancebynetworks_Negcorr__N_15.png)

#### **Weights and Explained Variance**

- AbsWeights_explainedVariance_by_Weight.png

![AbsWeights Explained Variance](./example1/PNRS/figures/Weights_ExplainedVariance/AbsWeights_explainedVariance_by_Weight.png)

- AbsWeights_explainedVariance_by_brainFeature.png
- AbsWeights_explainedVariance_by_explainedVariance.png
- Weights_explainedVariance_by_Weight.png
- Weights_explainedVariance_by_brainFeature.png
- Weights_explainedVariance_by_explainedVariance.png

![AbsWeights Explained Variance](./example1/PNRS/figures/Weights_ExplainedVariance/Weights_explainedVariance_by_explainedVariance.png)

#### **pValues and Explained Variance**

- pValue_Variance_by_brainFeature.png
- logpValue_Variance_by_brainFeature.png

![log pValue](./example1/PNRS/figures/pValues_ExplainedVariance/logpValue_Variance_by_brainFeature.png)

#### **pValues and Explained Variance - 2 Samples**

- pValue_Variance_by_brainFeature.png

![pValue 2 samples](./example1/PNRS/figures/pValues_ExplainedVariance_2_samples/logpValue_Variance_by_brainFeature.png)

- logpValue_Variance_by_brainFeature.png

#### **Scatter Plots**

- by_networks
    - dscatter
    - gscatter
    - scatter

- by_top_connection
    - dscatter

  ![dscatter top 2%](./example1/PNRS/figures/scatter_plots/by_top_connection/dscatter_top_002_point_0_percent.png)

    - gscatter

  ![gscatter top 10%](./example1/PNRS/figures/scatter_plots/by_top_connection/gscatter_top_010_point_0_percent_by_Diet.png)

    - scatter

  ![scatter top 25%](./example1/PNRS/figures/scatter_plots/by_top_connection/scatter_top_025_point_0_percent.png)

#### **Scores**

- Scores.png

![scores](./example1/PNRS/figures/scores/Scores.png)

- Scores_by_networks.png
