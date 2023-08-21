# Hangman
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
   - coeff = [2, 5, 1, 1] for words longer than 8 characters
   - coeff = [2, 5.5, 1, 1] for shorter words

This much was enough to get 50%+ accuracy. But on testing it had a deviation of 2-3%. The accuracy ranged from 48% to 55%. I didn’t want to take the risk so I used the optuna library to optimize these parameters.
 Optuna internally uses a Bayesian optimization method to optimize the parameters. And then I got coeff=[0.5559713631737067,0.4434516986858717,0.6508442576413604,0.42256878336138065] as the values 
Which resulted in 54% average accuracy. Mean 54% being the mean and a deviation of 2-3% on each run,i.e, accuracy could range from 50-58%.



Closing Note:
Before arriving at this approach, I explored various string similarity metrics available in Python libraries, but I struggled to consistently achieve the desired accuracy of over 50%. As a result, I developed this custom metric to address the challenge. It's important to note that while my code may not be optimized for factors like execution time, organization, or coding standards, my primary focus has been on maximizing accuracy within the constraints of my available time and resources.


