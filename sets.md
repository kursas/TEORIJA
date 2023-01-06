Sets
Set'ai taip pat yra duomenų kolekcija, kurios pagrindinės savybės yra:

neegzistuoja eiliškumas
negali būti dublikatų
vardai = {'Jurgis', 'Antanas', 'Aloyzas', 'Martynas'}

print(vardai)
print(type(vardai))

# {'Antanas', 'Aloyzas', 'Jurgis', 'Martynas'}
# <class 'set'>
print(vardai[1])
# TypeError: 'set' object is not subscriptable
sarasas = [1, 2, 3, 4, 4, 5, 5]
setas = set(sarasas)
print(setas)
# {1, 2, 3, 4, 5}
Iteracija
for num in setas:
    print(num)

# 1
# 2
# 3
# 4
# 5
Panaudojimo galimybės
be dublikatų išmetimo iš rinkinio galima sutikrinti, kokie nariai aptikti dviejuose skirtinguose set'uose:

miestai_1 = {'Vilnius', 'Kaunas', 'Klaipėda',}
miestai_2 = {'Kaunas', 'Klaipėda', 'Šiauliai', 'Panevėžys'}

sutampantys_miestai = miestai_1 & miestai_2
print(sutampantys_miestai)
# {'Klaipėda', 'Kaunas'}
Taip pat apjungti setus į vieną, eliminuojant dublikatus:

apjungti_setai = miestai_1 | miestai_2
print(apjungti_setai)
# {'Klaipėda', 'Kaunas', 'Šiauliai', 'Panevėžys', 'Vilnius'}
Metodai
.add()

miestai = {'Vilnius', 'Kaunas', 'Klaipėda'}
miestai.add('Šiauliai')
print(miestai)
# {'Vilnius', 'Kaunas', 'Klaipėda', 'Šiauliai'}
.remove():

miestai = {'Vilnius', 'Kaunas', 'Klaipėda', 'Šiauliai'}
miestai.remove('Šiauliai')
print(miestai)
# {'Vilnius', 'Klaipėda', 'Kaunas'}
.discard():

# Tas pats, kaip remove, tik nemeta klaidos, jeigu bandome ištrinti neegzistuojantį narį
miestai = {'Vilnius', 'Kaunas', 'Klaipėda', 'Šiauliai'}
miestai.discard('Alytus')
miestai.discard('Kaunas')
print(miestai)
# {'Vilnius', 'Šiauliai', 'Klaipėda'}
.clear() - išvalo set'ą .copy() - padaro set'o kopiją
