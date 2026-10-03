# Customer Segmentation with K-Means Clustering

A beginner-friendly **unsupervised learning** project: group 200 mall customers into natural segments so each group can get a different marketing action. There are no labels, so the algorithm finds groups of similar customers on its own.

## Dataset

`Mall_Customers.csv`: 200 customers from the public *Mall Customers* practice dataset.

| Column | Description |
|---|---|
| `Gender`, `Age` | Customer background |
| `Annual Income (k$)` | Annual income in thousands of dollars |
| `Spending Score (1-100)` | Score assigned by the mall; higher = spends more |

> **Note:** How this dataset was collected is not documented, and it is widely used as a teaching dataset. Treat it as a learning dataset, not real business data.

## Approach

1. **Explore:** distributions, missing values (none), and the income vs spending-score scatter plot, which shows visible clumps
2. **Scale** income and spending score with `StandardScaler`, because K-Means uses distances
3. **Choose k** with the **elbow method** and the **silhouette score** (both point to k = 5)
4. **Run K-Means** (k = 5, `n_init=10`) and profile each segment in original units
5. **Turn segments into actions:** a suggested marketing idea for each group
6. **Validate stability** (there are no true labels, so no accuracy):
   - different random starts, measured with the Adjusted Rand Index (ARI)
   - a different algorithm (hierarchical / Ward clustering)
7. **Test a design choice:** does adding Age as a third feature help?
8. **Assign new customers** to a segment

## Results

| Segment | Customers | Avg income (k$) | Avg spending score | Avg age |
|---|---|---|---|---|
| High income, high spending | 39 | 87 | 82 | 33 |
| Low income, high spending | 22 | 26 | 79 | 25 |
| Average income, average spending | 81 | 55 | 50 | 43 |
| High income, low spending | 35 | 88 | 17 | 41 |
| Low income, low spending | 23 | 26 | 21 | 45 |

### Key takeaways

- **k = 5** is supported by both the elbow method and the silhouette score (0.55, highest of k = 2 to 10).
- **The result is stable.** With one random start the segments sometimes changed (worst-case ARI 0.69); with 10 starts the result was identical every time (ARI 1.00). Hierarchical clustering found almost the same segments (ARI 0.94).
- **Adding Age lowers the silhouette score** (0.55 → 0.42), so the final model clusters on income and spending score only.
- **Marketing view:** *High income, low spending* is the biggest untapped opportunity, and *High income, high spending* is the most valuable group. The two high-spending segments are also the youngest. Gender does not separate the segments.

## How to run

```bash
pip install pandas numpy scikit-learn scipy seaborn matplotlib jupyter
jupyter notebook customer_segmentation_clustering.ipynb
```

Keep `Mall_Customers.csv` in the same folder as the notebook.

## Project structure

```
├── customer_segmentation_clustering.ipynb   # full walkthrough with explanations
├── Mall_Customers.csv                       # dataset
└── README.md
```

## Limitations

- Clustering has **no ground truth**: the number of clusters and segment names are judgement calls supported by metrics.
- Small dataset (200 customers), two behaviour features, and an undocumented "spending score".
- K-Means assumes roughly round, similar-sized clusters and is sensitive to scaling and the choice of k.
- Segments describe customers but do not explain *why* they behave that way. Segment-specific offers would need an A/B test.

## Tech stack

Python, pandas, NumPy, scikit-learn, SciPy, matplotlib, seaborn, Jupyter

## Author

Yogesh
