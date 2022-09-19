# Go Emotions Subset

These datasets were curated from [GoEmotions](https://ai.googleblog.com/2021/10/goemotions-dataset-for-fine-grained.html) and augmented with [HuggingFace's](https://huggingface.co/blog/sentiment-analysis-python) sentiment analysis pipeline. GoEmotions is a corpus of 58k carefully curated comments extracted from Reddit, with human annotations to 27 emotion categories or Neutral.

The `go_emot_subset_` files contains only the 9 most frequent labels:<br>
`admiration, amusement, anger, annoyance, approval, curiosity, disapproval, gratitude, neutral`

The `demo_four_class_` files contain 4 labels:<br>
`amusement, disapproval, gratitude, neutral`

The sentiment score was calculated using:<br>
`from transformers import pipeline`<br>
`pipe = pipeline("sentiment-analysis")`<br>
`sentiment = pipe(text)`<br>
`label =  sentiment["label"]`<br>
`scr = sentiment["label"]`<br>
`score = scr if label=="POSITIVE" else scr-1`<br>

This ensures that very positive texts get scores closer to 1.0, very negative closer to 0.0, and neutral close to 0.5.
# Data

The data is split into two csv files, includes all annotations as well as metadata on the comments. The files includes the following columns:

`go_emot_subset_train.csv` contains 25,106 samples<br>
`go_emot_subset_test.csv` contains 2,815 samples<br>

### Data Format
Both files contain 4 columns:
* `comment_text`: The text of the comment, with masked tokens for names, private info, etc.
* `sentiment`: Float in the range [0,1] calculated by HuggingFace sentiment analysis pipeline. Smaller number means more negative.
* `label`: numerical label of the text, in range [0, 8]
* `emotion`: text representation of the label

## Limitations
* `sentiment` is  purely derived from HuggingFace pipeline API. No work was done to train/test/verify this.
* Reminder this is a **subset** of the original GoEmotions dataset
* The `neutral` class has been randomly down-sampled to match the frequency of the nest most frequent class. Prior to this, the `neutral` class was significantly unbalanced.
* Those [limitations](https://github.com/google-research/google-research/blob/master/goemotions/README.md#disclaimer) inherited from original dataset.
