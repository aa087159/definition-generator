# A linguistic approach to training data for dictionary definition generation 
Several projects were introduced these past few years regarding definition modeling. They use distributed vector representations to 
generate dictionary definitions. Most of these projects, however, don't give importance to the point of view of linguistics, which means
that the training data they feed into the model mostly has low linguistic quality which may lead to bad performance. In this project, 
linguistic perspectives will be taken to prepare training data. After having produced dictionary definitions, instead of BLEU score which
is commonly used in computation assessment, questionnaire will be utilized to evaluate the results.

## Training data
In order to train the introduced language models, a few set of data needs to be prepared. The linguistic data includes **`headwords, 
definitions, and contexts`**, while the non-linguistic data such as vectors and word-embedding can be taken care of by the introduced 
models (such as https://github.com/agadetsky/pytorch-definitions) and Google Word2Vec.

## From linguistic perspectives

#### Defining vocabulary
For a learner's dictionary, it is debated that words used in their definitions should be simpler and more comprehensible than the entry
word. LDOCE first introduced controlled defining vocabulary which consists of about 3000 words, and they claim that all the words they 
use in their definitions are within the defining vocabulary, and they only use central meanings of these words to define head words. 
After this evolutionary move, many dictionaries follow their step as well. However, critiques point out that some definitions in LDOCE 
use words outside of defining vocabulary, and they also use a lot of complex words (since defining vocabulary includes affixes and 
morphemes combination is allowed) which may be difficult for learners to understand. For the training data, I will **create my own 
defining vocabulary with relatively strict diciplines.**

To build a defining vocabulary I need a corpus with a lot of non-biased texts, and thus the best solution is to use dictionary 
definitions according to Dr. Neubauer in his article *how to define a defining vocabulary*. I decided to use WordNet as my first attempt 
since it is open source and has function calls to collect words' synonyms, antonyms, definitions, synsets, etc. The steps I take to 
create a defining vocabulary is the following:

1. collect head words (I scraped Oxford ALD entries for temporary use)
2. use Wordnet NLTK package to get all definitions of synsets of these head words
3. put the collected definitions in Sketch Engine to get a frequency list (this may be done with some lines of codes but Sketch Engine 
gives you convenient tools)
4. filter out words that have high similarity. For example, the central meaning of the words *abandon* and *desert* is pretty much the
same, so one of them can be redundant in the list. Here WordNet provides a similarity function to account for the data collection for 
this step.
5. split the filtered data into two sections which are basic- and difficult words referencing pedagogical views. (for later use when 
creating definitions in training data)

#### Definition format for training data
Definition format differs in different POS. 

1. defining verbs

   COBUILD FSD is good with defining intransitive verbs, reflexive verbs, and transitive verbs in passive. FSD (full sentence definition)
means to include the head word in the definition, and their common structure is `[typical context the headword has + defining phrase]`.
For example, at the entry `seat` in the POS verb:

      `COBUILD`--If you seat yourself somewhere, you sit down.

      `Oxford ALD`--to give somebody a place to sit; to sit down in a place.
      
   It's apparent that the COBUILD one is the more succussful one because it instructs the users of its common verb structure.

2. defining nouns
   traditional definition format takes care of noun definitions pretty good.
     * hypernym(superordinate concept) + distinctive features. For example at the entry horse:
     
        *a solid-hoofed **mammal** with a flowing **mane** and tail...* (this is where the difficult words in defining vocabulary comes
        in handy, a definition allows one to two dificult words in order not to break the definition chain. Here the difficult words are         highlighted)



