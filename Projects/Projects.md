# Projects

For the remaining part of the course, we will work on projects together. Feel free to work on your own, or pair up and work on a team of two people.

Tomorrow, we will take a few minutes to share the results from your projects. There is no expectation for you to completely finish the project, just make as much progress as you can. Also, there is no need for you to prepare a formal presentation --- just share with us what project you picked; how you decided to approach the project; and any preliminary results.

# List of Suggested Projects

## Coding Practice

For anyone who wants a little more practice coding models and has not yet finished the exercises, you are welcome to continue working through the exercises. For an additional challenge, I recommend starting from the pseudocode exercises.

## Modeling a Treatment Scenario with the Plague Data

This project is included as an optional exercise at the end of Exercise 4 (under "Treatment of Infected Individuals"). We define a variation of our model that includes a subset of people who become infected who receive treatment and recover earlier.

1. Draw a new model diagram, creating a separate state and transitions for individuals who receive treatment. 
2. Write down the corresponding differential equations. Then, open a new notebook and create a new function `sir_model_treatment` to implement your new differential equations
3. Start by assuming that treatment reduces time to recovery by 25% ($\gamma \rightarrow \gamma/(.75)$), and that 25% of people (`p_treatment`) who become infected receive treatment. 
4. Implement your model
   1. Use the ode integration function to solve the equation and obtain a time series representing the outbreak with treatment.
   2. Create a plot comparing the model with and without treatment
   3. How many infections do you avoid through treatment?
5. Try varying the treatment coverage (`p_treatment`): how big does it need to be to stop an outbreak from happening?
6. If want to explore the mathematics further, derive a new expression for $R_0$ for your model which includes treatment. Use the condition $R_0=1$ to solve for the treatment coverage value which stops and outbreak from occurring, and compare your answer to the previous question.
7. Suppose that treatment does not shorten the duration of the infectious period, and instead it reduces the per-contact rate of transmission ($\beta$) by 50%. What structural changes would you need to make to the model in order to represent this? How do your answers to each of the above questions change in this alternate scenario?

## Refining the Plague Data Model - Undercounting Deaths

The number of plague deaths recorded is likely to be an underestimate --- here you will assume that the number of deaths recorded in the model is too small by an unknown factor $r$.

1. Repeat the calibration in Exercise 3, this time assuming that the data are larger than they appear by a factor of $r$ (you may need to re-define the way you calculate sum of squared errors during the calibration). Calibrate to find $\beta$, $\gamma$, and $r$. 
2. Create a plot of your new model against the data (rescaled by $r$).
3. What is the new value of $R_0$ in your new model? How does it compare to the previous estimate of $R_0$?
4. Suppose now that $r$ is allowed to vary over the course of the outbreak.   Design your own function $r(t)$ which varies over the course of the outbreak, and repeat the exercise from above. Your function may include free parameters which should themselves be calibrated. See if there is a function $r(t)$ which best improves the fit.

## Refining the Plague Data Model - Infections and Deaths

You may have noticed a mismatch between the plague data and the model we used to fit the data - the plague data enumerate "deaths" but we used the SIR model to count "infections." To more accurately model the data that we have, we would need to assume that there are more infections than deaths, and that only infections which result in deaths are recorded. Assume that individuals who die no longer are infectious - they become removed from the population.

Here are some suggestions for how to proceed:

1. Draw a new model diagram, one which includes a new transition and compartment --- infected individuals may die with a certain probability `p_mortality`.
2. Write down the corresponding differential equations. Then, open a new notebook and create a new function `sir_model_mortality` to implement your new differential equations.
3. Start by assuming that an infection leads to death 25% of the time.
4. Implement your model
   1. Use the ode integration function to solve the equation and obtain a time series representing the outbreak. Your time series will include the number of deaths over time and the number of infections over time.
   2. Create a plot to show your model results
5. Now, suppose you do not know what the death rate is for people who become infected with plague. Use the fitting tools from Exercise 3 to match the time series of deaths in your mortality model against the plague data by finding optimal parameters for beta, gamma, and the probability of death.
   1. Verify the accuracy of your model fit by plotting your calibrated model against the dataset.
6. What is $R_0$ in your new model? How does it compare to the value of R_0 that you calculated when you matched the number of infections against the number of deaths?

## The Ross-MacDonald Model of Malaria

## Stochastic Modelling with Gillespie's Algorithm

## Network Metapopulation SIR Model

## Introduction to Agent-Based Modeling

## Bayesian Model Fitting

For this final project, we will again use our plague dataset.

## Create Your Own

If there's a topic related to the workshop --- anything related to infectious disease dynamics, designing models, implementing models, etc --- that you are interested in but do not see in the list above, feel free to create your own project to work on.