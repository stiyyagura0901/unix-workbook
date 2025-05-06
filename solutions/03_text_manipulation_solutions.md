 # Exercise 3: Text Manipulation and Filtering - Solutions

## 3.2: Finding Patterns with grep

1. Find all employees in the Engineering department:
   ```bash
   grep "Engineering" employees.txt
   ```

2. Find all employees hired in 2020:
   ```bash
   grep ",2020$" employees.txt
   ```

3. Find employees with a salary higher than 70000:
   ```bash
   grep -E ",7[0-9]{4}," employees.txt
   # or a more precise regex
   grep -E ",([7-9][0-9][0-9][0-9][0-9]|[0-9]{6,})," employees.txt
   ```

4. List all employees who are NOT in Marketing:
   ```bash
   grep -v "Marketing" employees.txt
   ```

5. Find all entries containing "smith" regardless of case:
   ```bash
   grep -i "smith" employees.txt
   ```

## 3.3: Sorting Data

1. Sort alphabetically by name:
   ```bash
   sort employees.txt
   ```

2. Sort numerically by salary:
   ```bash
   sort -t, -k3,3n employees.txt
   ```

3. Sort by year hired (most recent first):
   ```bash
   sort -t, -k4,4nr employees.txt
   ```

4. Sort by department name, then by salary in descending order:
   ```bash
   sort -t, -k2,2 -k3,3nr employees.txt
   ```

## 3.4: Counting with wc

1. Count total number of lines:
   ```bash
   wc -l employees.txt
   ```

2. Count total number of words:
   ```bash
   wc -w employees.txt
   ```

3. Count how many employees are in Engineering:
   ```bash
   grep "Engineering" employees.txt | wc -l
   ```

4. Find which department has the most employees:
   ```bash
   cut -d, -f2 employees.txt | sort | uniq -c | sort -nr | head -1
   ```

## 3.5: Extracting Fields with cut

1. Extract just the names:
   ```bash
   cut -d, -f1 employees.txt
   ```

2. Extract departments and salaries:
   ```bash
   cut -d, -f2,3 employees.txt
   ```

3. Find average salary by department:
   ```bash
   # This is a complex operation requiring awk:
   awk -F, '{sum[$2] += $3; count[$2]++} END {for (dept in sum) print dept, sum[dept]/count[dept]}' employees.txt
   ```

## 3.6: Challenge - Data Analysis

1. Which region had the highest total revenue:
   ```bash
   # Skip header with tail -n +2
   tail -n +2 sales.txt | awk -F, '{sum[$1] += $5} END {for (region in sum) print region, sum[region]}' | sort -k2,2nr | head -1
   ```

2. Total number of Widget units sold:
   ```bash
   grep "Widgets" sales.txt | awk -F, '{sum += $4} END {print sum}'
   ```

3. Which month had highest sales overall:
   ```bash
   tail -n +2 sales.txt | awk -F, '{sum[$3] += $5} END {for (month in sum) print month, sum[month]}' | sort -k2,2nr | head -1
   ```

4. Calculate average revenue per unit for each product:
   ```bash
   tail -n +2 sales.txt | awk -F, '{units[$2] += $4; revenue[$2] += $5} END {for (prod in units) print prod, revenue[prod]/units[prod]}'
   ```

## Additional Notes

- The `-F` option in awk specifies the field separator (same as -d in cut)
- The `-t` option in sort specifies the field separator
- The `-k` option in sort specifies which field(s) to sort by
- The `uniq -c` command counts occurrences of each unique line
