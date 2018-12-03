# ES6 Basic Concepts

Simple ES6 concepts that you may (or may not) already know.

## Destructuring

Destructuring allows you to extract values from an array or object by specifying the elements to be extracted on LHS. Example:
```
const data = [1, 2, 3, 4]
const [a,b,c,d] = data;
```
```Console.log(a, b, c, d)``` prints => 1, 2, 3, 4

This is way more concise than the traditional way of extracting values from an array which would be as follows:
```
const data = [1,2,3,4]
const a = data[0]
const b = data[1]
const c = data[2]
const d = data[3]

```
This makes object value extraction even more concise.

Old code:
```
const restaurants = {
  name: 'tipe',
  zipcode: 12345,
  rating: 4
};

const name = restaurants.name;
const zipcode = restaurants.zipcode;
const rating = restaurants.rating;
```

New code:
```
const [name, zipcode, rating] = restaurants
```