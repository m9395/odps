# Data merging

This topic describes the join and union operations that are supported by Alibaba Cloud MaxCompute SDK for Python \(PyODPS\) DataFrame to merge data in tables.

Import data from MaxCompute tables and create DataFrame objects. The following sample code provides an example to show how to create DataFrame objects:

```
>>> from odps.df import DataFrame
>>> movies = DataFrame(o.get_table('pyodps_ml_100k_movies'))
>>> ratings = DataFrame(o.get_table('pyodps_ml_100k_ratings'))
>>> movies.dtypes
odps.Schema {
  movie_id                            int64
  title                               string
  release_date                        string
  video_release_date                  string
  imdb_url                            string
}
>>> ratings.dtypes
odps.Schema {
  user_id                     int64
  movie_id                    int64
  rating                      int64
  unix_timestamp              int64
}
```

## Join operation

PyODPS DataFrame allows you to `join` two Collection objects.

-   If you do not specify `join` conditions, the DataFrame API uses the columns of the same name to `join` the Collection objects.

    ```
    >>> movies.join(ratings).head(3)
       movie_id              title  release_date  video_release_date                                           imdb_url  user_id  rating  unix_timestamp
    0         3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...       49       3       888068877
    1         3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...      621       5       881444887
    2         3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...      291       3       874833936
    ```

-   You can specify explicit `join` conditions. For a `join` operation, if the column names specified in the `on` condition for the two Collection objects are the same, the system uses one of the specified columns for the new table.

    ```
    >>> movies.join(ratings, on='movie_id').head(3)
       movie_id              title  release_date  video_release_date                                           imdb_url  user_id  rating  unix_timestamp
    0         3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...       49       3       888068877
    1         3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...      621       5       881444887
    2         3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...      291       3       874833936
    ```

-   For other types of `join` operations such as `left_join`, if the column names specified in the on condition for the two Collection objects are the same, the system renames the specified columns for the new table.

    ```
    >>> movies.left_join(ratings, on='movie_id').head(3)
       movie_id_x              title  release_date  video_release_date                                           imdb_url  user_id  movie_id_y  rating  unix_timestamp
    0           3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...       49           3       3       888068877
    1           3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...      621           3       5       881444887
    2           3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...      291           3       3       874833936
    ```

    In the preceding sample code, the two `movie_id` columns are renamed as `movie_id_x` and `movie_id_y`. The renaming rule depends on the `suffixes` parameter. By default, the value of the suffixes parameter is `('_x', '_y')`. When columns of the same name are found, the system renames the columns by using the specified suffixes.

    ```
    >>> ratings2 = ratings[ratings.exclude('movie_id'), ratings.movie_id.rename('movie_id2')]
    >>> ratings2.dtypes
    odps.Schema {
      user_id                     int64
      rating                      int64
      unix_timestamp              int64
      movie_id2                   int64
    }
    >>> movies.join(ratings2, on=[('movie_id', 'movie_id2')]).head(3)
       movie_id              title  release_date  video_release_date                                           imdb_url  user_id  rating  unix_timestamp  movie_id2
    0         3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...       49       3       888068877          3
    1         3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...      621       5       881444887          3
    2         3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...      291       3       874833936          3
    ```

-   You can specify an expression that uses the equality operator in the `on` condition to rename columns for the new table.

    ```
    >>> movies.join(ratings2, on=[movies.movie_id == ratings2.movie_id2]).head(3)
       movie_id              title  release_date  video_release_date                                           imdb_url  user_id  rating  unix_timestamp  movie_id2
    0         3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...       49       3       888068877          3
    1         3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...      621       5       881444887          3
    2         3  Four Rooms (1995)   01-Jan-1995                      http://us.imdb.com/M/title-exact?Four%20Rooms%...      291       3       874833936          3
    ```

-   If you perform a `self-join` operation, you can use the `view` method to retrieve columns from the left and right Collection objects.

    ```
    >>> movies2 = movies.view()
    >>> movies.join(movies2, movies.movie_id == movies2.movie_id)[movies, movies2.movie_id.rename('movie_id2')].head(3)
       movie_id            title_x release_date_x video_release_date_x  \
    0         2   GoldenEye (1995)    01-Jan-1995                 True
    1         3  Four Rooms (1995)    01-Jan-1995                 True
    2         4  Get Shorty (1995)    01-Jan-1995                 True
    
                                              imdb_url_x  movie_id2
    0  http://us.imdb.com/M/title-exact?GoldenEye%20(...          2
    1  http://us.imdb.com/M/title-exact?Four%20Rooms%...          3
    2  http://us.imdb.com/M/title-exact?Get%20Shorty%...          4
    ```

    PyODPS DataFrame supports the `left_join`, `right_join`, and `outer_join` operations in addition to the `join` operation. In the left\_join, right\_join, and outer\_join operations, renamed columns are suffixed with `_x` and `_y` by default. You can use a 2-`tuple` to define the suffixes in the `suffixes` parameter.

-   When you perform the left\_join, right\_join, or outer\_join operation, to avoid duplicate columns in the new table, set the `merge_columns` option to True. The system then selects non-null values from the duplicate columns as the values in the new column.

    ```
    >>> movies.left_join(ratings, on='movie_id', merge_columns=True)
    ```


To use the `mapjoin` operation, set `mapjoin` to True. The system then performs the `mapjoin` operation on the right Collection object. You can `join` a Collection object from MaxCompute to a Collection object from pandas, or `join` a Collection object from MaxCompute to a Collection object from a database. In both cases, MaxCompute executes the computation.

## Union operation

If two tables have the same columns of which the data types are the same, you can perform the `union` or `concat` operation to combine the tables into a single table regardless of how the columns are ordered.

```
>>> mov1 = movies[movies.movie_id < 3]['movie_id', 'title']
>>> mov2 = movies[(movies.movie_id > 3) & (movies.movie_id < 6)]['title', 'movie_id']
>>> mov1.union(mov2)
   movie_id              title
0         1   Toy Story (1995)
1         2   GoldenEye (1995)
2         4  Get Shorty (1995)
3         5     Copycat (1995)
```

You can `union` a Collection object from MaxCompute to a Collection object from pandas, or `union` a Collection object from MaxCompute to a Collection object from a database. In both cases, MaxCompute executes the computation.

