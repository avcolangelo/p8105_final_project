Report
================
11/19/2019

Start of code (place holder until we have page)

## Group members (names and unis)

Alexis Colangelo, avc2129; Dania Jafar, dj2536; Si Li, sl4657; Kristal
Quispe, kq2127; Barik Rajpal, bsr2136

## Regression Analysis

It is likely that squirrels exhibit different behaviors in the morning
(AM) and evening (PM) due to sleep, eating, and predator habits. We can
examine if squirrel behaviors can allow us to predict whether a sighting
occured during the AM or PM using a logistic regression model. We will
tidy the dataset to include only squirrel behaviors, the am/pm response
variable, and an ID to partition the data. We’ll then split the data
into a training and testing set to test multiple models.

``` r
sq <- read_csv("./data/squirrel_data.csv") %>%
 mutate(AM = ifelse(Shift=="AM",1,0),
        id = row_number()) %>%
  janitor::clean_names() %>%
  select(id, am, running, chasing, climbing, eating, foraging, kuks, quaas, moans, tail_flags, tail_twitches,approaches,indifferent,runs_from)

set.seed(100)
sq_train <- sample_frac(sq, .8)
sq_test <- anti_join(sq,sq_train, by = "id")
```

We will create a glm using the step function, with the direction =
“backward”. This will start by creating a regression model with all
the potential covariates, and drop the least significant one until it is
only left with significant covariates. We are not showing the process of
the step function (as it can be quite long), but we will show a summary
of the model created.

``` r
summary(mod1)
```

    ## 
    ## Call:
    ## glm(formula = am ~ climbing + eating + foraging + kuks + approaches + 
    ##     indifferent + runs_from, family = "binomial", data = sq_train)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -1.5736  -1.0760  -0.8991   1.2056   1.6505  
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     -0.50476    0.09696  -5.206 1.93e-07 ***
    ## climbingTRUE     0.28940    0.10547   2.744 0.006070 ** 
    ## eatingTRUE      -0.36923    0.09875  -3.739 0.000185 ***
    ## foragingTRUE    -0.19213    0.08843  -2.173 0.029812 *  
    ## kuksTRUE         0.48025    0.23350   2.057 0.039710 *  
    ## approachesTRUE   0.48058    0.18211   2.639 0.008317 ** 
    ## indifferentTRUE  0.63072    0.09960   6.333 2.41e-10 ***
    ## runs_fromTRUE    0.20932    0.11645   1.798 0.072246 .  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 3322.3  on 2417  degrees of freedom
    ## Residual deviance: 3250.8  on 2410  degrees of freedom
    ## AIC: 3266.8
    ## 
    ## Number of Fisher Scoring iterations: 4

The step function left many covariates in, but since they are all binary
and some behaviors are not displayed very often, we believe it is likely
that the model has too many covariates, and could be reduced further. We
will also try one model using only the covariates with high degrees of
significance, climbing, eating, approaches, and indifferent. We will
then compare the two models.

``` r
mod2 <- glm(formula = am ~ climbing + eating + approaches +
    indifferent, family = "binomial", data = sq_train)

summary(mod2)
```

    ## 
    ## Call:
    ## glm(formula = am ~ climbing + eating + approaches + indifferent, 
    ##     family = "binomial", data = sq_train)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -1.3570  -1.0395  -0.9775   1.1698   1.5491  
    ## 
    ## Coefficients:
    ##                 Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)     -0.49020    0.07023  -6.980 2.96e-12 ***
    ## climbingTRUE     0.36965    0.10044   3.680 0.000233 ***
    ## eatingTRUE      -0.35130    0.09823  -3.576 0.000349 ***
    ## approachesTRUE   0.37665    0.17748   2.122 0.033818 *  
    ## indifferentTRUE  0.50806    0.08541   5.949 2.70e-09 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 3322.3  on 2417  degrees of freedom
    ## Residual deviance: 3263.4  on 2413  degrees of freedom
    ## AIC: 3273.4
    ## 
    ## Number of Fisher Scoring iterations: 4

