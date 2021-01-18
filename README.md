# EdNet - LightGBM Model
LightGBM approach for EdNet dataset with an AUC score of 0.764 on the test set.

## About

Riiid! competition aims to create algorithms for "Knowledge Tracing," the modeling of student knowledge over time, and to accurately predict how students will perform in future interactions. With advanced algorithms, it’s possible that any student can enjoy the benefits of a personalized learning experience. The model is evaluated on the area under the ROC curve between the predicted probability and the observed target. Additional information about the competition can be found on the [Kaggle Competition page](https://www.kaggle.com/c/riiid-test-answer-prediction).

## Model Overview
I built a LightGBM Model run on CPU. The final submission on Kaggle was a weighted average of LightGBM and a public solution based on the [Self-Attentive model for Knowledge Tracing Model - SAKT](https://arxiv.org/abs/1907.06837) (test AUC: 0.785).


## Datasets
[Riiid’s EdNet data](https://arxiv.org/pdf/1912.03072.pdf) is a large-scale hierarchical dataset consisting of student interaction logs collected over more than 2 years from Santa, a multi-platform, self-study solution equipped with an artificial intelligence tutoring system that aids students in preparing for the TOEIC test. 

### Train
The columns in the train file are described as:

- `row_id`: (int64) ID code for the row.

- `timestamp`: (int64) the time in milliseconds between this user interaction and the first event completion from that user.

- `user_id`: (int32) ID code for the user.

- `content_id`: (int16) ID code for the user interaction

- `content_type_id`: (int8) 0 if the event was a question being posed to the user, 1 if the event was the user watching a lecture.

- `task_container_id`: (int16) Id code for the batch of questions or lectures. For example, a user might see three questions in a row before seeing the explanations for any of them. Those three would all share a task_container_id.

- `user_answer`: (int8) the user's answer to the question, if any. Read -1 as null, for lectures.

- `answered_correctly`: (int8) if the user responded correctly. Read -1 as null, for lectures.

- `prior_question_elapsed_time`: (float32) The average time in milliseconds it took a user to answer each question in the previous question bundle, ignoring any lectures in between. Is null for a user's first question bundle or lecture. Note that the time is the average time a user took to solve each question in the previous bundle.

- `prior_question_had_explanation`: (bool) Whether or not the user saw an explanation and the correct response(s) after answering the previous question bundle, ignoring any lectures in between. The value is shared across a single question bundle, and is null for a user's first question bundle or lecture. Typically the first several questions a user sees were part of an onboarding diagnostic test where they did not get any feedback.


###  Questions
Question information table contains 5 columns: `question_id`, `bundle_id`, `correct_answer`, `part`, `tags`.

- `question_id`: foreign key for the train/test content_id column, when the content type is question (0).

- `bundle_id`: code for which questions are served together.

- `correct_answer`: the answer to the question. Can be compared with the train user_answer column to check if the user was right.

- `part`: the relevant section of the TOEIC test.

- `tags`: one or more detailed tag codes for the question. The meaning of the tags will not be provided, but these codes are sufficient for clustering the questions together.

### Lectures
Lecture information table contains 4 columns: `lecture_id`, `part`, `tags`, `type_of`.

- `lecture_id`: foreign key for the train/test content_id column, when the content type is lecture (1).

- `part`: top level category code for the lecture.

- `tag`: one tag codes for the lecture. The meaning of the tags will not be provided, but these codes are sufficient for clustering the lectures together.

- `type_of`: brief description of the core purpose of the lecture

## Contributing
Github issues and pull requests are welcome. Your feedback is much appreciated!

January 2021, Abdelghani Belgaid
