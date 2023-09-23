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

Later, the model is loaded and script selects documents (slides) to each query (topic). The program first matches all documents with a similarity to a query that is higher than the specifed threshold. Then the query is split into several fragments to account for topics that contain a list of important things to cover. If answer to a fragment is not found in the documents associated so far, the similairity is measured between the fragment and the documents. Again the documents with similarity higher than threshold are selected to be included in the result file.

After a list of topics and associated documents is created, the pdf file is generated. It generates a new page with the topic, and then selected slides are attached after the page with the topic. Then another topic follows with its pages, and so on.

## models used

- BM25 - model using frequency of the words to calculate similarity it achieved the best results
- intfloat/multilingual-e5-large - dense model

There was also an attempt to use a new model, that would be created specifically for this task, however the created models did not live up even to a minimal expectations.
The description of own models is available in the subdirectory.

## conclusions

The pipeline was setup correctly, however the most important part, which is calculating the similarity between documents and queries does not work so well:

- A lot of important slides are often skipped and are not included in relevant topics. It often happens because the explanation of the topic is split into several slides and (according to models) only the first slide is similair enough to the question.
- Sometimes irrelevant slides are being attached to a question. Probably there are more stopwords that need to be filtered.
- Number of slides is too big. It can be adjusted using thresholds mentioned earlier, but it is hard to estimate a proper values for those.

Better models may also be able to fix these issues.

## materials used:

stopwords used from: https://github.com/bieli/stopwords/blob/master/polish.stopwords.txt

dataset (used to train personal models) mtnq comes from this paper: https://arxiv.org/abs/2305.05486
