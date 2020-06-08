---
title: "Piece-wise exponential models"
author: "Philipp Kopper"
date: "02 June 2020"
output: pdf_document
---


# Data preprocessing

As outlined in the previous section, the __PEM data structure__ deals with the survival problem as a count
data problem. The main feature of this representation is the partition of the follow-up time into a finite
number of intervals. Within these intervals hazard rates are assumed to be piece-wise constant.

## Survival times & Events

Below, I show how to transform regular survival data into PEM format where the intervals are equidistant and
of size 0.4. Still, it should be chosen with great care. The data set contains a case id, a variable indicating
the observed event time (obs_times) and the observed event (status) As usual in survival analysis, the
status 1 refers to the event while 0 to a censoring. A priori the selected interval cut points defining the
baseline hazard in each interval are arbitrary.


```r
library(pammtools)
library(tidyr)
library(dplyr)
data_set <- data.frame(id = 1:3, obs_times = c(1, 0.5, 2), status = c(0, 1, 1))
ped <- as_ped(data = data_set, Surv(obs_times, status) ~ ., id = "id",
              cut = seq(0, max(data_set$obs_times), 0.4))
```

The offset reflects information on the observed survival time. Re-arranging the Poisson likelihood of the PEM
one can derive the following definition:
