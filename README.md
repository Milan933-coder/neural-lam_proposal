# Merge Plan: `prob_model_global` And `prob_model-lam` Into The `New_hirearchy branch` Hierarchy

## Scope

This document is now written against the `New_hirearchy branch` hierarchy:

- `New_hirearchy branch`

The earlier folders are treated as donor branches that must merge into this hierarchy, not replace it:

- `prob_model_global`
- `prob_model-lam`

## Current Target Hierarchy Read For This Proposal

I based this final merge plan on the current target branch files below:

- `README.md`
- `neural_lam/config.py`
- `neural_lam/create_graph.py`
- `neural_lam/weather_dataset.py`
- `neural_lam/train_model.py`
- `neural_lam/metrics.py`
- `neural_lam/vis.py`
- `neural_lam/models/__init__.py`
- `neural_lam/models/step_predictor.py`
- `neural_lam/models/ar_forecaster.py`
- `neural_lam/models/forecaster.py`
- `neural_lam/models/forecaster_module.py`
- `neural_lam/models/graph_lam.py`
- `neural_lam/models/hi_lam.py`
- `neural_lam/datastore/__init__.py`
- `neural_lam/datastore/base.py`
- `neural_lam/datastore/mdp.py`
- `neural_lam/datastore/npyfilesmeps/store.py`
- `tests/test_prediction_model_classes.py`
- `tests/test_training.py`
- `tests/test_graph_creation.py`

## Executive Summary

The right final destination is the `New_hirearchy branch` structure.

The old probabilistic branches should not be merged by copying their top-level layout. Instead:

1. Keep the current package hierarchy in `New_hirearchy branch`.
2. Port the shared probabilistic model files into `neural_lam/models/`.
3. Add the global data path as a new datastore lane under `neural_lam/datastore/`.
4. Extend the existing training path built around `StepPredictor`, `ARForecaster`, and `ForecasterModule`.
5. Keep graph generation, plotting, tests, and CLI centered on the files that already exist in this branch.

## Why This Current Hierarchy Is The Correct Final Landing

The `New_hirearchy branch` already has the right abstractions:

- `neural_lam/config.py` provides config-driven execution
- `neural_lam/datastore/base.py` already supports `is_forecast` and `is_ensemble`
- `neural_lam/weather_dataset.py` already handles forecast-style and analysis-style datastores
- `neural_lam/create_graph.py` is already the graph-generation entry point
- `neural_lam/models/step_predictor.py` defines the predictor interface
- `neural_lam/models/ar_forecaster.py` handles autoregressive rollout
- `neural_lam/models/forecaster_module.py` owns training, validation, testing, and logging
- `tests/` already covers graph creation, training, datasets, and model classes

So the merge should finish this architecture, not go back to the older branch layout.

## What The Old Branches Contribute

### Shared probabilistic model core

These files are the main reusable probabilistic contribution from both donor branches:

- `neural_lam/models/graph_efm.py`
- `neural_lam/models/base_latent_encoder.py`
- `neural_lam/models/base_graph_latent_decoder.py`
- `neural_lam/models/constant_latent_encoder.py`
- `neural_lam/models/graph_latent_encoder.py`
- `neural_lam/models/graph_latent_decoder.py`
- `neural_lam/models/hi_graph_latent_encoder.py`
- `neural_lam/models/hi_graph_latent_decoder.py`

### Global-specific donor logic

From `prob_model_global`, the final repo mainly needs:

- global ERA5 data handling from `neural_lam/era5_dataset.py`
- forecast export ideas from `neural_lam/forecast_to_xarr.py`
- spherical graph-generation logic from `create_global_mesh.py`
- global forcing and static feature logic from `create_global_forcing.py` and `create_global_grid_features.py`

### LAM-specific donor logic

From `prob_model-lam`, the final repo mainly needs:

- the probabilistic LAM model path
- existing MEPS assumptions as reference for compatibility
- current LAM graph conventions from `create_mesh.py`
- current LAM plotting behavior from `plot_graph.py`

## Final Merge Principle

The earlier branches should merge into this branch using three rules:

### Rule 1: Keep the current branch structure

Keep these current files and directories as the permanent skeleton:

- `neural_lam/config.py`
- `neural_lam/datastore/`
- `neural_lam/create_graph.py`
- `neural_lam/weather_dataset.py`
- `neural_lam/train_model.py`
- `neural_lam/models/`
- `tests/`

### Rule 2: Add shared probabilistic files inside the existing hierarchy

The probabilistic merge should happen by adding files **inside** `neural_lam/models/`, not by reviving the donor branch shape.

### Rule 3: Put global support into `datastore/`, not into a second training stack

Because `BaseDatastore` and `WeatherDataset` already support forecast and ensemble data, the cleanest final landing for the global branch is a new datastore implementation under the existing `neural_lam/datastore/` hierarchy.

