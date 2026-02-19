Este tema está relacionado con [[Herencia]].


# ¿En qué consiste?
Reunir elementos que puedan considerarse pertenecientes a una misma entidad, al mismo nivel de abstracción.
# Modificadores de acceso
3 niveles:
- public - puede utilizarse desde cualquier clase de nuestra aplicación.
- private - únicamente se puede utilizar dentro de la clase donde es declarado.
- protected - el acceso a los atributos o métodos declarados de esta manera, solo podrán ser utilizados además de la clase donde es declarado, también en las subclases que le pertenezcan a la hora de aplicar herencia.

# ¿Porqué encapsular?
Evitar errores y aumentar la seguridad del programa.

# Principio de Ocultación
Cada objeto está aislado y únicamente expone una interfaz de objetos donde especifica cómo pueden interactuar con él. Protege a las propiedades de un objeto de ser modificadas por quien no tenga acceso a ello.
