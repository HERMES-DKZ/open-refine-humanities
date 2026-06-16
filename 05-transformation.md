---
title: "Transforming Data"
teaching: 20
exercises: 20
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can we clean and standardize the `Artist Display Bio` values in OpenRefine?
- What is the difference between *finding* issues (facets) and *fixing* them (transformations & clustering)?
- What is clustering and how can it help identify inconsistent values?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Remove literal characters with GREL replacements.
- Split `Artist Display Bio` into nationality and life-dates.
- Inspect and normalize a column.


::::::::::::::::::::::::::::::::::::::::::::::::

## From Exploring Data to Cleaning Data
In the previous episodes, we used built-in and custom facets to explore our dataset and identify potential issues. Facets help us find patterns and problems, but they do not change the data itself. For further analysis of the dataset it helps to alter and reorganize the data.

In this episode, we move from exploration to cleaning, which is helpful for a later analysis. We will use transformations, column operations, and clustering to modify and standardize our data.

::::::::::::: discussion

### Discussion: What information is stored in `Artist Display Bio`?

Look at several values in the column.

- What different kinds of information are stored together?
- What problems could this cause for later analysis?
- How could we separate these different pieces of information?

::::::::instructor

Guide learners toward recognizing:

- nationality
- place names
- dates
- informations on multiple artists

Emphasize that the column currently contains several different types of information. In the documentation of the dataset it states that `artistDisplayBio` contains information about "Nationality and life dates of an artist, also includes birth and death city when known".

:::

:::::::::::::::::::
## Splitting Multi-Valued Cells

The pipe character separates information about multiple artists.

To separate these artists into individual rows:

1. Open the column menu for `artistDisplayBio`. 
2. Choose `Edit cells → Split multi-valued cells...`
3. Enter: `|`
4. Confirm with `OK`.



## Creating a New Column
Now that each row contains information about a single artist, we can begin extracting specific pieces of information. We would like to create a column that contains only the nationality. Instead of modifying the existing column, **we will create a new one**.

1. Open the column menu for `artistDisplayBio` 

2. Choose `Edit column->Add column based on this column...`

3. A new window appears.

:::instructor
![Screenshot of the Add column based on column ... ... ](fig/04_openrefine_grelfunction.png)

::::

4. Name the new column `Nationality`.
5. Enter the GREL expression: 
```grel
value.split(",")
```

and check what happens in the `Preview` tab below. The cell content is transformed from `American (born Germany), Frankfurt-am-Main 1863–1962 Staten Island, New York`
 to `[ "American (born Germany)", " Frankfurt-am-Main 1863–1962 Staten Island", " New York" ]`, a array with three elements in it. An array is simply a list of values stored in a specific order. We want the nationality of the artist so only the first part is intresting for us. 
 6. Add `[0]` to the GREL funtion
 7. Click `OK`.
 
 


## Clustering

  

These inconsistencies make it hard to analyze the data reliably.

Clustering is one of OpenRefine’s most powerful tools for identifying and normalizing such variations — especially when the inconsistencies appear similar but not identical.

### What is clustering?

Clustering is OpenRefine’s way of grouping together text values that *look* or *sound* similar.  
It does this by reducing each value to a **“key”** based on a transformation.

For example:

- You might remove vowels and make everything uppercase:  
  “Color” → “CLR”, “Colour” → “CLR” → **match**

- Or you might use phonetic rules:  
  “Smith” → “SM0”, “Smyth” → “SM0” → **match**

A “keying function” transforms two strings that *should* be the same into the **same key**, even if their spellings differ slightly. There are many more clustering methods, all of which can recognise different patterns. It helps to understand these in order to find the right method, but often it is enough to try them out and proceed step by step.

OpenRefine uses this idea to suggest groups of values you may want to merge.


1. Open: ArtistBio 2 → `Edit cells → Cluster and edit…`
2. Method: `Key collision`
3. Keying function: `Metaphone 3`


### What you'll see

The clustering window shows one line per suggested cluster:

- On the left: variations of a similar value  
- On the right: a field where you choose the unified form

You can then decide:

- **Merge and reformat them** into a consistent style  
- **Ignore** clusters if the variations are meaningful  
- **Edit only some entries**  

Clustering never changes anything automatically. **You are in control**—OpenRefine simply helps you notice patterns you would otherwise miss.

This makes clustering extremely effective for cleaning humanities datasets, where controlled vocabulary is uncommon and metadata comes from diverse sources.

:::::::::::::::::::::::::::::::::::::::::::: discussion

## 


::::::::::::::::::: solution

### Solution

:::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: keypoints

- Transformations modify the *content* of cells, while column operations reshape the *structure* of the dataset.
- Literal GREL replacements help remove unwanted characters and prepare text for further processing.
- Splitting columns separates different types of information, making the data easier to analyze and clean.
- Clustering identifies similar but inconsistently written values and supports manual standardization.

::::::::::::::::::::::::::::::::::::::::::::::::
