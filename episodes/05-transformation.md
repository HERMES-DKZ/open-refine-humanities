---
title: "Transforming Data"
teaching: 20
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can GREL be used to extract information?
- What is the difference between modifying existing values and creating new columns?
- How can clustering help identify inconsistent values in a dataset?



::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Apply GREL transformations to extract information.
- Create new columns based on existing data.
- Use arrays and indexing to select specific parts of a value.
- Use clustering to identify and review inconsistent values.


::::::::::::::::::::::::::::::::::::::::::::::::

## From Exploring Data to Cleaning Data

In the previous episodes, we used built-in and custom facets to explore our dataset and identify potential issues. Facets help us find patterns and problems, but they do not change the data itself. For further analysis, it is often useful to reorganize and standardize the data.

In this episode, we move from exploration to cleaning. We will use transformations, column operations, and clustering to modify and standardize our data.

::::::::::::: discussion

### Discussion: What information is stored in `Artist Display Bio`?

Look at several values in the column.

- What different kinds of information are stored together?
- What problems could this cause for later analysis?
- How could we separate these different pieces of information?

:::::::: instructor

Guide learners toward recognizing:

- nationality
- place names
- dates
- information on multiple artists

Emphasize that the column currently contains several different types of information. In the documentation of the dataset it states that `ArtistDisplayBio` contains information about "Nationality and life dates of an artist, also includes birth and death city when known".

::::::::::::

:::::::::::::::::::

### Splitting Multi-Valued Cells

The first step is to separate information about individual artists. As you learned in previous episodes, the pipe character (`|`) is used to separate information about multiple artists within a single cell.

To separate these artists into individual rows:

1. Open the column menu for `ArtistDisplayBio`.

2. Choose `Edit cells → Split multi-valued cells...`

3. Enter: `|`.

4. Confirm with `OK`.

You will notice that some artists have only partial information, while others have much more detailed entries. Most complete entries follow a pattern similar to: 
`Nationality, Place Year–Year Place`.



### Creating a New Column

Now we can begin extracting specific pieces of information. To preserve the original data, we will create a new column rather than modifying the existing one. This is an important distinction: unlike the previous operation, which changed the structure of the dataset by creating new rows, we are now creating an additional column while keeping the original values unchanged.

:::::: challenge
### How to extract the Nationality?

Look at the column `ArtistDisplayBio`. How could you separate the nationality from the remaining information?

::::: solution

Most values contain a comma immediately after the nationality. We can therefore split the text at the comma and keep only the first element.

:::::::::
:::::::::

1. Open the column menu for `ArtistDisplayBio` .

2. Choose `Edit column  → Add column based on this column...`

3. A new window appears.

:::instructor
![Screenshot of the Add column based on column ...](fig/04_openrefine_grelfunction.png)

Describe the new window to the learners and remind them of the similarities from window in the prevoius episode.
On top you enter the name of the new column. At the bottom, there is a `Preview` section where you can see the value (i.e. the value in the table) and, to the right of that, the new value produced by the function. Under the `History` tab, you can view the commands that have been used, and under `Help` you will find a detailed explanation.

::::

4. Name the new column `Nationality`.
5. Enter the GREL expression and check what happens in the `Preview` tab below
```grel
value.split(",")
```

The cell content is transformed into an array with three elements in it:
`American (born Germany), Frankfurt-am-Main 1863–1962 Staten Island, New York`
→  
 `[ "American (born Germany)", " Frankfurt-am-Main 1863–1962 Staten Island", " New York" ]` 
 An array is simply a list of values stored in a specific order. In OpenRefine arrays are displayed unsing square brackets with elements separated by commas. Since we are only interested in the nationality, we want the first element of the array. Array positions begin at 0, so the first element is accessed using:

 6. Add `[0]` to the GREL funtion.

 7. Click `OK` to create the new column.
 
What happens if you replace [0] with [1]?

:::::: discussion
### Additional cleaning

Look at the new `Nationality` column. Do all values contain only nationalities?
Discuss the following questions:

- Which additional information is still present?
- Can you identify recurring patterns?
- How might these patterns be removed?

You do not need to propose a GREL expression. A description in natural language is sufficient.

::::: solution

Common issues include:

- birth information in parentheses, such as (born Germany)
- dates that were not completely separated
- inconsistent formatting

Data cleaning is rarely finished after a single transformation. Manual review is still necessary, but transformations can greatly reduce the amount of work required.

if(value.contains(/[0-9]/), "", value)
value.replace("(?)", "")

:::::::::
:::::::::

:::::: challenge
### Extracting the Death City of an Artist


::::: solution

:::::::::
:::::::::



## Clustering

Clustering identifies values that may represent the same concept, even when they are written differently. It identifies and normalizes variations — especially when different inconsistencies appear similar but not identical. We will demonstrate clustering on the column `Object Name`.

1. Open the column menu for `Object Name`.

2. Choose `Edit cells  → Cluster and edit...`.

3. A new window appears. Click `Cluster`.

![Screenshot of the Cluster and edit column ... ](fig/05_openrefine_clustering.png)

The clustering window displays one suggested cluster per row. For each cluster the variations of a similar value with the count of rows they appear are shown on the left; a field on the right allows you to select or edit a preferred value.

For every suggested cluster you can decide to merge the values into a standardized form, to ignore the cluster if the variations are meaningful or to edit only some entries of the cluster. Clustering never changes anything automatically. OpenRefine simply helps you notice patterns you would otherwise miss.

Different clustering methods and keying functions identify different kinds of similarities. You do not need to understand the underlying algorithms. In practice, it is often sufficient to experiment with several methods and compare the results.



:::::::::::::::::::::::::::::::::::::::::::: challenge

## Cluster the Classification
 Look at the column `Classification` and cluster the values.  Try out different methods and keying functions. Which one works? And what could make clustering easier?

::::::::::::::::::: solution

### Solution

Clustering method and keying function that works best:
Splitting the values with the pipe helps.

:::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Transformations modify the *content* of cells, while column operations reshape the *structure* of the dataset.
- The `split()` function creates arrays that can be accessed using positions such as `[0]` and `[1]`.
- Structured information can be extracted from text using GREL expressions and pattern matching.
- Data cleaning often requires multiple transformation steps and manual review.
- Clustering helps identify potentially equivalent values and supports manual standardization.

::::::::::::::::::::::::::::::::::::::::::::::::
