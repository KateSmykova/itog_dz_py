# itog_dz_py
from decimal import Decimal, getcontext
from sympy import *
getcontext().prec = 20
# from sympy.abc import x
x = Symbol("x")
f = Function('f')
# Корни на промежутке от -50 до 50
f = collect((-12 * x ** 4 * sin(cos(x)) - 18 * x ** 3 + 5 * x ** 2 + 10 * x - 30), x)
roots = sorted(list([Decimal(str(r)) for r in {nsolve(f, x, i, prec=15, verify=False) for i in range(-50, 51)}]))
' '.join([str(root) for root in roots])

def fun(subx):
    return f.subs(x, subx)

def decimal_range(x, y, jump):
  while x < y:
    yield x
    x += Decimal(jump)

extremums = []
f_positive_ranges = []
f_negative_ranges = []

for i in range(len(roots)-1):
    points = [{'x': x, 'f': fun(x)} for x in decimal_range(roots[i], roots[i+1], '0.1')]

    fs = [p['f'] for p in points]

    if fs[len(fs)//2] > 0:
        extremums.append([p for p in points if p['f'] == max(fs)][0])
        f_positive_ranges.append((roots[i], roots[i+1]))
    else:
        extremums.append([p for p in points if p['f'] == min(fs)][0])
        f_negative_ranges.append((roots[i], roots[i+1]))

print('Функция возрастает на интервалах Х:')
for i in range(len(extremums)-1):
    if (extremums[i]['f'])<0:
        print(f'{extremums[i]["x"]} -> {extremums[i+1]["x"]}')

print('Функция убывает на интервалах Х:')
for i in range(len(extremums)-1):
    if (extremums[i]['f']) > 0:
        print(f'{extremums[i]["x"]} -> {extremums[i+1]["x"]}')

from sympy.plotting import plot
plot(f, (x, -50, 50))

print('Вершины и низины:\n', *[('x:'+str(e['x']), 'f:' + str(e['f'])) for e in extremums], sep='\n')

print(*[(str(r[0]), str(r[1])) for r in f_positive_ranges], sep='\n')

print(*[(str(r[0]), str(r[1])) for r in f_negative_ranges], sep='\n')

