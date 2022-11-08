---
title: Environment variables and keys
date: 2022-11-08
description: 'Programmers often write code that interacts directly with the cloud, requiring private keys. We need our code to be shareable and useable without sharing in the code. Environment variables are the keys to keys.'
---

## Environment Variables

Programmers often write code that interacts directly with the cloud, which requires the use of private keys. We need our code to be shareable and useable without sharing secret passwords, usernames, or keys in the code. Environment variables are the key to keys.

### What is an environment variable?

An environment variable is a variable that exists outside of your code but is created within your coding environment on startup. Thus each user of your code can use the same variable name but have different values for the variable. Environment files are within the family of _dot-files_ or hidden files on your computer that start with a `.`. This post focuses on how to access our environment files in R and Python.

### How do we store information in our environment file?

The environment file takes key-value pairs using an `=` to separate the key and value. You shouldn't have any spaces (` `) or quotes (`"`, `'`) in the file. Here is a simple example.

```
SAFEGRAPH_KEY=L8VjqR9mcuOfIUfgMCsR2NNmQ
GITHUB_PAT=ghp_dQY46I7T778990gggfgfgff0O1y
FAVQS_USER=mom@gmail.com
FAVQS_LOGIN=eatlunch856
```

If you use the file browser within VScode, you can see all of your _dot-files_ in any specific folder. You can also create your `.env` file using the new file option. Additionally, your OS allows you to show hidden files in folders.


## Python and the `.env` file

Python makes this process relatively easy using the [dotenv](https://github.com/theskumar/python-dotenv) and [os](https://docs.python.org/3/library/os.html#os.environ) packages.

### Creating the `.env` file

Using the `dotenv` package, you can store your `.env` file anywhere in the path from your working directory to the primary directory of your computer. If your environment variables are only needed on a specific project, you can store the `.env` file in your working directory and add the `.env` file to your `.gitignore` file. 

I store my `.env` file one step up from my working directory to access the environment variables in the file from multiple projects.

### Accessing the environment variables

You will need to import the `os` and `dotenv` packages within your Python code. The `os` package comes with Python. However, you will need to install the `dotenv` package using `pip install python-dotenv`. With your `.env` file in your working directory hierarchy, the following code will return the value associated with the key `SAFEGRAPH_KEY`. You can now use `wkey` in your code and expect all other users to use `wkey` but have different keys.

```python
import os
from dotenv import load_dotenv
load_dotenv()
wkey = os.environ.get("SAFEGRAPH_KEY")
```

## R and the `.Renviron` file

R uses the `.Renviron` and `.Rprofile` files to get user-specified environment variables into its environment. Using the [usethis](https://usethis.r-lib.org/) you can leverage `usethis::edit_r_environ()` to automagically open the file and make edits. I like to use [RStudio](https://posit.co/download/rstudio-desktop/) with this command as the file opens in the RStudio text editor instead of within the console.

If you haven't previously used the file, the `usethis::edit_r_environ()` will open a blank file. You can then create key-value pairs, as shown above.

With the `.Renviron` file created, you will have access to your environment variables when R starts. You can bring these variables into your environment using [`Sys.getenv()`](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Sys.getenv.html)

```R
wkey <- Sys.getenv("SAFEGRAPH_KEY")
```

## Within Databricks

Two options I have used for my Databricks notebooks are the `%run` and the environment configuration of the compute. When creating a cluster, you can navigate to the `Spark` sub-tab at the bottom and enter your `Environment variables`. These variables are available to every user that has access to the compute.

I like to use `%run ./keys` where _'keys'_ is the name of the notebook I have created that has my keys within it. Using [`%run`](https://docs.databricks.com/notebooks/notebooks-use.html#use-run-to-import-a-notebook) copies the code from the other notebook and executes it within this notebook. With a few notes in that chunk to tell the other users how to set up their keys, this option works quite well.

