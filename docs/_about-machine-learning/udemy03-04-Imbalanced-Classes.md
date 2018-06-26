---
title: "[Udemy03] 04. Imbalanced Classes" 
excerpt: Another scenario where Bayes rule comes into play
mathjax : True
---

### Imbalanced Classes
- Another scenario where Bayes rule comes into play
- Build a classifier for a disease test
- E.g. Input = blood sample, output = yes/no
- What would be a good accuracy?
- 80%? 90%? 95%?

### Realistic Situation
- Most of the population is non-diseased most of the time
- Let's suppose only 1% of the population has the disease
- We can build a classifier that just predicts "no" each time
- i.e. it doesn't learn anything
- It's already correct for 99% of cases
- Not a useful test
- Perhaps we do not care about overall accuracy

### What should we measure?
- What do we actually care about measuring?
- p(predict = 1 \| desease = 1)
- Called "true positive rate"
- Medical terminology : "sensitivity"
- Information retrieval : "hit rate" or "recall"
- Using Bayes rule:
- $p(pred = 1 \| disease = 1) = p(pred = 1, disease = 1) / p(disease = 1)$

### What data do we have?
- Typically we count 4 things:
    + True positives, true negatives, false positives, false negatives
    + 
|              | Pred = 1 | Pred = 0 |
|:------------:|:--------:|:--------:|
| Disease = 1  |TP        |FN        |
| Disease = 0  |FP        |TN        |

### Calculating sensitivity
```
p(pred = 1 | disease = 1) = p(pred = 1, disease =1) / p(disease = 1)
                          = [TP/N] / [(TP+FN)/N]
                          = TP / (TP + FN)
                N = total number of examples
```

### Specificity
- True negative rate
```
p(pred = 0 | disease = 0) = p(pred = 0, disease =0) / p(disease = 0)
                          = TN / (TN + FP)
```

### Mini-Quiz
- In information retrieval, rather than specificity, we are interested in "precision"
- Precision = TP / (TP + FP)
- Question : What is this the probability of?
    + Solution:
    + Work backwards!!
    + TP / (TP+FP)
    + = (TP/N) / \[(TP+FP)/N\]
    + = p(pred = 1, disease = 1) / p(pred=1)
    + = p(disease = 1 | pred = 1)
Useful measure! If your test result is positive, it does not mean you have the disease. Usually, more testing is required.
