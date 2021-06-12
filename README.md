# Project Instructions

Your project is to create a module named `moviedb` for managing a movie collection. This is to be done by SLT. Your code should be committed on a private repo in GitHub with repo name `moviedb` then submit this notebook by 16 June at 8pm. Only one member of the SLT should submit--there will be a penalty for submissions from multiple members of an SLT. The module should be at the top-level directory of this repo. Grant read access to this repo to Christian Alis (GitHub account `ianalis`). **Do not write your code on this notebook nor submit it along with this notebook**. Just specify your repo url in the cell below.

Only the following packages may be used for the implementation:

* Python standard libraries
* Numpy (but not scipy)
* Pandas

## The `MovieDB` class

The module should contain one class named `MovieDB`. It should have the following specifications:

### Initialization

The class initializer should accept a string `data_dir` which is the directory path to where the data files are located. This path should be stored in the `data_dir` attribute of the object.

### Data persistence

Data are stored in the CSV files `movies.csv` and `directors.csv` in `data_dir`. The CSV files begin with a column header and the columns of each CSV file are:

`movies.csv`:
  - `movie_id`: an integer assigned to the movie
  - `title`: movie title. It is enclosed in double quotes (") when stored in the CSV file if there is a comma (,) in the title
  - `year`: release year
  - `genre`: movie genre
  - `director_id`: id of the movie's director
  
`directors.csv`:
 - `director_id`: an integer assigned to the director
 - `given_name`: given name of the director
 - `last_name`: last name of the director

### Exception

The class has the associated exception `MovieDBError` which is a `ValueError`.

### Features

#### Adding a movie to the database

Create a method `add_movie` that accepts the following parameters:
  - `title`: title of the movie
  - `year`: year movie was released as integer
  - `genre`: genre of the movie
  - `director`: director of the movie as a string in `Last name, Given name` format
  
  The method should append the movie to the end of `movies.csv`, if it exists, or creates it, otherwise. The `movie_id` is _last `movie_id` in the file_ + 1, or `1` if there's no movie in the file yet. The `director_id` is the corresponding `director_id` in `directors.csv` based on the case-insensitive matches of `given_name` and `last_name`. It should append the director in `directors.csv` if the director is not yet there. The `director_id` of a new director is _last `director_id` in the file_ + 1, or `1` if there's no director in the file yet. The method should return the `movie_id` or raise a `MovieDBError` exception if the movie is already in `movies.csv`. A movie is said to be in `movies.csv` if there is a movie that matches the `title` (case-insensitive), `year`, `genre` (case-insensitive) and `director` (case-insensitive).

#### Adding movies in the database

Create a method `add_movies` that accepts a list of movies in the form of dictionaries with the following keys:
  - `title`: title of the movie
  - `year`: year movie was released as integer
  - `genre`: genre of the movie
  - `director`: director of the movie as a string in `Last name, Given name` format
  
  The method should add each movie to the database. It returns a list of the `movie_id`s of successfully added movies. If a movie is already in the database, skip it and print `Warning: movie {title} is already in the database. Skipping...` instead. If a movie has invalid or incomplete information, skip it and print `Warning: movie index {i} has invalid or incomplete information. Skipping...` instead. The movie index is the zero-based index of the movie in the passed list of movies.
  
#### Deleting a movie in the database

Create a method `delete_movie` that accepts the `movie_id` to delete then removes it from `movies.csv`. It will raise a `MovieDBError` if the `movie_id` is not found.
  
#### Searching for movies in the database

Create a method `search_movies` that accepts the following keyword arguments:
  - `title`: case-insensitive title of the movie
  - `year`: year movie was released as integer
  - `genre`: case-insensitive genre of the movie
  - `director_id`: id of the director of the movie

All of the arguments are optional but there should be at least one nontrivial argument passed to the method. It should raise a `MovieDBError` if there is no nontrivial argument that was passed. It should return the list of matching `movie_id`s.

#### Exporting data

Create a method `export_data` that returns all of the movies in the database as a pandas data frame with the following columns:
  - `title`: title of the movie
  - `year`: year movie was released as integer
  - `genre`: genre of the movie
  - `director_last_name`: last name of the movie director
  - `director_given_name`: given name of the movie director
  
Sort the rows by the corresponding `movie_id` of each movie.
  
#### Generating statistics

Create a method `generate_statistics` that returns a dictionary depending on the `stat` parameter passed to it:
  - `movie`: key is year, value is the number of movies for that year
  - `genre`: key is each unique genre, value is another dictionary with year as key and number of movies of that genre for that year as value
  - `director`: key is director name following the format `Last name, Given name`, value is another dictionary with year as key and number of movies of that director for that year as value
  - `all`: keys are `movie`, `genre` and `director`, values are the corresponding dictionary returned by those keywords

The `stat` values are case-sensitive and the method should raise `MovieDBError` if the passed `stat` is unknown.

#### Plotting statistics

Create a method `plot_statistics` that returns a matplotlib `Axes` depending on the `stat` parameter passed to it:
  - `movie`: bar plot of number of movies per year
  - `genre`: superimposed circle and line plots of the number of movies for each genre per year, one line per genre. Sort alphabetically by genre then show the legend.
  - `director`: superimposed circle and line plots of the number of movies for each director per year, one director per line. Sort the directors by decreasing number of movies then by increasing last name. Plot only the 5 directors with the most movies.
  
The `stat` values are case-sensitive and the method should raise `MovieDBError` if the passed `stat` is unknown.
  
#### Token frequency

Create a method `token_freq` that returns a dictionary with the token as key and the number of times that word appeared in all of the titles as value. A token is defined as a case-folded sequence of non-whitespace characters. 


## Grading guide

* The project has a highest possible score of 150 points.

* Each cell with an assert statement is worth 10 pts. Successfully passing all of the tests in a cell will earn you the entire 10 pts. Failure to pass any of the test in the cell, including hidden tests, will earn no point. No partial points will be given thus make sure that you run and pass all the visible tests in the test suite before submitting.

* Successful git cloning is worth 15 pts. Successful importing of the module is worth 5 pts. 

* If the module fails to clone or import, the professor will attempt to make it work but will merit additional deductions up to 10% of highest possible score.

* Methods should have a sensible docstring. The professor will deduct up to a total of 15 pts for missing, misleading or nonsensible docstrings. If you reasonably follow the numpy docstring format then you will likely not receive any deductions.

* The code should follow PEP8. The professor will run your python codes through [pycodestyle](https://pypi.org/project/pycodestyle/) and will deduct a point up to a total of 15 points for every instance of PEP8 violation (including warning).

**Hints**: 
* Instead of figuring out how to modify a specific line in the CSV files, recreate the entire CSV file then overwrite the original file.
* Use `os.path` for path operations.