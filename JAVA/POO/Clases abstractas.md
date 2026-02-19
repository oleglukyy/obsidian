Este tema está relacionado con [[Herencia]].

![[Pasted image 20251129010831.png]]

# No pueden ser instanciadas

Generalmente declara la existencia de métodos  pero no su implementación, siendo así una superclase. Se utilizan como molde para crear otras clases, mediante el proceso de herencia
Al menos uno de sus métodos debe ser abstracto -> vamos a tener la definición del método. Por ejemplo: correr() -> pero no vamos a decir CÓMO corre.
Una clase NO PUEDE heredar de varias clases abstractas a la vez.
Nos expresan el SER de un objeto.

# ¿Cuándo se usan?

Ejemplo: Programa de figuras geométricas.
Todas las figuras nos permitirán calcular su área , perímetro, radio, etc. Pero cada una lo hará de una forma distinta.

Tendrán atributos comunes como `posición X`, `posición Y`.
También tendrán el método de `calcularArea()`, pero será distinto para cada uno de ellos.

# Declaración de los atributos abstractos

![[Pasted image 20251129101237.png]]

Estos deberán ser protected, puesto que únicamente sus subclases pertenecientes deberán tener acceso a ellos.

# Declaración del método abstracto

![[Pasted image 20251129100938.png]]

Declaramos el método, pero no ponemos su implementación

# Implementación en las subclases

![[Pasted image 20251129101852.png]]