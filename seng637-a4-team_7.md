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

Full Output from the Pit Test Can be seen in this file:
[Output of Range Test](RangeMutationTest.xlsx)
# Analysis drawn on the effectiveness of each of the test classes

# A discussion on the effect of equivalent mutants on mutation score accuracy

# A discussion of what could have been done to improve the mutation score of the test suites

# Why do we need mutation testing? Advantages and disadvantages of mutation testing

# Explain your SELENUIM test case design process

# Explain the use of assertions and checkpoints

# how did you test each functionaity with different test data

# How the team work/effort was divided and managed

# Difficulties encountered, challenges overcome, and lessons learned

# Comments/feedback on the assignment itself
