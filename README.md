# XpathLocatorStrategies

This is a set of notes for Xpaths, from the Udemy Course ['XPath locators for Selenium'](https://www.udemy.com/course/xpath-locators-for-selenium/) by 'Dmitry Shyshkin'.

## Sección 1 - Recursos útiles del curso.

<details>

<summary> Seccion 1: Introducción - </summary>

<details>

<summary>Páginas Web</summary>

1. [Test Login](https://practicetestautomation.com/practice-test-login/)
2. [Test Exceptions](https://practicetestautomation.com/practice-test-exceptions/)

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

<summary>9 estragegias de localizacion</summary>

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

- Usando el 'Submit', hacia arriba, tiene 2 'hermanos' y 1 'padre.
- Usando el 'Form', hacia abajo, tiene 3 hijos.

![alt text](/images/image0.png)

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

- Tenemos un elemento 'PADRE' tipo 'Div' con un 'Id'

  - Dentro tiene otras cosas, pero las que nos interesa es el INPUT

- Elemento PADRE, usado como 'XPath Relativo' o referencia
  - > //div[@id='row2']

![alt text](image-2.png)

- Con este punto de partida, nos dirigimos al elemento HIJO, el único tipo 'INPUT'.
- Como no hay otro similar, el XPath queda:
  - > //div[@id='row2']/input

![alt text](image-3.png)

</details>

---

## Seccion 3

<details>

<summary>Seccion 3: Advanced XPath</summary>

You can add text within a collapsed section.

You can add an image or a code block, too.

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