## Final Repo Map

### A. Existing current files to keep as-is structurally

- `New_hirearchy branch/neural_lam/config.py`
- `New_hirearchy branch/neural_lam/create_graph.py`
- `New_hirearchy branch/neural_lam/weather_dataset.py`
- `New_hirearchy branch/neural_lam/train_model.py`
- `New_hirearchy branch/neural_lam/metrics.py`
- `New_hirearchy branch/neural_lam/vis.py`
- `New_hirearchy branch/neural_lam/models/step_predictor.py`
- `New_hirearchy branch/neural_lam/models/ar_forecaster.py`
- `New_hirearchy branch/neural_lam/models/forecaster.py`
- `New_hirearchy branch/neural_lam/models/forecaster_module.py`
- `New_hirearchy branch/neural_lam/models/graph_lam.py`
- `New_hirearchy branch/neural_lam/models/hi_lam.py`
- `New_hirearchy branch/neural_lam/models/hi_lam_parallel.py`
- `New_hirearchy branch/neural_lam/datastore/__init__.py`
- `New_hirearchy branch/neural_lam/datastore/base.py`
- `New_hirearchy branch/neural_lam/datastore/mdp.py`
- `New_hirearchy branch/neural_lam/datastore/npyfilesmeps/store.py`
- `New_hirearchy branch/tests/test_prediction_model_classes.py`
- `New_hirearchy branch/tests/test_training.py`
- `New_hirearchy branch/tests/test_graph_creation.py`

### B. New files to add into this branch

### Shared probabilistic model files

- `New_hirearchy branch/neural_lam/models/graph_efm.py`
- `New_hirearchy branch/neural_lam/models/base_latent_encoder.py`
- `New_hirearchy branch/neural_lam/models/base_graph_latent_decoder.py`
- `New_hirearchy branch/neural_lam/models/constant_latent_encoder.py`
- `New_hirearchy branch/neural_lam/models/graph_latent_encoder.py`
- `New_hirearchy branch/neural_lam/models/graph_latent_decoder.py`
- `New_hirearchy branch/neural_lam/models/hi_graph_latent_encoder.py`
- `New_hirearchy branch/neural_lam/models/hi_graph_latent_decoder.py`

### Global datastore lane

The cleanest final landing here is inferred from the current datastore layout:

- `New_hirearchy branch/neural_lam/datastore/era5/__init__.py` (inferred)
- `New_hirearchy branch/neural_lam/datastore/era5/config.py` (inferred)
- `New_hirearchy branch/neural_lam/datastore/era5/store.py` (inferred)

### Optional utility if forecast export stays standalone

- `New_hirearchy branch/neural_lam/forecast_to_xarr.py`

### C. Current files to extend

- `New_hirearchy branch/neural_lam/models/__init__.py`
- `New_hirearchy branch/neural_lam/train_model.py`
- `New_hirearchy branch/neural_lam/models/forecaster_module.py`
- `New_hirearchy branch/neural_lam/models/ar_forecaster.py`
- `New_hirearchy branch/neural_lam/metrics.py`
- `New_hirearchy branch/neural_lam/vis.py`
- `New_hirearchy branch/neural_lam/create_graph.py`
- `New_hirearchy branch/neural_lam/plot_graph.py`
- `New_hirearchy branch/neural_lam/datastore/__init__.py`
- `New_hirearchy branch/tests/test_prediction_model_classes.py`
- `New_hirearchy branch/tests/test_training.py`
- `New_hirearchy branch/tests/test_graph_creation.py`
- `New_hirearchy branch/tests/test_datasets.py`
- `New_hirearchy branch/tests/test_datastores.py`

### D. Donor files that should stay reference-only

These older files are useful as sources, but they should not become the final main structure in this branch:

- `prob_model_global/train_model.py`
- `prob_model-lam/train_model.py`
- `prob_model_global/create_global_mesh.py`
- `prob_model-lam/create_mesh.py`
- `prob_model_global/create_global_grid_features.py`
- `prob_model-lam/create_grid_features.py`
- `prob_model_global/create_global_forcing.py`
- `prob_model-lam/create_parameter_weights.py`
- `prob_model_global/neural_lam/era5_dataset.py`
- `prob_model-lam/neural_lam/weather_dataset.py`
- `prob_model_global/neural_lam/models/ar_model.py`
- `prob_model-lam/neural_lam/models/ar_model.py`

## Exact Merge Mapping Into The Current Hierarchy

