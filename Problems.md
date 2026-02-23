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

## Top K Frequent Elements (https://neetcode.io/problems/top-k-elements-in-list/question)


How I solved it:
I created a 2d array as size of the input array + 1. This will be my frequency array, the index will represent the frequency. Why? because I know for sure that the frequency of any number cannot exceed the size of the array.
Each element of this array will carry the number of whose frequency is same as its index (there can be multiple numbers hence created array of array)
Then I will simply create a map of the frequency (using unordered_map: key as the number, val as the frequency).
Then I will iterate through this map and start filling our 2d array (will place the number to the exact indexes)
Then, I will iterate the 2d array from the far right (max index for max frequency), there I will simply iterate through each index to check if there are numbers or not until I found the exact K numbers and append them in the answer.

## # Encode and Decode Strings (https://neetcode.io/problems/string-encode-and-decode/question)

What I did:
I made a string, by concatinating each individual string by adding "{size}#" 
`#` because I need to maintain the more than 1 digit size string also.
Then in the decoder, I simply started from the start, defined `activeSize` initially to be 0, then initially started until I find the next `#` after `i` position, as soon as I find, that index will be my end of the size number, so the size will be in this range: `that index - intial index`. Then, I converted this range into `int`, then iterated for this much size while extracting charecters to make the string.