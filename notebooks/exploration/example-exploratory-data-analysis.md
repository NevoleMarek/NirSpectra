---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.4.0
  kernelspec:
    display_name: nirspectra
    language: python
    name: nirspectra
---

<!-- #region Collapsed="false" -->
# Exploration Data Analysis

This notebook shows initial exploratory data analysis using DataAnalysisReportTool (dart) and generates an html report.
<!-- #endregion -->

```python Collapsed="false"
# Enable autoreloading
%load_ext autoreload
%autoreload 2
import os
from pathlib import Path

import pandas as pd
import dart
```

```python Collapsed="false"
# Setup and create directory for our report
reports_dir = "../../results/reports/exploratory_data_analysis"
Path(reports_dir).mkdir(parents=True, exist_ok=True)

```

<!-- #region Collapsed="false" -->
## Load data
<!-- #endregion -->

```python Collapsed="false"
# Load your data here
```

```python Collapsed="false"
titanic_df = dart.example_datasets.dataset_titanic()
dataset_name = "Titanic dataset"
dataset_desc = "Data about passangers of a titanic ship and information whether they survived."
```

<!-- #region Collapsed="false" -->
## Show exploratory data analysis
<!-- #endregion -->

```python Collapsed="false"
report = dart.Report(titanic_df)
report.show()
```

<!-- #region Collapsed="false" -->
## Conclusion

What can be concluded? Are the goals of the analysis met?
<!-- #endregion -->

<!-- #region Collapsed="false" -->
#### Generate report
<!-- #endregion -->

```python Collapsed="false"
report.export_html(os.path.join(reports_dir, "dart_titanic.html"),
                   dataset_name=dataset_name,
                   dataset_description=dataset_desc)
```

```python Collapsed="false"

```

