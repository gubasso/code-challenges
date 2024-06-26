# code-challenges

### Overview of the SM-2 Algorithm

The SM-2 algorithm adjusts the interval between reviews based on how well you remember the information. It uses a scoring system to assess your recall and calculate the next review date. The core idea is to increase the intervals for well-remembered items and decrease them for poorly remembered items, thus maximizing learning efficiency.

#### Key Concepts

1. **Rating Scale** : Each time you review a flashcard, you rate your recall on a scale from 0 to 5:
  - **0** : Complete blackout.
  - **1** : Incorrect response, even after a hint.
  - **2** : Incorrect response, but correct after a hint.
  - **3** : Correct response, but with serious difficulty.
  - **4** : Correct response, but with hesitation.
  - **5** : Perfect response.

2. **Initial Interval** : The first interval for a new flashcard is typically set to 1 day.

3. **Repetition Number (n)** : This indicates how many times a flashcard has been successfully recalled.

4. **E-Factor (E)** : This is the easiness factor, which determines how easily you recall a flashcard. It starts at 2.5 and is adjusted based on your recall rating.

#### Steps of the SM-2 Algorithm

1. **Initial Review** :
  - When a flashcard is reviewed for the first time, it is given an initial interval (usually 1 day).

2. **Rating the Recall** :
  - After reviewing the flashcard, you rate your recall using the 0-5 scale.

3. **Adjusting the E-Factor** :
  - Based on the recall rating, the E-Factor is adjusted as follows:
  ```css
  E' = E + (0.1 - (5 - q) * (0.08 + (5 - q) * 0.02))
  ```
  where `q` is the recall rating (0-5).

  - The E-Factor should not fall below 1.3, so if the calculated E' is less than 1.3, it is set to 1.3.

4. **Calculating the Next Interval** :
  - If the recall rating is 3 or higher, the interval for the next review is calculated based on the current interval (I) and the E-Factor (E):
    - For the first successful review (`n = 1`), the next interval is 1 day.
    - For the second successful review (`n = 2`), the next interval is 6 days.
    - For subsequent successful reviews (`n > 2`), the next interval is calculated as:

```scss
I_n = I_(n-1) * E
```

  - If the recall rating is less than 3, the flashcard is treated as if it is being reviewed for the first time (interval is reset to 1 day, and the repetition number is reset).

5. **Updating the Flashcard** :
  - The repetition number (n) is incremented if the recall rating is 3 or higher.
  - The new interval (I) and the updated E-Factor (E) are stored for the flashcard.

#### Example

1. **First Review** :
  - Interval: 1 day
  - E-Factor: 2.5
  - Rating: 4
  - Next Interval: 1 day (initial review always 1 day)
  - Updated E-Factor: 2.5 + (0.1 - (5 - 4) * (0.08 + (5 - 4) * 0.02)) = 2.5 + 0.1 - 0.08 - 0.02 = 2.5

2. **Second Review**  (after 1 day):
  - Interval: 1 day
  - E-Factor: 2.5
  - Rating: 5
  - Next Interval: 6 days (for the second successful review)
  - Updated E-Factor: 2.5 + (0.1 - (5 - 5) * (0.08 + (5 - 5) * 0.02)) = 2.5 + 0.1 = 2.6

3. **Third Review**  (after 6 days):
  - Interval: 6 days
  - E-Factor: 2.6
  - Rating: 4
  - Next Interval: 6 * 2.6 = 15.6 days
  - Updated E-Factor: 2.6 + (0.1 - (5 - 4) * (0.08 + (5 - 4) * 0.02)) = 2.6 + 0.1 - 0.08 - 0.02 = 2.6

#### Summary

The SM-2 algorithm dynamically adjusts review intervals based on recall performance, ensuring that you review flashcards just before you are likely to forget them. This method helps to optimize learning and retention by spacing out reviews more effectively over time.

