# fMRI Preprocessing & Functional Connectivity

My first neuroimaging pipeline (July 2024). It takes raw BIDS data from OpenNeuro through FSL
preprocessing to region-level functional connectivity, with a focus on language-network and
prefrontal regions.

## Pipeline

1. **Preprocessing in Nipype + FSL** (`notebooks/fsl_preprocessing_workflow.ipynb`): a reusable
   workflow graph (`SelectFiles` → `ExtractROI` → `BET` skull-stripping → `MCFLIRT` motion
   correction → `SliceTimer` → `FLIRT` registration → `Smooth` → `DataSink`), run per
   subject with iterables. `nipype_basics.ipynb` builds the same pieces step by step.
2. **ROI masks** (`roi/`): language-network and prefrontal masks built from Harvard-Oxford labels.
3. **Time series & connectivity**:
   - `task_connectivity_nilearn.ipynb`: Harvard-Oxford atlas, `NiftiLabelsMasker` (standardized,
     detrended), correlation connectomes computed separately for each **task condition** by
     slicing the time series with the BIDS events file.
   - `roi_timeseries_analysis.ipynb`: registration to the MNI152 template, mean ROI time series, pairwise Pearson connectivity and
     per-subject connectivity plots.
   - `preprocessing_and_plots.ipynb`: AAL-atlas connectivity and brain visualizations (`figures/`).

## Data

OpenNeuro **ds000051** and **ds000001** (balloon analog risk task), not included. Put them under
`data/` (for example `data/ds000051/sub-01/...`). Preprocessing needs a local FSL install.

## Tech

Nipype · FSL · nilearn · nibabel · NumPy / SciPy · matplotlib
