## Pruebas automatizadas OCU - Universitas XXI Investigación	 

Tecnologías utilizadas:
- Typescript
- Protractor
- Selenium-Webdriver
- Jasmine
---

### Protractor

Protractor es un framework de pruebas end-to-end para Angular and AngularJS applications. Protractor ejecuta los tests contra tu aplicación en un navegador real, interactuando con él como un usuario lo haría.
![Logo Protractor](/assets/images/protactor-angular.png)

---

### Instalación

Para poder ejecutar las pruebas automatizadas sobre el portal investigador debemos realizar los siguientes pasos: 

1. Instalar node (versión >= 6.9.0) y npm.

2. Instalar máquina virtual de Java (JRE)

3. Instalar protractor en modo administrador 
```sh
npm install -g protractor
```
4. Instalar typescript en modo administrador
```sh
npm install -g typescript
```
5. Instalar angular-cli en modo administrador
```sh
npm install -g @angular/cli
```
6. Descargar el repositorio de pruebas.

7. Instalar dependencias de npm.
```sh
npm install
```
---

### Ejecución

Para ejecutar las pruebas debemos realizar los siguientes pasos:

1. Actualizar los drivers de Selenium para controlar los navegadores:
```
webdriver-manager update
```
2. Seguidamente debemos levantar el servidor de Selenium: 
```
webdriver-manager start
```

---
Este método nos sirve tanto para el navegador Chrome como para firefox, en cambio para Internet explorer debemos actulizar los driver específicos para este navegador: 

```
webdriver-manager update --ie
```
y seguidamente levantamos el servidor:
```
webdriver-manager start
```
3. Finalmente, en la consola de administración lanzamos el siguiente comando: 
```
protractor **nombreFicheroConfiguración.js**
```
En ese momento empezará a ejecutarse las pruebas que tengamos habilitadas en el fichero de configuración.

---

### Fichero de configuración

En las siguientes diapositivas explicaremos diversas partes que contiene el fichero de configuración:

Atributo **specs**:

Este atributo es el que nos permite configurar los archivos de pruebas que deben de ejecutarse:
```javascript
specs: [

    /**                      LOGIN, PERFIL, CONTRASENA Y PUBLICAR FOTO                       */
     './e2e/**/login/*.e2e-spec.ts',
    // './e2e/**/modificarContrasena/*.e2e-spec.ts',
    
     /**                      ACTUALIDAD                                                     */
    // './e2e/**/eventos/*.e2e-spec.ts',
    // './e2e/**/noticias/*.e2e-spec.ts',
    ...
  ],
  ```

  En el momento de ejecutar el archivo de configuracuón la prueba que se va a ejecutar es la del login de la aplicación
  
  ---
  Atributo **suites**:

Este atributo nos permite configurar los archivos de pruebas que deben de ejecutarse, agrupándolos por una característica y configurando por parámetro en la ejecución del fichero de configuración. Ejemplo:
```javascript
suites: {
    login: './e2e/**/login/*.e2e-spec.ts',
    actualidad: [
        './e2e/**/eventos/*.e2e-spec.ts',
        './e2e/**/noticias/*.e2e-spec.ts'
    ],
    ...
}
  ```
  ---
  A la hora de ejecutar el fichero de configuración, le pasaremos por un parámetro el suite que queremos ejecutar. Ejemplo:
  ```
  protractor protractor.conf.js --suite login
  ```
  o también podríamos ejecutar varios
  ```
  protractor protractor.conf.js --suite login, actualidad
  ```
  ---

  Atributo **capabilities**

  A través de este atributo configuramos el/los navegadores donde desplegar las pruebas:
  ```javascript
  capabilities: {
    browserName: 'chrome'
    // browserName: 'firefox'
    // 'browserName': 'internet explorer',
    // 'platform': 'ANY',
    // 'version': '11'
  },
  multiCapabilities: [{
  'browserName': 'firefox'
    }, {
  'browserName': 'chrome'
}],
```
---
Atributo **onPrepare**:
Aqui establecemos si queremos realizar alguna acción antes de ejecutar las pruebas, por ejemplo:

```javascript
onPrepare: function () {
    browser.manage().window().maximize(); // Maximiza la pantalla antes de comenzar las pruebas
    return global.browser.getProcessedConfig().then(function (config) {
    });
  },
  ```
  ---
### Añadiendo flexibilidad a protractor con parámetros



---
### Selectores

A continuación detallaremos algunos de los selectores utilizados a la hora de la realización de las pruebas automatizadas en el portal del investigador.

Listado basado en la contribución de Javier Arques: https://gist.github.com/javierarques/0c4c817d6c77b0877fda

