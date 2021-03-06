# LeetCoder User Manual
Leetcoder is a convenient Leetcode Python3 local evaluation/execution infrastructure. This is a debugging framework. It helps you test your Leetcode algorithms locally by copy-pasting content from Leetcode. (*Playground Debug* doesn't even work for some questions). This is especially useful when your algorithm hits *Output Limit Exceeded*, *Runtime Error* (recursion limit), *Time Limit Exceeded* (but you want to see what has been printed). 

Remember that **YOU** are responsible for checking whether the output matches with the correct answer.

## Tested Environmen
This framework has been tested using

* Windows 10 + Anaconda with Python 3.5.5
* MacOS + Python 3.6.5 (no Anaconda)
* If it works on Windows and MacOS, it probably works on most Linux distributions.

## To Use
* `cd` to root directory of **Leetcoder**
* Create a new file in directory *Leetcoder/problems*. File Name must follow patter `q###(question number)_question_name.py`.
* Follow [Basics](#Basics) to set up the file
* Run `python3 leetcoder.py`
  * You **DO NOT** need to specify filename. The most recent file will be executed automatically.
  * To test a specific file/problem, use `python3 leetcoder.py <filename>` (do not include directory *problems*)
* You will see something like the following

```

    ############################################################
    #                                                          #
    #=====>>>   Leetcode Local Evaluation Framework   <<< =====#
    #                                                          #
    ############################################################

Signature is def convertToBase7(self, num):
Modifers are
Method is    %--convertToBase7--%
Param Names: %--num--%
=====> Test case 0 <=====
Matched parameters: num=100
Executing problems.q504_base7.convertToBase7

===> stdout
===================
Time to execute problems.q504_base7.convertToBase7 is 0.0s

--- Result is as follows ----
'202'
===> Completed test case 0 <===s
```

### Simply put, do the following for each question
* Create a file with a descriptive file name that follows our `regex` pattern
* Copy paste starter funciton definition from Leetcode (except `class Solution`)
  * Yes, all you need is `def this_is_your_question(self, param1, param2)`
  * Yes, leave `self` in there. It is okay to not have `self`. Copy-paste.
* Set up infrastructure variables like [below](#Basics).


## Basics
For example, we solve [Problem 504. Base 7](https://leetcode.com/problems/base-7/) using this framework. Alternatively, the solution file is [here](problems/q504_base7.py).

```python
def convertToBase7(self, num):
    """
    :type num: int
    :rtype: str
    """
    if num == 0:
        return '0'

    vals = []
    minus = False
    if num < 0:
        minus = True

    num = abs(num)
    while num > 0:
        num, r = divmod(num, 7)
        vals.insert(0, str(r))

    if not minus:
        return ''.join(vals)
    else:
        return '-' + ''.join(vals)

# --- Things needed for this infrastructure to run ----
modifier = ''
signature = 'def convertToBase7(self, num):'
test_cases = None
input_string = """100
"""  
```

Files in [problems](problems/) folder are problems that I have solved. They are **NOT** necessarily correct and are **NOT** guaranteed to run at all. They are only there as references to using this infrastructure. 
(Back to [To Use](#To-Use))

## Advanced Use Cases

### Multiple Input Parameters
Multiple input parameters is handled automatically. See [Q451 Add Strings](problems/q451_add_strings.py) for more details. But simply, the following works,

```python
modifier = ''
signature = 'def addStrings(self, num1, num2):'
test_cases = None
input_string = """100
-200
""" 
```

### Multiple Test Cases

For our *Add String* example, we only used 1 test case. To test multiple cases, simply append to `test_string` variable like the following

```python
test_cases = None
input_string = """100
200
-100
-200
123
-123
"""  
```

The above essentially checks cases `(100, 200), (-100, -200), (123, -123)`. 

To only execute *select* test cases, supply `test_cases` with an iterable. Things like `[0,2]` means you want to test the first and the third case.

### Special Data Structures

Notice that the first part is exactly the same as how you would solve the problem on Leetcode. The last few lines describe what input is required. 

*modifier* Leetcode represents binary tree, linked list and intervals as lists. Check the following files as references
  * *Binary Tree* See [Q236 Common Ancestor](problems/q236_ancestor.py)
  * *Intervals* See [Q57 Insert Interval](problems/q57_insert_interval.py)
  * *Linked List* See [Q92 Partial Linked List Reversal](problems/q92_reverse_linkedlist.py)

Printing of these data structures are handled too. Binary tree and linked lists print recursively (you can write your own if you wish to print node value only). 

To support additional data structure, modify [lt_helper.py](lt_helper.py) and [leetcoder.py](leetcoder.py). If the content is generic enough but is not a Leetcode input parameter, check out [Useful Generic Things](#Useful-Generic-Things) section for more details.

### Design Questions

To handle design questions like [Q716 Max Stack](problems/q716_max_stack.py), you will need to modify function signature. Parameters that are special data structures are not supported yet. Simply, you need to do the following 

```python
modifier = ''
signature = 'class MaxStack:'
test_cases = None
input_string = """["MaxStack","push","push","push","top","popMax","top","peekMax","pop","top"]
[[],[5],[1],[5],[],[],[],[],[],[]]
"""    
```

## Useful Generic Things

I have created a file that contains algorithms/data structures that you might find useful. Look at [lt_algos.py](lt_algos.py). If you use content from this file, you can `from lt_algos import *`. Remember to copy-paste the content you used to submission code.

The following is a list of things available at the moment
* [Trie](https://en.wikipedia.org/wiki/Trie)
  * `class Trie`
  * `class TrieNode`

## Warning

* `global`  variables should be used with extreme caution. My framework and Leetcode's evaluation framework do not clear up `global` variables. You should manually clean up `global` variables before hand.
* Runtime provided here is very different from Leetcode runtime estimate. Timer here is for your reference only.