``` r
anova(mod2,mod1,test="Chisq")
```

    ## Analysis of Deviance Table
    ## 
    ## Model 1: am ~ climbing + eating + approaches + indifferent
    ## Model 2: am ~ climbing + eating + foraging + kuks + approaches + indifferent + 
    ##     runs_from
    ##   Resid. Df Resid. Dev Df Deviance Pr(>Chi)   
    ## 1      2413     3263.4                        
    ## 2      2410     3250.8  3   12.586 0.005622 **
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

In both models, eating as a squirrel behavior is more associated with
the PM, and climbing, indifferent, and approaches behaviors are more
associated with the AM.

The AIC increased slightly when removing three covariates, but only
slightly, so the smaller model may be better due to parsimony. The ANOVA
however, did tell us that the model with more covariates was
statistically significant. We can further evaluate the results of the
models using a 2x2 table of accuracy in predictions in the test dataset.

| am |   0 |  1 |
| -: | --: | -: |
|  0 | 281 | 52 |
|  1 | 205 | 67 |

Accuracy for mod1

| am |   0 |   1 |
| -: | --: | --: |
|  0 | 237 |  96 |
|  1 | 154 | 118 |

Accuracy for mod2

Model 1 with more covariates had better specificity 281/(281+52) = 84%,
but very poor sensitivity 67/(67+205) = 24.5%. Model 2 with fewer
covariate had a slightly lower specificity 237/(237+96) = 71%, and
slightly better sensitivity 118/(118+154) = 43%.

Regarding overall accuracy, model 2 was slightly better, but both models
predictions are quite poor (below 60% accuracy for both), so it does not
appear to be reasonable to attempt to predict the time of day based on
observed squirrel behavior. Part of this may be due to the limited
amount of squirrel behaviors recorded per row.

    ##  total_behaviors
    ##  Min.   :0.000  
    ##  1st Qu.:2.000  
    ##  Median :2.000  
    ##  Mean   :2.288  
    ##  3rd Qu.:3.000  
    ##  Max.   :8.000

With a median of only 2 behaviors recorded per row, it is possible we
would have been able to create a better logisitic regression model if
the squirrels were observed for a longer period of time per recorded
observation, so that many behaviors could be observed and recorded at
once.

## End of Regression Analysis

The written report produced by your team is central to this project.
This will detail how you completed your project, and should cover data
collection and cleaning, exploratory analyses, alternative strategies,
descriptions of approaches, and a discussion of results. We anticipate
that your project will change somewhat over time; these changes and the
reasons for them should be documented\! You will write one report
document per group, and be sure to include all group member names in the
document.

Your report should include the following topics. Depending on your
project type the amount of discussion you devote to each of them will
vary:

  - Motivation: Provide an overview of the project goals and motivation.

  - Related work: Anything that inspired you, such as a paper, a web
    site, or something we discussed in class.

  - Initial questions: What questions are you trying to answer? How did
    these questions evolve over the course of the project? What new
    questions did you consider in the course of your analysis?:

– What is the probability of a certain squirrel in a certain area;
visualize this probability/heat map – regression question: number
(count) of squirrels seen eating as predicted by foraging – Where are
squirrels are most concentrated? – What are the most common squirrel
behaviors?

  - Data: Source, scraping method, cleaning, etc.

  - Exploratory analysis: Visualizations, summaries, and exploratory
    statistical analyses. Justify the steps you took, and show any major
    changes to your ideas.

  - Additional analysis: If you undertake formal statistical analyses,
    describe these in detail Discussion: What were your findings? Are
    they what you expect? What insights into the data can you make? As
    this will be your only chance to describe your project in detail,
    make sure that your report is a standalone document that fully
    describes your process and results. We also expect you to write
    high-quality code that is understandable to an outside reader.
    Coding collaboratively and actively reviewing code within the team
    will help with this
