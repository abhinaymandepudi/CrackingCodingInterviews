
# https://wiki.python.org/moin/TimeComplexity

Refer that link for time complexities

# 1.1 Is Unique: Implement an algorithm to determine if a string has all unique characters. What if you cannot use additional data structure ?


Method 1: Using hash table that is dictionary in python.<br>
Running Time : O(n) <br>
Extra Space : O(1) byte*size of characters, i.e total different characters in the given string. <br>


```python
def isUnique(string):
    characters = dict()
    for char in string:
        if char in characters:
            return False
        else:
            characters[char] = True
    return True
print ("Using dictionary-",isUnique("abhin"),isUnique("abhinaya"))

def isUnique(string):
    return len(string) == len(set(string))
print ("Using set() -",isUnique("abhin"),isUnique("abhinaya"))
print("first method is faster as it can be stopped right after finding a repeated characters\n \
    and it won't stop till last character in second")
```

    Using dictionary- True False
    Using set() - True False
    first method is faster as it can be stopped right after finding a repeated characters
         and it won't stop till last character in second


# Method 2:
Using Bit Vector Method. reduces space by a factor of Byte as we save the seen character as a bit instead of giving it an extra byte.<br>
Running Time : O(n)<br>
Extra Space : O(1) bit*size of characters, i.e total different characters in the given string.


```python
def isUnique(string):
    seenBits = 0
    for char in string:
        #python allows us to bit manipulations directly on integers and the processing is done at their bit level
        #here i'm talking about & and |
        if ((1 << ord(char)) & seenBits) > 0: 
        # checking with & whether there is 1 already in seenBits at position ord(char)
        # 1 & 1 = 1 ; 0 & 1 = 0
            return False
        seenBits = seenBits | (1 << ord(char)) 
        # 1 | 0 = 1
        #adding one to a position of the ascii i.e if a is encountered,
        #add 1 to the seemBits's bit's position (97+1) from left as the bit count starts from 0
    return True

print ("Using dictionary-",isUnique("abhin"),isUnique("abhinaya"))

```

    Using dictionary- True False


Function Definitions:
# ord(c) :
Given a string of length one, return an integer representing the Unicode code point of the character when the argument is a unicode object, or the value of the byte when the argument is an 8-bit string. For example, ord('a') returns the integer 97, ord(u'\u2020') returns 8224. This is the inverse of chr() for 8-bit strings and of unichr() for unicode objects. If a unicode argument is given and Python was built with UCS2 Unicode, then the character’s code point must be in the range [0..65535] inclusive; otherwise the string length is two, and a TypeError will be raised.

# chr(i)
Return a string of one character whose ASCII code is the integer i. For example, chr(97) returns the string 'a'. This is the inverse of ord(). The argument must be in the range [0..255], inclusive; ValueError will be raised if i is outside that range. See also unichr().

# unichr(i)
Return the Unicode string of one character whose Unicode code is the integer i. For example, unichr(97) returns the string u'a'. This is the inverse of ord() for Unicode strings. The valid range for the argument depends how Python was configured – it may be either UCS2 [0..0xFFFF] or UCS4 [0..0x10FFFF]. ValueError is raised otherwise. For ASCII and 8-bit strings see chr().

# unicode(object='')¶
unicode(object[, encoding[, errors]])
Return the Unicode string version of object using one of the following modes:

If encoding and/or errors are given, unicode() will decode the object which can either be an 8-bit string or a character buffer using the codec for encoding. The encoding parameter is a string giving the name of an encoding; if the encoding is not known, LookupError is raised. Error handling is done according to errors; this specifies the treatment of characters which are invalid in the input encoding. If errors is 'strict' (the default), a ValueError is raised on errors, while a value of 'ignore' causes errors to be silently ignored, and a value of 'replace' causes the official Unicode replacement character, U+FFFD, to be used to replace input characters which cannot be decoded. See also the codecs module.

If no optional parameters are given, unicode() will mimic the behaviour of str() except that it returns Unicode strings instead of 8-bit strings. More precisely, if object is a Unicode string or subclass it will return that Unicode string without any additional decoding applied.

