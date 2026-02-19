Este tema está relacionado con [[Poo]].

Interfaz gráfica -> conexión entre la persona que ve la aplicación y el código

Interfaz -> del lado de backend nos permite interconectar distintas partes del programa. Por definición son colecciones de métodos abstractos, donde no tenemos atributos o tenemos atributos constantes.
Se puede implementar la cantidad de veces necesarias-> herencia múltiples.

TODOS los métodos deben ser abstractos.

# Declaración 

![[Pasted image 20251129104147.png]]

Ya NO se trata de class, sino de interface

A la hora de declarar un método dentro de la interfaz, java da por hecho que se trata de un método abstracto

# Declaración de clases que hagan uso de una interfaz
Por el contrario de cuando se trata de una superclase, en lugar de implementar la palabra reservada `extends`, usaremos `implements`.
Como hemos mencionado, podemos implementar en una clase más de una interfaz.

![[Pasted image 20251129105306.png]]

