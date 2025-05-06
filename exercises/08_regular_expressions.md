 # Exercise 8: Regular Expressions

## Concepts Covered
- Basic regular expression syntax
- Pattern matching
- Character classes
- Quantifiers
- Regex tools in Unix

## Commands to Learn
- `grep` with regex options
- `sed` for search and replace
- `awk` for pattern scanning and processing

## Exercises

### 8.1: Basic Pattern Matching

Let's create a file called `emails.txt` with the following content:

```
john.doe@example.com
jane_smith@company.org
support@website.co.uk
info@my-site.net
user123@gmail.com
contact@subdomain.example.com
not-an-email
just.a.word
admin@localhost
test.user@sub.domain.co
```

1. Use grep to find all lines that contain an email address
2. Find all email addresses from the "example.com" domain
3. Find all email addresses that use a ".org" or ".net" top-level domain
4. Find email addresses that have numbers in the username part

### 8.2: Character Classes and Quantifiers

Create a file called `data.txt` with this content:

```
Product1: $19.99, SKU: ABC-123-X
Product2: $5.50, SKU: DEF-456-Y
Product3: $199.95, SKU: GHI-789-Z
Phone: (555) 123-4567
IP Address: 192.168.1.1
IP Address: 10.0.0.255
Date: 2023-08-15
Date: 01/15/2023
Time: 14:30:45
ID: A12345
ID: B-9876-C
```

Use grep with regular expressions to:

1. Find all price values (dollar amounts)
2. Match all SKU codes
3. Find phone numbers
4. Match valid IP addresses
5. Find all dates in either format (YYYY-MM-DD or MM/DD/YYYY)

### 8.3: Using sed for Search and Replace

1. Create a file called `document.txt` with several paragraphs of sample text
2. Use `sed` to:
   - Replace all occurrences of "Unix" with "UNIX"
   - Change all American spellings to British (e.g., "color" to "colour")
   - Remove all lines containing a specific word
   - Add a prefix to the beginning of each line
   - Insert a blank line after each existing line

### 8.4: Advanced Pattern Matching with awk

Create a file called `transactions.csv` with the following content:

```
Date,Customer,Product,Quantity,Price,Total
2023-01-15,John Smith,Widget A,2,19.99,39.98
2023-01-16,Jane Doe,Gadget B,1,49.95,49.95
2023-01-16,Bob Johnson,Widget A,3,19.99,59.97
2023-01-17,Alice Brown,Tool C,2,29.99,59.98
2023-01-18,John Smith,Tool C,1,29.99,29.99
2023-01-19,Jane Doe,Widget A,5,19.99,99.95
2023-01-20,Bob Johnson,Gadget B,2,49.95,99.90
```

Use `awk` to:

1. Print only the Date and Total columns
2. Calculate the sum of all Total values
3. Find all transactions for a specific customer
4. Calculate the total spent on each product
5. Find the customer who spent the most money overall

### 8.5: Validating Data with Regex

Create a script called `validate.sh` that:

1. Takes a filename as an argument
2. Reads each line and validates whether it matches a specific pattern
3. For example, check if each line contains a valid:
   - Email address
   - URL
   - Date in ISO format (YYYY-MM-DD)
   - Credit card number (simplified pattern)
4. Print out which lines are valid and which are invalid

### 8.6: Challenge - Log Parsing with Regex

Given a web server log file (`access.log`) with entries in this format:

```
192.168.1.5 - - [15/Mar/2023:08:23:45 +0000] "GET /index.html HTTP/1.1" 200 2048 "http://example.com" "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
10.0.0.2 - - [15/Mar/2023:08:25:30 +0000] "POST /login HTTP/1.1" 302 0 "http://example.com/login" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)"
192.168.1.7 - - [15/Mar/2023:08:26:45 +0000] "GET /images/logo.png HTTP/1.1" 200 4096 "http://example.com" "Mozilla/5.0 (iPhone; CPU iPhone OS 15_4)"
10.0.0.5 - - [15/Mar/2023:08:30:15 +0000] "GET /api/data HTTP/1.1" 404 1024 "http://example.com/app" "Mozilla/5.0 (X11; Linux x86_64)"
```

Use a combination of grep, sed, and awk to:

1. Extract all unique IP addresses that accessed the server
2. Find all 404 (Not Found) errors
3. Count the number of requests per hour
4. Extract all user agents and categorize them by operating system
5. Find the most requested URL path

## Reflection Questions
- How do regular expressions make text processing more powerful?
- What are the differences between basic and extended regular expressions?
- When would you use sed versus awk for text processing?
- What are the limitations of regular expressions, and when might they not be the best tool?
