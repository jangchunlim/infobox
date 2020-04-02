가중치 알고리즘
===========

import random
my_list = ['A'] * 5 + ['B'] * 5 + ['C'] * 90
random.choice(my_list)


A => 5%
B => 5%
C => 90%

import random

x = random.randint(1, 100)

if x <= 5:
    return 'A'
elif x > 5 and x <= 10:
    return 'B'
else:
    return 'C'