---
#### Controles del navegador

* browser.get('yoururl'); // Nos permite cargar una url pasada por parámetro
* browser.navigate().back(); // Página anterior
* browser.navigate().forward(); // Página siguiente
* browser.sleep(10000); // Esperamos un tiempo al navegador
* browser.waitForAngular(); // Esperamos a que Angular acade de hacer sus operaciones
* browser.getCurrentUrl() // Devuelve la URL en la que estamos
* browser.ignoreSynchronization = true; // Si está a true, Protractor no intentará sincronizarcon la página antes de realizar las acciones. Se usa para probar páginas que no sean Angular

---

#### Controladores de visibilidad

* element(by.id('create')).isPresent() // Indica si un elemento está presente en el DOM
* element(by.id('create')).isEnabled() // Indica si un elemento está habilitado o no 
* element(by.id('create')).isDisplayed() // Indica que un elemento está presente y está visible en la página

---

#### Encontrando elementos (1)

* element(by.id('user_name')) --> $('#user_name').sendKeys('texto')
* element(by.css('#myItem')) --> $('.user_name')
* element(by.buttonText('Save'));
* element(by.partialButtonText('Save'));
* element(by.partialLinkText('Save'));
* element(by.css('[ng-click="cancel()"]')); --> $('[ng-click="cancel()"]') 
* const dog = element(by.cssContainingText('.pet', 'Dog'));

---

#### Encontrando elementos (2)

* element.all(by.css('.items)); --> $$('.items');
* element.all(by.xpath('//div')
* expect(list.count()).toBe(3);
* expect(list.get(0).getText()).toBe('First’)
* expect(list.get(1).getText()).toBe('Second’)
* expect(list.first().getText()).toBe('First’)
* expect(list.last().getText()).toBe('Last’)

---
#### Matcher de Jasmine

* to(N­ot)­Be( null | true | false )
* to(N­ot)­Equ­al( value )
* to(N­ot)­Con­tain( string )
* toBe­Les­sTh­an( number )
* toBe­Gre­ate­rTh­an( number )

---
#### Expected Conditions

Es una librería que nos permite añadir unas condiciones adicionales a protractor que son de gran ayuda:

* elementToBeClickable --> Elemento se vuelva clicable
* textToBePresentInElement --> Aparezca el texto en un elemento
* titleContains --> Título de la pantalla contenga el valor
* urlContains --> URL actual de la pantalla contenga el valor
* presenceOf --> Presencia del elemento en la pantalla
* visibilityOf --> Visibilidad del elemento en la pantalla

Eston son sólo algunos. La lista completa se puede encontrar en: [Protractor API](https://www.protractortest.org/#/api)
---
Ejemplo: Esperamos hasta que un elemento en pantalla sea visible:

```javascript
import { browser, ExpectedConditions, protractor } from 'protractor';

const EC = protractor.ExpectedConditions;
browser.wait(EC.visibilityOf(elemento), 15000);
```
El código anterior realizará la prueba de esperar durante 15s si aparece el elemento. Si no aparece lanzará la prueba como fallida.

---

### Page Objects

Para evitar la duplicidad de código y tener nuestro código lo mas localizado posible, utilizamos page objects. Ejemplo:

```javascript
it('comprobamos que podemos cumplimentar panelSearch de investigadores', () => {
        const listaPanelSearch = $$('ad-panel-search2');
        const index = 0;
        listaPanelSearch.get(index).$('.boton-icono').click(); // Abrimos panel-search
        listaPanelSearch.get(index).$('.modal-body').getCssValue('display').then(valor => {
            if (valor === 'block') {
              $$('ad-select-search').get(0).$('input').click();
              $$('ad-select-search').get(0).$('input').sendKeys('Xalabarder');
              browser.wait(EC.visibilityOf($$('ad-select-search').get(0).$('.select-search-resultados')),5000);
              browser.actions().sendKeys(protractor.Key.TAB).perform();
              browser.actions().sendKeys(protractor.Key.ENTER).perform();
            }
        });
    });
```

---
Este código tendría que repetirse en nuestro fichero de pruebas para probar cada uno de los panelSearch en una pantalla.
Usando page objects, el código en nuestro ficher de pruebas en mucho mas secillo y evitamos tanta duplicidad de código. Ejemplo:

```javascript
it('comprobamos que podemos añadir investigadores', () => {
        panelSearch.anadirPanelSearch(0, 0, 'xala', 'Investigador', false);
    });
```
Lo que hacemos es crear un método y llamar a ese método en cada fichero de prueba en el que necesitemos realizar la prueba de cumplimentar un panelSearch

