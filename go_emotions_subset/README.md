# Go Emotions Subset

These datasets were curated from [GoEmotions](https://ai.googleblog.com/2021/10/goemotions-dataset-for-fine-grained.html) and augmented with [HuggingFace's](https://huggingface.co/blog/sentiment-analysis-python) sentiment analysis pipeline. GoEmotions is a corpus of 58k carefully curated comments extracted from Reddit, with human annotations to 27 emotion categories or Neutral.

The `go_emot_subset_` files contains only the 9 most frequent labels:<br>
`admiration, amusement, anger, annoyance, approval, curiosity, disapproval, gratitude, neutral`

The `demo_four_class_` files contain 4 labels:<br>
`amusement, disapproval, gratitude, neutral`

The `demo_neutral_class_` files contain only the neutral labels from `demo_four_class_`.

The sentiment score was calculated using:<br>
`from transformers import pipeline`<br>
`pipe = pipeline("sentiment-analysis")`<br>
`sentiment = pipe(text)`<br>
`label =  sentiment["label"]`<br>
`scr = sentiment["label"]`<br>
`score = scr if label=="POSITIVE" else scr-1`<br>

This ensures that:
* very positive: ~1.0
* neutral: ~0.5
* very negative: ~0.0

# Data

The data is split into a few csv files that include annotations as well as metadata on the comments. 

`go_emot_subset_train.csv` contains 25,106 samples<br>
`go_emot_subset_test.csv` contains 2,815 samples<br>
`demo_four_class_train.csv` contains 11,385 samples<br>
`demo_four_class_test.csv` contains 1,328 samples<br>
`demo_neutral_class_train.csv` contains 4,634 samples<br>
`demo_neutral_class_test.csv` contains 488 samples

### Data Format
All files contain 4 columns:
* `comment_text`: The text of the comment, with masked tokens for names, private info, etc.
* `sentiment`: Float in the range [0,1] calculated by HuggingFace sentiment analysis pipeline. Smaller number means more negative.
* `label`: numerical label of the text, in range [0, 8]
* `emotion`: text representation of the label

## Limitations
* `sentiment` is  purely derived from HuggingFace pipeline API. No work was done to train/test/verify this.
* Reminder these are **subsets** of the original GoEmotions dataset
* The `neutral` class has been randomly down-sampled to match the frequency of the nest most frequent class in the `demo_four_class_`files. Prior to this, the `neutral` class was significantly unbalanced. The `demo_neutral_class_` files contains the `neutral` subset.
* Those [limitations](https://github.com/google-research/google-research/blob/master/goemotions/README.md#disclaimer) inherited from original dataset.