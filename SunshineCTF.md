# Overview

Team: Null Terminator
My teammates completed other challenges (and gave help to these), but here are my contributions to the SunshineCTF 2024.

## Reversing: Twine

0) Assess the problem
   
All we are given is a file called challenge.

1) Gain Information 
- `file challenge` shows that challenge is an ELF file
- `strings challenge` revealed the flag

By starting with strings prior to running the program, I get an idea of what to expect during the run. In this case I was just handed the flag lol.

<details>
  <summary>Flag</summary>

  - flag{youd_be_better_off_reading_the_source_code_directly_but_i_guess_this_is_fine}

</details>


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

<details>
  <summary>Profit</summary>

  - sun{the_almighty_alaric_and_blaze}

</details>

## Reversing: Build a Flag Workshop

NOTE: I did not finish this but I got pretty close

0) Assess the problem
All we are given is a file called build_a_flag.

1) Gain info
- `file build_a_flag` shows that challenge is an ELF file
- `strings build_a_flag` shows a bunch of quotes, "sun{%s} %s", and some strings about "chompy"

2) Run the program
- `chmod +x build_a_flag` to add execution to the file
- `./build_a_flag` to run the program

It starts by giving the flag "live long and prosper" and some options to change like length, genre, vibe, and signature, as well as a [NA] option for a chompy checker. 

3) Disassemble the program
- I used Ghidra to analyze the program
- There were several function that mapped to the features you could run during execution, but one stood out as something that did not occur during run-time.

- This function contained `printf(sun{%s} %s)` which means if that line of code ran, we would have our flag.
- From the lines with `strok` you can tell there are three parts to the flag each separated by a `-`
- From running the program, the flag generated started was a quote and a signature could be added to the end, therefore we can assume the flag follows the patterm `quote-<something>-signature`.
- - This is backed up by the function
  - Part 1 check if the quote contains `decide` (strstr(part1, "decide");)
  - Part 2 compare an MD5 sum to DATA_00104010 (the data in address 104010)
  - Part 3 compares directly to `chompy`

4) Put together the flag

4a) Part 1:
 -  Check if the quote contains `decide` (strstr(part1, "decide");)
 -  `strings build_a_flag | grep decide` reveals only two entries, one of which is a quote
<details>
  <summary>Quote</summary>

  - all_we_have_to_decide_is_what_to_do_with_the_time_given_to_us

</details>

4b) Part 2: (TOOK ME WAY LONGER THAN IT SHOULD HAVE LOL)
 - Compare an MD5 sum to the data in address 00104010
 - .
 <details>
  <summary>MD5 Sum</summary>

  - `AB17850978E36AAF6A2B8808F1DED971`
    
</details> 

 - Using (Dcode)[https://www.dcode.fr/md5-hash], we uncovered an answer

<details>
  <summary>Answer</summary>

  - gandalf
    
</details>

4c) Part 3: 
 - Compare directly to chompy
 - There's nothing more to this part Yay!

   
<details>
  <summary>Flag</summary>

  - sun{all_we_have_to_decide_is_what_to_do_with_the_time_given_to_us-gandalf-chompy}

</details>

