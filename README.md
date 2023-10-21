# bdd-tsc-cucumber

El siguiente documento contiene pasos para automatizar un guión escrito en el lenguaje Gherkin.

**Gherkin** es un lenguaje de dominio específico (DSL) utilizado en la programación y el desarrollo de software para escribir especificaciones de comportamiento de software de una manera legible por humanos. Se utiliza comúnmente en el contexto de las pruebas de comportamiento o BDD (Behavior-Driven Development).

### Tecnologías que utilizaremos para esta implementación:

- **Nodejs:** es un entorno de tiempo de ejecución de JavaScript de código abierto que permite ejecutar código JavaScript en el lado del servidor. Fue desarrollado sobre el motor de JavaScript V8 de Google Chrome y se utiliza para construir aplicaciones de servidor, aplicaciones web en tiempo real y diversas herramientas de línea de comandos.
    
    NPM, que significa "Node Package Manager" es el administrador de paquetes oficial de Node.js. Es una herramienta que permite a los desarrolladores de Node.js gestionar las dependencias de sus proyectos, así como compartir y distribuir paquetes.
    
- **WebdriverIO:** es un framework de automatización de pruebas que permite a los desarrolladores y tester automatizar la interacción con aplicaciones web y móviles.
- **Cucumber:** con Cucumber, las especificaciones de comportamiento se escriben en Gherkin en forma de "características" (features) que describen el comportamiento esperado del software. Estas características se dividen en "escenarios" que detallan casos específicos de uso.
- **Typescript:** es un lenguaje de programación de código abierto desarrollado por Microsoft que se basa en JavaScript. Aunque comparte muchas similitudes con JavaScript, TypeScript agrega características adicionales que lo hacen más robusto y adecuado para el desarrollo de aplicaciones a gran escala y mantenibles.
- **Visual Studio Code:** es un editor de código fuente gratuito y de código abierto desarrollado por Microsoft.

A modo de ejemplo utilizaré la web AdminCES, dónde es posible entrar ingresando un código hash. Luego de ingresar en hash, validamos mediante un assertion que en la siguiente página se encuentre el texto “Taller de Automatización del Testing Funcional”. 

Este simple ejemplo es para hacer didáctica la implementación, el procedimiento explicado en este documento, se puede escalar a escenarios más complejos.

