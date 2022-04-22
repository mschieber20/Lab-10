Lab 10 - Grading the professor, Pt. 2
================
Marq Schieber
4/21/22

### Load packages and data

``` r
library(tidyverse) 
library(tidymodels)
library(openintro)
library(broom)
library(MASS)
```

### Exercise 1 Fit a linear model (one you have fit before): m_bty, predicting average professor evaluation score based on average beauty rating (bty_avg) only. Write the linear model, and note the r-square and the adjusted r-square.

score = 3.88 + BeautyAvg\*.066 R-square = .035 Adjusted r-square = .0329

``` r
m_bty = lm(score ~ bty_avg, data=evals) 

summary(m_bty)
```

    ## 
    ## Call:
    ## lm(formula = score ~ bty_avg, data = evals)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.9246 -0.3690  0.1420  0.3977  0.9309 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.88034    0.07614   50.96  < 2e-16 ***
    ## bty_avg      0.06664    0.01629    4.09 5.08e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5348 on 461 degrees of freedom
    ## Multiple R-squared:  0.03502,    Adjusted R-squared:  0.03293 
    ## F-statistic: 16.73 on 1 and 461 DF,  p-value: 5.083e-05

### Exercise 2 Fit a linear model (one you have fit before): m_bty_gen, predicting average professor evaluation score based on average beauty rating (bty_avg) and gender. Write the linear model, and note the r-square and the adjusted r-square.

score = 3.74 + BeautyAvf*.074 + Gender*.172 R-square = .059 Adjusted
r-square = .055

``` r
m_bty_gen <- lm(score ~ bty_avg + gender, data=evals)

summary(m_bty_gen)
```

    ## 
    ## Call:
    ## lm(formula = score ~ bty_avg + gender, data = evals)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.8305 -0.3625  0.1055  0.4213  0.9314 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.74734    0.08466  44.266  < 2e-16 ***
    ## bty_avg      0.07416    0.01625   4.563 6.48e-06 ***
    ## gendermale   0.17239    0.05022   3.433 0.000652 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5287 on 460 degrees of freedom
    ## Multiple R-squared:  0.05912,    Adjusted R-squared:  0.05503 
    ## F-statistic: 14.45 on 2 and 460 DF,  p-value: 8.177e-07

### Exercise 3 Interpret the slope and intercept of m_bty_gen in context of the data.

The intercept correpsond to scores of female professors with beauty
ratings of zero. Male professors (increase .172) that are attractive
(incease .074) have the highest evaluation rating.

### Exercise 4 What percent of the variability in score is explained by the model m_bty_gen.

Around 6%

### Exercise 5 What is the equation of the line corresponding to just male professors?

score = 3.74 + BeautyAvg\*.074 +.172

### Exercise 6 For two professors who received the same beauty rating, which gender tends to have the higher course evaluation score?

Male

### Exercise 7 How does the relationship between beauty and evaluation score vary between male and female professors?

Unclear based on current analyses, but I expect beauty has a larger
effect for women than men.

### Exercise 8 How do the adjusted r-square values of m_bty_gen and m_bty compare? What does this tell us about how useful gender is in explaining the variability in evaluation scores when we already have information on the beauty score of the professor.

m_bty_gen has a larger adjusted r-square. This suggests that gender
contributes to the explained variance of the model.

### Exercise 9 Compare the slopes of bty_avg under the two models (m_bty and m_bty_gen). Has the addition of gender to the model changed the parameter estimate (slope) for bty_avg?

Including gender increased the coefficient corresponding to bty_avg.
(.066 to .074)

### Exercise 10 Create a new model called m_bty_rank with gender removed and rank added in. Write the equation of the linear model and interpret the slopes and intercept in context of the data.

score = 3.98 + BeautyAvg*.068 - TenureTrack*.161 - Tenured\*.130

3.98 is the score of 0 beauty teaching level professors

``` r
m_bty_rank <- lm(score ~ bty_avg + rank, data=evals)

summary(m_bty_rank)
```

    ## 
    ## Call:
    ## lm(formula = score ~ bty_avg + rank, data = evals)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.8713 -0.3642  0.1489  0.4103  0.9525 
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)       3.98155    0.09078  43.860  < 2e-16 ***
    ## bty_avg           0.06783    0.01655   4.098 4.92e-05 ***
    ## ranktenure track -0.16070    0.07395  -2.173   0.0303 *  
    ## ranktenured      -0.12623    0.06266  -2.014   0.0445 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5328 on 459 degrees of freedom
    ## Multiple R-squared:  0.04652,    Adjusted R-squared:  0.04029 
    ## F-statistic: 7.465 on 3 and 459 DF,  p-value: 6.88e-05

### Exercise 11 Which variable, on its own, would you expect to be the worst predictor of evaluation scores? Why? Hint: Think about which variable would you expect to not have any association with the professor’s score.

I would say number of students who completed the course eval. I don’t
think this by itself is very strong. Percentage is a close second, but
still has more predictive power because a low percentage of responses
indicates a potential self selection bias.

### Exercise 12 Check your suspicions from the previous exercise. Include the model output for that variable in your response.

score = 4.15 + ClassSz \* .0007 Multiple R-squared: 0.003946, Adjusted
R-squared: 0.001786

Definitely a weak predictor…

