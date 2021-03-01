# DiaLex - A Benchmark for Evaluating Multidialectal Arabic Word Embeddings

Word embeddings are a core component of modern natural language processing systems, making the ability to thoroughly evaluate them  a vital task. We describe DiaLex, a benchmark for intrinsic evaluation of dialectal Arabic word embeddings. DiaLex covers five important Arabic dialects: Algerian, Egyptian, Lebanese, Syrian, and Tunisian. Across these dialects, DiaLex provides a testbank for six syntactic and semantic relations, namely male to female, singular to dual, singular to plural, antonym, comparative, and genitive to past tense. DiaLex thus consists of a collection of word pairs representing each of the six relations in each of the five dialects. To demonstrate the utility of DiaLex, we use it to evaluate a set of existing and new Arabic word embeddings that we developed. Our benchmark, evaluation code and new word embedding models are publicly available https://github.com/dialex2020/DiaLex.
## Word Embeddings Evaluation 

Given our benchmark, we generate a test bank consisting of 260,827 tuples. 
Each tuple consists of two word pairs (a, b) and (c, d) from the same relation. 
For each of our six relations, we generate a tuple by combining two different word pairs from the same relation. 
Once tuples have been generated, they can be used as word analogy questions to evaluate different word embeddings 
as defined by Mikolov et al. (Mikolov et al., 2013a). A word analogy question for a tuple consisting of two word 
pairs (a, b) and (c, d) can be formulated as follows: ”a to b is like c to ?”. Each such question will then be answered by 
calculating a target vector t = b − a + c. We then calculate the cosine similarity between the target vector t 
and the vector representation of each word w in a given word embeddings V. Finally, we retrieve the most similar word w to t 
If w = d (i.e., the same word) then we assume that the word embeddings V has answered the question correctly. 

Moreover, we also extend the traditional word analogy task by taking into consideration if the correct answer is among 
the top K, with K in {5, 10}, closest words in the embedding space to the target vector t, which allows us to more leniently
evaluate the embeddings. This is particularly important in the case of Arabic since many forms of the same word exist, 
usually with additional prefixes or suffixes  such as the equivalent of the article "the"  or possessive determiners such
as "her", "his", or "their".  For example, consider one question which asks راجل to ست is like أمير to "?", i.e., 
"man to woman is like prince to ?", with the answer being "أميره" or "princess". Now, if we rely only on the top-1 
word and it happens to be "للأميرة" which means "for the princess" in English, the question would be considered to be
answered wrongly. To relax this, and ensure that different forms of the same word will not result in a mismatch, we use the 
top-5 and top-10 words for evaluation rather than the top-1. 

## Running Evaluations

After running jupyer notebook and opening *Evaluating_Word_Embeddings.ipynb*. The user would need to define the following constants:
* **MODEL_PATH** - Path to the pre-trained model
* **BENCHMARKS_PATH** – Path to folder containing the benchmark files, where lines are 4-tuples of words, split into sections by “: SECTION NAME” lines.
* **TOP_K** - The final aggregated report includes a top-1, top-5, and top-10 accuracy result for each section, in which the b word is searched for in the model’s top-k most similar words
* **DUMMY4UKNOWN** (bool, optional) – If True - produce zero accuracies for 4-tuples with out-of-vocabulary words. Otherwise, these tuples are skipped entirely and not used in the evaluation.

**Output**
* An xls workbook containing the accuracy spreasheet for each benchmark file included. 
* A logger file containing the gensim output of the *evaluate_word_analogies()* used.
