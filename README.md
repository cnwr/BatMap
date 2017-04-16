# BatMap - The Mapper we deserve, not the one we need.
Opininated (yet another) mapper, mainly to convert between EF Entities and DTOs.

[![Coverage Status](https://coveralls.io/repos/github/DogusTeknoloji/BatMap/badge.svg?branch=master)](https://coveralls.io/github/DogusTeknoloji/BatMap?branch=master)

Let's first obey the number one rule for mappers, a benchmark (using [BenchmarkDotNet](http://benchmarkdotnet.org/)):

|      Method |      Mean |    StdDev |
|------------ |---------- |---------- |
|     BatMap :boom: | 1.9211 ms :boom: | 0.0108 ms :boom: |
| Mapster | 2.0357 ms | 0.0161 ms |
|    SafeMapper | 2.0797 ms | 0.0273 ms |
|      HandWritten | 2.1018 ms | 0.0906 ms |
|    AutoMapper | 2.8359 ms | 0.0253 ms |
|    TinyMapper | 2.9644 ms | 0.0768 ms |
| ExpressMapper | 5.1630 ms | 0.0130 ms |
|    FastMapper | 5.8862 ms | 0.0277 ms |

<sup>Results may (probably) vary</sup>
    
* Fast (enough)
* NOT over-engineered, code is really simple
* Instantiatable mapper
* Convention based, zero configuration static shortcut exists too (obviously named Mapper)
* Does not crash when faced with circular-dependencies during registration
* In fact, can resolve recurring instances to same target instance (yaay no StackOverflowException!)
* Can project IQueryable\<TSource\> to IQueryable\<TTarget\> with respect to includes (via auto-detection or with custom parameters)
* and much more...

# API
Registration with static API:
```csharp
Mapper.RegisterMap<Customer, CustomerDTO>();
```
or use an instance:
```csharp
var mapper = new MapConfiguration(dynamicMapping: DynamicMapping.MapAndCache, preserveReferences: true);
mapper.RegisterMap<Customer, CustomerDTO>();
```
Note: You don't have to register type mappings when using a MapConfiguration with Dynamic Mapping enabled (like the static API uses).


You can customize expressions for members:
```csharp
mapper.RegisterMap<Order, OrderDTO>(b => b.MapMember(o => o.Price, (o, mc) => o.Count * o.UnitPrice));
```

Map an object:
```csharp
Mapper.Map<CustomerDTO>(customer);
```
Map an enumerable:
```csharp
customers.Map<Customer, CustomerDTO>(preserveReferences: true);  // extension methods FTW!
```
Project a query:
```csharp
customerQuery.ProjectTo<CustomerDTO>(checkIncludes: true);
```
or with expanding specific navigations:
```csharp
customerQuery.ProjectTo<Customer, CustomerDTO>(c => c.Addresses, c => c.Orders);
```

Note: If you want to change mapping behavior, create a class that inherits from ExpressionProvider, override CreateMemberBinding and inject an instance of your class to MapConfiguration.

Developed with :heart: at [Doğuş Teknoloji](http://www.d-teknoloji.com.tr).