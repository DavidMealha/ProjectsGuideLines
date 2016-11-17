#Python Guidelines#

##String Concatenation##
```python
phrase = "This first string is boring"
new_phrase = ""

#c is each character of the phrase
for c in phrase:
    if(c == 'o'):
        #another way to do it - new_phrase.append("X")
        new_phrase += 'o'
    else:        
        #another way to do it - new_phrase.append(c)
        new_phrase += c
        
print phrase
print new_phrase
#if the other way is used, we will have an array of chars, so we need to join them to make a string
#print ''.join(nova_frase)
```
##Defining a class##
```python
class Car:
    #class constructor, called when creating a new object
    def __init__(self, brand, model, year, horsepower):
        self.brand = brand
        self.model = model
        self.year = year
        self.horsepower = horsepower

    def is_powerful(self):
        return self.horsepower > 200
```

##Object Instanciation/Creation##
```python
object = Car('Porsche', '911', 2017, 510)
```
