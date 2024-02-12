# Teaching Tutorial
### This is a replication of the project "Tie-breaker: Using language models to quantify gender bias in sports journalism"
Our language model inference code is based on the original code from kenlm (https://github.com/kpu/kenlm)

### Prerequisites
1. Installing KenLM and background setting
```bash
! pip install https://github.com/kpu/kenlm/archive/master.zip
! sudo apt -y install build-essential cmake libboost-system-dev libboost-thread-dev libboost-program-options-dev libboost-test-dev libeigen3-dev zlib1g-dev libbz2-dev liblzma-dev
! wget -O - https://kheafield.com/code/kenlm.tar.gz | tar xz
! mkdir kenlm/build && cd kenlm/build && cmake .. && make -j2
! ls kenlm/build/bin
```
At this step, you should have the kenlm installed and the kenlm/bin folder should be in your virtual path on google colab. Here is the example of the path: /content/kenlm/build/bin, this path is for later use in the code.

## 2. Read the data for perplexity calculation

```bash
with open('sent_train.txt', 'w') as f:
    for i in pd_text_commentaries['commentary']:
        doc = nlp(i)
        for sent in doc.sents:
            f.write(sent.text.lower() + '\n')
```
* `spacy` is used to split the commentary into sentences. The sentences are then written into a text file. This text file is used to calculate the perplexity of the language model.



### Running the perplexity calaultion

The perplexity for a sequence of words \( w_1w_2...w_N \) can be calculated using the following formula:

\[
\text{Perplexity}(w_1w_2...w_N) = \sqrt[N]{\frac{1}{P(w_1w_2...w_N)}}
\]

Where:
- \( w_1w_2...w_N \) represents a sequence of N words.
- \( P(w_1w_2...w_N) \) is the probability assigned by the language model to the sequence of words.
- \( \sqrt[N]{\cdot} \) denotes taking the Nth root.


```bash
model = kenlm.Model('train.binary')
model.score('this is a sentence.')
model.score('game')
```
* `model.score` is basically a way of 'grading' how well a language prediction model works.
* `bos = True` means that the string you are scoring is treated as the beginning of a sentence. This can affect the score because the language model has different expectations for what words are likely to appear at the start of a sentence.
* `eos = True` means that the string you are scoring is treated as if it should end a sentence, which can also affect its probability.

| perplexity | question    | score    |
| :---:   | :---: | :---: |
| Low |  What about your serve, Rafa?  | 6007.399629655489   |
| Low |  The tiebreak, was that the key to the match?  | 502.87260511339133 |
| High |  Who designed your clothes today?  |  17948.953341974695 |
| High |  Do you normally watch horror films to relax?  |  12527.278528622757 |



## 3. Questions Evaluations in different experiments

### Atypicality Score Calculation

Given a question \( q \) containing a set of unique words \( \{w_1, w_2, ..., w_N\} \), excluding stop words and entity names:

\[ Sc(q) = \frac{1}{N} \sum_{i=1}^{N} IDF(w_i) \]

Where:
- \( N \) is the number of unique words in the question.
- \( IDF(w_i) \) is the inverse document frequency of word \( w_i \).



**a) Gender Bias in interview questions**

| Feature | Mann-Whitney U Statistic  | P-Value    |
| :---:   | :---: | :---: |
| Gender |  827480822.5  | 0.01766126180457031   |
| Gender with at least 10 questions|  3.5  | 0.8221867672380183 |


**b) Question Type Bias in interview questions**

| Type | Question | atypicality_score  |
| :---:   | :---: | :---: |
| Typical |  Have you played each other before?  | 1.59648  |
| Atypical|  Are you a vodka drinker?  | 6.580595 |

![](/workspaces/Fu_Tiebreaker_Replication/result_images/question_type_plot.png)

**c) Gender Bias in both types of questions**

| Feature | Mann-Whitney U test p-value |
| :---:   | :---: | 
| Atypical| 3.114719928349132e-05 |
| Typical | 0.5979210176093868 |

**c) Visualization of the results**
![](/workspaces/Fu_Tiebreaker_Replication/result_images/question_type_plot.png)



### Top10 Players Score Calculation
| Feature | Mann-Whitney U test p-value |
| :---:   | :---: | 
| Top10| 0.7062678440155711 |
| Rest | 0.008823000524376021 |

**a) Visualization of the results**
![](/workspaces/Fu_Tiebreaker_Replication/result_images/top10_plot.png)

### Winning vs. Losing Score Calculation
**a) Visualization of the results**
![](/workspaces/Fu_Tiebreaker_Replication/result_images/win_lose_plot.png)