| Donor file | Final landing in this branch |
| --- | --- |
| `prob_model_global/neural_lam/models/graph_efm.py` | `neural_lam/models/graph_efm.py` |
| `prob_model_global/neural_lam/models/graph_latent_encoder.py` | `neural_lam/models/graph_latent_encoder.py` |
| `prob_model_global/neural_lam/models/graph_latent_decoder.py` | `neural_lam/models/graph_latent_decoder.py` |
| `prob_model_global/neural_lam/models/hi_graph_latent_encoder.py` | `neural_lam/models/hi_graph_latent_encoder.py` |
| `prob_model_global/neural_lam/models/hi_graph_latent_decoder.py` | `neural_lam/models/hi_graph_latent_decoder.py` |
| `prob_model_global/neural_lam/models/constant_latent_encoder.py` | `neural_lam/models/constant_latent_encoder.py` |
| `prob_model_global/neural_lam/models/base_latent_encoder.py` | `neural_lam/models/base_latent_encoder.py` |
| `prob_model_global/neural_lam/models/base_graph_latent_decoder.py` | `neural_lam/models/base_graph_latent_decoder.py` |
| `prob_model_global/neural_lam/era5_dataset.py` | `neural_lam/datastore/era5/store.py` (adapted, inferred) |
| `prob_model_global/neural_lam/forecast_to_xarr.py` | `neural_lam/forecast_to_xarr.py` or `neural_lam/datastore/era5/` utility |
| `prob_model_global/create_global_mesh.py` | logic folded into `neural_lam/create_graph.py` |
| `prob_model_global/plot_global_graph.py` | logic folded into `neural_lam/plot_graph.py` |
| `prob_model-lam/neural_lam/models/graphcast.py` | no new file, aligns with current `neural_lam/models/graph_lam.py` |
| `prob_model-lam/neural_lam/models/graph_fm.py` | no new file, aligns with current `neural_lam/models/hi_lam.py` |
| `prob_model-lam/create_mesh.py` | reference for extending `neural_lam/create_graph.py` |
| `prob_model-lam/plot_graph.py` | reference for extending `neural_lam/plot_graph.py` |

## Diagram: Donor Branches Into Current Repo

```mermaid
flowchart LR
    A["prob_model_global
    graph_efm.py
    era5_dataset.py
    create_global_mesh.py"] --> C["Port shared models + global datastore logic"]

    B["prob_model-lam
    graph_efm.py
    create_mesh.py
    MEPS conventions"] --> D["Port shared models + keep LAM datastore lane"]

    C --> E["New_hirearchy branch"]
    D --> E

    E --> F["config.py"]
    E --> G["datastore/"]
    E --> H["weather_dataset.py"]
    E --> I["create_graph.py"]
    E --> J["models/"]
    E --> K["tests/"]
```

## Diagram: Final Repo After Merge

```mermaid
flowchart TD
    A["neural_lam/config.py"] --> B["neural_lam/datastore/__init__.py"]

    B --> C["neural_lam/datastore/mdp.py"]
    B --> D["neural_lam/datastore/npyfilesmeps/store.py"]
    B --> E["neural_lam/datastore/era5/store.py"]

    C --> F["neural_lam/weather_dataset.py"]
    D --> F
    E --> F

    F --> G["neural_lam/train_model.py"]
    G --> H["neural_lam/models/__init__.py"]

    H --> P["neural_lam/models/step_predictor.py"]
    P --> Q["neural_lam/models/base_graph_model.py"]
    Q --> R["neural_lam/models/base_hi_graph_model.py"]

    Q --> I["graph_lam.py"]
    R --> J["hi_lam.py"]
    P --> K["graph_efm.py"]

    K --> L["latent encoder and decoder files"]

    I --> M["ar_forecaster.py"]
    J --> M
    K --> M

    M --> N["forecaster_module.py"]
    N --> O["tests/"]
```

## Four-Week Timeline

### Week 1

- compare `prob_model_global` and `prob_model-lam` model files with the current `New_hirearchy branch`
- add the shared probabilistic model files under `neural_lam/models/`
- update `neural_lam/models/__init__.py` so the new model classes are visible in the current hierarchy

### Week 2

- extend `neural_lam/train_model.py` to load the probabilistic model path
- update `neural_lam/models/ar_forecaster.py` and `neural_lam/models/forecaster_module.py` for probabilistic rollout and loss handling
- add any metric changes needed in `neural_lam/metrics.py`

### Week 3

- add the global ERA5 datastore under `neural_lam/datastore/era5/`
- register it in `neural_lam/datastore/__init__.py`
- extend `neural_lam/create_graph.py` and `neural_lam/plot_graph.py` so global graph generation fits the same branch structure

### Week 4

- add and update tests for models, datastore loading, dataset slicing, training, and graph creation
- run integration checks to make sure deterministic LAM, probabilistic LAM, and global paths work inside one hierarchy
- do final cleanup of documentation and merge notes

My Propsal for issue 49  Will not Create any issue as the CNN Predictor ,ARForecasterSmapler and EnsembleForecasterModule will be separate files we just have to modify the training script or adda separate script to use the files to train
