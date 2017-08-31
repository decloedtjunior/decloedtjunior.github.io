
---
layout: page
sitemap: false
noindex: true
title: Algebra
permalink: /algebra/
---

Testing

```python
class Vector(object):
    def __init__(self, coordinates):
        try:
            if not coordinates:
                raise ValueError
            self.coordinates = tuple(coordinates)
            self.dimension = len(coordinates)

        except ValueError:
            raise ValueError('The coordinates must be nonempty')

        except TypeError:
            raise TypeError('The coordinates must be an iterable')

    def plus(self, v):
        new_coordinates = [x+y for x,y in zip(self.coordinates, v.coordinates)]
        return Vector(new_coordinates)
    
    
    def minus(self, v):
        new_coordinates = []
        n = len(self.coordinates)
        for i in range(n):
            new_coordinates.append(self.coordinates[i]-v.coordinates[i])
        return Vector(new_coordinates)

    def times_scalar(self, c):
        new_coordinates = [c*x for x in self.coordinates]
        return Vector(new_coordinates)


    def __str__(self):
        return 'Vector: {}'.format(self.coordinates)


    def __eq__(self, v):
        return self.coordinates == v.coordinates

```

```python
my_vector = Vector([1,2,3])
print(my_vector)
```

```python
my_vector_2 = Vector([1,2,3])
my_vector_3 = Vector([-1,2,3])
print(my_vector==my_vector_2)
print(my_vector==my_vector_3)

```

```python
my_vector_4a = Vector([8.218,-9.341])
my_vector_4b = Vector([-1.129,2.111])
my_vector_4 = my_vector_4a.plus(my_vector_4b)
print(my_vector_4)

my_vector_5 = Vector([7.119,8.215]).minus(Vector([-8.223,0.878]))
print(my_vector_5)

my_vector_6 = Vector([1.671,-1.012,-0.318]).times_scalar(7.41)
print(my_vector_6)

```


```python

```


```python
```


```python
```


```python
```