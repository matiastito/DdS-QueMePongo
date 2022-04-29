# Qué Me Pongo

# 1era

Como usuarie de QuéMePongo, quiero especificar qué tipo de prenda estoy cargando (zapatos, camisa de mangas cortas, pantalón, etc).
```java
enum TipoPrenda{

  Categoria categoria;

  TipoPrenda(categoria)

  ZAPATO(Categoria.CALZADO),
  CAMISA(Categoria.SUPERIOR),
  PANTALON(Categoria.INFERIOR)
}
```
Como usuarie de QuéMePongo, quiero identificar a qué categoría pertenece una prenda (parte superior, calzado, parte inferior, accesorios).
```java
enum Categoria{ SUPERIOR, CALZADO, INFERIOR, ACCESORIO }
```
Como usuarie de QuéMePongo, quiero poder indicar de qué tela o material está hecha una prenda
```java
enum Material{ ... }
```
Como usuarie de QuéMePongo, quiero poder indicar un color principal para mis prendas
Como usuarie de QuéMePongo, quiero poder indicar, si existe, un color secundario para mis prendas.

```java
class Color{
  Int red;
  Int green;
  Int blue;
}
```
- Como usuarie de QuéMePongo, quiero evitar que haya prendas sin tipo, tela, categoría o color primario.
- Como usuarie de QuéMePongo, quiero evitar que haya prendas cuya categoría no se condiga con su tipo. (Ej, una remera no puede ser calzado
```java
class Prenda {
  TipoPrenda tipo;
  Material material;
  Color colorPrimario;
  Color colorSecundario;

  Prenda(TipoPrenda tipoPrenda, Material material, Color colorPrimario)
    if (tipoPrenda == null || material == null || colorPrimario == null)
      throw new PrendaException("Debe indicar tipo, material y color primario")
      
  public Categoria getCategoria(){
    return tipo.getCategoria()
  }
}

class PrendaException extrends RuntimeException
```

# 2da

Como usuarie de QuéMePongo, quiero especificar qué trama tiene la tela de una prenda (lisa, rayada, con lunares, a cuadros o un estampado).
```java
enum Trama{ LISA, RAYADA, LUNARES, CUADROS, ESTAMPADO }
```
Como usuarie de QuéMePongo, quiero crear una prenda especificando primero de qué tipo es. [1]

Como usuarie de QuéMePongo, quiero crear una prenda especificando en segundo lugar los aspectos relacionados a su material (colores, material, trama, etc) para evitar elegir materiales inconsistentes con el tipo de prenda. [2]

Como usuarie de QuéMePongo, quiero guardar un borrador de la la última prenda que empecé a cargar para continuar después. [3]

Como usuarie de QuéMePongo, quiero poder no indicar ninguna trama para una tela, y que por defecto ésta sea lisa. [4]

Como usuarie de QuéMePongo, quiero poder guardar una prenda solamente si esta es válida. [5]

```java
class PrendaBorrador [3]
    Trama trama = Trama.LISA [4]
    TipoPrenda tipoPrenda;
    Color colorPrincipal;
    Color colorSecundario;
    
    PrendaBorrador(tipoDePrenda)
       validateNonNull(tipoDePrenda)
       this.tipoDePrenda = tipoDePrenda

    conColorPrincipal(color) [2]
       validateNonNull(color)
       this.colorPrincipal = color
    
    conColorSecundario(color) [2]
       validateNonNull(color)
       this.colorSecudario = color

    deMaterial(material) [2]
       validateNonNull(material)
       tipoPrenda.validateMaterial(material) [2]
       this.material = material
       
    conTrama(trama) [2]
       validateNonNull(material)
       this.material = material

    method crearPrenda [5]
       return new Prenda(....)
```
```java
interface TipoPrenda{
  categoria()
  validateMaterial(Material)
}

class Zapato implements TipoPrenda {
  Categoria categoria = Categoria.CALZADO;
  List materialesPosibles;
  Material material;
  
  validateMaterial(Material)
    return materialesPosibles.contains(material)
}

class Camisa implements TipoPrenda {
  Categoria categoria = Categoria.SUPERIOR;
  List materialesPosibles;
 
  validateMaterial(Material)
    return materialesPosibles.contains(material)
}

class Pantalon implements TipoPrenda {
  Categoria categoria = Categoria.INFERIOR;
  List materialesPosibles;
  Material material;
  
  validateMaterial(Material)
    return materialesPosibles.contains(material)
}
```

Como usuario QueMePongo, quiero poder recibir sugerencias de uniformes armados. [1]

Como usuario QueMePongo, quiero que un uniforme siempre conste de una prenda superior, una inferior y un calzado [2]

Como administrador de QueMePongo, quiero poder configurar diferentes uniformes para distintas instituciones [3]
(Ej: para el colegio San Juan debe ser una chomba verde de piqué, un pantalón de acetato gris y zapatillas blancas, mientras que para el Instituto Johnson siempre será una camisa blanca, pantalón de vestir negro y zapatos negros) 

```java
class Uniforme [1] {
  Prenda superior [2]
  Prenda inferior [2]
  Prenda calzado [2]
}

FarbicaDeUniforme{ [3]
  fabricar() {
    return new Uniforme(fabricarParteSuperior(), fabricarParteInferior(), fabricarCalzado())
  }
  abstract method fabricarParteSuperior()
  abstract method fabricarParteInferior()
  abstract method fabricarCalzado()

FabricaDeUniformeSanJuan extendss FarbicaDeUniforme {
  fabricarParteSuperior()
    return new PrendaBorrador(Chomba).deMaterial(Material.PIQUE).conColor(new Color()).....
  
  fabricarParteInferior()
    return new PrendaBorrador(Pantalon).deMaterial(Material.ACETATO).conColor(new Color()).....
    
  fabricarCalzado()
    return new PrendaBorrador(Zapatillas).deMaterial(Material.CUERO).conColor(new Color()).....
}

FarbicaDeUniformeInstitutoJhonson extendss FarbicaDeUniforme {
  fabricarParteSuperior()
    return new PrendaBorrador(Camisa).deMaterial(Material.PIQUE).conColor(new Color()).....
  
  fabricarParteInferior()
    return new PrendaBorrador(Pantalon).deMaterial(Material.ACETATO).conColor(new Color()).....
    
  fabricarCalzado()
    return new PrendaBorrador(Zapatos).deMaterial(Material.CUERO).conColor(new Color()).....
}
```