For objects which provide a __unicode__() method, it will call this method without arguments to create a Unicode string. For all other objects, the 8-bit string version or representation is requested and then converted to a Unicode string using the codec for the default encoding in 'strict' mode.

For more information on Unicode strings see Sequence Types — str, unicode, list, tuple, bytearray, buffer, xrange which describes sequence functionality (Unicode strings are sequences), and also the string-specific methods described in the String Methods section. To output formatted strings use template strings or the % operator described in the String Formatting Operations section. In addition see the String Services section. See also str().




# 1.2 Check Permutation: Given two strings, write a method to decide if one is a permutation of the other.


# Approch 1: keep counts of each character in each string and tally their count

<br>
Running Time : O(n) n is length of string, when lengths are not equal we directly return False<br>
Space Complexity : O(n)


```python
from collections import Counter
def checkPermutation(s1,s2):
    return Counter(s1) == Counter(s2)

print (checkPermutation("abhinaya","aaanbhiy"),checkPermutation("abhinaya","Daanbhiy"))

def checkPermutation(s1,s2):
    #without using counter
    if len(s1) != len(s2):
        return False
    s1Dict = dict()
    for char1 in (s1):
        if char1 in s1Dict: s1Dict[char1] += 1
        else: s1Dict[char1] = 1
    for char in s2:
        if char not in s1Dict: return False
        s1Dict[char] -= 1
        if s1Dict[char] == 0: s1Dict.pop(char)
    return len(s1Dict) == 0

print (checkPermutation("abhinaya","aaanbhiy"),checkPermutation("abhinaya","Daanbhiy"))

```

    True False
    True False


# Approch 2 : Sort it and iterate through each character 
<b>sorted()</b> returns a <b>new sorted list</b>, leaving the original list unaffected. <b>uses Merge Sort</b><br>
<b>list.sort()</b> sorts the list <b>in-place</b>, mutating the list indices, and returns None (like all in-place operations). <b>Uses Heap Sort</b><br>

