
# 1. Data Import

look at variables and see what models you want to build:
input variables (e.g. cause) and output variable ()

# 2. Data exploration

convert characters to factors:
```
mutate_if(is.character, as.factor)
```

## 2.1 Check missing data in ALL variables

If it is missing for numeric variables:

- option 1. remove missing values with `filter()`
- option 2. replace with 0 IF IT MAKES SENSE
- option 3. use imputation: `step_impute_...()`

https://recipes.tidymodels.org/reference/step_impute_knn.html
https://recipes.tidymodels.org/reference/step_impute_mean.html
https://recipes.tidymodels.org/reference/step_impute_linear.html

If data is missing for factor variables (have you converted them to factors btw?)

- option 1. remove missing values with `filter()`
- option 2. use imputation: `step_impute_...()`

https://recipes.tidymodels.org/reference/step_impute_mode.html

## 2.2 Check distribution of data

For numeric variables `geom_histogram()` and `geom_density()` could be a big help.
Combine them to see:
```
ggplot(data, aes(x=your_variable)) +
geom_histogram(aes(y=..density..), # Histogram with density instead of count on y-axis
               binwidth=.5,
               colour="black", fill="white") +
geom_density(alpha=.2, fill="#FF6666")  # Overlay with transparent density plot
``` 

For factors
```
ggplot .... + 
 geom_boxplot() + 
 guides(fill=FALSE) + 
 coord_flip() #removes the legend and flips coordinates
```

# 3. Recipe

Split the data into training and testing.

Selector for recipe steps: https://www.tidymodels.org/find/recipes/

https://recipes.tidymodels.org/reference/index.html

Set up the recipe considering these steps:
 
- `step_naomit()` to remove missing values if not done before
- for skewed data - see histogram! - `step_log()`
- for numeric data:
   - `step_normalize()` to scale and center
   - `step_corr()` to check correlations
   - `step_zv()` to check that the values DO change within a variable
- for factor data
  - `step_dummy()`
  - for underrepresented factor data - `step_downsample()`

Remember you can refer to ALL particular variables using:

- `all_predictors()`
- `all_numeric_predictors()`
- `all_nominal_predictors()`
- `all_outcomes()`
- `all_numeric()`
- `all_nominal()`

To prep a recipe - APPLY steps but DO NOT change the data -> `prep()`

To apply a PREPPED recipe -> `bake()` OR in one go -> `bake(prep(RECIPE1))`

Can be used on BOTH, the test data and the training data, if a recipe was `prep()`'ed before (with ANY data):
```
bake(prep(recipe_test_data), new_data=test_data)
bake(prep(recipe_test_data), new_data=train_data)
```

To apply UN_PREPPED recipe with SOME data `recipe(data=data1,....)` to the SAME data `data1` -> `juice()`

# 4. Modeling

Selector for models/engines: https://www.tidymodels.org/find/parsnip/

# 5. Evaluation

