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

## Network Metapopulation SIR Model

Imagine that you have geographically separated populations. People from location A occasionally travel to location B, and people from location B occasionally travel to location A. However, most of the time, people from location A stay in location A and people from location B stay in location B.

Suppose that an outbreak begins in location A. Under what circumstances does the outbreak spread to location B? Assume that you may use the SIR model to represent the dynamics of location A and location B, and that parameters (beta, gamma) are the same in both locations.

Let $p_{i,j}$ = the amount of time someone from location i spends in location j.

1. Draw a new model diagram, one with a set of compartments representing the SIR dynamics in A (S_a, I_a, R_a) and a separate set of compartments representing the SIR dynamics in B (S_b, I_b, R_b). Make sure that you draw the interactions within and between populations in the two locations.
2. Write down the corresponding differential equations. Use the $\{p_{i,j}\}$ as coefficients for weighting the interactions between susceptibles and infecteds witin and between different populations. Before moving on, double-check that you have written down an expression for transmission between all susceptibles and all infecteds in each of the two locations.
3. Implement your model
   1. Assume that the number of people in A and the number of people in B are the same: $N_A = N_B = 1000$
   2. Assume that people in A spend $p_{A,A} = 0.95$ of their time interacting with people in A, and $p_{A,B} = 0.05$ of their time interacting with people in B. 
   3. Assume that beta = 2.56 and gamma = 2.11 in both locations
   4. Assume that 1 person begins infected in location A, 0 people become infected in location B. You will need to rewrite your initial conditions to represent this.
   5. Integrate your model again
   6. Create a plot showing the outbreaks in the two locations.
   7. How much later is the epidemic peak in location B compared to the epidemic peak in location A? Are the outbreaks the same size in both locations? Why do you think this is?
4. Try varying your parameters (beta, gamma, p, N) and seeing how the model dynamics change. Under what circumstances is it possible to prevent an outbreak from happening in location B while an outbreak occurs in location A?
5. Consider also how your model will change with 3 or more populations. Try implementing that model if you have time.

## The Ross-Macdonald Model of Malaria

Malaria is a disease caused by a blood-borne pathogen (*Plasmodium*) and is spread through contact with mosquitoes (*Anopheles*). The key to understanding malaria transmission is the interactions between humans and mosquitoes. The mosquitoes spread the pathogen to humans; infected humans are then bitten by other mosquitoes who then become infected; and so on. 

### Part 1

Any malaria model that we make should include both the population of mosquitoes and the population of humans. One such model is the Ross-Macdonald model, which you can read more about [here](https://journals.plos.org/plospathogens/article?id=10.1371/journal.ppat.1002588). This model has many variations, and we will start with a simple version:

$\frac{dx}{dt} = mabz(1-x) - rx$

Our state variables are
* $x$, the fraction of infected humans (prevalence of malaria).  
* $z$, the fraction of infectious mosquitoes

Our parameters are:
  * $r = 1/200$ is the rate of recovery (in days) for an average untreated malaria case
  * $a = 0.27$ - Mosquito bites per human per day 
  * $b = 0.55$ Mosquito-to-human transmission efficiency
  * $m = 0.5$ population density of mosquitoes (mosquitoes-per-human)

For now, let's assume that $z$ remains constant over time.

1. Draw a diagram of the compartmental model which corresponds to this version of the Ross-Macdonald model
2. Numerically integrate your differential equation, and show how the prevalence of malaria changes over time
   1. Try varying the mosquito population density $m$ or fraction of infectious mosquitoes $z$: how does your solution change?
   2. (Optional) This version of the model happens to be possible to solve analytically - use your numerical solution to derive an expression for $R_0$. See how it depends on each of the parameters $(m,a,b,z)$
3. Mosquito populations are known to be highly seasonal (very active in rainy season, less active when it is dry). Change your model such that that the density of mosquitoes now depends on time: $m(t) = 1/2(1 - \sin(2\pi/365*t))$ and see how the solution changes.

### Part 2

The prevalence of malaria in humans should also affect the fraction of infectious mosquitoes (think: if there are no infected humans, there will be no infectious mosquitoes). Add a second equation that explicitly models the population of infectious mosquitoes as well:

$\frac{dz}{dt} = acx(1-z) - gz$

Our new parameters are:
  * $c = 0.15$ - human-to-mosquito transmission efficiency. 
  * $g = 0.1$ - Mosquito mortality

1. Draw a diagram of the compartmental model which corresponds to this second version of the Ross-Macdonald model. Your diagram should include bi-directional interactions between humans and mosquitoes.
2. Numerically integrate your differential equation, and show how the prevalence of malaria and the fraction of infectious mosquitoes change over time
   1. Allow the parameter $m$ to vary, and see how that changes your model output
   2. (Optional) This version of the model happens to be possible to solve analytically - use your numerical solution to derive an expression for $R_0$. See how it depends on each of the parameters $(m,a,b,c,g)$
3. Change your model again such that that the density of mosquitoes now depends on time: $m(t) = 1/2(1 - \sin(2\pi/365*t))$ and see how the solution changes.


## Stochastic Modelling with Gillespie's Algorithm

We will reformulate our model to incorporate stochastic effects using Gillespie's algorithm.

Start by defining your state variables
* S, I, R are now *discrete* variables that only take whole number values (non-negative integers)

Initial conditions and parameters
* S = 999, I = 1, R = 0 at time=0
* For now, use beta = 2.56, gamma = 2.11

We no longer use a set of differential equations. Instead, Gillespie's algorithm works by calculating the time until the next event happens. Each event occurs to a different transition. The transitions in our model are now:
* Someone susceptible becomes infected: 
  * (S, I, R) -> (S-1, I + 1, R)
  * This occurs at rate $r_{si} = \beta*S*I/N$
* Someone infected becomes recovered: 
  * (S, I, R) -> (S, I-1, R+1)
  * This occurs at rate $r_{ir} = \gamma I$

Here are the steps of the algorithm:
1. Start at t=0, and pick and end time $t_f$
2. Calculate all rates of all transitions for the current state of the system
3. Find the total rate $r_{tot} = r_{si} + r_{ir}$
4. Random draw: pick the time at which the next event occurs $t_{next}$ from the exponential distribution $r_{tot} Exp[-r_{tot} t]$
5. If
   1. $t_{next} > t_f$ end
   2. Else
      1. Increment t -> t_{next}
      2. Pick a random number in the range $[0, r_{tot}]$
      3. If your random number 
         1. is less than $r_{si}$, allow that transition to occur
         2. Else, allow the other transition to occur
6. Repeat


Here are some questions to consider
1. Run 100 different versions of this model using different random number seeds. Plot all of them together - this is an ensemble of simulation runs.
2. Compare your ensemble of solutions against the ODE solution?
3. How does your ensemble of simulation runs change with the population size?
4. Think about: how would you fit this model to data?

## Introduction to Agent-Based Modeling

My colleague has designed a demo for creating an agent-based model.

Github repo [here](https://github.com/starsims/abm_training_course)

## Create Your Own

If there's a topic related to the workshop --- anything related to infectious disease dynamics, designing models, implementing models, etc --- that you are interested in but do not see in the list above, feel free to create your own project to work on.