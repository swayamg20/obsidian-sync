[[2026-02-21]]
- [x] Contains Duplicate 
- [x] Valid Anagram 
- [x] Two Sum 

maps: [[maps]] in group anagram


## group anagram (https://neetcode.io/problems/anagram-groups/question)
I get an array of string, I need to return after grouping the anagrams.
**First solution:** 
I first made a copy of the main array into `cp`, then sorted the whole array's each elements(string). then, created an unordered hashmap. Added each of the strings as key and the value as the array of indices of correspnoding appearance of each string in the main string.
Then finally made the final answer array but iterating through it and making arrays of all the same anagrams.

**More optimized solution:**
I read somewhere that I can infact create a hash key of each string according to the alphabetical order appearance of each string. Then, make a hashmap of it with key as this hashkey and value as the array of those strings matching. then simply return all these values (arrays).



