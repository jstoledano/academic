---
title: Encontrar el producto más grande 
summary: Cómo encontrar el producto más grande de números adyancentes.
date: "2021-12-31 14:27:48"
authors: 
  - javier
tags: 
  - swift
  - project euler
categories: desarrollo
---

Para aprender un poco de como manejar arreglos de cadenas y
caracteres en Swift, realicé un problema que más o menos dice
así:

> Dada una cadena de mil números encontrar el producto más
> grande de **trece** dígitos adjacentes.

Esto quiere decir que de una cadena de 1000 números tomamos
los números del primero al 13 (que serían del elemento 0 al
12, por ejemplo), los multiplicamos entre sí y comparamos este
resultado con el anterior para ver cuál es mas grande.

En este ejemplo, usamos los siguientes 1,000 números generados
de forma aleatoria.

```
    23377796661007193501261043060813089411852476794737
    80413012317548826352074571145201287743855763207380
    18289493824811970706504947035623696589177036118345
    08135112564412001348912308298836017490080620601646
    14027173677132411381984807991084865311637607974147
    41141784480862471934157122559507779183247514435364
    24426830422303707821688274688511615310311675701320
    24949662378508202440620472821085255510995663583220
    18026535592195291102805186702202481758309181331700
    58053909544294194751760333177437004689480175102869
    71835199860174618721511721055808178058926675992016
    28502351573211951399405947887703568452031251365528
    26425676865779726696440132957483862277919448395602
    89540675110208952633924027929694428260289855175517
    94327112138284503577995929042666691072360845682240
    83612839448793349185063072163303110523917957326031
    32866227211148605460815438688873553945358220466637
    19380581371950031852943450744685897320547645783659
    58402852135348888750866191015544110690933306285745
    45982782368493360446385135850694844047820449322406
``` 

Así como la ven, la cadena tiene 20 líneas, cada una empieza con 4 espacios en blanco y termina con un salto de línea.

Para poder eliminar esos espacios en blanco y los saltos de línea `\n`, usaremos el método `replacingOccurrences()` del objeto `String`, que necesita la biblioteca `Foundation` de swift.

Esta función se usa así `replacingOccurrences(of: String, with: String)` y devuelve un objeto `String`. Luego, como tenemos que hacer dos reemplazos, al resultado de la primera sustitución aplicamos nuevamente este método para hacer la segunda. 

```swift
import Foundation

var serie:String  = """
    23377796661007193501261043060813089411852476794737
    80413012317548826352074571145201287743855763207380
    18289493824811970706504947035623696589177036118345
    08135112564412001348912308298836017490080620601646
    14027173677132411381984807991084865311637607974147
    41141784480862471934157122559507779183247514435364
    24426830422303707821688274688511615310311675701320
    24949662378508202440620472821085255510995663583220
    18026535592195291102805186702202481758309181331700
    58053909544294194751760333177437004689480175102869
    71835199860174618721511721055808178058926675992016
    28502351573211951399405947887703568452031251365528
    26425676865779726696440132957483862277919448395602
    89540675110208952633924027929694428260289855175517
    94327112138284503577995929042666691072360845682240
    83612839448793349185063072163303110523917957326031
    32866227211148605460815438688873553945358220466637
    19380581371950031852943450744685897320547645783659
    58402852135348888750866191015544110690933306285745
    45982782368493360446385135850694844047820449322406
"""
serie = serie
    .replacingOccurrences(of:" ", with:"")
    .replacingOccurrences(of: "\n", with: "")
```

Ahora que tenemos una sola cadena, vamos a crear un arreglo de mil elementos, convirtiendo cada caracter en un entero y lo agregaremos a este arreglo.

```
var integers:[Int] = []
for number in serie {
    if let i:Int = Int(String(number)) {
        integers.append(i)
    }
}
```

Usamos esta condicional `if let i:Int = Int(String(number))` porque la función `Int()` devuelve un opcional (que tal que le pasamos como argumento una letra o un caracter no imprimible) y tenemos que asegurar la conversión. Si tiene éxito, el entero obtenido lo agrega al arreglo.

Ahora necesitamos algunos datos. Por ejemplo, para hacer este programa genérico, el número de datos lo vamos a obtener contando los elementos del arreglo.

```swift
let n:Int = integers.count
```

El número de elementos adjacentes `m`, lo volvemos una constante.

```swift
let m:Int = 13
```

Y definimos la respuesta al problema inicianizándola en 0.

```swift
let answer:Int = 0
```

Para obtener nuestra respuesta, iniciamos el recorrido de nuestro arreglo de enteros, desde el primer elemento hasta `n-m`. La razón de este límite es que el último grupo de dígitos adjacentes empieza en el elemento `n-m` y termina en `n`, si lo hacemos hasta `n` el último elemento del último grupo sería `n+m` y estaría fuera de rango.

Es decir, vamos a recorrer el arreglo desde el elemento 0 hasta el 987, el último grupo de adjacentes va a empezar desde el elemento 987 y el último (987+13) será el 1000. 

Luego usamos el método `reduce()` del objeto `Array`. Este [método `reduce(_:_:)`](https://developer.apple.com/documentation/swift/array/2298686-reduce) aplica combina los elementos del arreglo usando la _closure_ dada. Este método tiene dos argumentos, el primero es el valor inicial (que en este caso es el 1, la identidad multiplicativa) y la _closure_. Esta función es muy simple, multiplica el primer elemento del arreglo con el valor inicial y se llama repetidamente con el valor anterior y el siguiente elemento del arreglo. Cuando se agotan los elementos del arreglo, se devuelve el valor obtenido.

Al final, comparamos la respuesta con el producto para obtener el valor mayor. Cuando termina el ciclo, obtenemos la respuesta buscada.

```swift
for i in 0..<(n-m) {
    let subset:[Int] = Array(integers[i...(i+m-1)])
    let product = subset.reduce(1, {x, y in x*y})
    answer = max(answer, product)
}
```

Así se ve el programa final.

```swift
import Foundation

var serie:String  = """
    23377796661007193501261043060813089411852476794737
    80413012317548826352074571145201287743855763207380
    18289493824811970706504947035623696589177036118345
    08135112564412001348912308298836017490080620601646
    14027173677132411381984807991084865311637607974147
    41141784480862471934157122559507779183247514435364
    24426830422303707821688274688511615310311675701320
    24949662378508202440620472821085255510995663583220
    18026535592195291102805186702202481758309181331700
    58053909544294194751760333177437004689480175102869
    71835199860174618721511721055808178058926675992016
    28502351573211951399405947887703568452031251365528
    26425676865779726696440132957483862277919448395602
    89540675110208952633924027929694428260289855175517
    94327112138284503577995929042666691072360845682240
    83612839448793349185063072163303110523917957326031
    32866227211148605460815438688873553945358220466637
    19380581371950031852943450744685897320547645783659
    58402852135348888750866191015544110690933306285745
    45982782368493360446385135850694844047820449322406
"""
serie = serie
    .replacingOccurrences(of:" ", with:"")
    .replacingOccurrences(of: "\n", with: "")

var integers:[Int] = []
for number in serie {
    if let i:Int = Int(String(number)) {
        integers.append(i)
    }
}

let n:Int = integers.count
let m:Int = 13
var answer:Int = 0
for i in 0..<(n-m) {
    let subset:[Int] = Array(integers[i...(i+m-1)])
    let product = subset.reduce(1, {x, y in x*y})
    answer = max(answer, product)
}

print(answer)
```

Este es uno de los problemas del [__Proyecto Euler__](https://projecteuler.net/about). 


