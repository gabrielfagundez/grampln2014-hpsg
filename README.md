# Entrega HPSG 2014

Entrega de "Gramáticas formales para el procesamiento del lenguaje natural" edición 2014.

Grupo 1: Gabriel Fagúndez y Enrique Rodríguez.

## Contenido de archivos

En **lexicon.tdl** intentamos mantener cada entrada del léxico con el menor tamaño posible y para eso creamos tipos que ya definen los rasgos de forma que cada lexema no deba definir mucho más que su ortografía.

En **types.tdl** se encuentra definida la jerarquía de tipos. Este se divide en:

* Categorías gramaticales: Básicamente una enumeración de las categorías que maneja nuestra gramática.
  * Nomenclatura: nombre completo de la categoría.

* Concordancia: Tipos utilizados para implementar la concordancia de número, género y persona.
  * Nomenclatura: (num|gen|per)-cat para enumerar los tipos de número, género y persona respectivamente.
                  agr-cat-(p|g|pg) para definir los rasgos de concordancia en persona, género o ambos.
                  agr-pos y agr-pos-pg utilizan los tipos agr-cat-* y definen un rasgo AGR que es utilizado por las categorías gramaticales.
 
* Partes de la oración: Estos definen la mayor parte de la jerarquía de tipos, contienen las restricciones inerentes a la categoría y sobre ellos se basan la mayoría de reglas para formar oraciones. Todas contienen: un rasgo HEAD de tipo pos, y los rasgos SPR, COMPS y MODS de tipo lista, utilizados para especificador, complementos y modificadores respectivamente.
  * Nomenclatura: abreviatura-(pl|sg)-(ml|fm): 'abreviatura' es una forma breve de la categoría gramatical que tiene en el rasgo HEAD, más la terminación en 'p' por 'phrase; pl, sg, ml y fm significan plural, singular, male y female respectivamente.

* Tipos básicos: tipo string y listas, útiles en todo el resto de la jerarquía.
  * Nomenclatura: nombre completo o comúnmente utilizado.

En **rules.tdl** se encuentran las reglas, intentamos nombrar cada una de ellas de forma intuitiva y coherente en inglés para ser consistentes con el resto del código. Las reglas de complemento y especificador son muy similares a como fueron vistas en el curso y además agregamos reglas para la conjunción, modificación y voz pasiva.

El resto de los archivos están definidos de la misma forma que las gramáticas de ejmplo g8gap o g5lex.

**Idioma:** Decidimos nombrar todos los tipos y rasgos en inglés ya que preferimos tener todo el código en el mismo idioma y no nos pareció bueno cambiar algunos nombres muy utilizados como por ejemplo "HEAD", "pos", "AGR" o "string". Sin embargo, los comentarios se encuentran en español ya que no tiene sentido redactarlos en otro idioma.

## Problemas encontrados

* Un problema que encontramos fue con los modificadores, ya que existen varios tipos y muy distintos entre ellos. Por ejemplo los modificadores adjetivales modifican un sustantivo y deben concordar con el mismo, mientras que los modificadores preposicionales no tienen por qué concordar, además de formarse de manera distinta. La solución planteada contiene una regla para cada tipo de modificador, de forma de poder tratar distinto cada caso. Otra opción era tratar de utilizar los tipos, pero lo encontramos muy difícl ya que una misma categoría podía ser modificada de muchas formas lo cual nos llevaba a agregar tipos para cada caso y repetirlos a lo largo de la jerarquía, quedando un código muy extenso y repetitivo.

* La oración "Las estudiantes retiran los libros de gramática" es considerada como válida. Para nuestra gramática no hay diferencia entre frases preposicionales, entonces interpreta "los libros" y "de gramática" como los dos argumentos del verbo ditransitivo "retiran". Entendemos que lo correcto sería interpretar "de gramática" como un modificador preposicional de "libros" y luego la oración es agramatical ya que hay un solo argumento para el verbo ditransitivo.

* Notar que esto último también sucede en la oración "Las estudiantes retiran los libros de gramática de la biblioteca", para la cual nuestra gramática da dos posibles árboles y en uno de ellos considera "de gramática" como segundo argumento de "retiran" y "de la biblioteca" como un modificador del mismo.

