SLIDE 1

Problem Breakdown:
Problem Statement You are given a list that can contain positive and negative integers. You need to find two elements such that their sum is closest to zero.
For example, you are given a list [-4, 7, 6, 2, -5]. The two elements with the sum closest to zero will be -5 and 6.

Input: A list of integers, which may contain both positive and negative numbers.
Output: A pair of numbers whose sum is closest to zero.

Sample Test Cases:
Test case 1:
Input: [-4, 7, 6, 2, -5]
Output: (-5, 6)

Test case 2:
Input: [-50, 34, -19, 24, 33, 10, -46, -38]
Output: (-38, 34)
___________________________________________________________________________________________________
![image](https://github.com/user-attachments/assets/92c557b2-29cf-4867-8c9d-45ca9ef69c91)
___________________________________________________________________________________________________
###################################################################################################

SLIDE 2

Approach:
Brute Force: We will check every possible pair of elements in the list and compute their sum.
Track the Closest Sum: As we compute each sum, we compare it to the smallest absolute sum we’ve encountered and keep track of the pair that gives the closest sum to zero.
Edge Cases: We assume that the list has at least two elements, as mentioned in the problem statement.
___________________________________________________________________________________________________
![image](https://github.com/user-attachments/assets/6d56a187-5f0e-407a-a777-913d37050780)
___________________________________________________________________________________________________

Steps:
Loop through all pairs of elements.
For each pair, compute their sum.
Keep track of the pair whose sum has the smallest absolute value.
___________________________________________________________________________________________________
![image](https://github.com/user-attachments/assets/f732bf5e-3291-4263-8016-be15f3e38566)
___________________________________________________________________________________________________

###################################################################################################
SLIDE 3

Explanation:

Nested Loops:
The outer loop goes through each element in the list, and the inner loop compares it with every subsequent element to form a pair. Track Closest Pair: We calculate the sum of the pair, and if it's closer to zero than any previous pair, we update closest_pair and closest_sum. Return Result: After checking all pairs, we return the pair whose sum is closest to zero.

Time Complexity:
The time complexity is O(n²) because we are checking every pair in the list.

Space Complexity:
The space complexity is O(1), as we are only using a couple of extra variables (closet_pair , closest_sum), which don't depend on the size of the input.
___________________________________________________________________________________________________
![image](https://github.com/user-attachments/assets/1b7bc2a8-addb-4231-bfdf-fc36bfc51402)
___________________________________________________________________________________________________
###################################################################################################
PYTHON IMPLEMENTATION


![image](https://github.com/user-attachments/assets/d5216065-2d93-44db-8129-fc2a21752562)


![image](https://github.com/user-attachments/assets/146ae98b-ab50-45f0-8411-c82f24122111)


____________________________________________________________________________________________________
![image](https://github.com/user-attachments/assets/0216579f-ac4c-4ee7-ae3b-60cf71864dad)


