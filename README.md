# recap-maker

The main goal of this project is to create a python program that would automatically create a pdf file with answers to provided questions based on its database. 

The idea was to use it, in order to prepare a summary from academic courses, so that students may use the generated pdf for revising the material before exams. The program would create a pdf file with selected slides from its database, that would correspond to the list of required topics for the exam.

## how it works?

The script loads file with topics from the spicified directory. Script loads all txt files, where each line corresponds to a different topic. Each topic is filtered to remove the number of the topic which may often be present at the beginning of the line.

The materials, from which the summary will be made, are loaded from another directory. Each of those files should be a pdf split into pages. If a page is deemed suitable for the topic, it will be attached to the topic in the result file.

The pipeline is made using haystack, and utilises retriever models to select appropiate slides for the topic. 

## step by step

Firstly input files are loaded and are split into text files, each contataining text from one page of an input document. The text is lemmatised and stopwords are removed.

Then, document store is created and all the files are loaded into it. Moreover, dictionary with mappings from entry in document store to a page in pdf input document is created. 

Later, the model is loaded and script selects documents (slides) to each query (topic). The program first matches all documents with a similarity to a query that is higher than the specifed threshold. Then the query is split into several fragments to account for topics that contain a list of important things to cover. If answer to a fragment is not found in the documents associated so far, the similairity 


stopwords used from: https://github.com/bieli/stopwords/blob/master/polish.stopwords.txt

dataset mtnq comes from this paper: https://arxiv.org/abs/2305.05486
