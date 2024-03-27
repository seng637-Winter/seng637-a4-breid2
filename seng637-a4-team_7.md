**SENG 637 - Dependability and Reliability of Software Systems**

**Lab. Report \#4 – Mutation Testing and Web app testing**

| Group \#: 7     |
| -------------- |
| Student Names: |
| Carissa Chung               |
| Benjamin Reid               |
| Braden Tink               |
| Christian Valdez               |
| Alton Wong               |

# Introduction
Mutation testing is a white-box method of testing which tests both the code, as well as the unit tests (such as in assignment 3). Mutations (changes to the code or variables) are introduced to see how the unit tests handle this. The mutations can slip through the testing (Survive) or can be caught by the testing (Killed).

GUI testing is the process of testing and interacting with the GUI portion of the code, rather than writing directly for the code base. For this assignment Selenium was used to test the web interface of a website.

# Analysis of 10 Mutants of the Range class 
The following mutants were found in the Range Class:

1. Incremented (a++) double local variable number 1 -> KILLED (Line 90: if (lower>upper){)

   This mutation was killed since the line it is checking is the check for the constructor of Range. This is called whenever a new Range is created by the test class. Therefore when it mutates the value at the check, it results in an error being thrown, and the mutation is caught.
2. Decremented (a--) double local variable number 1 -> SURVIVED (Line 91: String msg = "Range(double, double): require lower (" + lower)

   This mutation was not caught by the testing, as it is within the creation of the error message of the mutation above. This line is only accessed during the throwing of an error, and does not change behaviour when this mutation happens.
3. Negated double field upper -> KILLED (Line 123: return this.upper - this.lower;)

   This mutation was caught since changing the return will lead to a mismatch between the expected and result of the test. This return was for getLength(), which was covered in the "testGetLengthPositiveRange()" method.
4. Substituted 1 with -1 -> SURVIVED (Line 158: return (b1 > this.lower);)

   This line was mutated 34 times through the testing, and there was survival for 13 of those. Since this is a comparison, if the value for this.lower is less than -1, the value for b1 could be anything above -1 and still return the same answer. So this mutation is only caught when the mutation changes the outcome of the comparison, not necessarily just the values.
5. Negated conditional -> KILLED (Line 217: if (range1 == null) {)

   This line determines which return statement will be executed, so all four mutations on this line were killed. When the conditional is negated, the return of range2 is not executed, resulting in a mismatch between the expected and result of the test. This was specifically tested in "testCombine()" method.
6. Removed conditional - replaced equality check with false -> KILLED (Line 274: if (Double.isNaN(d2)){)

    Since this line is checking for NaN values, a value for d2 of NaN will not take the desired route. This will cause an issue on the later return statement, resulting in the NaN value being returned, which is not what was expected by the test. 
7. Negated integer local variable number 3 -> SURVIVED (Line 366: if (allowZeroCrossing){)

    The variable of allowZeroCrossing is a boolean, so the negation does not change the outcome of the test. This specific method is tested in "testRepeatedShifts()", and 9 of the 13 mutations on this line are killed.
8. Replaced boolean return with true for org/jfree/data/Range::equals -> KILLED (Line 434: return false;)

    This return statment is the return when checking if two ranges have the same upper bound, so if they do not, but the boolean is changed to true, then the test has an unexpected result. This was specifically tested for in the "testEqualsOfDifObjects()" method.  
9. Substituted 29 with 1 -> KILLED (Line 463: result = 29 * result + (int) (temp ^ (temp >>> 32));)
  
    This line is the main calculation to return the hash code. By using 1 rather than 29 the calculation is off, and this is caught by the "testHashCode()" method.    
10. Removed conditional - replaced equality check with false -> KILLED (Line 220: if(range2 == null) {)
    
    This condition check determines if the range is null, and adjusts the flow through the code as required. By just setting it to false, it allows a 'null' to pass through which gives an unexpected result. This is specifically tested for in "testCombineB()" method. 


# Report all the statistics and the mutation score for each test class
Range Testing from Assignment 3 Mutation Results:

<img width="607" alt="image" src="https://github.com/seng637-Winter/seng637-a4-breid2/assets/49459800/38263c16-1432-4271-9992-760e0fee1642">


Data Utilities Testing from Assignment 3 Mutation Results: 

<img width="607" alt="image" src="https://github.com/seng637-Winter/seng637-a4-BradenTink/blob/342f684363b3e00b3207d6c10124d88ef64637ad/Screenshot%202024-03-21%20181845.png">

Full Output from the Pit Test Can be seen in this file:

[Output of Range Test](RangeMutationTest.xlsx)

[Output of DataUtilities Test](DataUtilitiesMutationTest.xlsx)
# Analysis drawn on the effectiveness of each of the test classe"
Overall the test cases were fairly effective. The Range test had a line coverage of 98%, a mutation coverage of 73%, and a test strength of 74%. For DataUtilities it had a line coverage of 88%, mutation coverage of 73%, and a test strength of 82%. This shows that line coverage is good for both tests, and mutation coverage is good, but could be improved. There were several mutations for both classes that survived, so those should be investigated to ensure the test suite is a comprehensive as possible. 

# A discussion on the effect of equivalent mutants on mutation score accuracy
Equivalent mutants are mutations in which the function is the same as the original. As such they are often not killed since the function of the code remains in place. This is illustrated by mutation 4 listed above, where the change in value did not change the outcome of the comparison. Mutations such as these make it almost impossible to get a 100% score since these mutations are not always real issues. If these equivalent mutations are identified and removed, then the score can increase. 

Detecting equivalent mutations is not a simple task, but some types might be able to be caught more than others. For example when mutating a value, if that value is being used in a comparison, and the comparison gives the same result, then you likely have an equivalent mutant. Changing a conditional like an equality check will typically not be an equivalent mutant since the path through the code is now different.

# A discussion of what could have been done to improve the mutation score of the test suites
To improve the score, equivalent mutants can be removed, so that false failures are no longer counted towards the score. In Range, the following mutants can be removed for being equivalent:

Line 91 Mutant 4-7, these simply increment the value at the message, and don't affect the function.

Line 92 Mutant 5-8, this again is a simple increment in the message, and don't affect the function.

Line 144 Mutants 2-6, 8-22, these change values or set the comparison, but only affect the outcome if the change is significantly different. In many cases the value still lands in the acceptable range to output the correct result.

Line 157 Mutants 1-11, this line just checks if b0 is less than the lower value, so setting the value often does not change the function of the code.

Line 158 Mutants 1-13, the return is a boolean, so changing the value that still results in the same boolean result is an equivalent mutant.

Line 161 Mutants 8-21, this again is a return of a boolean, so incrementing the variable often doesn't change the outcome of the comparison, so the result is an equivalent mutant.

Line 193 Mutants 1-8, this is another boolean check, so incrementing the value, or changing from >= to > often results in an equivalent mutant.

Line 255 Mutants 1-12, this is a NaN check, so incrementing or negating the values does not change the function and is an equivalent mutation.

Line 277 Mutants 1-4, this line returns the minimum of two values, so incrementing the larger value does not change the function and results in an equivalent mutation.

Line 287 Mutants 1-4, this line returns the maximum of two values, so incrementing the larger value does not change the function and results in an equivalent mutation.

Line 388 Mutants 1-4, this line returns the maximum of two values, so incrementing the larger value does not change the function and results in an equivalent mutation.

Line 391 Mutants 1-4, this line returns the minimum of two values, so incrementing the larger value does not change the function and results in an equivalent mutation.

Line 410 Mutants 4-7, this line checks that the value is less than zero, so an increment often doesn't change the result, meaning it is an equivalent mutant.

Removing just these 107 equivalent mutants changes the score from being 917/1259=73% to 917/1152=80%.


In Data Utilities, the following mutants can be removed for being equivalent:

Line 76 Mutants 1-2, Changing the values of b did not change the function as they are getting compared to a null value so the are equivalent mutations
	
Line 85 Mutants 2-3, Incrementing the values in the function does not change the comparison jus the values being compared so they equivalent mutations
	
Line 89 Mutants 1-2, The return type being changed results in the same boolean being returned so they equivalent mutations 

Line 133 Mutants 12-19, These mutations are incremeneting and decrementing the local value which leads to the same overall function being an equivalent mutation

Line 158 Mutants 1,4-5, This line checks to see if the total is greater than 0 changing the value by incremeneting it by one does not often change the result and results in an equivalent mutation

Line 172 Mutants 1-2, These muations are incremeneting and decrementing the returned value which does not lead to any changes in the functionality of the method leading to equivalent mutations

Line 194 Mutants 12-19, These changes are adjusting how the function is incrementing the counter however as it will lead to the same final reuslt these are equivalent mutations 
	
Line 200 Mutants 1-2, These muations are incremeneting and decrementing the returned value which does not lead to any changes in the functionality of the method leading to equivalent mutations

Line 221 Mutants 13-14, The mutations are incremeneting and decrementing the comparison where if the column count is less than 0 so looking at column count less than zero will lead to equivalent mutations 

Line 248 Mutants 1-2, These mutations are incremeneting and decrementing the returned value of the function which leads a new value of the data being returned. It does not change the base functionality of the method so they are equivalent muations

Line 291 Mutants 7,9, These changes dont effect the starting point of where to total the values however as its not missing any values the mutation would lead to the same end result. 

Line 291 Mutants 12,14, These changes are adjusting how the function is incrementing the counter however as it will lead to the same final reuslt these are equivalent mutations 

Removing just these 39 equivalent mutants changes the score from being 646/881=73% to 646/=77%.


# Why do we need mutation testing? Advantages and disadvantages of mutation testing
Mutation testing is useful for not only checking the code, but checking the testing process. 

Advantages:
- Can find new bugs/issues
- Fairly automated
- Provides a second layer of testing after unit test

Disadvantages:
- Can take a while to run
- Lots of results to analyze
  
# Explain your SELENUIM test case design process
As a group we decided to perform Selenium testing on homedepot.ca using the Selenium IDE. We decided to test functionality that would be often be used by everyday users. We excluded making purchases from the test plan. 
Here is a list functions that were tested from the website:
- Login/Logout
- Adding/Removing items from cart
- Changing the preferred store
- Testing the chat functionality
- Filtering products
- Searching for products
- Switching the website language

The selenium IDE test cases can be found [here](SENG637_A4.side)

# Explain the use of assertions and checkpoints
Assertions and checkpoints are used at specific points of the test case to ensure the functionality is working as intended.

In our test cases, each test case has at least one assert statement to ensure the functionality is implemented properly

| Test Script Name        | Assert Statement                         |
|-------------------------|------------------------------------------|
| Add item to cart        | Asserts the cart qty is 1                |
| Change 'My Store'       | Asserts the new store title is updated   |
| Chat Functionality      | Asserts the chatbox is loaded            |
| Login Test              | Asserts proper user is logged in         |
| Login Test Fail         | Asserts error message occurs             |
| Login/Logout Test       | Asserts logged out message occurs        |
| Product Filtering       | Asserts filter is applied                |
| Remove item from cart   | Asserts cart is empty                    |
| Search Functionality    | Asserts search term is correctly applied |
| Switch website language | Asserts language is switched             |
# how did you test each functionaity with different test data
Not all functionality required different test data.
Only the login functionality was tested with an incorrect password to test the login test failure. 

Other functionality such as product filtering and adding/removing items to the cart cannot be tested with different data.
# How the team work/effort was divided and managed
The team members looked at both the mutation testing and the Selenium testing. This way the students could be familiar with both. The mutation testing was split between some of the members, with half doing Range and half doing DataUtilities. Then for the Selenium testing this was done independently by the students, and then pulled together into the final report.

# Difficulties encountered, challenges overcome, and lessons learned
Initially there was some difficulty in getting the mutation testing to run, but that turned out to be environment setup issues. After that the process went well. Being able to see how these different mutations affected the program was interesting, and came with some interesting outcomes.

# Comments/feedback on the assignment itself
The assignment was good, was interesting to actually work with some of these testing tools. Overall instructions were clear, but some of the analysis/discussion was a bit open ended, so we were not quite sure the best way to answer the questions.
