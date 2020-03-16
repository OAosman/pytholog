# pytholog
Python module, a library to be, that enables using prolog logic in python. The aim of the library is to explore ways to use symbolic reasoning with machine learning.

future version will have implementation of logical operators and probability with logics.
pl_query function will be changed totally using probabilities and recursive backtracking but this was a good one to start.

#### Examples

```python
from pytholog import *
```
```python
# test unify function 
var = {}
print(unify(pl_expr("likes(noor, X)"), pl_expr("likes(noor, Y)"), var, {"Y": ["sausage", "steak"]}))
print(var)

#True
#{'X': ['sausage', 'steak']}
```

```python
# construct knowledge base of facts and rules
from pprint import pprint
new_kb = knowledge_base("flavor")
print("KB before filling:")
print(new_kb)
print(new_kb.db)
new_kb(["likes(noor, sausage)",
        "likes(melissa, pasta)",
        "likes(dmitry, cookie)",
        "likes(nikita, sausage)",
        "likes(assel, limonade)",
        "food_type(gouda, cheese)",
        "food_type(ritz, cracker)",
        "food_type(steak, meat)",
        "food_type(sausage, meat)",
        "food_type(limonade, juice)",
        "food_type(cookie, dessert)",
        "flavor(sweet, dessert)",
        "flavor(savory, meat)",
        "flavor(savory, cheese)",
        "flavor(sweet, juice)",
        "food_flavor(X, Y) :- food_type(X, Z), flavor(Y, Z)",
        "dish_to_like(X, Y) :- likes(X, L), food_type(L, T), flavor(F, T), food_flavor(Y, F)"])
print("\nKB after filling:")
pprint(new_kb.db)
print("\nlength: ", len(new_kb.db))

#KB before filling:
#Knowledge Base: flavor
#[]

#KB after filling:
#[likes(noor,sausage),
# likes(melissa,pasta),
# likes(dmitry,cookie),
# likes(nikita,sausage),
# likes(assel,limonade),
# food_type(gouda,cheese),
# food_type(ritz,cracker),
# food_type(steak,meat),
# food_type(sausage,meat),
# food_type(limonade,juice),
# food_type(cookie,dessert),
# flavor(sweet,dessert),
# flavor(savory,meat),
# flavor(savory,cheese),
# flavor(sweet,juice),
# food_flavor(X,Y):-food_type(X,Z),flavor(Y,Z),
# dish_to_like(X,Y):-likes(X,L),food_type(L,T),flavor(F,T),food_flavor(Y,F)]

#length:  17
```

```python
# searching for variable
pl_query(pl_expr("food_flavor(What, sweet)"), new_kb)

#[{'What': 'cookie'}, {'What': 'limonade'}]
```

```python
# answering questions
pl_query(pl_expr("food_flavor(sausage, savory)"), new_kb)
#['Yes']
```

```python
pl_query(pl_expr("food_flavor(gouda, sweet)"), new_kb)
#[]
```

```python
pl_query(pl_expr("food_flavor(Food, Flavor)"), new_kb)
#[{'Food': 'cookie', 'Flavor': 'sweet'},
# {'Food': 'limonade', 'Flavor': 'sweet'},
# {'Food': 'sausage', 'Flavor': 'savory'},
# {'Food': 'steak', 'Flavor': 'savory'},
# {'Food': 'gouda', 'Flavor': 'savory'}]
```

```python
# recommend dishes based on tastes
pl_query(pl_expr("dish_to_like(noor, What)"), new_kb)

#[{'What': 'sausage'}, {'What': 'steak'}, {'What': 'gouda'}]
```

###### P.S. I wanted to build the whole library from scratch without searching for help or hints. But actually I couldn't :D. I got stuck in the query function, I was using stack structure for backtracking but something was going wrong. all new target domains were getting the same output from previous unify function because of "=" assignment, which led to infinite number of variables. Until I decided to search and after opening lots of links I found a useful trick [here](http://www.openbookproject.net/py4fun/prolog/intro.html). The deepcopy trick of the target domain to keep it independent solved everything. so I used the trick in the query function and modified the unify function.

