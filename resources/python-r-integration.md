# PowerBI with Python & R Integration 🐍📊

## Introduction

PowerBI supports integration with Python and R for advanced analytics, custom visuals, and data science workflows. This guide covers setup, use cases, and best practices.

## 1. Enabling Python & R in PowerBI
- Install Python (Anaconda or standard) and/or R
- In PowerBI Desktop: File → Options → Python scripting / R scripting
- Set the path to your Python/R executable

## 2. Python in PowerBI
### Use Cases
- Data cleaning and transformation
- Advanced analytics (regression, clustering)
- Custom visuals (matplotlib, seaborn, plotly)

### Example: Python Visual
```python
import matplotlib.pyplot as plt
import pandas as pd
plt.bar(dataset['Category'], dataset['Value'])
plt.title('Category Breakdown')
plt.show()
```

### Example: Data Transformation
```python
import pandas as pd
dataset['LogValue'] = dataset['Value'].apply(lambda x: np.log(x+1))
```

## 3. R in PowerBI
### Use Cases
- Statistical analysis
- Time series forecasting
- Custom visuals (ggplot2, plotly)

### Example: R Visual
```r
library(ggplot2)
ggplot(dataset, aes(x=Category, y=Value)) + geom_col() + theme_minimal()
```

### Example: Data Transformation
```r
dataset$LogValue <- log(dataset$Value + 1)
```

## 4. Best Practices
- Keep scripts efficient and well-commented
- Limit data volume passed to scripts
- Install required packages in your Python/R environment
- Test scripts outside PowerBI before integration

## 5. Limitations
- No interactive plots (static images only)
- Data passed to scripts is limited (150k rows for visuals)
- Requires local Python/R installation for refresh

## Resources
- [Python in PowerBI](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-python-scripts)
- [R in PowerBI](https://docs.microsoft.com/en-us/power-bi/connect-data/desktop-r-scripts)

---

**Supercharge PowerBI with Python and R for advanced analytics and custom visuals!** 🐍📊 