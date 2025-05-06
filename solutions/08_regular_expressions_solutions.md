# Exercise 8: Regular Expressions - Solutions

## 8.1: Basic Pattern Matching

Working with the `emails.txt` file:

```bash
# Find all lines that contain an email address
grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" emails.txt

# Find email addresses from the "example.com" domain
grep "@example\.com" emails.txt

# Find email addresses with .org or .net top-level domain
grep -E "\.(org|net)$" emails.txt

# Find email addresses that have numbers in the username part
grep -E "^[^@]*[0-9][^@]*@" emails.txt
```

## 8.2: Character Classes and Quantifiers

Working with the `data.txt` file:

```bash
# Find all price values (dollar amounts)
grep -E "\$[0-9]+(\.[0-9]{2})?" data.txt

# Match all SKU codes
grep -E "[A-Z]{3}-[0-9]{3}-[A-Z]" data.txt

# Find phone numbers
grep -E "\([0-9]{3}\) [0-9]{3}-[0-9]{4}" data.txt

# Match valid IP addresses
grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" data.txt

# Find all dates in either format (YYYY-MM-DD or MM/DD/YYYY)
grep -E "([0-9]{4}-[0-9]{2}-[0-9]{2}|[0-9]{2}/[0-9]{2}/[0-9]{4})" data.txt
```

## 8.3: Using sed for Search and Replace

Using `sed` with the `document.txt` file:

```bash
# Replace all occurrences of "Unix" with "UNIX"
sed 's/Unix/UNIX/g' document.txt

# Change American to British spelling
sed 's/color/colour/g' document.txt

# Remove all lines containing a specific word
sed '/specific_word/d' document.txt

# Add a prefix to the beginning of each line
sed 's/^/PREFIX: /' document.txt

# Insert a blank line after each existing line
sed 's/$/\n/' document.txt
```

## 8.4: Advanced Pattern Matching with awk

Working with the `transactions.csv` file:

```bash
# Print only Date and Total columns
awk -F, '{print $1, $6}' transactions.csv

# Calculate the sum of all Total values
awk -F, 'NR>1 {sum += $6} END {print "Total:", sum}' transactions.csv

# Find all transactions for a specific customer
awk -F, '$2 ~ /John Smith/ {print}' transactions.csv

# Calculate total spent on each product
awk -F, 'NR>1 {spent[$3] += $6} END {for (product in spent) print product, spent[product]}' transactions.csv

# Find the customer who spent the most money
awk -F, 'NR>1 {customer_total[$2] += $6} END {max=0; max_customer=""; for (customer in customer_total) {if (customer_total[customer] > max) {max = customer_total[customer]; max_customer = customer}} print "Customer who spent the most:", max_customer, "Amount:", max}' transactions.csv
```

## 8.5: Validating Data with Regex

Creating a validation script `validate.sh`:

```bash
#!/bin/bash

# Check if a filename was provided
if [ $# -ne 1 ]; then
    echo "Usage: $0 <filename>"
    exit 1
fi

# Check if the file exists
if [ ! -f "$1" ]; then
    echo "Error: File $1 not found."
    exit 1
fi

# Initialize line counter
line_num=0

# Process each line of the file
while IFS= read -r line; do
    # Increment line counter
    ((line_num++))

    # Skip empty lines
    if [ -z "$line" ]; then
        echo "Line $line_num: Empty line"
        continue
    fi

    # Email validation
    if [[ $line =~ ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ ]]; then
        echo "Line $line_num: Valid email address"
        continue
    fi

    # URL validation (simplified)
    if [[ $line =~ ^https?://[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}(/[a-zA-Z0-9._~:/?#[\]@!$&'()*+,;=%-]*)?$ ]]; then
        echo "Line $line_num: Valid URL"
        continue
    fi

    # Date in ISO format (YYYY-MM-DD)
    if [[ $line =~ ^[0-9]{4}-[0-9]{2}-[0-9]{2}$ ]]; then
        echo "Line $line_num: Valid ISO date"
        continue
    fi

    # Credit card number (simplified)
    if [[ $line =~ ^[0-9]{4}(-[0-9]{4}){3}$ ]]; then
        echo "Line $line_num: Valid credit card format"
        continue
    fi

    # No pattern matched
    echo "Line $line_num: Invalid format"

done < "$1"

exit 0
```

Make the script executable:

```bash
chmod +x validate.sh
```

## 8.6: Challenge - Log Parsing with Regex

Working with the `access.log` file:

```bash
# 1. Extract all unique IP addresses
grep -oE "^([0-9]{1,3}\.){3}[0-9]{1,3}" access.log | sort | uniq

# 2. Find all 404 (Not Found) errors
grep ' 404 ' access.log

# 3. Count the number of requests per hour
grep -oE '[0-9]{2}/[A-Za-z]{3}/[0-9]{4}:[0-9]{2}' access.log | sort | uniq -c

# 4. Extract user agents and categorize by OS
grep -o '"Mozilla[^"]*"' access.log | grep -o '([^()]*)' | sort | uniq -c

# 5. Find the most requested URL path
grep -oE 'GET [^ ]*' access.log | cut -d ' ' -f2 | sort | uniq -c | sort -nr | head -1
```

## Additional Notes

### Common Regular Expression Patterns

- `.` - Match any single character
- `*` - Match 0 or more of the preceding character
- `+` - Match 1 or more of the preceding character
- `?` - Match 0 or 1 of the preceding character
- `^` - Match the start of a line
- `$` - Match the end of a line
- `[]` - Character class, match any character inside the brackets
- `[^]` - Negated character class, match any character not inside the brackets
- `()` - Group patterns together
- `|` - Alternation, match either the pattern before or after the pipe
- `{n}` - Match exactly n occurrences of the preceding character
- `{n,}` - Match at least n occurrences of the preceding character
- `{n,m}` - Match between n and m occurrences of the preceding character

### Differences Between Basic and Extended Regular Expressions

- Basic regular expressions (BRE): `^`, `$`, `.`, `*`, `[`, `]` are special
- Extended regular expressions (ERE): Adds `?`, `+`, `{`, `}`, `|`, `(`, `)` as special
- Use `-E` with grep for extended regex (or use egrep)
- In sed, use `-E` for extended regex, or escape special characters in basic regex

### When to Use Different Tools

- `grep`: For simple pattern matching and filtering
- `sed`: For search and replace operations
- `awk`: For complex text processing involving fields/columns and calculations

### Tips for Working with Regular Expressions

- Start with simple patterns and gradually add complexity
- Test your regex on small samples before processing large files
- Use online regex testers to debug complex patterns
- Remember that different tools may have slightly different regex syntax
- When performance matters, consider using specific, optimized patterns
