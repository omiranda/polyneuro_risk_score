# Installation

## Matlab

## Using a container

You can run the container version of this toolbox. To do it, you need to:

1. Make sure you have [singularity](https://sylabs.io/guides/3.5/user-guide/introduction.html) in your system. Here are [instructions on how to install it](https://sylabs.io/guides/3.0/user-guide/installation.html).
2. Download the container (WE NEED TO PROVIDE THE PATH).
3. Run it from terminal. See detailed instructions in the provided examples that run a BWAS [using functional connectivity](basic_example.md) and [cortical thickness data](./going_deeper_1.md).
4. Memory and execution time varies depending on the number of brain features and participants, you might need to request resources using slurm. See section for some examples about time and memory needs.

## Memory considerations

Memory and time depends on number of subjects, brain features and number of covariates. 
Runing the examples provided here should require less than 4GB of RAM and less than 30 mins. Doing a BWAS analysis using ~6,000 participants and 62,000 brain featues can take 4-8 hours.