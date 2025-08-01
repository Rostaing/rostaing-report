# Rostaing Report: The All-in-One Data Analysis Toolkit

<p align="center">
  <a href="https://pypi.org/project/rostaing-report/"><img src="https://img.shields.io/pypi/v/rostaing-report?color=blue&label=PyPI%20version" alt="PyPI version"></a>
  <a href="https://pypi.org/project/rostaing-report/"><img src="https://img.shields.io/pypi/pyversions/rostaing-report.svg" alt="Python versions"></a>
  <a href="https://github.com/Rostaing/rostaing-report/blob/main/LICENSE"><img src="https://img.shields.io/pypi/l/rostaing-report.svg" alt="License"></a>
  <a href="https://pepy.tech/project/rostaing-report"><img src="https://static.pepy.tech/badge/rostaing-report" alt="Downloads"></a>
</p>

**rostaing-report** is a comprehensive Python toolkit for data professionals, designed to accelerate the entire data analysis workflow, from initial exploration to final presentation. With a single, intuitive interface, it transforms a raw Pandas DataFrame into a rich, interactive analysis environment.

This package provides a powerful, unified interface for:
1.  **Automated Exploratory Data Analysis (EDA)**: Generate a complete statistical report.
2.  **Advanced Statistical Testing**: Perform a wide range of inferential tests.
3.  **Interactive Data Visualization**: Create publication-quality, interactive plots with a single command.

It is built for **Data Scientists**, **Data Analysts**, and **Researchers** who need to quickly understand, validate, and visualize their data with maximum efficiency.

---

## Table of Contents

