# Controle Structures - Problem Set

### **Exercise 1: Conditional Statements (if-else)**

Write a Python program that takes a user’s income and tax status as input. If the income is greater than $50,000, they should be taxed at 25%. Otherwise, they are taxed at 15%. Print the amount of tax they owe.


```python
# Your code
```

### **Exercise 2: Nested if-else with Multiple Conditions**
Modify the following code to check if a user is eligible for a loan:
1. The user must be 21 or older.
2. They must have an income greater than $\$$30,000.
3. If their debt is greater than $\$$10,000, they are ineligible for the loan.


```python
age = int(input("Enter your age: "))
income = float(input("Enter your income: "))
debt = float(input("Enter your debt amount: "))
# Add the conditions here
```

### **Exercise 3: While Loop for Savings Target**
Write a program that asks the user for a savings target. The program should simulate monthly savings of $500 and keep adding it to the balance until the target is reached. Print how many months it took to reach the target.


```python
# Your code
```

### **Exercise 4: Loan Repayment Simulation with While Loop**
Simulate a loan repayment process where the loan balance is $\$$50,000, the monthly payment is $\$$1,000, and the monthly interest rate is 3%. The program should keep updating the balance and print how long it takes to repay the loan, as well as the total interest paid.


```python
# Your code
```

### **Exercise 5: For Loop Over a List**
Create a list of daily stock prices and calculate the total gain or loss by summing up the differences between consecutive days' prices.


```python
stock_prices = [100, 105, 98, 110, 120]
# Write the loop here
```

### **Exercise 6: For Loop with Condition**
Write a program that takes a list of numbers and prints out the squares of only the even numbers using a `for` loop.


```python
numbers = [1, 2, 3, 4, 5, 6]
# Write the loop here
```

### **Exercise 7: Nested For Loop with Matrix**
Given a 3x3 matrix, write a Python program to print the sum of each row using nested `for` loops.


```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
# Write the nested loop here
```

### **Exercise 8: Range-based For Loop**
Using a `for` loop, print the numbers from 1 to 20 that are divisible by 3.


```python
# Your code
```

### **Exercise 9: Break and Continue**
Write a Python program that asks the user to enter a number. If the number is greater than 100, print "Success" and exit the loop using `break`. If the number is less than 0, print "Invalid" and skip the iteration using `continue`.


```python
# Your code
```

### **Exercise 10: List Comprehension**
Given a list of grades, create a new list of grades where every grade is increased by 5, but only if the original grade is less than 90, using list comprehension.


```python
grades = [85, 92, 88, 76, 95]
# Write the list comprehension here
```

## More exercises on **list comprehension**

### **Exercise 1: Convert Temperatures**
Given a list of temperatures in Celsius, use list comprehension to convert them to Fahrenheit. The formula for conversion is:

$$
    F = \frac{9}{5} \times C + 32
$$


```python
celsius = [0, 20, 37, 100]
# Write the list comprehension to convert celsius to fahrenheit
```

### **Exercise 2: Filter and Square Odd Numbers**
Given a list of numbers, use list comprehension to create a new list containing the squares of the odd numbers only.


```python
numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
# Write the list comprehension to filter and square odd numbers
```

### **Exercise 3: Extract Vowels from a String**
Given a string, use list comprehension to create a list of all the vowels in the string.


```python
text = "List comprehensions are very powerful!"
# Write the list comprehension to extract vowels from the string
```

### **Exercise 4: Flatten a Nested List**
Given a list of lists (nested list), use list comprehension to flatten it into a single list.


```python
nested_list = [[1, 2, 3], [4, 5], [6, 7, 8, 9]]
# Write the list comprehension to flatten the nested list
```

### **Exercise 5: Dictionary from List**
You have a list of tuples where the first element is a country and the second element is its population. Use list comprehension to convert the list of tuples into a dictionary.


```python
countries = [("USA", 331002651), ("India", 1380004385), ("China", 1439323776)]
# Write the list comprehension to convert the list into a dictionary
```
