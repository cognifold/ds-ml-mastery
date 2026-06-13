# Module 01 Quiz: Python for Data Science

**Instructions:** Choose the single best answer for each question.

---

**Q1.** Which NumPy operation correctly converts an array of Celsius temperatures to Fahrenheit *without* a Python loop?

A) `for t in temps: (t * 9/5) + 32`  
B) `np.convert(temps, "celsius", "fahrenheit")`  
C) `(temps * 9/5) + 32`  
D) `temps.apply(lambda t: t * 9/5 + 32)`  

---

**Q2.** What does `arr.shape` return for a 2-D NumPy array with 50 rows and 8 columns?

A) `50`  
B) `(8, 50)`  
C) `400`  
D) `(50, 8)`  

---

**Q3.** You have `df = pd.read_csv("data.csv")`. Which call gives you a quick summary of column names, non-null counts, and dtypes?

A) `df.describe()`  
B) `df.info()`  
C) `df.head()`  
D) `df.shape`  

---

**Q4.** What is the result of `pd.Series([1, 2, 3]) + pd.Series([10, 20, 30])`?

A) `[1, 2, 3, 10, 20, 30]`  
B) `pd.Series([11, 22, 33])`  
C) `pd.Series([10, 20, 30])`  
D) A `TypeError` because Series do not support `+`  

---

**Q5.** `df.loc[df["score"] > 90, "grade"]` selects:

A) Rows where score > 90, all columns  
B) The "grade" column for all rows  
C) The "grade" column only for rows where score > 90  
D) All rows where grade is in the "score" column  

---

**Q6.** What does `df["city"].value_counts()` return?

A) The unique values of the "city" column as a list  
B) A Series of unique city names sorted alphabetically  
C) A Series of city names and their occurrence counts, sorted descending  
D) The total number of non-null values in "city"  

---

**Q7.** Which statement about `df.iloc[2, 3]` is correct?

A) Selects the row labelled 2 and the column labelled 3  
B) Selects the third row and fourth column by integer position  
C) Selects the second row and third column by integer position  
D) Raises an error because iloc does not accept two arguments  

---

**Q8.** You run `df.groupby("department")["salary"].mean()`. What is the output?

A) The mean salary of the entire DataFrame  
B) A DataFrame with one row per department and one column per unique salary  
C) A Series indexed by department with each group's mean salary  
D) A grouped object that must be iterated manually  

---

**Q9.** What does `np.zeros((3, 4))` create?

A) A 1-D array of 12 zeros  
B) A 3×4 array filled with zeros  
C) A 4×3 array filled with zeros  
D) A Python list of lists  

---

**Q10.** Which method removes rows that have *any* missing value?

A) `df.fillna(0)`  
B) `df.dropna()`  
C) `df.isnull()`  
D) `df.notna()`  

---

**Q11.** `df["date_col"] = pd.to_datetime(df["date_col"])` is necessary because:

A) Pandas stores all columns as strings by default  
B) Without it, date arithmetic like subtraction will raise a TypeError  
C) `read_csv` always drops date columns  
D) `to_datetime` compresses memory usage  

---

**Q12.** Which list comprehension builds a list of squares for even numbers in `range(10)`?

A) `[x**2 for x in range(10) if x % 2 == 0]`  
B) `[x**2 if x % 2 == 0 for x in range(10)]`  
C) `(x**2 for x in range(10) if x % 2 == 0)`  
D) `[x for x**2 in range(10) if x % 2 == 0]`  

---

**Q13.** Broadcasting in NumPy means:

A) Sending array data over a network  
B) Automatically aligning and operating on arrays of compatible (but not identical) shapes  
C) Copying an array to multiple variables simultaneously  
D) Converting a 1-D array to a 2-D array  

---

**Q14.** Which Pandas method is most appropriate for replacing NaN values in a numeric column with the column's median?

A) `df["col"].dropna()`  
B) `df["col"].fillna(df["col"].median())`  
C) `df["col"].replace(np.nan, "median")`  
D) `df["col"].interpolate(method="median")`  

---

**Q15.** You want the 5 complaint types with the highest count in a "Complaint Type" column. Which code is correct?

A) `df["Complaint Type"].head(5)`  
B) `df.groupby("Complaint Type").head(5)`  
C) `df["Complaint Type"].value_counts().head(5)`  
D) `df["Complaint Type"].sort_values().tail(5)`  

---

**Q16.** What is the key difference between a Pandas `Series` and a `DataFrame`?

A) A Series can hold mixed types; a DataFrame cannot  
B) A Series is 1-D with a single index; a DataFrame is 2-D with row and column indices  
C) A DataFrame is faster than a Series for all operations  
D) A Series is only used for integer data  

---

**Q17.** `df["Created Date"].dt.month` requires that "Created Date" has dtype:

A) `object`  
B) `int64`  
C) `datetime64[ns]`  
D) `category`  

---

**Q18.** Which statement best describes the `split → apply → combine` pattern?

A) Split a string, apply a regex, combine the matches  
B) Split a DataFrame into groups, apply an aggregation function, combine results into a new structure  
C) Split training/test sets, apply a model, combine predictions  
D) Split a file by line, apply a transformation, combine into a single list  

---

**Q19.** Why should you generally avoid `df.iterrows()` in production code?

A) It raises an error for DataFrames larger than 10 000 rows  
B) It returns tuples instead of Series  
C) It is significantly slower than vectorised operations for large DataFrames  
D) It modifies the DataFrame in-place  

---

**Q20.** `df.sort_values("revenue", ascending=False).head(10)` does what?

A) Returns the 10 rows with the lowest revenue  
B) Sorts the DataFrame permanently by revenue descending  
C) Returns the top 10 rows sorted by revenue in descending order  
D) Raises an error because `ascending=False` is not a valid argument  

---

## Answer Key

| Q | Answer | Q | Answer |
|---|--------|---|--------|
| 1 | C | 11 | B |
| 2 | D | 12 | A |
| 3 | B | 13 | B |
| 4 | B | 14 | B |
| 5 | C | 15 | C |
| 6 | C | 16 | B |
| 7 | B | 17 | C |
| 8 | C | 18 | B |
| 9 | B | 19 | C |
| 10 | B | 20 | C |
