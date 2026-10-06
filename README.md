# PROJECT_NLP

Hotel Review Sentiment Analysis

Compare VADER and a pretrained RoBERTa model on positive/negative hotel reviews, including a separate evaluation of reviews containing negation cues.

## Run

1. Use Python 3.11 and Jupyter Notebook. Open `Sentiment Analysis.ipynb` from this folder or its parent folder.
2. Keep `archive/Datafiniti_Hotel_Reviews.csv` in place. The other two CSV files in the archive folder are not used.
3. Restart the kernel and run every cell in order. The first setup cell installs the specified package versions into the active kernel. Internet is needed for missing packages, the NLTK VADER lexicon and the pinned Hugging Face model. The model download is about 500 MB. CPU works; a CUDA GPU is used when available.
4. In **Try it**, change `new_review` and run that cell again. This does not retrain the model. Empty input returns a helpful message.

If Jupyter is not installed, install it with `python -m pip install notebook`, then run `python -m notebook`. Use a dedicated Python environment if another project requires different package versions.

The setup pins `optree==0.19.1` because the previously installed 0.19.0 caused a verified Windows kernel crash. The [official 0.19.1 release](https://github.com/metaopt/optree/releases/tag/v0.19.1) fixes this import issue.

## Experiment

- Ratings from 1 to 2 are negative; ratings from 4 to 5 are positive. Middle ratings are excluded.
- Empty/missing reviews, invalid ratings, conflicting duplicated labels and normalized duplicate texts are removed.
- The deterministic stratified split is 70% training, 15% development and 15% test (seed 42).
- The models have fixed rules or pretrained weights; no local model-weight training is claimed. Training data determine the majority baseline. Development macro F1 selects decision thresholds and the model used by Try it.
- Both models are evaluated on the same final test reviews. All predictions, including originally neutral RoBERTa outputs, receive a binary label.
- Accuracy, macro F1, class counts, confusion matrices and the negation-subset results are saved in `results/`.
- Negation cues are a simple heuristic. Ratings are imperfect text labels; middle ratings are excluded; hotels can occur across splits; near duplicates are not comprehensively detected; RoBERTa was trained on tweets and truncates long input. These limit generalization.

## Files

- `Sentiment Analysis.ipynb`: editable implementation with saved outputs.
- `Project Proposal.docx` and `Project Proposal.pdf`: one-page proposal for the Natural Language Processing course project (Group 5).
- `team_examples.csv`: place two original labelled examples per member here. Columns: `member,text,expected`; labels: `negative` or `positive`. Demonstration sentences in the notebook are AI-assisted and do not count as member-authored examples.
- `results/`: measured scores, predictions, split manifest, figures and run metadata.
- `requirements.txt`: notebook dependencies.
- `SUBMISSION_STATUS.md`: remaining team-specific requirements.

## Verified run

All 13 code cells completed in order in a fresh Python kernel on this machine. The run used a CUDA GPU and the model files already cached at the declared revision. The project-local NLTK resource was downloaded through the setup cell. A completely new package environment and a CPU-only full run were not separately tested.

The held-out set contains 1,264 reviews. VADER accuracy is 0.8639 and macro F1 is 0.7466; RoBERTa accuracy is 0.9312 and macro F1 is 0.8574. On the 542 reviews with negation cues, macro F1 is 0.7766 for VADER and 0.8706 for RoBERTa. These are results for the filtered binary task, so they are not directly comparable with the original three-class notebook scores.

The five-page report, one-page proposal, one-page appendix, nine-page notebook PDF and ten-slide presentation PDF were visually checked. All exported files are next to the notebook. The ZIP is a prepared package with the team-specific items in `SUBMISSION_STATUS.md` still pending.

## Sources and assistance

Data: [Datafiniti Hotel Reviews](https://www.kaggle.com/datasets/datafiniti/hotel-reviews).
VADER: Hutto and Gilbert (2014), implemented by [NLTK](https://www.nltk.org/api/nltk.sentiment.vader.html).
RoBERTa: [CardiffNLP model card](https://huggingface.co/cardiffnlp/twitter-roberta-base-sentiment) and [Barbieri et al. (2020), TweetEval](https://aclanthology.org/2020.findings-emnlp.148/).

The original notebook was supplied in the project folder; its authorship was not independently established. OpenAI Codex assisted with code corrections, experiment setup, testing, demonstration examples and preparation of documents. Team members should review and understand the work, add their own examples and real contributions, and credit any additional sources.