[Login](http://cestore.ces.com.uy/adminces/)

![Screenshot from 2023-10-10 22-43-21.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c5ada87e-e5d4-4511-b265-9fee52aa4f65/21c0ee7b-d4d9-434a-9100-d3ed0d83ccab/Screenshot_from_2023-10-10_22-43-21.png)

Como primer paso definiremos el guión que estamos automatizando, en español y luego para seguir una buena práctica lo adaptamos al idioma inglés.

**Paso 1:**

**Característica:** Prueba de inicio de sesión AdminCES

**Escenario:** Como usuario, puedo iniciar sesión en el área segura con un hash válido

**Dado** que abro la página de inicio de sesión
**Cuando** proporciono un hash válido
**Entonces**, en la siguiente página, verifica la existencia del texto

```gherkin
Feature: Login test AdminCES

Scenario: As a user, I can log into the secure area with valid hash

**Given** open login page
**When** providing valid hash
**Then** at the next page, it check the existence of the text
```

Con el guión definido pasaremos a levantar el proyecto desde cero. 

Este tutorial tendrá dos etapas, la primera será la automatización con lo mínimo indispensable para que funcione y en la segunda etapa mejoraremos el código. 

**Paso 2:**

Crear una carpeta con el nombre del proyecto.

```bash
mkdir bdd-adminces
```

```bash
cd bdd-adminces/
```

Luego abrimos la carpeta en VSCode(Visual Studio Code)

![Screenshot from 2023-10-10 23-45-07.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c5ada87e-e5d4-4511-b265-9fee52aa4f65/1ee7a0d1-e87c-4c5e-88a0-cabd229b8398/Screenshot_from_2023-10-10_23-45-07.png)

Con el siguiente atajo de teclado podemos abrir una terminal en VSCode

```bash
Ctrl + Shift + `
```

Ahora digitaremos el siguiente comando en la terminal:

```bash
npm init -y
```

El comando **`npm init -y`** se utiliza para inicializar un nuevo proyecto de Node.js y crear un archivo **`package.json`** con valores predeterminados sin requerir ninguna entrada del usuario.

![Screenshot from 2023-10-10 23-52-20.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c5ada87e-e5d4-4511-b265-9fee52aa4f65/cc3b20c4-5062-45d6-822c-ce792b248632/Screenshot_from_2023-10-10_23-52-20.png)

Iniciamos webdriverIO con el siguiente comando:

```bash
npm init wdio .
```

![Screenshot from 2023-10-11 18-20-40.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c5ada87e-e5d4-4511-b265-9fee52aa4f65/769f8bf3-6802-457f-9ba3-40519a79e536/Screenshot_from_2023-10-11_18-20-40.png)

A partir de ahora configuraremos el ambiente:

```bash
? A project named "bdd-adminces" was detected at "/home/csilvera/Documents/www/ces/bdd-adminces", correct? (Y/n)
Yes
```

```bash
? What type of testing would you like to do? (Use arrow keys)
❯ E2E Testing - of Web or Mobile Applications
Enter
```

```bash
? Where is your automation backend located? (Use arrow keys)
❯ On my local machine
Enter
```

```bash
? Which environment you would like to automate? (Use arrow keys)
❯ Web - web applications in the browser
Enter
```

```bash
? With which browser should we start? (Press <space> to select, <a> to toggle all, <i> to invert selection, and <enter> to 
proceed)
❯◉ Chrome
 ◯ Firefox
 ◯ Safari
 ◯ Microsoft Edge
Enter
```

```bash
? Which framework do you want to use? 
  Mocha (https://mochajs.org/) 
  Jasmine (https://jasmine.github.io/) 
❯ Cucumber (https://cucumber.io/)
Enter
```

```bash
? Do you want to use a compiler? 
  Babel (https://babeljs.io/) 
❯ TypeScript (https://www.typescriptlang.org/) 
  No!
Enter
```

```bash
? Do you want WebdriverIO to autogenerate some test files? (Y/n) No
```

```bash
? Which reporter do you want to use? (Press <space> to select, <a> to toggle all, <i> to invert selection, and <enter> to 
proceed)
 ◉ spec
 ◯ dot
 ◯ junit
❯◯ allure
 ◯ sumologic
 ◯ concise
 ◯ reportportal
Enter
```

Para seleccionar una de las opciones se lo hace con la tecla espaciadora como hace mención la descripción.

```bash
? Do you want to add a plugin to your test setup? (Press <space> to select, <a> to toggle all, <i> to invert selection, and
 <enter> to proceed)
❯◉ wait-for: utilities that provide functionalities to wait for certain conditions till a defined task is complete.
   > https://www.npmjs.com/package/wdio-wait-for
Enter
```

En las siguientes opciones no selecciono ninguna y doy Enter

```bash
? Do you want to add a service to your test setup? (Press <space> to select, <a> to toggle all, <i> to invert selection, 
and <enter> to proceed)
 ◯ slack
 ◯ cucumber-viewport-logger
 ◯ intercept
❯◯ docker
 ◯ image-comparison
 ◯ novus-visual-regression
 ◯ rerun
(Move up and down to reveal more choices)
```

```bash
? What is the base url? (http://localhost)
Enter
```

```bash
? Do you want me to run `npm install` (Y/n) Yes
```

Finalmente se cargarán todos los componentes configurados.

![Screenshot from 2023-10-11 19-11-14.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c5ada87e-e5d4-4511-b265-9fee52aa4f65/76389835-1ea8-4ca9-9a47-ccfefc3778e8/Screenshot_from_2023-10-11_19-11-14.png)

![Screenshot from 2023-10-11 19-12-37.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c5ada87e-e5d4-4511-b265-9fee52aa4f65/085ed33b-400a-4b30-a595-bcdbcbf0984e/Screenshot_from_2023-10-11_19-12-37.png)

### Importante configuración en *wdio.conf.ts*

Es importante que configuremos lo siguiente en nuestro archivo `wdio.conf.ts`:

```tsx
specs: [
        './features/**/*.feature'
    ],
```

**`specs`** se utiliza para especificar las rutas de los archivos de especificaciones (archivos **`.feature`** en el contexto de pruebas de comportamiento o BDD) que se ejecutarán como parte de tus pruebas automatizadas.

Esta configuración **`require`** garantiza que todas las definiciones de pasos definidas en archivos TypeScript (**`.ts`**) en el directorio **`./features/step-definitions/`** estén disponibles para su ejecución cuando se ejecuten las pruebas Cucumber con WebdriverIO.

```tsx
cucumberOpts: {
        // <string[]> (file/dir) require files before executing features
        require: ['./features/step-definitions/*.ts'],
```

A continuación trabajaré la siguiente estructura de directorios y archivos en mi proyecto:

```flow
bdd-adminces/
│
└── features/
    ├── pageobjects/
    │   ├── adminces.page.ts
    │   
    │
    └── step-definitions/
        └── adminces-steps.ts
    └── adminces-login.feature
```

![Screenshot from 2023-10-12 15-25-23.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c5ada87e-e5d4-4511-b265-9fee52aa4f65/03773ed3-e326-4f72-89e2-5d55e8494455/Screenshot_from_2023-10-12_15-25-23.png)

Como punto de partida depués de las configuraciones crearemos una carpeta **features**, dentro de la misma colocar el archivo **adminces-login.feature,** donde escribiremos nuestro guión en el lenguaje gherkin.

En la primera parte trabajaremos con éste guión:

```gherkin
Feature: Login test AdminCES

Scenario: As a user, I can log into the secure area with valid hash

Given open login page
When providing valid hash
Then at the next page, it check the existence of the text
```

Ahora dentro del directorio **features**, crearemos una carpeta llamada **pageobjects** y dentro de la misma un archivos, **`adminces.page.ts`**

**Código a trabajar:** 

*`adminces.page.ts`*

```tsx
import { $ } from '@wdio/globals'

class loginPageAdmin{
    get enterUser(){

        return $('#pass');
    }
    get clickLogin(){
        return $('//*[@id="loginForm"]/div[2]/button');
    }
    async loginMethod (hash) {

        await this.enterUser.setValue(hash);
        await this.clickLogin.click();
        
    }

}

module.exports = new loginPageAdmin();
```

También crearemos el directorio `step-definitions`, siguiendo la estructura de archivos presentada arriba. Dentro del mismo crearemos el archivo *`adminces-steps.ts`*

```tsx
import { expect, $ } from '@wdio/globals'
import { Given, Then, When } from "@wdio/cucumber-framework";
import { browser } from '@wdio/globals';

const login = require('../pageobjects/adminces.page');

Given('open login page', async function () {
    browser.maximizeWindow(); // Maximiza la ventana del navegador
    await browser.url("http://cestore.ces.com.uy/adminces/")

});

When('providing valid hash', async function () {
    await login.loginMethod('3)ea60e0be3ba12c6ecd%7297868%5c4');
});

Then('at the next page, it check the existence of the text', async function () {

    // Espera a que la página de destino se cargue completamente.
    await browser.waitUntil(async () => {
        const url = await browser.getUrl();
        return url.includes('http://cestore.ces.com.uy/adminces/'); // Reemplaza con la URL de la página después de iniciar sesión
    }, {
        timeout: 10000, // Ajusta el tiempo de espera según tus necesidades
        timeoutMsg: 'La página de destino no se cargó correctamente después de iniciar sesión'
    });

    // Verifica si el texto esperado está presente en el cuerpo de la página
    const bodyText = await $('body').getText();
    expect(bodyText).toContain('Taller de Automatización del Testing Funcional');
});
```

Para correr las pruebas ejecutamos en consola el siguiente comando:

```bash
npm run wdio
```

![Screenshot from 2023-10-12 11-10-19.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c5ada87e-e5d4-4511-b265-9fee52aa4f65/17249603-1ce0-4902-9c83-aa2322275b46/Screenshot_from_2023-10-12_11-10-19.png)

Ahora voy a setear los datos de entrada en el mismo guión:

*`adminces-login.feature`*

```gherkin
Feature: Login test AdminCES

Scenario Outline: As a user, I can log into the secure area with valid hash

Given open login page
When providing valid code <hash>
Then at the next page, it check the existence of the <text> 

    Examples:
      | hash                             | text                                           |
      | 3)ea60e0be3ba12c6ecd%7297868%5c4 | Taller de Automatización del Testing Funcional |
```

**`Scenario Outline`**: Esto define un escenario de prueba. Es una plantilla que se llenará con valores específicos de los ejemplos.

Los símbolos **`<`** y **`>`** que rodean una palabra o un término, como **`<hash>`**, se utilizan para denotar marcadores o parámetros en el lenguaje Gherkin.

A continuación veremos en los ejemplos de codigo las variaciones con respecto al primar ejemplo:

*`adminces.page.ts`*

```tsx
import { $ } from '@wdio/globals'

class loginPageAdmin{
    get enterUser(){

        return $('#pass');
    }
    get clickLogin(){
        return $('//*[@id="loginForm"]/div[2]/button');
    }
    async loginMethod (hash) {
        await this.enterUser.setValue(hash);
        await this.clickLogin.click();
        
    }

}

module.exports = new loginPageAdmin();
```

1. Importación de módulos:
    - Importa el módulo **`$`** desde **`@wdio/globals`**. Este módulo se utiliza para seleccionar elementos en la página web y realizar acciones en ellos.
2. Definición de la clase **`loginPageAdmin`**:
    - Define una clase llamada **`loginPageAdmin`**, que se utiliza para encapsular la lógica relacionada con la página de inicio de sesión.
3. Declaración de propiedades **`get`**:
    - **`get enterUser()`**: Esta propiedad es un "getter" que devuelve el elemento de entrada (input) del usuario en la página de inicio de sesión. Selecciona el elemento utilizando la función **`$`** y el selector CSS **`$('#pass')`**. El selector **`#pass`** representa un elemento con el atributo **`id`** igual a "pass".
    - **`get clickLogin()`**: Esta propiedad es un "getter" que devuelve el botón de inicio de sesión en la página. Selecciona el botón utilizando un selector XPath **`//*[@id="loginForm"]/div[2]/button`**.
4. Método **`loginMethod`**:
    - **`async loginMethod (hash)`**: Este método acepta un argumento **`hash`**, que probablemente sea la contraseña o información de inicio de sesión. El método se utiliza para realizar el proceso de inicio de sesión en la página web.
    - **`await this.enterUser.setValue(hash)`**: Utiliza la propiedad **`enterUser`** (el campo de entrada del usuario) y la función **`setValue`** para establecer el valor del campo de entrada con el valor proporcionado en **`hash`**. Esto simula el ingreso de la contraseña en el campo de usuario.
    - **`await this.clickLogin.click()`**: Utiliza la propiedad **`clickLogin`** (el botón de inicio de sesión) y la función **`click`** para hacer clic en el botón de inicio de sesión. Esto simula el envío del formulario de inicio de sesión.
5. Exportación de la clase:
    - **`module.exports = new loginPageAdmin();`**: Exporta una instancia de la clase **`loginPageAdmin`**. Esto permite que otros módulos utilicen esta instancia para interactuar con los elementos de la página de inicio de sesión.

*`adminces-steps.ts`*

```tsx
import { expect, $ } from '@wdio/globals'
import { Given, Then, When } from "@wdio/cucumber-framework";
import { browser } from '@wdio/globals';
const login = require('../pageobjects/adminces.page');

Given('open login page', async function () {
		browser.maximizeWindow(); // Maximiza la ventana del navegador
    await browser.url("http://cestore.ces.com.uy/adminces/")
});

When(/^providing valid code (.+)$/, async function (hash) {
    await login.loginMethod(hash);
});

Then(/^at the next page, it check the existence of the(.+)$/, async function (text) {
    // Espera a que la página de destino se cargue completamente.
    await browser.waitUntil(async () => {
        const url = await browser.getUrl();
        return url.includes('http://cestore.ces.com.uy/adminces/'); // Reemplaza con la URL de la página después de iniciar sesión
    }, {
        timeout: 10000, // Ajusta el tiempo de espera según tus necesidades
        timeoutMsg: 'La página de destino no se cargó correctamente después de iniciar sesión'
    });

    // Verifica si el texto esperado está presente en el cuerpo de la página
    const bodyText = await $('body').getText();
    expect(bodyText).toContain(text);
});
```

1. Importación de módulos y configuraciones:
    - Importa varios módulos necesarios de WebdriverIO y el marco Cucumber.
    - Importa **`expect`** y **`$`** de **`@wdio/globals`**, que se utilizan para realizar aserciones y seleccionar elementos en la página.
    - Importa **`Given`**, **`Then`**, y **`When`** de **`@wdio/cucumber-framework`**, que se utilizan para definir los pasos de prueba.
    - Importa **`browser`** de **`@wdio/globals`**, que se utiliza para interactuar con el navegador.
    - Importa el módulo **`adminces.page`** desde **`../pageobjects/`**, que probablemente contiene funciones relacionadas con la página web bajo prueba.
2. Definición del paso "Given":
    - En este paso, se abre la página de inicio de sesión en un navegador web.
    - **`browser.maximizeWindow()`** maximiza la ventana del navegador para asegurarse de que se muestre correctamente.
    - **`await browser.url("http://cestore.ces.com.uy/adminces/")`** navega a la URL especificada, que es la página de inicio de sesión.
3. Definición del paso "When":
    - Este paso describe la acción de proporcionar un código válido durante el inicio de sesión.
    - Utiliza una expresión regular **`When(/^providing valid code (.+)$/, ...)`** para capturar el argumento **`hash`** que se pasa. Esto permite que se utilicen diferentes códigos en las pruebas.
    - Llama a la función **`login.loginMethod(hash)`** para proporcionar el código de inicio de sesión.
4. Definición del paso "Then":
    - Este paso verifica si, después de la acción de inicio de sesión, la página de destino se carga completamente y si un texto específico está presente en el cuerpo de la página.
    - Utiliza **`browser.waitUntil`** para esperar a que la URL de la página se cargue completamente. Esto asegura que la página de destino sea la correcta después del inicio de sesión.
    - Se verifica si la URL incluye "http://cestore.ces.com.uy/adminces/". Esta URL se puede ajustar según la URL de la página de destino real.
    - Luego, verifica si el texto esperado (capturado como **`text`** del argumento del paso) está presente en el cuerpo de la página. Utiliza **`$('body').getText()`** para obtener el contenido del cuerpo y **`expect(bodyText).toContain(text)`** para realizar la aserción.

Comando para ejecutar los test:

```bash
npm run wdio
```

![Screenshot from 2023-10-12 13-11-51.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/c5ada87e-e5d4-4511-b265-9fee52aa4f65/189434a5-a4c8-4bb3-b2f6-ea24f9e5e9b4/Screenshot_from_2023-10-12_13-11-51.png)
