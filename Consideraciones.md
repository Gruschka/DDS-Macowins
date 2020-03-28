## Requerimientos

### Identifique 3 requerimientos principales:

1) Poder consultarle a una prenda su precio de venta
2) Poder consultarle a una prenda su tipo
3) Saber las ganancias de un determinado dia.

## Consideraciones preliminares a la explicacion de mi solucion:
- Por practicidad no inclui constructores en los diagramas
- Para poder explicar mejor, explicare en cada clase, con pseudocodigo, como pense la implementacion de los metodos.

## Explicacion de la solucion que busque

En base a esto, cree un sistema con 6 clases distintas:

#### EstadoDePrenda:
Esta clase tiene un metodo que recibe un precio (int) y devuelve el otro int segun el modificador que corresponda. 
Como hay distintos tipos de estados, y podrian haber mas en el futuro, considere oportuno hacer una subclase por cada estado existente para que cada una pueda hacer su propia implementacion del modificador de precio. La idea de esto es sacar provecho del polimorfismo para que la clase Prenda pueda abstraerse del estado de la misma y como esto afecta el precio.

#### TipoDePrenda:
Esta clase tiene dos atributos (precio y tipo) y un metodo que devuelve un entero correspondiente al tipo de la prenda. Similarmente a EstadoDePrenda, como a futuro pueden haber mas tipos de prenda con distintos precios, me parecio buena idea tener esto separado en clases, donde cada una haga un override del metodo getPrecio para poder sacar ventaja del polimorfismo en la clase Prenda, abstrayendome de cual sea el tipo de prenda y el costo que puede tener.

#### Prenda: 
Esta clase tiene dos atributos: tipo y estado que son instancias de TipoDePrenda y EstadoDePrenda correspondientemente.
A su vez, tiene dos metodos: calcularPrecio y getTipo (que son los primeros dos requerimientos del usuario).
La idea es que esta clase se permita abstraer por completo de que tipo de prenda y estado es (mencionado anteriormente) para  calcular su precio. En pseudocodigo, estaba pensando algo asi:

```
calcularPrecio() {
   precioBase = tipo.getPrecio()
   precioConEstado = estado.aplicarModificador(precioBase)
   return precioConEstado
}
```

#### FormaDePago:
Similarmente a TipoDePrenda y EstadoDePrenda, hice una clase con un metodo para aplicar un modificador a un monto (int) de precio segun corresponda y cree una subclase por cada tipo forma de pago existente. En el caso de efectivo, retornaria el mismo valor. En el caso de tarjeta, retornaria el monto al aplicar el modificador solicitado por el cliente. 

#### Venta:
Esta clase modela lo que seria una venta. Tiene un atributo que es un array de prendas, un string para la fecha y una instancia de formaDePago. Segun la premisa, cada venta tiene asociada la cantidad de prendas. Entiendo que esto no se podria hacer con un atributo ya que depende del largo del array de prendas (quizas con lazy initialization pero no se como funciona esto en Java) asi que lo hice con un metodo getCantidadDePrendas.
El metodo getMonto, pensado en pseudocodigo, haria algo asi:

```
getMonto() {   
    for each prenda in prendas {
      sumaTotal += prenda.calcularPrecio()
    } 
    
    return formaDePago.aplicarModificador(sumaTotal)
}
```
#### Gestor:
Esta clase la pense como si fuera el "modelo" del sistema. Es decir, una clase a traves de la cual yo pueda manejar las distintas ventas/prendas.  Esta clase tiene el metodo calcularGananciasDelDia que seria para cumplir el 3er requisito que pide el cliente. La idea de los otros dos metodos son usarlos en el metodo anteriormente mencionado. Por ej:

```
calcularGananciasDelDia(unDia){
  ventasEnEseDia = getVentasDelDia(unDia)
  return sumarGananciasDeVentas(ventasEnEseDia)
}
```

Un idea que tenia en mente era agregarle dos metodos getPrecioDePrenda(prenda: Prenda):Int y getTipoDePrenda(prenda: Prenda):String pero como ya lo hago en la clase Prenda no estoy seguro si no estaria delegandole a la clase 'Gestor' mas responsabilidades que la que le corresponde.

