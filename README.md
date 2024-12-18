# XpathLocatorStrategies

This is a set of notes for Xpaths, from the Udemy Course ['XPath locators for Selenium'](https://www.udemy.com/course/xpath-locators-for-selenium/) by 'Dmitry Shyshkin'.

## Sección 1 - Recursos útiles del curso.

<details>

<summary> Seccion 1: Introducción - </summary>

<details>

<summary>Páginas Web</summary>

1. [Test Login](https://practicetestautomation.com/practice-test-login/)
2. [Test Exceptions](https://practicetestautomation.com/practice-test-exceptions/)
3. [GitHub Repo - XPath-locators-for-Selenium](https://github.com/dimashyshkin/XPath-locators-for-Selenium)

</details>

<details>

<summary>Plugins para facilitar la vida.</summary>

1. FireFox

   - xPath Finder

2. Chrome
   - Ranorex Selocity
   - SelectorsHub
   - CSS and XPath checker
   - Relative Xpath Helper
   - TruePath
   - Chro Path
   - Selectors Hub - XPath helper

</details>

### Shortcuts de Teclado

<details>

<summary>TRUCOS</summary>

En el explorador Google CHROME:

1. Abrir el Inspector de Elementos, para ver el Document Object Model (DOM).

   1. > CTRL + SHIFT + I
   2. > F12
   3. > Click Derecho > Inspeccionar

2. Abrir directamente abre el Selector de WebElements en el DOM.
   1. > CTRL + SHIFT + C
   2. > CTRL + F - Abre un buscador para validar Xaths

</details>

</details>

---

## Sección 2

<details>

<summary>Seccion 2: XPath Basics</summary>

### Xpath Meaning

<details>

<summary>Xpath significa</summary>

XML Path  Language it's a Query Language for selecting **nodes** from a XML document.

</details>

### XPATH Formula:

<details>

<summary>Xpath explicado</summary>

```
//tag[@attribute="value"]
```

</details>

### Estrategias de localización:

<details>

<summary>9 estrategias de localizacion</summary>

1. By locator = By.id("id_del_elemento");

2. By locator_name = By.name("name_elemnt");

3. By locator_className = By.className("clase_elemento");

4. By locator_tagName = By.tagName("tag");

5. By locator_linktext = By.linkText("texto_link");

6. By locator_partialLinkText = By.partialLinkText("parte_texto");

7. By locator_cssSelector = By.cssSelector("input[name='q']");

8. By locator_Xpath = By.xpath("//input[@name='q']");

9. JavaScript

```
JavascriptExecutor js = (JavascriptExecutor) driver;

WebElement searchBox = (WebElement)js.executeScript("return document.getElementsByName('q')[0]");
```

</details>

### Inspector de Elementos

<details>

<summary>Todo es relativo</summary>

- Usando el elemento 'Submit', hacia arriba, tiene 2 'hermanos' y 1 'padre.

![alt text](image-13.png)

- Usando el 'Form', hacia abajo, tiene 3 hijos.


![alt text](image-15.png)


</details>

### Terminlogía de los XPaths

<details>

<summary>Conceptos</summary>

1. Tipos de **nodos** en Xpath:
   - Element
   - Attribute
   - Text
   - Document
   - etc..
2. **Atomic Values**:
   - Nodos SIN hijos ni Padres.
3. **Relaciones** entre Nodos:
   - Padre
   - Hijo
   - Hermano
   - Ancestro
   - Descendiente
4. Tipos de XPath:
   1. Absolutos
      - Manera directa de localizar un elemento
      - Comienzan desde el Origen del DOM.
      - No son robustos ni confiables  (se arruinan con cualquier cambio en la página antes de nuestro elemento)
   2. Relativos (los que debemos usar)
      - Comienzan desde un Nodo que nosotros elegimos.
      - Mas cortos y fáciles de leer.
      - Estrategia de localización mas robusta.

</details>

### Sintaxis Básica de XPath

<details>

<summary>XPath Relativo e Hijo</summary>

- Tenemos un elemento 'PADRE' tipo 'Div' con un 'Id'

  - Dentro tiene otras cosas, pero las que nos interesa es el INPUT

- Elemento PADRE, usado como `XPath Relativo` o referencia:

```
//div[@id='row2']
```

![alt text](image-2.png)

- Con este punto de partida, nos dirigimos al elemento HIJO, el único tipo `INPUT`.
- Como no hay otro similar, el XPath queda:

```
//div[@id='row2']/input
```

![alt text](image-3.png)

</details>

### Diferencia entre '/' vs '//' vs './' vs '..//'

<details>

<summary>Diferencias clave en Nodos</summary>

---

1.  / - una diagonal
    - Usado al inicio del XPath, selecciona un elemento RAÍZ.
    - Usado para crear XPaths Absolutos
    - Abreviación de 'Child Node' - Nodo Hijo

#### Ejemplo 1:

```
/HTML/Body
```

El elemento raíz `HTML` contiene 2 hijos, `HEAD`y `BODY`

![alt text](image-4.png)

---

2.  // - doble diagonal

    - Abreviatura de 'descendiente' o 'Self Node'.
    - Para 'XPaths Relativos'
    - Selecciona un elemento en cualquier lugar de la página.

#### Ejemplo 2:

- Usando un elemento relativo, vamos buscando todos los elementos hijos en el árbol del DOM hasta encontrar los de tipo 'INPUT'.

- En este caso son 2 distintos.

```
//div[@id='rows']/div/div/input
```

![alt text](image-5.png)

**NOTA:** podemos `anidar` NODOS RELATIVOS para usar el Nodo Padre como nuevo RAIZ.

El XPath anterior, se puede reescribir como

```
//div[@id='rows']//input
```

Obteniendo el mismo resultado:

- **Dentro** del DIV element con ID = 'rows' (`Nodo Relativo`), **BUSCA** en cualquier lugar **elementos descendiente** con TAG tipo `INPUT`.

![alt text](image-6.png)

---

3. Uso de .// con 'Context Element'

- Este necesita un 'Context Element' en SELENIUM para funcionar, de otro modo no hace nada en el Navegador.
- Este es el código

```java
package com.practicetestautomation;

import java.util.List;

import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;
import org.testng.Assert;
import org.testng.annotations.Test;

public class RelativeXpathTests extends BaseTest {

	private String url = "https://practicetestautomation.com/practice-test-exceptions/";

	@Test(priority = 1)
	public void relativeXpathTest() {
		driver.get(url);

		// Find and click 'Add' button to add second row
		WebElement addButton = driver.findElement(By.id("add_btn"));
		addButton.click();

		// Use Explicit wait to wait for the second row to be visible
		WebDriverWait wait = new WebDriverWait(driver, 15);
		wait.until(ExpectedConditions.visibilityOfElementLocated(By.xpath("//div[@id='row2']")));

		// Get list of all rows
		List<WebElement> rows = driver.findElements(By.xpath("//div[@id='row2']/*[@id='save_btn']"));

		String actualText = null;

		// Iterate over each row in the list - this 'row' are the CONTEXT ELEMENT
		for (WebElement row : rows) {
			// Get text from label element for each 'row'
			String label = row.findElement(By.xpath(".//label")).getText(); // Look for //label inside CONTEXT ELEMENT
			System.out.println("Label text is: " + label);

			if (label.equals("Row 2")) {
				// If label equals Row 2, type Sushi into input field
				System.out.println("Typing 'sushi' into input field");
				row.findElement(By.xpath(".//input")).sendKeys("Sushi"); // Look for //label inside CONTEXT ELEMENT

				// Save new value by pushing Save button
				driver.findElement(By.xpath("//div[@id='row2']/*[@id='save_btn']")).click();

				// Get new value to use in the assertion
				actualText = row.findElement(By.xpath(".//input")).getAttribute("value"); // Look for //input inside CONTEXT ELEMENT
				break;
			}
		}
		Assert.assertEquals(actualText, "Sushi");
	}
}
```

</details>

### Diferencia entre 'Position' e 'Index'

<details>

<summary>XPath con múltiples resultados, y los métodos para definir elementos.</summary>

- Cuando encontramos una página que nos arroja varios resultados para un XPath, requerimos definir cuál es el elemento que queremos.

- Por ejemplo, en la página bajo prueba, si elegimos un elemento `H5` nos encuentra 6 resultados.

- `NOTA`: En los XPaths, los índices comienzan en '1', no en 'CERO' como los lenguajes de programación.

```
//H5
```

![alt text](image-7.png)


</details>


---


<details>

<summary>INDEX</summary>

- Un Índice XPath comienza en '1'

```
//H5[2]
```

- Para elegir un único elemento XPath, debemos encerrar entre `paréntesis cuadrados` la dirección del elemento.

![alt text](image-8.png)


- Para XPaths con `atributos`, se debe encerrar entre `paréntesis` el XPath completo y al final entre `corchetes` se coloca el índice.

```
(//div[@class='row'])[2]
```

![alt text](image-20.png)

---

- Ahora, digamos que dentro de este elemento, queremos el botón `REMOVE`. 
- Podemos usar la búsqueda de elementos hijos de este elemento.

```
(//div[@class='row'])[2]/button[3]
```

![alt text](image-21.png)

- Pero, podemos mejorar el XPath, teniendo en cuenta una consideración del método `INDEX`.

- `OJO` : si escribimos este comando, nos dará como resultado que efectivamente encontró los 6 botones, que coinciden con el criterio de búsqueda:

![alt text](image-23.png)

```
//div[@class='row']/button
```

![alt text](image-22.png)


- `PERO` si ponemos los corchetes, NO va encontrar nuestro elemento.

```
//div[@class='row']/button[6]
```

- Por que, XPath va a buscar dentro del `PRIMER` elemento que cumpla la condición de ser DIV y tener clase ROW, el sexto elemento tipo BUTTON.

- Y sabemos por el DOM que sólo contiene 3 elementos.

- La sintaxis correcta, es nuevamente, envolver TODO el XPath entre `paréntesis` para que PRIMERO busque TODOS los elementos tipo BUTTON dentro de TODOS los DIV con clase ROW.



```
(//div[@class='row']/button)[6]
```

- Y ahora sí, encontrados TODOS, elegir ya sea el último, o el índice '6'.

![alt text](image-24.png)



```
(//div[@class='row']/button)[last()]
```

- De esta manera, encerrando entre paréntesis todo el XPath para buscar primero todos los elementos, también funciona el truco del comando 'last()'.

</details>



---

<details>

<summary>position()</summary>

- Así como el Index, el comando `[position()=X]` también arroja un único elemento XPath por `punteros`.

```
//H5[position()=3]
```

- Si hacen lo mismo, ¿para qué sirve?

![alt text](image-9.png)

Por que, a diferencia de INDEX, con `POSITION` podemos jugar con los `punteros`.

![alt text](image-10.png)

---

---


- Imaginemos que por alguna razón NO necesitamos el primer resultado, sólo los que dicen 'Test Case X: ...'

- SABIENDO que el elemento que NO queremos está en la `primera posición`, podemos excluirlo de la búsqueda con XPath.


```
//H5[position()!=1]
```

- De esta manera podemos elegir todos los elementos `H5`, excluyendo el primero, y ahora únicamente encontrará 5 elementos.

![alt text](image-16.png)

- `POSITION`es un método versátil, con el que podemos conseguir el mismo resultado combinando los operadores.

![alt text](image-17.png)

</details>


---

<details>

<summary>last()</summary>

- Si no sabemos el número de elementos, pero estamos seguros de que el que necesitamos es el último, podemos usar `LAST` como comando.

```
//H5[last()]
```

- Y seleccionará el último elemento del tipo que le indiquemos:

![alt text](image-18.png)

- Siendo posible además, elegir en reversa desde esa posición, similar al sistema de `arrays`de Python.

```
//H5[last()-1]
```


![alt text](image-19.png)


</details>

### XPaths Functions - TEXT

---

<details>

<summary>text()</summary>

- La fórmula para encontrar un elemento por su TEXTO es:

```
//tag[text()='value']
```

- PERO, se debe escribir el texto COMPLETO tal cual aparece en el DOM.

```
//h5[text()='Create list of your favorite foods']
```

![alt text](image-25.png)

- Otra manera de lograr el mismo resultado es:

```
//h5[normalize-space()='Create list of your favorite foods']
```

- Funciona para cualquier tipo de elemento que contenga TEXTO.

```
//a[text()='Selenium WebDriver with Java for beginners program']
```

![alt text](image-26.png)

</details>

---

### XPath Functions - contains

<details>

<summary>contains()</summary>

- Permite seleccionar un elemento por el contenido `parcial` de un `atributo` en el DOM.
- Esta función es útil para `elementos parcialmente dinámicos`.


Por ejemplo, si un ID tiene valores `parcialmente dinámicos`.

![alt text](image-27.png)

- La fórmula es:

```
//tag[contains(@attribute,'partial value')]
```

![alt text](image-28.png)


- También es útil con, por ejemplo, `etiquetas` tipo `CLASS` que contienen atributos muy largos.
- Podemos encontrar el mismo elemento sin necesidad de escribir todo el `valor`.

```
//body[contains(@class,'page-template-test_exceptions')]
```

![alt text](image-29.png)

- También podemos encontrar varios elementos que comparten cierta característica.

```
id="edit_btn"
id="save_btn"
id="add_btn"
```

![alt text](image-30.png)

```
$x("//button[contains(@id,'_btn')]")
```

![alt text](image-31.png)

- Finalmente, podemos encontrar un elemento por su `texto parcial` usando XPath.
- En lugar del atributo, usamos la función `text` dentro de `contains` de la siguiente manera.

![alt text](image-32.png)

- Escribimos sólo una parte del texto del elemento.

```
$x("//p[contains(text(),'This page is created')]")
```

![alt text](image-33.png)

- FIN

</details>

---

### XPath Function - Starts with

<details>

<summary>starts-with()</summary>

- Esta función es similar a `contains()`, pero es mas específica en su sintaxis.
- Requiere, como su nombre lo indica, únicamente el inicio del `valor` del `atributo`.
- La fórmula es:

```
//tag[starts-with(@attribute,'beginning')]
```

Por ejemplo, para el elemento:

![alt text](image-34.png)

- Con código HTML:

```
 <input type="text" class="input-field" value="Pizza" disabled="true">
```

- Podemos usar el `valor` del  `atributo` 'CLASS'
- Y el XPath con la función `starts-with()` quedaría de la siguiente manera:

```
//input[starts-with(@class,'input')]
```

![alt text](image-35.png)

- De la misma manera que con `contains()`, podemos usar esta función para encontrar `TEXTO`.

![alt text](image-36.png)

- Por ejemplo, para este elemento, podemos usar el siguiente XPath.

```
//p[starts-with(text(),'This page')]
```

![alt text](image-37.png)

</details>

---

### XPath Function - NOT

<details>

<summary>not()</summary>

- La fórmula es muy sencilla:

```
//tag[not(anything we learned before)]
```

- Busca todos los elementos que cumplan la condición, PERO IGNORA el que te estoy definiiendo.

![alt text](image-38.png)

- Por ejemplo, si buscamos únicamente la etiqueta `botón` encontraremos 5 resultados:

![alt text](image-39.png)

- Si elegimos uno en particular, el resulado es ese botón:

```
//button[@id='edit_btn']
```

![alt text](image-40.png)

---

- Ahora, si usamos la función `not()`, el resultado son los 5 elementos originales, `excepto` el que definimos.

```
//button[not(@id='edit_btn')]
```

![alt text](image-41.png)


---

- Podemos excluir cualquier elemento, con cualquier método que aprendimos:

```
//H5
```

- Recordemos que en el ejercicio del `position()` exlcuímos el primer elemento.

- Eso, también se puede lograr con la función `not()` de varias maneras.

```
//H5[not(position()=1)]
//H5[not(text()='Create list of your favorite foods')]
//H5[not(contains(text(),'favorite foods))]
```

![alt text](image-42.png)

</details>



</details>

---

## Seccion 3

<details>

<summary>Seccion 3: Advanced XPath</summary>

---

### Operador - OR

<details>

<summary>or</summary>

- A veces, un mismo loclaizador se necesita escribir distinto dependiendo del navegador.

![alt text](image-45.png)

- Para esto, existen distintos operadores de XPath, pero sólo `OR` y `AND` son útiles en Automation.

![alt text](image-44.png)

- Con estas herramientas, podemos hacer estrategias de localización complejas.

- Por ejemplo, de nuestra página muestra, podemos elegir múltiples elementos.

```
//button[@name='Add']
```

![alt text](image-46.png)

```
//button[@name='Remove']
```

![alt text](image-47.png)

- En la página, en total, hay 8 botones:

![alt text](image-48.png)

- Entonces, si ambos comparten `etiqueta` para elegir `ambos` debemos agrupar el `predicado` del XPath.

```
//button[@name='Add' or @name='Remove']
```

- Podemos usar cualquier atributo del Xpath para la función.

```
//button[@name='Add' or @id='remove_btn']
```



</details>

---

### Operador - AND

<details>

<summary>and</summary>

- Cuando necesitamos definir exactamente la unicidad de un elemento web, entonces debemos añadir condiciones para su cumplimiento simultáneo.

- Para ello, debemos `añadir` condiciones al predicado (no siempre con `valor` en ellos)

```
//button[@class and @name='Save']
```

- Este ejemplo nos arroja 2 resultados, por que uno está oculto.

- Los elementos ocultos tienen este atributo.

```
style="display: none;"
```

- Entonces, debemos añadir a la condición de la estrategia de loclaización, este dato:

```
//button
```

- 8 resultados

```
//button[@class='btn']
```

- 6 resultados

```
//button[@class='btn' and not(@style='display: none;')]
```

- 2 resultados, los que queremos:

![alt text](image-49.png)

- Otra manera de usar la sintaxis `AND` es sencillamente, agrupar entre `corchetes` los `predicados`.

```
//button [@class='btn'] [not(@style='display: none;')]
```

</details>

---

### Wildcards

<details>

<summary>//*[@*='valor'] </summary>

- Podemos utilizar el caracter comodín `*` para codificar una búsqueda rápida.

- Asterísco en lugar de la etiqueta, encontrará cualquier `elemento nodo`.
- Asterísco en lugar del atributo, encontrará cualquier `nodo atributo`.

```
//*[@class]
```

- Esto significa, busca cualquier elemento, que tenga un atributo llamado `class`.

- A veces, también queremos un elemento donde algún atributo tenga un valor específico.

```
//button[@*='btn']
```

- Esto nos arroja 3 elementos

```
//button[@*='add_btn']
```

- Esto nos arroja 1 elemento

</details>

---

### XPath Axes

<details>

<summary>Elementos EJE</summary>

- Recordando la clase de `Índices`, si buscamos un elemento desde la `raíz` del `DOM` debemos
comenzar sabiendo que el primer resultado se encontrará en la dirección `[1]` en sentido `descendente`.

- La fórmula de los `EJES XPath` es:

```
axisname::notetest[predicate] 
```

- Con esta técnica de `ejes`, podemos <u>invertir el sentido de la búsqueda</u> para que ocurra en forma `ascendente`.

- Los comandos de `Xpath Axes del NODO` son:
```
1. DESCENDENTES
   1. descendant  - Selects all of: Children / GrandChildren of the current node.
   2. following-sibling  - Select all Sibling BEFORE the current node. 

2. ASCENDENTES
   1. preceding-sibling - Select all Sibling AFTER the current node. 
   2. parent - Only the PARENT of the current node.
   3. ancestor - Selects all of: Parents / GrandParents / etc, of the current node.
```

![alt text](image-50.png)

---

> EJEMPLO 1 - Búsqueda ascendente:

Encontrar este elemento `<div>` imaginando que su `PADRE` no tuviera manera de ser definido. pero podemos partir de un hijo fácilmente localizable.

![alt text](image-51.png)

<u>**Solución:**</u>

Se localiza al hijo con XPath: `//div[@id='row1']`

Se utilza la fórmula de Axes: el símbolo es con `'/'` como si fuera nodo `derivado`.

Se especifica el tipo de nodo `derivado`, en este caso, es un `parent::` 

Se define el `TAG` del nodo al que queremos llegar; en este caso es un `div`.

Quedando el Xpath Axes como:

> `//div[@id='row1']/parent::div`

**NOTA 1:** en este caso, como hay un único elemento padre también se pueden utilizar los comodines o `wildcards'  pudiendo ser el Xpath :

`//div[@id='row1']/parent::*`

Pero, se recomienda abogar por la especificidad.

**NOTA 2:** En este ejemplo no se usaron 'predicados' para el elemento 'PADRE', por que no tiene ninguno, pero sí se pueden usar.

![alt text](image-52.png)

---

**EJERCICIO 2: - búsqueda descendente pero ROBUSTA**

- Obtener el segundo 'paso' debajo del 'Test Case 2': `Click Add button`

![alt text](image-53.png)

Sabiendo que el elemento buscado está dentro de un `<li>`, hijo de un `<ol>`, hijo de un `<section>` con atributos, sería muy sencillo elegir esta estragegia:

`//section/ol[2]/li[2]`

Pero, es muy similar a un XPath absoluto, es frágil ante cualquier cambio en el diseño de la página.

![alt text](image-54.png)


- Entonces:

<u>Solución recomendada:</u>

Elegir los elementos mas cercanos posible

En este caso, el mismo nodo H5 que contiene el texto 'Test case 2:'

Teniendo ese `eje` o 'referencia', podemos buscar sus elementos 'HERMANOS', que son los `<ol>`.

Sabiendo que los 'ejes' son nuevas referencias, podemos usar 'Index' teniendo en cuenta que el elemento `<ol>` mas cercano debe ser el que queremos, entonces queda:

```
//h5[contains(text(),'Test case 2:')]/following-sibling::ol[1]/li[2]
```

![alt text](image-55.png)

---

**EJERCICIO 3: - búsqueda ascendente y ROBUSTA**

- Referencias 'hacia arriba'.

- Obtener el encabezado de una lista de pasos 'Test Case 2',  a partir de uno de sus 'Pasos' listados mas abajo, `Verify text saved`.

![alt text](image-56.png)

- Pistas:

   - Usar la función 'TEXT' para encontrar nuestro elemento 'eje'.

   - Luego, usar el 'eje' PARENT.

   - Luego, usar el 'eje' PRECEDING-SIBLING

   ![alt text](image-57.png)

```
//li[contains(text(),'Verify text saved')]/parent::ol/preceding-sibling::h5[1]
```

** Lección: ** 

OJO: 

Siempre, al realizar una búsqueda a partir de un elemento éste se convierte en el referente a partir del cuál se 'reinicia' el conteo con 'Índices'.

Esto es importante, por que queremos siempre elegir al elemento mas cercano y para ello nos valemos de los 'Índex'.

</details>

---

### Elementos relativos a otros elementos.

<details>

<summary>.//</summary>

- Esta expresión es útil dentro de un predicado, con un elemento usado como 'eje relativo'.

- **OJO**  
No es lo mismo esto, una busqueda 'descendente'...

```
//div/input
//div/child::input         (alternativa 1)
//input[parent::div]       (alternativa 2)
```

![alt text](image-58.png)


**QUE ESTO** 

Una búsqueda relativa a otros elementos.

```
//div[./input]
```

![alt text](image-59.png)


- Estamos buscando un elemento `div` en cualquier parte del DOM, que sea `relativo` a un elemento
`HIJO` tipo `input`.

- O sea, un elemento 'div'`PADRE` relativo al `HIJO` tipo 'input'.

> Estamos buscando al Papá de este chamaco.

- Sabiendo esto, podemos escribir también:

```
//input/parent::div
```

---

**TRUCAZO**

- Podemos usar predicados dentro de otros Predicados, para definir elementos.

- Si sólo usamos esto:

```
//input[parent::div]
```

Da esto:

![alt text](image-60.png)


- Pero si somos mas específicos, encontraremos exactamente lo que estamos buscando:

```
//input[parent::div[@id='row2']]
```

![alt text](image-61.png)

- Si bien funciona, hay maneras mas sencillas y legibles de lograr los mismos resultados.

```
//div[@id='row2']/input
```



</details>

---

### Seleccionar varios Xpaths simultáneamente

<details>

<summary>'|'</summary>

- Con el símbolo `|` podemos seleccionar varios elementos XPath, sin hacer uso del `OR`.

- De esta manera podemos combinar 2 expresiones Xpath simultáneamente.

- Generalmente se usa cuando tenemos una lista de elementos y éstos tienen distinto localizador.

- Imaginemos que necesitamos todos los elementos con texto en negritas,  `H5` y `H2`. Queda:

```
//h5 | //h2
```

![alt text](image-62.png)


- Ahora, podemos también usar XPath completos y encadenarlos:

```
//div[@id='row1']/button | //div[@id='row1']/input
```

![alt text](image-63.png)


- Lo mejor, es que no está limitado a un número determinado de expresiones XPath

```
//h5 | //h2 | //p
```

![alt text](image-64.png)

</details>

---

### SVG WebElements
<details>

<summary>SVG</summary>

- Estas porquerías se manejan con reglas distintas a todos los demás elementos del DOM.

- Esto incluye a sus `hijos` dentro de la etiqueta `svg`.

![alt text](image-65.png)

- Suponiendo que necesitamos el segundo elemento con etiqueta `rect`, a pesar de que tiene atributos, no lo podemos seleccionar de manera convencional.

```
//rect[@y='11']
```

- La solución rápida, es utilizar un `wildcard` en lugar de la etiqueta.

```
//*[@y='11']
```

- La verdadera solución es usar el **wildcard** pero con la función `NAME` para definir la etiqueta.


```
//*[name()='rect' and @y='11']
```

- Y si el elemento tiene atributos, pues mejor aún.

![alt text](image-66.png)


- De nuevo, la lección es que para los elementos `SVG` o sus hijos, usamos `name()='tag'`.

- Imaginemos que necesitamos el elemento con etiqueta `path`, nieto de un `SVG`.

```
//*[name()='symbol' and @id='icon-amazon']/*[name()='path']
```


![alt text](image-67.png)

- El truco del `wildcard + name()='tag'` aplica también a los hijos.

</details>


### Detener la página Web

<details>

<summary>Pausar la ejecución.</summary>

- Si queremos cachar cosas que suceden rápidamente, como elementos visibles que desaparecen...


![alt text](image-68.png)

- Tenemos que determinar qué es exactamente lo que dispara el `evento`, para luego abrir el `Inspector de elementos`.

- En este caso, sabemos que el mensaje `Loading` se dispara después de `hacer click` en el botón `ADD`.

- Seguimos la ruta en la configuración del navegador:

> Sources > Event Listener Breakpoints > Mouse > 'click'


![alt text](image-69.png)

- Damos click en el botón 'ADD'.

- Y debemos dar varios 'click' en el botón `Step Over`.

![alt text](image-70.png)

- Veremos cómo se ejecutan las acciones de manera secuencial, desaparecen los botones `Edit` y `Add`, pero finalmente aparece el mensaje `Loading`.

- Para encontrar el localizador del elemento, o inspeccionar el DOM, hacemos lo usual. Con el botón auxiliar del `Inspector de Elementos`.

![alt text](image-71.png)

- Observese que automáticamente nos regresa al menú `Elements`.

- Para terminar la ejecución del `Debugger`, sencillamente damos click en `Resume script execution` en cualquiera de las dos posiciones.

![alt text](image-72.png)


</details>






</details>

---

## Seccion 4

<details>

<summary>Seccion 4: Pros & Cons de XPath</summary>

You can add text within a collapsed section.

You can add an image or a code block, too.

</details>

---

## Seccion 5

<details>

<summary>Seccion 5: Trucos locos</summary>

---

<details>

<summary>Podemos usar un '*' en lugar del TAG.</summary>

```
//tag[@attribute="value"]      ->  //*[@id="validationCustom01"]
```

![alt text](image.png)

</details>

---

<details>

<summary>Podemos probar los XPaths directamente en el Navegador</summary>

> CTRL + SHIFT +I > Console

Usamos la siguiente sintaxis:

> $x("//\*[@id='validationCustom01']")

![alt text](image-1.png)

**TIP:** Todo dento de comillas dobles, deben ser comillas SIMPLES, por Sintaxis. 'Valores' de las etiquetas.

</details>

</details>

---