- [Installation](#installation)
- [The Core Concept](#the-core-concept-the-rostaing_report-object)
- [Part 1: The Automated EDA Report](#part-1-the-automated-eda-report)
- [Part 2: Interactive Data Visualization](#part-2-interactive-data-visualization)
  - [Basic Charts](#-basic-charts)
  - [Distributions (1D)](#-1d-distributions)
  - [Distributions (2D)](#-2d-distributions)
  - [Part-of-Whole Charts](#-part-of-whole-charts)
  - [3-Dimensional Charts](#-3-dimensional-charts)
  - [Multidimensional Charts](#-multidimensional-charts)
  - [Map-Based Charts](#-map-based-charts)
  - [Polar and Ternary Charts](#-polar-and-ternary-charts)
- [Part 3: The Statistical Testing Suite](#part-3-the-statistical-testing-suite)
  - [Normality & Distribution Tests](#-normality--distribution-tests)
  - [Parametric Group Comparison](#-parametric-group-comparison-t-tests--anova)
  - [Non-Parametric Group Comparison](#-non-parametric-group-comparison)
  - [Correlation Analysis](#-correlation-analysis)
  - [Survival Analysis](#-survival-analysis)
  - [Other Statistical Utilities](#-other-statistical-utilities)
- [Why rostaing-report?](#why-rostaing-report)
- [Contributing](#contributing)
- [License](#license)
- [Useful Links](#useful-links)

---

## Installation

Install the package and all its dependencies from PyPI:

```bash
pip install rostaing-report
```

## The Core Concept: The `rostaing_report` Object

The entire toolkit is accessed through a single object. You initialize it once with your DataFrame, and then use that same object to run reports, create plots, and perform tests.

```python
import pandas as pd
from rostaing import rostaing_report
import plotly.express as px

# Load a versatile sample dataset
df = px.data.tips()

# Initialize the report object
# This one object is now your gateway to all functionalities
report = rostaing_report(df)
```

## Part 1: The Automated EDA Report

The fastest way to get insights. Simply displaying the object in a notebook (or printing it in a script) generates a full descriptive analysis.

```python
# In a Jupyter Notebook or similar environment:
report
```

This command produces a detailed, multi-section report:
- **Overview Statistics:** Key metrics about the entire dataset (rows, columns, memory, duplicates).
- **Variable Types:** A summary table of all data types and their counts.
- **Numerical Variables Analysis:** A deep dive into each number-based column, including mean, std, quantiles, skew, kurtosis, and outlier detection.
- **Categorical Variables Analysis:** A summary of all text-based or boolean columns.
- **Top Correlations:** A sorted list of the most correlated numerical variables, with plain-English interpretations.

## Part 2: Interactive Data Visualization

Create over 40 types of interactive plots with an API identical to Plotly Express. The `report` object automatically handles passing the DataFrame. All methods return a `plotly.graph_objects.Figure` that you can display with `.show()` or customize further.

<br>

<details>
<summary><strong>📊 Basic Charts</strong></summary>

-   **`.scatter()`**: For showing the relationship between two numeric variables.
    ```python
    fig = report.scatter(x="total_bill", y="tip", color="time", title="Tip vs. Total Bill by Time of Day")
    # fig.show()
    ```
-   **`.line()`**: For showing trends over a continuous variable, often time.
    ```python
    df_stocks = px.data.stocks()
    stock_report = rostaing_report(df_stocks)
    fig = stock_report.line(x="date", y=["GOOG", "AAPL"], title="Google vs. Apple Stock Price")
    # fig.show()
    ```
-   **`.bar()`**: For comparing categorical data.
    ```python
    fig = report.bar(x="day", y="total_bill", color="sex", barmode="group", title="Total Bill by Day, Grouped by Sex")
    # fig.show()
    ```
-   **`.area()`**: Like a line chart, but with the area below the line filled in.
    ```python
    fig = stock_report.area(x="date", y="GOOG", title="Google Stock Price (Area Chart)")
    # fig.show()
    ```
-   **`.funnel()`**: For visualizing stages in a process (e.g., a sales funnel).
    ```python
    df_funnel = px.data.funnel()
    funnel_report = rostaing_report(df_funnel)
    fig = funnel_report.funnel(x='number', y='stage', title="Sales Funnel")
    # fig.show()
    ```
-   **`.timeline()`**: A type of Gantt chart for visualizing events over time.
    ```python
    df_gantt = pd.DataFrame([
        dict(Task="Job A", Start='2023-01-01', Finish='2023-02-28', Resource="Alex"),
        dict(Task="Job B", Start='2023-03-05', Finish='2023-04-15', Resource="Max"),
        dict(Task="Job C", Start='2023-02-20', Finish='2023-05-30', Resource="Alex")
    ])
    gantt_report = rostaing_report(df_gantt)
    fig = gantt_report.timeline(x_start="Start", x_end="Finish", y="Task", color="Resource", title="Project Timeline")
    # fig.show()
    ```
</details>

<details>
<summary><strong>📈 1D Distributions</strong></summary>

-   **`.histogram()`**: To understand the distribution of a single numeric variable.
    ```python
    fig = report.histogram(x="total_bill", nbins=30, marginal="box", title="Distribution of Total Bill")
    # fig.show()
    ```
-   **`.box()`**: For visualizing the five-number summary (min, Q1, median, Q3, max) and identifying outliers.
    ```python
    fig = report.box(x="day", y="tip", color="smoker", title="Tip Distribution by Day and Smoker Status")
    # fig.show()
    ```
-   **`.violin()`**: A combination of a box plot and a kernel density estimate.
    ```python
    fig = report.violin(x="day", y="tip", color="sex", box=True, points="all", title="Tip Distribution (Violin Plot)")
    # fig.show()
    ```
-   **`.strip()`**: A scatter plot for a single variable, useful for seeing all data points.
    ```python
    fig = report.strip(x="day", y="tip", color="time", title="Individual Tips by Day and Time")
    # fig.show()
    ```
-   **`.ecdf()`**: To visualize the Empirical Cumulative Distribution Function.
    ```python
    fig = report.ecdf(x="tip", color="sex", title="ECDF of Tips by Sex")
    # fig.show()
    ```
</details>

<details>
<summary><strong>🌡️ 2D Distributions</strong></summary>

-   **`.density_heatmap()`**: Shows the density of points in a 2D space as a heatmap.
    ```python
    fig = report.density_heatmap(x="total_bill", y="tip", marginal_x="histogram", marginal_y="histogram", title="Density Heatmap of Tips vs. Total Bill")
    # fig.show()
    ```
-   **`.density_contour()`**: Shows the density of points as contour lines.
    ```python
    fig = report.density_contour(x="total_bill", y="tip", color="sex", title="Density Contour of Tips vs. Total Bill by Sex")
    # fig.show()
    ```
-   **`.imshow()`**: For displaying images or matrices.
    ```python
    # Note: data is passed directly for imshow
    from skimage import data
    img = data.chelsea()
    fig = report.imshow(img=img, title="imshow Example with scikit-image")
    # fig.show()
    ```
</details>

<details>
<summary><strong>🥧 Part-of-Whole Charts</strong></summary>

-   **`.pie()`**: For showing proportions of a whole.
    ```python
    fig = report.pie(names="day", values="tip", title="Total Tips by Day")
    # fig.show()
    ```
-   **`.sunburst()`**: A hierarchical pie chart.
    ```python
    fig = report.sunburst(path=['sex', 'day', 'time'], values='total_bill', title="Hierarchical Breakdown of Total Bill")
    # fig.show()
    ```
-   **`.treemap()`**: An alternative hierarchical view using nested rectangles.
    ```python
    fig = report.treemap(path=['smoker', 'day'], values='total_bill', title="Treemap of Total Bill by Smoker and Day")
    # fig.show()
    ```
-   **`.icicle()`**: Another hierarchical view, laid out as a "flame" chart.
    ```python
    fig = report.icicle(path=['time', 'day'], values='tip', title="Icicle Chart of Tips")
    # fig.show()
    ```
-   **`.funnel_area()`**: For analyzing sequences, showing the flow between states.
    ```python
    df_flow = px.data.wind()
    flow_report = rostaing_report(df_flow)
    fig = flow_report.funnel_area(names='direction', values='frequency', title='Wind Direction Funnel')
    # fig.show()
    ```
</details>

<details>
<summary><strong>🧊 3-Dimensional Charts</strong></summary>

-   **`.scatter_3d()`**: A scatter plot in 3D space.
    ```python
    df_iris = px.data.iris()
    iris_report = rostaing_report(df_iris)
    fig = iris_report.scatter_3d(x='sepal_length', y='sepal_width', z='petal_width', color='species', title="3D Iris Dataset")
    # fig.show()
    ```
-   **`.line_3d()`**: A line plot in 3D space.
    ```python
    df_election = px.data.election()
    election_report = rostaing_report(df_election)
    fig = election_report.line_3d(x="Joly", y="Coderre", z="Bergeron", color="winner", title="3D Election Results")
    # fig.show()
    ```
</details>

<details>
<summary><strong>🌐 Multidimensional Charts</strong></summary>

-   **`.scatter_matrix()`**: A grid of scatter plots for comparing multiple variables at once (also known as a pairs plot).
    ```python
    df_iris = px.data.iris()
    iris_report = rostaing_report(df_iris)
    fig = iris_report.scatter_matrix(dimensions=["sepal_width", "sepal_length", "petal_width", "petal_length"], color="species", title="Iris Dataset Scatter Matrix")
    # fig.show()
    ```
-   **`.parallel_coordinates()`**: For plotting multivariate numerical data.
    ```python
    fig = iris_report.parallel_coordinates(color="species_id", labels={"species_id": "Species", "sepal_width": "Sepal Width", "sepal_length": "Sepal Length"}, title="Parallel Coordinates for Iris Dataset")
    # fig.show()
    ```
-   **`.parallel_categories()`**: For plotting multivariate categorical data.
    ```python
    fig = report.parallel_categories(dimensions=['sex', 'smoker', 'day', 'time'], color="size", color_continuous_scale=px.colors.sequential.Inferno, title="Parallel Categories for Tips Dataset")
    # fig.show()
    ```
</details>

<details>
<summary><strong>🗺️ Map-Based Charts</strong></summary>

-   **`.scatter_geo()`** and **`.line_geo()`**: For plotting points or lines on an outline map.
    ```python
    df_gap = px.data.gapminder().query("year==2007")
    gap_report = rostaing_report(df_gap)
    fig = gap_report.scatter_geo(locations="iso_alpha", color="continent", hover_name="country", size="pop", projection="natural earth", title="Global Population in 2007")
    # fig.show()
    ```
-   **`.choropleth()`**: For creating filled geographical maps where color represents a value.
    ```python
    fig = gap_report.choropleth(locations="iso_alpha", color="lifeExp", hover_name="country", color_continuous_scale=px.colors.sequential.Plasma, title="Global Life Expectancy in 2007")
    # fig.show()
    ```
-   **Mapbox Charts** (e.g., **`.scatter_mapbox()`**, **`.choropleth_mapbox()`**): Similar to geo charts but use tile-based maps. *Requires a Mapbox access token*.
    ```python
    # Set your token first: px.set_mapbox_access_token("YOUR_TOKEN")
    df_car = px.data.carshare()
    car_report = rostaing_report(df_car)
    # fig = car_report.scatter_mapbox(lat="centroid_lat", lon="centroid_lon", color="peak_hour", size="car_hours", title="Car Share Activity in Montreal")
    # fig.show()
    ```
</details>

<details>
<summary><strong>🧭 Polar and Ternary Charts</strong></summary>

-   **`.scatter_polar()`** and **`.bar_polar()`**: For plotting data in a polar coordinate system.
    ```python
    df_wind = px.data.wind()
    wind_report = rostaing_report(df_wind)
    fig = wind_report.bar_polar(r="frequency", theta="direction", color="strength", title="Wind Frequency by Direction and Strength")
    # fig.show()
    ```
-   **`.scatter_ternary()`**: For plotting data with three components that sum to a constant (e.g., compositions).
    ```python
    df_election = px.data.election()
    election_report = rostaing_report(df_election)
    fig = election_report.scatter_ternary(a="Joly", b="Coderre", c="Bergeron", color="winner", size="total", hover_name="district", title="Ternary Plot of Election Results")
    # fig.show()
    ```
</details>

<br>

## Part 3: The Statistical Testing Suite

Perform a wide range of statistical tests directly from the `report` object. Each test returns a well-formatted dictionary or DataFrame.

<br>

<details>
<summary><strong>🔬 Normality & Distribution Tests</strong></summary>

- **`.normality_test()`**: Checks if data is likely drawn from a normal distribution.
    - **Purpose**: Key for deciding between parametric and non-parametric tests.
    - **H0**: The data follows a normal distribution.
    - **Example**:
        ```python
        # Using 'shapiro', 'jarque_bera', or 'normaltest'
        norm_results = report.normality_test('total_bill', test='shapiro')
        print(pd.Series(norm_results))
        ```
    - **Output**:
        ```
        test                                                 Shapiro-Wilk
        column                                               total_bill
        statistic                                              0.913576
        p_value                                          8.20467e-11
        conclusion (alpha=0.05)    The null hypothesis (normality) is rejected (p < 0.05).
        dtype: object
        ```
- **`.ks_test()`**: Kolmogorov-Smirnov test for goodness of fit against a theoretical distribution.
    - **Purpose**: To check if your data's distribution significantly differs from a known one (e.g., 'norm', 'expon').
    - **H0**: The data follows the specified distribution.
    - **Example**:
        ```python
        ks_results = report.ks_test('tip', dist='norm')
        print(pd.Series(ks_results))
        ```
    - **Output**:
        ```
        test                       Kolmogorov-Smirnov
        column                                    tip
        distribution_tested                      norm
        statistic                            0.480036
        p_value                          1.19662e-54
        conclusion (alpha=0.05)    The data does not follow a 'norm' distribution (p < 0.05).
        dtype: object
        ```
</details>

<details>
<summary><strong>⚖️ Parametric Group Comparison (T-Tests & ANOVA)</strong></summary>

- **`.ttest_1samp()`**: Compares the mean of a single sample to a known or hypothesized population mean.
    - **H0**: The sample mean is equal to the specified population mean.
    - **Example**:
        ```python
        # Is the average tip significantly different from $2.50?
        ttest1_results = report.ttest_1samp(col='tip', popmean=2.5)
        print(pd.Series(ttest1_results))
        ```
    - **Output**:
        ```
        test                       T-test (one-sample)
        column                                     tip
        population_mean                            2.5
        t_statistic                           5.62125
        p_value                           4.3417e-08
        conclusion (alpha=0.05)    Statistically significant difference from the population mean (p < 0.05).
        dtype: object
        ```
- **`.ttest_ind()`**: Compares the means of two *independent* columns.
    - **H0**: The means of the two columns are equal.
    - **Example**:
        ```python
        # Note: A more common use is comparing one variable across two groups (see non-parametric section).
        # This example tests if the mean of 'total_bill' is equal to the mean of 'tip'.
        ttest_ind_results = report.ttest_ind('total_bill', 'tip')
        print(pd.Series(ttest_ind_results))
        ```
- **`.ttest_paired()`**: Compares the means of two *dependent* (paired) samples (e.g., "before" and "after").
    - **H0**: The mean of the differences between paired observations is zero.
    - **Example**:
        ```python
        # Create synthetic paired data
        paired_df = pd.DataFrame({'before': [20, 21, 25, 18], 'after': [22, 24, 26, 21]})
        paired_report = rostaing_report(paired_df)
        paired_results = paired_report.ttest_paired('before', 'after')
        print(pd.Series(paired_results))
        ```
- **`.anova_oneway()`**: Compares the means of three or more independent groups.
    - **H0**: The means of all groups are equal.
    - **Example**:
        ```python
        # Is there a difference in tip amount across different days of the week?
        anova1_results = report.anova_oneway(dv='tip', between='day')
        print(pd.Series(anova1_results))
        ```
- **`.anova_twoway()`**: Examines the influence of two different categorical independent variables on one continuous dependent variable.
    - **H0**: There are no main effects or interaction effects.
    - **Example**:
        ```python
        # How do 'sex' and 'day' (and their interaction) affect the tip amount?
        anova2_results = report.anova_twoway(dv='tip', between=['sex', 'day'])
        print(anova2_results)
        ```
- **`.anova_rm()`**: Repeated Measures ANOVA for comparing means across three or more dependent conditions.
    - **H0**: The means of all related conditions are equal.
    - **Example**:
        ```python
        # Requires data in long format with a subject identifier.
        rm_df = pg.read_dataset('rm_anova')
        rm_report = rostaing_report(rm_df)
        rm_results = rm_report.anova_rm(dv='Desire', within='Time', subject='Subject')
        print(rm_results)
        ```
</details>

<details>
<summary><strong>📊 Non-Parametric Group Comparison</strong></summary>

- **`.chi2_test()`**: Tests for independence between two categorical variables.
    - **H0**: The two categorical variables are independent.
    - **Example**:
        ```python
        chi2_results = report.chi2_test('sex', 'smoker')
        print(pd.Series(chi2_results).drop('contingency_table'))
        ```
- **`.mann_whitney_u_test()`**: Non-parametric alternative to the independent T-test. Compares distributions of two independent groups.
    - **H0**: The distributions of the two groups are equal.
    - **Example**:
        ```python
        # Is the tip distribution the same for smokers and non-smokers?
        mwu_results = report.mann_whitney_u_test(dv='tip', group_col='smoker')
        print(pd.Series(mwu_results))
        ```
- **`.wilcoxon_test()`**: Non-parametric alternative to the paired T-test.
    - **H0**: The distribution of the differences between pairs is symmetric about zero.
    - **Example** (using the same paired data as `ttest_paired`):
        ```python
        # paired_report = ... (see ttest_paired example)
        # wilcoxon_results = paired_report.wilcoxon_test('before', 'after')
        # print(pd.Series(wilcoxon_results))
        ```
- **`.kruskal_wallis_test()`**: Non-parametric alternative to the one-way ANOVA. Compares distributions of three or more groups.
    - **H0**: The medians of all groups are equal.
    - **Example**:
        ```python
        # Is the tip distribution the same across different days?
        kruskal_results = report.kruskal_wallis_test(dv='tip', group_col='day')
        print(pd.Series(kruskal_results))
        ```
- **`.friedman_test()`**: Non-parametric alternative to the repeated measures ANOVA.
    - **H0**: The distributions for each condition are the same.
    - **Example**:
        ```python
        # Create synthetic repeated data
        friedman_df = pd.DataFrame({'cond1': [1,2,3,4], 'cond2': [2,3,4,5], 'cond3': [3,4,5,6]})
        friedman_report = rostaing_report(friedman_df)
        friedman_results = friedman_report.friedman_test('cond1', 'cond2', 'cond3')
        print(pd.Series(friedman_results))
        ```
</details>

<details>
<summary><strong>🔗 Correlation Analysis</strong></summary>

- **`.correlation_matrix()`**: Calculates the full correlation matrix.
    - **Purpose**: Get a complete overview of linear relationships between all numeric variables.
    - **Example**:
        ```python
        # Methods can be 'pearson', 'spearman', or 'kendall'
        corr_matrix = report.correlation_matrix(method='spearman')
        # print(corr_matrix)
        ```
- **`.point_biserial_corr()`**: Measures the correlation between a binary and a continuous variable.
    - **H0**: There is no correlation.
    - **Example**:
        ```python
        # Create a binary column for the example
        df_copy = df.copy()
        df_copy['is_male'] = (df_copy['sex'] == 'Male').astype(int)
        binary_report = rostaing_report(df_copy)
        pbc_results = binary_report.point_biserial_corr(binary_col='is_male', continuous_col='tip')
        print(pd.Series(pbc_results))
        ```
- **`.partial_corr()`**: Measures the correlation between two variables while controlling for the effect of one or more other variables.
    - **Purpose**: To find the "true" relationship between two variables by removing the influence of a confounder.
    - **Example**:
        ```python
        # What is the correlation between 'tip' and 'total_bill' after controlling for party 'size'?
        partial_corr_results = report.partial_corr(x='tip', y='total_bill', covar=['size'])
        print(partial_corr_results)
        ```
</details>

<details>
<summary><strong>🕰️ Survival Analysis</strong></summary>

- **Setup**: Survival analysis requires a dataset with duration and event status.
    ```python
    from lifelines.datasets import load_waltons
    surv_df = load_waltons()
    surv_report = rostaing_report(surv_df)
    ```
- **`.kaplan_meier_curve()`**: Plots the survival probability over time.
    - **Purpose**: To visualize how a group's survival chances change over a period.
    - **Example**:
        ```python
        # Plot survival curves for different treatment groups
        ax = surv_report.kaplan_meier_curve(duration_col='T', event_col='E', group_col='group')
        # ax.figure.show()
        ```
- **`.logrank_test()`**: Compares the survival curves of two or more groups.
    - **H0**: There is no difference between the survival curves of the groups.
    - **Example**:
        ```python
        logrank_results = surv_report.logrank_test(duration_col='T', event_col='E', group_col='group')
        print(logrank_results)
        ```
- **`.cox_ph_regression()`**: Models the relationship between covariates and the risk of an event (Cox Proportional Hazards).
    - **Purpose**: To identify which factors significantly predict survival time.
    - **Example**:
        ```python
        cox_summary = surv_report.cox_ph_regression(duration_col='T', event_col='E', 'group')
        print(cox_summary)
        ```
</details>

<details>
<summary><strong>🛠️ Other Statistical Utilities</strong></summary>

- **`.test_levene()`**: Tests the assumption of equal variances across groups.
    - **H0**: The variances of all groups are equal (homoscedasticity).
    - **Purpose**: A prerequisite check for ANOVA and T-tests.
    - **Example**:
        ```python
        levene_results = report.test_levene(dv='tip', group_col='day')
        print(pd.Series(levene_results))
        ```
- **`.effect_size_cohen_d()`**: Calculates Cohen's d, a measure of effect size for the difference between two means.
    - **Purpose**: To quantify the *magnitude* of a difference, not just its statistical significance.
    - **Example**:
        ```python
        cohen_d_results = report.effect_size_cohen_d(dv='tip', group_col='sex')
        print(pd.Series(cohen_d_results))
        ```
- **`.cronbachs_alpha()`**: Measures the internal consistency or reliability of a set of scale items (e.g., a questionnaire).
    - **Purpose**: To check if multiple items reliably measure the same underlying concept.
    - **Example**:
        ```python
        # Create synthetic questionnaire data
        q_data = pg.read_dataset('cronbach_alpha')
        q_report = rostaing_report(q_data)
        item_cols = q_data.columns.tolist()
        alpha_results = q_report.cronbachs_alpha(*item_cols)
        print(pd.Series(alpha_results))
        ```
- **`.cohens_kappa()`** and **`.fleiss_kappa()`**: Measures inter-rater agreement for categorical items.
    - **Purpose**: To see how consistently two or more raters classify items.
    - **Example (Cohen's Kappa for 2 raters)**:
        ```python
        # Create synthetic rater data
        rater_df = pd.DataFrame({'rater1': ['A','B','C','A','B'], 'rater2': ['A','B','C','B','B']})
        rater_report = rostaing_report(rater_df)
        kappa_results = rater_report.cohens_kappa('rater1', 'rater2')
        print(pd.Series(kappa_results))
        ```
</details>

<br>

## Why rostaing-report?

-   **Speed and Efficiency:** Go from a raw DataFrame to a full, insightful analysis in seconds. Drastically reduce boilerplate code for both statistics and plotting.
-   **Unified Interface:** No need to import and manage dozens of functions from different libraries. One object provides a cohesive and consistent API for your entire analysis workflow.
-   **Clarity and Communication:** The structured output, plain-English interpretations, and publication-quality plots help you understand and communicate findings faster.
-   **Informed Decision-Making:** By quickly identifying outliers, distributions, correlations, and statistical significance, you can make smarter, evidence-backed decisions on how to proceed with your data.

## Contributing

Contributions are welcome! If you have ideas for new features, find a bug, or want to improve the documentation, please feel free to open an issue or submit a pull request on the project's repository.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

## Useful Links
- [GitHub Repository](https://github.com/Rostaing/rostaing-report)
- [PyPI Project Page](https://pypi.org/project/rostaing-report/)
- [Author's LinkedIn](https://www.linkedin.com/in/davila-rostaing/)
- [Author's YouTube Channel](https://youtube.com/@rostaingai?si=8wo5H5Xk4i0grNyH)