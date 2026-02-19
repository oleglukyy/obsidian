Este tema está relacionado con [[Herencia]].

# Sobrecarga de métodos
Dentro de una misma clase podemos tener más de un método con el mismo nombre.
Estos métodos deberán recibir parámetros distintos.

# Sobreescritura de métodos 
Permite a una subclase proporcionar una implementación específica de un método que ya está definido en su superclase.
Se indica mediante la palabra reservada `@Override` 

Ejemplo: 
Método de la superclase Animal()
```
public void hacerSonido(){
	System.out.printLn("El animal hace un ruido");
}
```

Método de la subclase Perro()
```
@Override
public void hacerSonido(){
	System.out.printLn("El perro ladra");
	}
```

De esta forma, se "personaliza" el método para la sublase.


