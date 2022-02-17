# Exploring outputs

This section explores the various outputs of the code. Of note, there are slight differences in the outputs based on your choice of input (functional connectivity or cortical thickness).

- The beta-weights for functional connectivity data are a summation of the absolute values. For cortical thickness, the beta-weights are not summed.
- Given that cortical thickness data does not entail network-network interactions, all of the network figures (and related subfolders) discussed below do not apply.

```markdown
├── BWAS  
    ├── case2_yes_covariates_g1
        └── tables
        ├── figures
            └── weights_explainedvariance
            ├── scatter_plots
               └── by_networks
            └── relativecontributions_plots
            └── pvalues_explainedvariance
            └── manhattan_plots
        └── ciftis
├── PNRS
    ├── case2_yes_covariates_g1
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
Manhattan plot:
![Manhathan plot](./example1/BWAS/figures/Manhattan_plots/BWAS_manhattan_plot_like.png)