sorted() works on any iterable, not just lists. Strings, tuples, dictionaries (you'll get the keys), generators, etc., returning a list containing all elements, sorted.

Use list.sort() when you want to mutate the list, sorted() when you want a new sorted object back. Use sorted() when you want to sort something that is an iterable, not a list yet.

For lists, list.sort() is faster than sorted() because it doesn't have to create a copy. For any other iterable, you have no choice.

you cannot retrieve the original positions. Once you called list.sort() the original order is gone.

Running Time : O(n*log(n))
Space Complexity : O(n)

a = list("sssaaaww") <br>
sorted(a) <br>
print(a) <br>
a.sort() <br>
print(a) <br>

Output:<br>
['s', 's', 's', 'a', 'a', 'a', 'w', 'w'] <br>
['a', 'a', 'a', 's', 's', 's', 'w', 'w'] <br>


```python
def checkPermutation(s1,s2):
    if len(s1) != len(s2):
        return False
    s1 = list(s1)
    s1.sort()
    s2 = list(s2)
    s2.sort()
    for index in range(len(s1)):
        if s1[index] != s2[index]:
            return False
    return True

print (checkPermutation("abhinaya","aaanbhiy"),checkPermutation("abhinaya","Daanbhiy"))
```

    True False


# 1.3 URLify : Write a method to replace all spaces in a string with '%20'. You may assume that the string has sufficient space at the end to hold the additional characters, and that you are given the "true" length of the string.

# Note: Strings are  Immutable in Java and Python

# Approach:
As Strings are immutable in python, we break that into a list of characters, and then we count the actual spaces within our string (not the one that exists at the end). and the starting from the last, as there are enough spaces at the end, even if we replace with the characters, we wont loose any sensible information. if we find any space between words, we copy %20 else character as it is.<br>
RunningTime : O(n)<br>
Space Complexity : O(n) but in-place modification. counting the space used for the list in the beginning of the sentence


```python
def URLify(s,trueLength):
    s = list(s)
    flag = False
    trueSpaceCount = 0
    for char in range(0,trueLength):
        if s[char] == " ":
            trueSpaceCount += 1
    finalLength = trueLength + 2*trueSpaceCount #each space is replaced by %20 in final string
    #addition of 2 more characters for each space
    for char in range(trueLength-1,-1,-1):
        if s[char] != " ":
            s[finalLength-1] = s[char]
            finalLength -= 1
        else:
            s[finalLength-3:finalLength] = ["%","2","0"]
            finalLength -= 3
    return "".join(s)
URLify("Mr John Smith    ",13)
```




    'Mr%20John%20Smith'



# 1.4 Palindrome Permutation: Given a string, write a function to check if it is a permutation of a palindrome. A palindrome is a word or phrase that is the same forwards and backwards. A permutation is a rea rrangement of letters. The palindrome does not need to be limited to just dictionary words.

Approach : Counting the frequency of each characters appeared, and checking the total number of characters with odd count and checking if they are > 1 or not. spaces won't be considered while counting odd counts
Running Time : O(n) <br>
Space Complexity : O(n)

Alternatively: we can check the Odd count's in the first for loop itself and reduce the second for loop, but we may check the condition of even|odd for each character. it may or may not lead to a faster algorithm.

Alternatively we can even go with the bit vector method, like we did it for 1.1 instead of using a hashmap. and still the running time is the same.


```python
def PalindromePermutation(s):
    charCounts = dict()
    for char in s:
        if char.lower() in charCounts: 
            charCounts[char.lower()] += 1
        else:
            charCounts[char.lower()] = 1
    countOdds = 0
    for each in charCounts:
        if each != " " and charCounts[each]%2 != 0:
            if countOdds == 1: #this is where you encounter one more odd count character
                return False
            countOdds += 1
    return True
PalindromePermutation("Taco Coa")

```




    True



# 1.5 One Away: There are three types of edits that can be performed on strings: insert a character, remove a character, or replace a character. Given two strings, write a function to check if they are one edit (or zero edits) away.

Approach:<br> As it is mentioned, there can be atmost one edit that can be made to make it right. So that means their difference in lengths should be atmost 1.this is what we intitially.<br>And then, we should be knowing which one is the shorter string, as we may endup accessing an element which is out of bounds when there is some Remove type edit. and we might need to know which index has to updated when an element is to inserted.<br>
Generally, Insertion is done into smaller String and Deletion in larger String and replacement when their lengths are equal.<br>
Running Time: O(n)<br>
Space Complexity: O(1)<br>


```python
def oneAway(s1,s2):
    if abs(len(s1)-len(s2)) > 1:
        return False
    if len(s1) > len(s2):   
        smallS,bigS = s2,s1
    else:
        smallS,bigS = s1,s2
    sIndex = 0
    bIndex = 0
    firstError = False
    while sIndex < len(smallS):
        if smallS[sIndex] == bigS[bIndex]:
            sIndex += 1
            bIndex += 1
        elif firstError == False:
            firstError = True
            if len(s1) == len(s2):
                sIndex += 1
                bIndex += 1
            else:
                bIndex += 1
        else:
            return False
    return True
oneAway("abcdcd","abc")
```




    False



# Remember This :). Address Things :p


```python
one = [1,2,3,4,3,3]
two = one
one.sort()
print(one,two)

one = [1,2,3,4,3,3]
two = one
one = sorted(one)
print(one,two)
```

    [1, 2, 3, 3, 3, 4] [1, 2, 3, 3, 3, 4]
    [1, 2, 3, 3, 3, 4] [1, 2, 3, 4, 3, 3]


# 1.6 String Compression: Implement a method to perform basic string compression using the counts of repeated characters. For example, the string aabcccccaaa would become a2b1c5a3. If the "compressed" string would not become smaller than the original string, your method should return the original string. You can assume the string has only uppercase and lowercase letters (a - z).

Approach: whenever the adjacent characters are different or the char is the last string, append the character and its count till now.<br>
Running Time: O(n); remember list.append is O(1) in worst case and concatination is O(n) in worst case as we endup creating a new string instead of modifying like we do it for append.<br>
Space Complexity: O(n) ; worst case, if all the characters are distinct, we may endup copying the same string and and 1 after each character.<br>


```python
def stringCompression(s):
    newList = []
    count = 1
    index = 0
    while index < (len(s)):
        if index == (len(s) - 1) or s[index] != s[index+1]:
            newList.append(s[index]) #append is O(1) time
            newList.append(str(count))
            count = 1
            index += 1
        else:
            count += 1
            index += 1
    if len(newList) == len(s): return s
    else : return "".join(newList)
stringCompression("aabcccccaaa")
```




    'a2b1c5a3'



# 1.7 Rotate Matrix: Given an image represented by an NxN matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees. Can you do this in place?

Approach: Assuming that the rotation is clock wise. as once we copy our first row to last column,last column to last row, last row to first column as is, we copy n elements in this loop. once we are done with that, when we go to second row, we can see that first and last elements of this row is already rotated, so we start from index 1 and end by index n-2... 2 n-3  and so on. if you can see, we continue this till the middle element is rotated.
and copying elements like we do the swapping to save space.<br>
Running Time: O(n2)<br>
Space Complexity:O(1) <br>


```python
def rotateMatrix(matrix):
    if len(matrix) == 0: return []
    if len(matrix) != len(matrix[0]): return matrix
    n = len(matrix)
    for row in range(int((n)/2)):
        startIndex = row
        endIndex = n - 1 - startIndex
        for index in range(startIndex,endIndex):
            #saving top
            temp = matrix[row][index]
            #copying left to top
            matrix[row][index] = matrix[endIndex - index][startIndex]
            #copying bottom to left
            matrix[endIndex - index][startIndex] = matrix[endIndex][endIndex - index]
            #copying right to bottom
            matrix[endIndex][endIndex - index] = matrix[startIndex + index][endIndex]
            #copying top to right
            matrix[startIndex + index][endIndex] = temp           
    return matrix
matrix1 = [[1,2,3,4],[3,4,5,5],[2,4,5,4],[3,4,5,4]] 
#even when each interger is replaced by a 2x2 matrix, code looks the same
print (rotateMatrix(matrix1) )
matrix2 = [[1,2],[3,4]]
print(rotateMatrix(matrix2) )  
```

    [[3, 2, 3, 1], [4, 4, 5, 2], [5, 5, 4, 3], [4, 4, 5, 4]]
    [[3, 1], [4, 2]]


# 1.8 Zero Matrix: Write an algorithm such that if an element in an MxN matrix is 0, its entire row and column are set to 0.

Approach: As we need to replace all the columns and rows to 0's if there exist a 0 in that row or column. and as we trying to do in-place replacement to optimize memory, without keeping track of all the rows, columns which has 0's in it, will lead to wrong answers as we may insert 0's in a row where there are no prior 0's and we added 0 to it because, there is a 0 in that particular column. so we keep track of all rows, columns with 0s and then substitute them with 0's as stated.
Running Time: O(n2)<br>
Space Complexity:O(n) <br>


```python
def zeroMatrix(matrix):
    if len(matrix) == 0:
        return matrix
    n = len(matrix)
    m = len(matrix[0])
    firstRow = set()
    firstColumn = set()
    for r in range(n):
        for c in range(m):
            if matrix[r][c] == 0:
                firstRow.add(r)
                firstColumn.add(c)
    for row in firstRow:
        matrix[row] = [0]*m
    for column in firstColumn:
        for row in range(n):
            matrix[row][column] = 0
    return matrix
matrix = [[3,4,5],[1,0,3],[3,4,5]]
zeroMatrix(matrix)
```




    [[3, 0, 5], [0, 0, 0], [3, 0, 5]]



# String Rotation: Assume you have a method isSubstring which checks if one word is a substring of another. Given two strings, S1 and S2, write code to check if S2 is a rotation of S1 using only one call to isSubstring (e.g., "waterbottle" is a rotation of"erbottlewat").

Approach: when you add a string to itself, all the rotations of the original string will be a substring to that mdofied version of the string. check it :) <br>
Running Time : O(n) <br>
Space Complexity : O(n) <br>


```python
def stringRotation(s1,s2): #check s2 is rotation of s1
    if len(s1) > 0 and len(s1) != len(s2):
        return False
    s2 = s2 + s2
    return (s2.find(s2) != -1)
stringRotation("waterbottle","terbottlewa")
```




    True


