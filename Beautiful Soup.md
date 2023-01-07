Beautiful Soup yra Python biblioteka, duomenų traukimui iš HTML ir XML. BS moka naviguoti tekste pagal html blokus, klases, id ar kitus atributus. Vienas iš populiariausių įrankių web scraping užduotims atlikti. Dokumentaciją rasite čia. Diegiasi pip install beautifulsoup4.

from bs4 import BeautifulSoup

html = """
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>First HTML Page</title>
</head>
<body>
  <div id="first">
    <h3 data-example="yes">hi</h3>
    <p>more text.</p>
  </div>
  <ol>
    <li class="special">This list item is special.</li>
    <li class="special">This list item is also special.</li>
    <li>This list item is not special.</li>
  </ol>
  <div data-example="yes">bye</div>
</body>
</html>
"""

soup = BeautifulSoup(html, "html.parser")
print(type(soup))
Importuojame BeautifulSoup klasę, ir iš HTML stringo išsiverdame buljoną :) Dabar, turėdami soup objektą, galime naudotis jo funkcionalumu duomenų traukimui. Pvz.:

print(soup.body)

# <body>
# <div id="first">
# <h3 data-example="yes">hi</h3>
# <p>more text.</p>
# </div>
# <ol>
# <li class="special">This list item is special.</li>
# <li class="special">This list item is also special.</li>
# <li>This list item is not special.</li>
# </ol>
# <div data-example="yes">bye</div>
# </body>
Ištraukiame body bloką. Arba:

print(soup.body.div)

# <div id="first">
# <h3 data-example="yes">hi</h3>
# <p>more text.</p>
# </div>
Gavome pirmą div bloką bloke body.

find(), find_all()
Lygiai tą patį gautumėm panaudoję .find() metodą:

print(soup.find('div'))

# <div id="first">
# <h3 data-example="yes">hi</h3>
# <p>more text.</p>
# </div>
find() ištraukia pirmą sutampančią atkarpą, find_all išgaudo visas ir grąžina sąraše:

print(soup.find_all('div'))

# [<div id="first">
# <h3 data-example="yes">hi</h3>
# <p>more text.</p>
# </div>, <div data-example="yes">bye</div>]
Elementus taip pat galime traukti pagal bloko atributus, pvz id, class ir pan.:

print(soup.find_all(class_='special'))

# [<li class="special">This list item is special.</li>, <li class="special">This list item is also special.</li>]
Atkreipkite dėmesį, kaip užrašytas class_ parametras

jeigu naudotumėm su unikaliais atributais, pvz id (HTML dokumente gali būti tik 1 unikalus id), analogiškai galime panaudoti metodą .find().

Jeigu ieškome pagal 'custom' atributus, pvz šiame dokumente data-example="yes", naudosime tokią techniką:

print(soup.find_all(attrs={'data-example': 'yes'}))

# [<h3 data-example="yes">hi</h3>, <div data-example="yes">bye</div>]
Išrinkimas CSS stiliumi
Susipažinti su CSS selektoriais galite čia

Naudosime metodą .select():

print(soup.select('#first'))

# [<div id="first">
# <h3 data-example="yes">hi</h3>
# <p>more text.</p>
# </div>]
Naudojant select(), visada gausime sąrašą, netgi jeigu jame tik vienas narys, todėl norėdami pasiekti konkretų jo narį turėsite nurodyti indeksą.

Dar keletas pavyzdžių:

print(soup.select('.special'))

# [<li class="special">This list item is special.</li>, <li class="special">This list item is also special.</li>]
print(soup.select('div'))

# [<div id="first">
# <h3 data-example="yes">hi</h3>
# <p>more text.</p>
# </div>, <div data-example="yes">bye</div>]
print(soup.select('[data-example]'))

# [<h3 data-example="yes">hi</h3>, <div data-example="yes">bye</div>]
get_text()
Kuomet norime ištraukti patį tekstą iš HTML bloko, darome taip:

element = soup.select('.special')[0]
print(element.get_text())

# This list item is special.
Galime ir praiteruoti:

elements = soup.select('.special')
for element in elements:
  print(element.get_text())

# This list item is special.
# This list item is also special.
.name, .attrs
norėdami gauti blokų pavadinimus pagal (pvz) klasę, darysime taip:

elements = soup.select('.special')
for element in elements:
  print(element.name)

# li
# li
norėdami išgauti bloko atributus, naudosime .attrs:

elements = soup.select('meta')
print(elements[0].attrs)

# {'charset': 'UTF-8'}
Atributų reikšmes galime sužinoti tokiu būdu:

attribute = soup.select('div')[0]['id']
print(attribute)

# first
arba:

attribute = soup.find('div')['id']
print(attribute)

# first
Navigacija tarp HTML elementų
contents:
print(soup.div.contents)

# ['\n', <h3 data-example="yes">hi</h3>, '\n', <p>more text.</p>, '\n']
būdas turi trūkumą, nes į sąrašą įtraukia naujos eilutės simbolius. Panagrinėkime html atkarpą:

<body>
  <div id="first">
    <h3 data-example="yes">hi</h3>
    <p>more text.</p>
  </div>
  <ol>
    <li class="special">This list item is special.</li>
    <li class="special">This list item is also special.</li>
    <li>This list item is not special.</li>
  </ol>
  <div data-example="yes">bye</div>
</body>
body turi tris 'vaikus' - div, ol, div, vienas kitam jie yra 'broliai'(siblings), nes yra viename hierarchijos lygyje.

li = soup.find('li')
print(li.next_sibling.next_sibling) #todėl, kad '\n' užskaito už siblingą

# <li class="special">This list item is also special.</li>
suradome sekantį pirmojo li elemento 'brolį' (analogiškai galima dirbti su previous_sibling)

print(li.parent)

# <ol>
# <li class="special">This list item is special.</li>
# <li class="special">This list item is also special.</li>
# <li>This list item is not special.</li>
# </ol>
išsiaiškinome, kas tėvas.

print(li.parent.parent)

# <body>
# <div id="first">
# <h3 data-example="yes">hi</h3>
# <p>more text.</p>
# </div>
# <ol>
# <li class="special">This list item is special.</li>
# <li class="special">This list item is also special.</li>
# <li>This list item is not special.</li>
# </ol>
# <div data-example="yes">bye</div>
# </body>
sužinojome, kas senelis :)

Iki šiol navigavome per Tags, dabar pabandykime per metodus:

li = soup.find('li')
print(li.find_next_sibling())

# <li class="special">This list item is also special.</li>
li = soup.find('li')
print(li.find_next_siblings())

# [<li class="special">This list item is also special.</li>, <li>This list item is not special.</li>]
# reikia suteikti klasę not-special paskutiniam li elementui
li = soup.find('li')
print(li.find_next_sibling(class_='not-special'))

# <li class="not-special">This list item is not special.</li>
Kombinacijos
naviguojant galime kurti tokias ir panašias grandines:

li = soup.find('li')
res = li.find_parent().find_previous_sibling()['id']
print(res)

# first
res = soup.body.next_element.next_element.next_element.next_element.get_text()
print(res)

# hi
