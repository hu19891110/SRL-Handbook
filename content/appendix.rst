Appendix
========

.. _appendix-good-test-cases-design:

Good test cases design
----------------------

It is common misunderstanding which leads to bad test cases design. The number of test cases assigned to the problem is limited. The individual test case is not intended to test only one problem instance.

.. tip::
  We recommend you to redesign your input / output specification to handle with multiple problem instances in one test case. 

Consider the following elementary problem as an example:

.. admonition:: Task example
  :class: note

  For given integer numbers *a* and *b* calculate the sum *a + b*.

We could design the input / output specification to calculate the sum only for two numbers:

.. admonition:: Poor input / output design example
  :class: note

  **Input**
    In the only line of the input there will be two integer numbers *a* and *b* separated by a single space character.

  **Output**
    Program should write a single number which is the value of *a* + *b*.
            
It is correct but it is highly not recommended. First of all using even all possible slots for test cases cover a small part of the possible problem instances. Secondly, the execution of each test case is time consuming (about *2s* additional time for each test case).
           
We recommend to redesign the input / output specification in following manner:

.. admonition:: Proper input / output design example
  :class: note

  **Input**
    In the first line there will be the number *t* which is the number of instances for the problem. In each of the next *t* lines there will be the pair of two numbers *a* and *b* for which you should calculate the value of *a* + *b*.

  **Output**
    For each *a,b* pair print the calculated sum. Separate answers with new line character.
       
As you can see it is possible to pack a large number of problem instance into single test case.

.. note::
  Multiple test cases should rather be used to test different aspect of the problem.


.. _appendix-testing-time-complexity:

Testing the time complexity of algorithms
-----------------------------------------

Test cases give a possibility of verification time complexity of algorithms.

.. note::
  Presented method **is not** a real time complexity testing, slower algorithm can beat the faster one when well optimised for the test cases and the machine. 

  It **is also not** a universal method - changing the machine can allow slower algorithms to pass test cases designed for faster algorithms only.


.. _appendix-statuses:

Statuses
--------

There are two levels when the status is assigned to the submission:

 * **test case** the status produced by the test case judge
 * **master judge**

The master judge is a high order level component and it can arbitrary assign any status to the submission. We are going to focus on the test case judge statuses.

We separate statuses into two groups: semantic and systemic. The semantic statuses are strictly related to the correctness of the answer to the problem. On the other hand, the systemic statuses are syntactic related and the judge gets it from the system.

**Semantic statuses**
  * **Accepted (AC)** the submission is a correct solution to the problem.
  * **Wrong answer (WA)** the submission is an incorrect solution.     

**Sytemic statuses**
  * **Time limit exceeded (TLE)** the submission execution took too long.
  * **Runtime error (RE)** the error occurred during program execution.

    * **NZEC** (Non-Zero Exit Code) main function returned error signal (for example main function in C/C++ should return 0).
    * **SIGSEGV** the program accessed unallocated memory (segmentation fault).
    * **SIGABRT** the program received abort signal, usually programmer controls it (for example when C/C++ assert function yields false).
    * **SIGFPE** the floating point error, usually occurs when dividing by 0.
  * **Compilation error (CE)** the error occurred during compilation or syntax validation in interpreter.
  * **Internal error (IE)** the error occurred on the serivice side. One of the possible reasons can be poorly designed test case judge or master judge.

.. note::
  The Internal error covers wide area of errors (including server errors) thus in the near future we will introduce another type of error for judge and master judge errors.

To ilustrate errors consider again the following example:

.. admonition:: Example
  :class: note

  For a positive integer *n* calculate the value of the sum of all positive integers that are not greater than *n* i.e. *1* + *2* + *3* + ... + *n*. For example when *n* = *5* then the correct answer is *15*.

  **Input**
    In the first line there will be the number *1* |le| *t* |le| *10000000* which is the number of instances for your problem. In each of the next *t* lines there will be one number *n* for which you should calculate the described initial sum.

  **Output**
    For each *n* print the calculated initial sum. Separate answers with new line character.

The first error which can occur is the *compilation error*, for example submitting the following source code would produce the *CE* status:

.. code-block:: cpp
   
   long long initsum(long long n)
   {
     return n*(n+1)/2;
   }
   
   int main()
   {
     int t // missing semicolon
     long long n;
     scanf("%d", &t);
     while (t > 0)
     {
       scanf("%lld", &n);
       printf("%lld\n", initsum(n));
       t--;
     }
     return 0;
   }

.. image:: ../_static/status-appendix-ce.png
    :width: 700px
    :align: center

|

To obtain *runtime error* we can refer to unallocated memory:

.. code-block:: cpp
   
   long long initsum(long long n)
   {
     return n*(n+1)/2;
   }
   
   int main()
   {
     int t;
     long long n;
     scanf("%d", &t);
     while (t > 0)
     {
       scanf("%lld", n); // referring to unallocated memory 
       printf("%lld\n", initsum(n));
       t--;
     }
     return 0;
   }

.. image:: ../_static/status-appendix-re.png
    :width: 700px
    :align: center

|

We will *exceed time limit* with worse algorithm (if test cases are rich enough):

.. code-block:: cpp
   
   // suboptimal algorithm
   long long initsum(long long n)
   {
     int i;
     long long sum = 0;
     for (i=1; i <= n; i++)
     {
       sum += i;
     }
     return sum;
   }
   
   int main()
   {
     int t;
     long long n;
     scanf("%d", &t);
     while (t > 0)
     {
       scanf("%lld", &n);
       printf("%lld\n", initsum(n));
       t--;
     }
     return 0;
   }

.. image:: ../_static/status-appendix-tle.png
    :width: 700px
    :align: center

|

Bad output formatting causes *wrong answer* status:

.. code-block:: cpp

   long long initsum(long long n)
   {
     return n*(n+1)/2;
   }
   
   int main()
   {
     int t;
     long long n;
     scanf("%d", &t);
     while (t > 0)
     {
       scanf("%lld", &n);
       printf("%lld", initsum(n)); // missing new line character
       t--;
     }
     return 0;
   }

.. image:: ../_static/status-appendix-wa.png
    :width: 700px
    :align: center

|

At the end we present correct and optimal solution which passes all test cases and obtains *accepted* status:

.. code-block:: cpp
   
   long long initsum(long long n)
   {
     return n*(n+1)/2;
   }
   
   int main()
   {
     int t;
     long long n;
     scanf("%d", &t);
     while (t > 0)
     {
       scanf("%lld", &n);
       printf("%lld\n", initsum(n));
       t--;
     }
     return 0;
   }

.. image:: ../_static/status-appendix-acc.png
    :width: 700px
    :align: center