``` r
m_cls_did_eval <- lm(score ~ cls_did_eval, data=evals)

summary(m_cls_did_eval)
```

    ## 
    ## Call:
    ## lm(formula = score ~ cls_did_eval, data = evals)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.8545 -0.3595  0.1303  0.4269  0.8485 
    ## 
    ## Coefficients:
    ##               Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  4.1469347  0.0325682 127.331   <2e-16 ***
    ## cls_did_eval 0.0007589  0.0005616   1.351    0.177    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5434 on 461 degrees of freedom
    ## Multiple R-squared:  0.003946,   Adjusted R-squared:  0.001786 
    ## F-statistic: 1.827 on 1 and 461 DF,  p-value: 0.1772

### Exercise 13 Suppose you wanted to fit a full model with the variables listed above. If you are already going to include cls_perc_eval and cls_students, which variable should you not include as an additional predictor? Why?

I htink I’ve covered them all…Whoops I think cls_students would have
been better than what I chose.

### Exercise 14 Fit a full model with all predictors listed above (except for the one you decided to exclude) in the previous question.

``` r
m_big_model <- lm(score ~ rank + ethnicity + gender + language + age + cls_perc_eval + cls_students + cls_level + cls_profs + cls_credits + bty_avg, data=evals)

summary(m_big_model)
```

    ## 
    ## Call:
    ## lm(formula = score ~ rank + ethnicity + gender + language + age + 
    ##     cls_perc_eval + cls_students + cls_level + cls_profs + cls_credits + 
    ##     bty_avg, data = evals)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.84482 -0.31367  0.08559  0.35732  1.10105 
    ## 
    ## Coefficients:
    ##                         Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)            3.5305036  0.2408200  14.660  < 2e-16 ***
    ## ranktenure track      -0.1070121  0.0820250  -1.305 0.192687    
    ## ranktenured           -0.0450371  0.0652185  -0.691 0.490199    
    ## ethnicitynot minority  0.1869649  0.0775329   2.411 0.016290 *  
    ## gendermale             0.1786166  0.0515346   3.466 0.000579 ***
    ## languagenon-english   -0.1268254  0.1080358  -1.174 0.241048    
    ## age                   -0.0066498  0.0030830  -2.157 0.031542 *  
    ## cls_perc_eval          0.0056996  0.0015514   3.674 0.000268 ***
    ## cls_students           0.0004455  0.0003585   1.243 0.214596    
    ## cls_levelupper         0.0187105  0.0555833   0.337 0.736560    
    ## cls_profssingle       -0.0085751  0.0513527  -0.167 0.867458    
    ## cls_creditsone credit  0.5087427  0.1170130   4.348  1.7e-05 ***
    ## bty_avg                0.0612651  0.0166755   3.674 0.000268 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.504 on 450 degrees of freedom
    ## Multiple R-squared:  0.1635, Adjusted R-squared:  0.1412 
    ## F-statistic: 7.331 on 12 and 450 DF,  p-value: 2.406e-12

``` r
m_cls_did_eval <- lm(score ~ cls_did_eval, data=evals)
```

### Exercise 15 Using backward-selection with adjusted R-squared as the selection criterion, determine the best model. You do not need to show all steps in your answer, just the output for the final model. Also, write out the linear model for predicting score based on the final model you settle on.

``` r
m_best_model<-stepAIC(m_big_model , direction = "backward" , trace = FALSE)

summary(m_best_model)
```

    ## 
    ## Call:
    ## lm(formula = score ~ ethnicity + gender + language + age + cls_perc_eval + 
    ##     cls_credits + bty_avg, data = evals)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.9067 -0.3103  0.0849  0.3712  1.0611 
    ## 
    ## Coefficients:
    ##                        Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)            3.446967   0.203191  16.964  < 2e-16 ***
    ## ethnicitynot minority  0.204710   0.074710   2.740 0.006384 ** 
    ## gendermale             0.184780   0.049889   3.704 0.000238 ***
    ## languagenon-english   -0.161463   0.103213  -1.564 0.118427    
    ## age                   -0.005008   0.002606  -1.922 0.055289 .  
    ## cls_perc_eval          0.005094   0.001438   3.543 0.000436 ***
    ## cls_creditsone credit  0.515065   0.104860   4.912 1.26e-06 ***
    ## bty_avg                0.064996   0.016327   3.981 7.99e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.503 on 455 degrees of freedom
    ## Multiple R-squared:  0.1576, Adjusted R-squared:  0.1446 
    ## F-statistic: 12.16 on 7 and 455 DF,  p-value: 2.879e-14

score = 3.45 + Ethnc*.20 + Gender*.18 - Language*.16 - Age*.005 +
PercEval*.005 + Credit*.52 + BTY\*.064

### Exercise 16 Interpret the slopes of one numerical and one categorical predictor based on your final model.

As age increases, scores decrease by .005 per year. Gender: Male eval
scores are higher than females by .18.

### Exercise 17 Based on your final model, describe the characteristics of a professor and course at University of Texas at Austin that would be associated with a high evaluation score.

A high scoring professors would have the following characteristics:
White, male, english firt language, young, attractive, teaching a one
credit class (aka low stakes class) and there’s a high response rate for
evaluations.

### Exercise 18 Would you be comfortable generalizing your conclusions to apply to professors generally (at any university)? Why or why not?

I would feel comfortable generalizing this finding to other universities
within the US and likely to Western countries (generally speaking, there
are definitely exceptions).
