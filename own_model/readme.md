# Creating own language model

The purpose of this subproject was to create a language model to be used in recap-maker. The results were not satisfactory, but are included here any way.

## General plan

The idea was to train model on pairs of words and sentences that would either be considered similar or not. The training dataset was created using mtnq dataset and sliding window technique: 
if 2 words appeared within the window, they were put together as similar words, the pairs of words that are not similar were created by randomly adding other words that did not appear in the window, anywhere within the dataset.
Sentences were deemed similar if they were a pair of corresponding question and answer, and not similar if the answer was from other question.

The words have been encoded using bag of words method.

Then the feed-forward model was trained. The final layer was to contain dense encoding of the word in n-dimensional numerical space. The loss function would compare dot products between 2 outputs - either a pair of similar or not words.
If a dot score was high and the words were similar, the model was rewarded, if the score was low the model was punished. And vice-versa for words that were not similar.

There were 2 models trained and evaluated:

model 1:

        self.linear_relu_stack = nn.Sequential(
            nn.Linear(vocab_size, 8192),
            nn.ReLU(),
            nn.Linear(8192, 8192),
            nn.ReLU(),
            nn.Linear(8192, 4096),
            nn.ReLU(),
            nn.Linear(4096, 2048),
            nn.ReLU(),
            nn.Linear(2048, 1024),
            nn.ReLU(),
            nn.Linear(1024, 512)
        )

model 2:

        self.linear_relu_stack = nn.Sequential(
            nn.Linear(vocab_size, 4096),
            nn.ReLU(),
            nn.Linear(4096, 2048),
            nn.ReLU(),
            nn.Linear(2048, 1024),
            nn.ReLU(),
            nn.Linear(1024, 512),
            nn.Tanh()
        )

## preprocessing

The detailed preprocessing scripts are located in the subdirectory. They should be run in the following order:
  - mtnq_preprocessing
  - Word Count
  - CreatingData
  - concat_data

## conclusions

There were 2 significant problems with the models:
- training time was extremely long
- Dot products returned by the model would always be 1 or -1. Hence it would be difficult to rank words based on similarity, since there would only be 2 possible places.

The second problem was the reason why the models were not actually used in the pipeline of recap-maker.

## files in this directory:
 - model1-v3 - notebook with training script for model 1
 - model1-v3resume - notebook which allows to continue previously interrupted training of model 1
 - model2 - notebook with training script for model 2
 - preprocessing - notebooks used in preprocessing
