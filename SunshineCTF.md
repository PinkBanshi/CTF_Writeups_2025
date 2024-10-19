# Overview

Team: Null Terminator
My teammates completed other challenges (and gave help to these), but here are my contributions to the SunshineCTF 2024.

## Reversing: Twine

0) Assess the problem
All we are given is a file called challenge.

1) Gain Information 
- `file challenge` shows that challenge is an ELF file
- `strings challenge` revealed the flag

TIP: By starting with strings prior to running the program, you can get an idea of what to expect during the run. And in this case I was just handed the flag lol.

HIDDEN:
flag{youd_be_better_off_reading_the_source_code_directly_but_i_guess_this_is_fine}


## Crypto: Adventure Cipher

0) Assess the problem
We are given a text file with several words that repeat in a seemingly random pattern. We also know that our alphabet is "[a-z]_{}<space>".

1) Frequency Analysis
- From the repetition of words and the inclusion of an alphabet, we can assume it is a word to letter substitution cypher. 
- Since many words have the same starting letter, we decided to use a word frequency analyzer (wordfrequency.org)[https://wordfrequency.org/]
- The results of this analysis are saved as a PDF in this directory
- Matching these to the avaerge frequencies of letters we have an idea for initial replacements. (Wikipedia: Frequency Analysis)[https://en.wikipedia.org/wiki/Frequency_analysis]

2) Initial Substitutions
- We start by removing spaces since we know a word maps to the value of <space>. 
- Now we can match the most frequent word to <space>, and follow down the list of replacements.
- We ended with 2 words with only one appearnce and replaced those with the braces. `{...}`
- We assume underscores are mainly found within the flag.
- All words are now letters, spaces, braces, or underscores.
- Unfortunately this didn't make a legible letter.

TIP: Substitute the first pass as all capital letters. When you confirm a substitution change them to the lower case answer.

3) Search for the flag
- We know flags take the form of `sun{}` and given the presence of underscores we can assume most would be withint the flag.
- This implies that the three letters before the braces equal `s`, `u`, and `n` respectively.
- We change these three letters to `sun` lowcase.
- The flag is still illegible, but we can easily see other places `sun` is within a word like "sunsHInE".
- This confirms new letters. 

4) Fixing letters one at a time
- Assume one letter words are "i" or "a"
- Try to get common small words to be correct: "the", "is", "was", "to", "on", etc.
- At this point we have a semi-legible flag, so we focused on the remaining unknown (capitalized) flag letters.

5) Profit
HIDDEN:

## Reversing: Build a Flag Workshop

NOTE: I did not finish this but I got pretty close

0) Assess the problem
All we are given is a file called build-a-workshop.

1) Gain info
- `file challenge` shows that challenge is an ELF file
- `strings challenge` shows a bunch of quotes, "sun{%s} %s", and some strings about "chompy"

2) Run the program
- `chmod +x build_a_flag` to add execution to the file
- `./build_a_flag` to run the program

It starts by giving the flag "live long and prosper" and some options to change like length, genre, vibe, and
