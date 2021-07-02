# Mining-pubmed
A repository where I try different approaches from NLP to mine abstracts from pubmed, mostly to try NLP methods and elucidate links between breakfast/foods and sleep/insomnia.

## Documents

**Mining_pubmed_abstract_head_tokens.md** : find recursively head tokens of texts in order to got an intuition of how the notions like "sleep" are connected to others.
Query of the abstract based on "breakfast" and "sleep".

**Mining_pubmed_abstract.md** :  using Dependencies parsing to browse content of abstract on breakfast and sleep.

**Mining_pubmed_abstract_head_tokens_filter.md** : similar to Mining_pubmed_abstract_head_tokens, but addition filter to only parse sentence containing 
"sleep duration" and "breakfast". Looking at the unique sentences with this two constrains if probably the most efficient approach until now.

