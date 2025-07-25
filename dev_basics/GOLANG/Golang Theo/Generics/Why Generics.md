## Generics Reduce Repetitive Code

You should care about generics because they mean you don’t have to write as much code! It can be frustrating to write the same logic over and over again, just because you have some underlying data types that are slightly different.

## Generics Are Used More Often in Libraries and Packages

Generics give Go developers an elegant way to write amazing utility packages. While you will see and use generics in application code, I think it will be much more common to see generics used in libraries and packages. Libraries and packages contain importable code intended to be used in _many_ applications, so it makes sense to write them in a more abstract way. Generics are often the way to do just that!

## Why Did It Take So Long to Get Generics?

Go places an emphasis on simplicity. In other words, Go has purposefully left out many features to provide its best feature: being simple and easy to work with.

According to [historical data from Go surveys](https://go.dev/blog/survey2020-results), Go’s lack of generics has always been listed as one of the top three biggest issues with the language. At a certain point, the drawbacks associated with the lack of a feature like generics justify adding complexity to the language.