 # Exercise 3: Text Manipulation and Filtering

## Concepts Covered
- Searching within files
- Filtering text output
- Text processing and manipulation
- Basic regular expressions

## Commands to Learn
- `grep` - Search for patterns in files
- `sort` - Sort lines of text
- `uniq` - Report or filter out repeated lines
- `wc` - Count lines, words, and characters
- `cut` - Extract sections from lines of files
- `tr` - Translate characters

## Exercises

### 3.1: Working with Sample Data

First, let's create a sample data file to work with. Create a file called `employees.txt` with the following content:

```
John Smith,Engineering,75000,2018
Mary Johnson,Marketing,65000,2020
Robert Brown,Engineering,78000,2017
Lisa Davis,HR,62000,2019
Michael Wilson,Marketing,68000,2020
Sarah Miller,Engineering,82000,2016
David Garcia,Finance,70000,2018
Jennifer Lopez,HR,63000,2021
James Taylor,Finance,72000,2017
Patricia Moore,Engineering,80000,2019
```

### 3.2: Finding Patterns with grep

1. Use `grep` to find all employees in the Engineering department
2. Find all employees hired in 2020
3. Find employees with a salary higher than 70000 (hint: use regular expressions)
4. Use `grep` with the `-v` option to list all employees who are NOT in Marketing
5. Use `grep` with the `-i` option to find all entries containing "smith" regardless of case

### 3.3: Sorting Data

1. Use `sort` to display the file alphabetically by name
2. Sort the file numerically by salary
3. Sort the file by year hired (most recent first)
4. Use the appropriate option to sort the file by department name, then by salary in descending order

### 3.4: Counting with wc

1. Count the total number of lines in the file
2. Count the total number of words
3. Count how many employees are in the Engineering department (combine with grep)
4. Which department has the most employees? Write a command pipeline to find out

### 3.5: Extracting Fields with cut

1. Extract just the names from the file
2. Extract the departments and salaries
3. Find the average salary by department (you'll need to combine several commands)

### 3.6: Challenge - Data Analysis

1. Create a new file `sales.txt` with this data:

```
Region,Product,Month,Units,Revenue
North,Widgets,Jan,150,7500
South,Gadgets,Jan,200,12000
East,Widgets,Jan,130,6500
West,Gadgets,Jan,160,9600
North,Widgets,Feb,120,6000
South,Gadgets,Feb,180,10800
East,Widgets,Feb,140,7000
West,Gadgets,Feb,190,11400
North,Widgets,Mar,170,8500
South,Gadgets,Mar,210,12600
```

2. Write commands to answer these questions:
   - Which region had the highest total revenue?
   - What was the total number of Widget units sold?
   - Which month had the highest sales overall?
   - Calculate the average revenue per unit for each product

## Reflection Questions
- How can you combine multiple commands to perform complex data processing?
- When would you use `grep` versus `cut` or other text processing tools?
- How would these commands be useful in your own work or projects?
