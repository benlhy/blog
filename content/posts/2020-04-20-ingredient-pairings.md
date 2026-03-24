---
title: "Common Ingredient Pairings"
date: 2020-04-20T14:25:22+00:00
slug: "ingredient-pairings"
draft: false
description: "Exploring 170k recipes from Food.com and visualizing it in D3. Find out 10 most common ingredient pairs and the top 10 condiments people use! Spoiler: salt is the most common condiment. "
cover:
  image: "/images/2020/04/d3chord.JPG"
  relative: false
  hiddenInList: false
tags: ["d3", "javascript", "kaggle", "data science", "food", "Python"]
---

This is a Chord diagram made in D3 displaying the relationship between 10 most common ingredients for all the recipes on Food.com.

{{< figure src="/images/2020/04/image-4.png" alt="" caption="Common Incredient Pairings" >}}

Play with the chord here: [https://benlhy.github.io/common-food-pairings/](https://benlhy.github.io/common-food-pairings/)

---

"Ben," my mom said, "you always buy the same things."

My mom was referring to my latest purchase from the supermarket. "You know we haven't finished the celery from the last time right?" She held up a bag. "And what am I going to do with all these onions?"

"Make onion soup?" I suggested. "I like onion soup."

"You'll be the only one who drinks it," she sniffed as she packed away the products from the latest supermarket run.

So I set out to prove that my intuition for buying the same ingredients is actually grounded in data, and not just because celery and 10 sticks of carrots was all that was left in the produce section.

I used a Kaggle dataset of recipes from Food.com, processed them for the 10 most common ingredients, and arranged them in a chord diagram with D3.

# Insights

- Because these are the 10 most common ingredients, it makes sense to grab all of these if you don't know what to cook as they will most commonly feature in whatever recipe you find.
- Top 3 things to get are eggs, milk, and onions.

![](/images/2020/04/image-6.png)

3. Onions are seriously useful, with significant pairings with all the other common ingredients.

![](/images/2020/04/image-5.png)

4. Tomatoes go very commonly with onions.

![](/images/2020/04/image-7.png)

5. Egg and milk commonly appear together, which might be due to baking recipes.

6. I'm not particularly interested in finding out which recipes have onion and milk.

7. No recipe has the word "tomato" in it. The ingredient listed is always "tomatoes".

---

This started as an exercise to practice some D3 material. I saw someone else produce a rather pretty chord diagram the other day so I wanted to try to replicate it in D3. I thought it would be interesting to investigate the relationships amongst food.

The data file came from Kaggle, where someone compiled a csv of all of food.com's recipes over the last 18 years. It consists of 180k recipes, and 700k recipe reviews. Some basic lemmatizing was done on the ingredients to make sure similar instances (like onions and onion) were not counted as two separate ingredients. Common condiments were also filtered out. However, if you don't have a stocked kitchen, then these are probably the top 10 things you would want to get:

- Salt
- Butter
- Sugar
- Olive Oil
- Flour
- Garlic Cloves
- Pepper
- Brown sugar
- Baking powder
- Parmesan cheese

After filtering out the condiments, the top 10 ingredients were identified and their frequency pairs with the other ingredients were counted. This was then used as an input to the D3 chord, which fades out as you mouse over each segment, allowing you to see the relationships between each type of ingredient.

Further investigations show that actually, for eggs and milk, their top 10 pairings are not with the other ingredients in the chord diagram, but are instead with brown sugar, vanilla, and cream cheese. This indicates that they might actually belong to another category of ingredients that cater for a primarily sweet palate, rather than a savory one.

This is interesting because if we included perhaps a top 100 list of ingredients instead of a top 10 list, we might see two groups of ingredients strongly connected within themselves and only weakly combined between each group.

---

Showing my mom the results, she waved a hand. "It's all *western* recipes. Doesn't count. And two bags of onions is excessive."

Sure. Repeat after me. Onion. Eggs. Milk.
