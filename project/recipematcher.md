---
layout: page
title: Recipe Matcher
subtitle: Matching Ingredients to similar recipes
---

Check out the program hosted online [here!](http://recipefinder.pythonanywhere.com/)

### Background 

One of the things I wanted to practice and learn was working with text data and learning about NLP (natural language processing) techniques.  I also I like food and sorta like cooking, so I thought it would be cool to come up with a program that finds similar based on a list of ingredients that you provide.  So immediately two things come to mind.

1. How to represent ingredients as vectors
2. How to define 'similar'

First I needed a list of recipes and their associated ingredients so I could draw similar recipes to an input list of ingredients .So I found a [large dataset](https://www.reddit.com/r/datasets/comments/94awca/thousands_of_recipes_from_epicurious_bbc/) of recipes that included their title, ingredients, and instructions that were scraped from various food websites.  The dataset compiler used this [recipe scraper](https://github.com/hhursev/recipe-scrapers).

I used 3 datasets that were scraped from the BBC food section, Allrecipes.com, and epicurious.com.  In total there about 130,000 unique recipes.

#### Model Design

First I decided to model the recipe into a bag of words model.  We we can get a list of all ingredients we can simply encode recipe as $p$-dimesional vector where $p$ is the number of unique ingredients.  So for a certain recipe $i$ can be modeled as:

$$ X_{i,j} =
 \begin{cases} 
      1 & \text{if ingredient $j$ is present in recipe $i$} \\
      0 & \text{otherwise} \\
   \end{cases}
$$

Next to find the similarity between two recipes, I used the [jaccard index](https://en.wikipedia.org/wiki/Jaccard_index).  The similary index between recipe vectors $A$ & $B$ is defined as:

$$ J(A,B) = \frac{|A \cap B|}{|A \cup B|} $$

The numerator represents the number of shared ingredients between the 2 vectors, while the denominator represents the total number of unique ingredients between both the recipes. 

Using the recipe matrix above, two recipes represented by $X_A$ and $X_B$ share an ingredient share an ingredient $j$ only if $X_{A,j}$ = $X_{B,j} = 1$  

The jaccard index should an appropriate measure of similarity because it of course measures the number of shared ingredients but also at the same time controls for the sizes of the 2 recipes.

For example if a recipe A, recipe B, and recipe C all share 2 ingredients, but recipe A contains 3 ingredients, recipe B contains 4 ingredients, and recipe 10 contains 10 ingredients.  I think it's intuitive to say that recipe A is closer to recipe B than it is to recipe C since the proprtion of shared ingredients to the total number of ingredients is much larger between A & B compared to A & C.

#### Parsing through the data

So the first thing I figured I had to do was to parse through the list of ingredients and extract the actual ingredient.

Take a look at this sample recipe:

* 2 large onions, chopped into cubes
* 1 tablespoon of salt

Looking at this we would to extract "onion" from the first ingredient and "salt" from second ingredient. The word "large" was relevant to the ingredient, since a large onion was still an onion regardless.

I looked at how other projects parsed through ingredients and saw that a most recipes followed a format of:

[quantity] [unit] [ingredient] , [preparation instructions].

So I went through the ingredients, I removed:
* all quantity words, removed all measurement and size quantifying words,
* everything after the first comma 
* everything within parentheses since they usually indicate preparation instructions or alternatives
* puncuation


Hopefully this should have removed most irrelevant terms
I used the nltk package and lemmatized all the words, to make sure all the words were singular, ie. we want "onions" to "onion"

To make sure we get only relevant ingredients, I utilized the nltk POS (part of speech) tagger to remove adverbs or verbs, and to keep only adjectives and nouns.

Finally I was left with a list of ingredients which I converted the list to a single string ready to be converted into a text matrix.  I used the sci-kit learn's [Count Vectorizer](https://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.text.CountVectorizer.html).  The vectorizer takes each individual word within the string separated by space as a separate word.

A problem I came across was with dealing with compound words and adjectives.  For example take: baking soda.  It's comprised of two words, "baking" and "soda" each on their own has different meanings.  The count vectorizer would separate the two words as two seprate ingredients which I didn't think was appropriate since I another recipe containing pop soda could match with the "soda" part in baking soda.  So for certain ingredients like "sesame oil" and "soy milk" I combined them into a single word to make sure we captured the meaning of the ingredient.  For ingredients like "white onion,"  some recipes just called it "onions" while others made the color distinction between onions, so I just left them as two separate words.



