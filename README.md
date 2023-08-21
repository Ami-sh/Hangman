# Hangman
Problem Statement:
For this coding test, your mission is to write an algorithm that plays the game of Hangman through our API server.
When a user plays Hangman, the server first selects a secret word at random from a list. The server then returns a row of underscores (space separated)—one for each letter in the secret word—and asks the user to guess a letter. If the user guesses a letter that is in the word, the word is redisplayed with all instances of that letter shown in the correct positions, along with any letters correctly guessed on previous turns. If the letter does not appear in the word, the user is charged with an incorrect guess. The user keeps guessing letters until either (1) the user has correctly guessed all the letters in the word or (2) the user has made six incorrect guesses.
You are required to write a "guess" function that takes current word (with underscores) as input and returns a guess letter. You will use the API codes below to play 1,000 Hangman games. You have the opportunity to practice before you want to start recording your game results.
Your algorithm is permitted to use a training set of approximately 250,000 dictionary words. Your algorithm will be tested on an entirely disjoint set of 250,000 dictionary words. Please note that this means the words that you will ultimately be tested on do NOT appear in the dictionary that you are given. You are not permitted to use any dictionary other than the training dictionary we provided. This requirement will be strictly enforced by code review.
You are provided with a basic, working algorithm. This algorithm will match the provided masked string (e.g. a _ _ l e) to all possible words in the dictionary, tabulate the frequency of letters appearing in these possible words, and then guess the letter with the highest frequency of appearence that has not already been guessed. If there are no remaining words that match then it will default back to the character frequency distribution of the entire dictionary.
This benchmark strategy is successful approximately 18% of the time. Your task is to design an algorithm that significantly outperforms this benchmark.


Hangman solver:
Initial Note:
When I initially set out to tackle the hangman game, my first thought was to employ unsupervised learning. However, after encountering several unsuccessful attempts, I shifted my focus to devising a custom algorithm that bears resemblance to the k-nearest neighbors (KNN) approach. Due to time constraints, I found it challenging to derive a mathematical solution and instead opted for a trial-and-error approach.

Algorithm Overview:
The algorithm I developed is composed of several distinct components:

1. **Initial Guessing**: The first step involves generating an initial guess based on the word's length. This initial guess is hardcoded to follow a specific pattern, leveraging the frequencies of letters that appear in words. For instance, the letter "p" in both "apple" and "pen" would have a value of 1.

2. **Progressive Guessing**: As correct letters are identified, the process of guessing becomes more manageable. It becomes apparent that guessing longer words is often easier, as they contain a greater amount of discernible information.

. **Binary Similarity Index Comparison**: Once we have a partially revealed word, such as "_ _ e _ o n i a," the algorithm constructs various templates for binary comparison. These templates come in four distinct types:
   - "a___t": letter-blanks-letter
   - "an_it": letters-blank-letters
   - "_abc/abs_": blank-letters/letters-blank (prefix/suffix)
   - "_klw_": blank-letters-blank

   By substituting blanks with dots ("."), regular expressions are employed for pattern matching across all four template types. When a match is found with a word from the given dictionary, the algorithm keeps track of letter counts in the place of blank spaces. This process generates four dictionaries, each assigning a value to different alphabet letters.

3. **Normalization and Final Selection**: The four dictionaries are then normalized and combined to create a final dictionary using coefficients: c1 * d1 + c2 * d2 + c3 * d3 + c4 * d4. The letter with the highest value in the final dictionary is selected as the next guess, excluding letters that have already been guessed. Coefficient values c1, c2, c3, and c4 are determined based on word length:
coeff=[0.5559713631737067,0.4434516986858717,0.6508442576413604,0.42256878336138065] as the values 

The accuracy ranges from 48% to 55% with an average of 51%. I didn’t want to take the risk so I used the optuna library to optimize these parameters. Optuna internally uses a Bayesian optimization method to optimize the parameters. 



Closing Note:
Before arriving at this approach, I explored various string similarity metrics available in Python libraries, but I struggled to consistently achieve the desired accuracy of over 50%. As a result, I developed this custom metric to address the challenge. It's important to note that while my code may not be optimized for factors like execution time, organization, or coding standards, my primary focus has been on maximizing accuracy within the constraints of my available time and resources.



