# DSC8307 Data Mining, Modeling and Analytics: Advent 2026 Assignment
## MUGANGA Charles B35391

This repository contains my submission for the two projects in the DSC8307 assignment. Each project has its own Jupyter notebook, and both run from top to bottom after installing the packages in `requirements.txt`.

## What is in here

`project1_bank_marketing.ipynb` is Project 1. It predicts whether a bank customer will subscribe to a term deposit, ranks three modelling approaches and recommends a decision threshold.

`project2_concrete_strength.ipynb` is Project 2. It predicts concrete compressive strength from the mix and curing age, compares linear and nonlinear models, studies the residuals and discusses whether the cement link is causal.

`requirements.txt` lists the exact package versions used.

The `data` folder holds the two datasets and their README files, exactly as supplied with the assignment.

## How to run

```
python -m venv .venv
source .venv/bin/activate        # on Windows use  .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook
```

Then open either notebook and choose Run All. The notebooks read their data from the `data` folder using relative paths, so start Jupyter from the repository folder. Project 1 takes about three minutes on a normal laptop because of the cross validation and tuning, and Project 2 takes about two minutes.

To run both without opening a browser:

```
jupyter nbconvert --to notebook --execute --inplace project1_bank_marketing.ipynb
jupyter nbconvert --to notebook --execute --inplace project2_concrete_strength.ipynb
```

Every split, fold and model uses the same random seed (42), so the numbers quoted in the text should match the outputs on a rerun.

## Summary of findings

### Project 1, bank marketing

The target is rare (11.3 percent said yes), so models are judged mainly on the area under the precision and recall curve rather than accuracy. The call `duration` column was removed because it is only known after the call and would leak the answer. The rows span the 2008 to 2010 financial crisis and the subscription rate changes a lot over time, so the models were tested on forward time windows as well as on a random split.

The ranking is random forest first, gradient boosting second and logistic regression third. The random forest was at or near the top on every test, while gradient boosting's small lead on the random split did not survive the forward in time test. The recommended decision threshold is about 0.10, which on the test set means calling roughly 23 percent of customers, reaching about 69 percent of subscribers, with about one call in three converting, compared with one in nine if everyone is called.

### Project 2, concrete strength

The 1,030 tests come from only 428 distinct recipes, so all validation keeps each recipe on one side of the split. Ignoring this makes the tree models look 15 to 24 percent better than they really are on new mixes.

Against a mean baseline error of about 18 MPa, a plain linear model reaches about 10.8 MPa, a linear model with engineered features (log age, water to cement ratio) about 7.4 MPa, and the random forest and gradient boosting models about 5.8 to 6.0 MPa, which are statistically tied. All models are weakest on high strength concrete above 60 MPa, which they underpredict. The final section explains why a predictive link between cement and strength does not, on its own, show what would happen if a mix were changed.

## Data sources

Bank Marketing data: Moro, S., Cortez, P. and Rita, P. (2014). A data driven approach to predict the success of bank telemarketing. Decision Support Systems, 62, 22 to 31. Distributed through the UCI Machine Learning Repository.

Concrete data: Yeh, I. C. (1998). Modeling of strength of high performance concrete using artificial neural networks. Cement and Concrete Research, 28(12), 1797 to 1808. Distributed through the UCI Machine Learning Repository.

Full references for the methods used are listed at the end of each notebook.
