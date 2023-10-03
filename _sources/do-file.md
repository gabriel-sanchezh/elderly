---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Do File
```{code}
// Clear the console and change the directory to the specified path
clear
cls
cd "/Users/gabrielsanchez/Documents/Academia/Papers/elderly/stata"

// Set global figure size and table options
global figure_size xsize(8) ysize(4)
global table_options showstars showstarsnote stars(.10 "*" .05 "**" .01 "***", attach(_r_b)) cstat(_r_b) keep($exogenas)

// Load the dataset 'creles-main'
use creles-main

// Keep only the desired variables and rename them as needed
keep id3 amp2 ev14 ev8 a1 a2a hn2 a3 c1 f1 in1 in2

// Rename and label the variables
rename id3 age
label variable age "Age"

rename amp2 sex
label variable sex "Sex"

rename ev14 exercise
label variable exercise "Exercise"

rename ev8 havesmoked
label variable havesmoked "Have smoked"

rename a1 readwrite
label variable readwrite "Knows To Read or Write"

rename a2a educ
label variable educ "Educational Level"

rename c1 health
label variable health "Self Reported Health"

rename f1 ebais
label variable ebais "Visits EBAIS"

rename in1 childpov
label variable childpov "Child Poor"

rename in2 childhealth
label variable childhealth "Child Health"

rename hn2 food
label variable food "Enough Money for Food"

rename a3 marital
label variable marital "Marital Status"

// Remove observations where age is greater than 60
drop if age < 60

// Create a 'female' variable based on 'sex'
gen female = .
replace female = 1 if sex == 2
replace female = 0 if sex == 1
label define female 0 "Male" 1 "Female"
label values female female
drop sex

// Remove observations where exercise is missing or equal to 9, and recode it
drop if exercise == 9
drop if exercise == .
recode exercise 1=1 2=0
label define exercise 0 "Does not exercise" 1 "Does exercise"
label values exercise exercise

// Remove observations where 'readwrite' is missing or equal to 9, and recode it
drop if readwrite == 9
recode readwrite 2=0
label define readwrite 0 "Does not know" 1 "Does know"
label values readwrite readwrite

// Label 'havesmoked' variable
label define havesmoked 0 "Have not smoked" 1 "Have smoked"
label values havesmoked havesmoked
recode havesmoked 2=0
drop if havesmoked==9

// Remove observations where 'educ' is missing or equal to 9
drop if educ == 9

// Remove observations where 'food' is missing or equal to 9, and recode it
drop if food == 9
drop if food == .
recode food 2=0
label define food 0 "Not enough money" 1 "Enough money"
label values food food

// Recode 'health' variable
recode health 1=3 2=3 3=2 4=1 5=1
label define health 1 "Low" 2 "Moderate" 3 "Good"
label values health health
drop if health == 9

// Remove observations where 'ebais' is missing or equal to 9, and recode it
drop if ebais == 9
drop if ebais == .
recode ebais 2=0
label define ebais 0 "Does not visit EBAIS" 1 "Does visit EBAIS"
label values ebais ebais

// Remove observations where 'childpov' is missing or equal to 9, and recode it
drop if childpov == 9
recode childpov 2=0
label define childpov 0 "Not poor in childhood" 1 "Poor in childhood"
label values childpov childpov

// Remove observations where 'childhealth' is missing or equal to 9, and recode it
drop if childhealth == 9
drop if childhealth == .
recode childhealth 4=1 3=2 1=3

// Recode 'marital' variable and label it
recode marital 2=1 3=1 4=1 5=1 6=1 7=1 8=0
label define marital 0 "Has not been in a relationship" 1 "Has been in a relationship"
label values marital marital

// Perform ordered probit regression and store estimates
ologit health age educ marital i.female i.exercise i.havesmoked i.readwrite i.food i.ebais i.childpov i.childhealth, nolog
estimates store ordered

// Restore estimates and calculate marginal effects for 'age'
estimates restore ordered
mtable, dydx(age)
margins, dydx(age) noci

// Restore estimates and calculate marginal effects for 'educ'
estimates restore ordered
mtable, dydx(educ)
margins, dydx(educ) noci

// Restore estimates and calculate marginal effects for 'marital'
estimates restore ordered
mtable, dydx(marital)
margins, dydx(marital) noci

// Restore estimates and calculate marginal effects for 'female'
estimates restore ordered
mtable, dydx(female)
margins, dydx(female) noci

// Restore estimates and calculate marginal effects for 'exercise'
estimates restore ordered
mtable, dydx(exercise)
margins, dydx(exercise) noci

// Restore estimates and calculate marginal effects for 'havesmoked'
estimates restore ordered
mtable, dydx(havesmoked)
margins, dydx(havesmoked) noci

// Restore estimates and calculate marginal effects for 'readwrite'
estimates restore ordered
mtable, dydx(readwrite)
margins, dydx(readwrite) noci

// Restore estimates and calculate marginal effects for 'ebais'
estimates restore ordered
mtable, dydx(ebais)
margins, dydx(ebais) noci

// Restore estimates and calculate marginal effects for 'childpov'
estimates restore ordered
mtable, dydx(childpov)
margins, dydx(childpov) noci

// Restore estimates and calculate marginal effects for 'childhealth'
estimates restore ordered
mtable, dydx(childhealth)
margins, dydx(childhealth) noci
```
