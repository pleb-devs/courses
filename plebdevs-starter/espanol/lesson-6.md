# JavaScript: Construyendo tu Primera Aplicación Web Interactiva

## Introducción
En las lecciones anteriores, hemos aprendido HTML para estructurar contenido y CSS para darle estilo. Ahora es momento de dar vida a nuestras páginas web con JavaScript. JavaScript es el lenguaje que permite que las páginas web sean interactivas, dinámicas y respondan a las acciones del usuario.

## ¿Qué es JavaScript?

### Definición
JavaScript es el lenguaje de programación de la web. Es un lenguaje versátil que se extiende a:
- Sitios web interactivos
- Aplicaciones web
- Servidores (Node.js)
- Aplicaciones móviles
- Juegos y mucho más

### Características Principales
- **Lenguaje interpretado**: No necesita compilación
- **Orientado a objetos**: Soporta programación orientada a objetos
- **Dinámico**: Puede modificar el contenido de la página en tiempo real
- **Basado en eventos**: Responde a acciones del usuario (clics, teclas, etc.)

## Proyecto: Tracker de Precio de Bitcoin

### Objetivo del Proyecto
Construiremos una aplicación web que:
- Muestre el precio actual de Bitcoin
- Se actualice automáticamente cada 3 segundos
- Permita cambiar entre diferentes monedas
- Incluya controles interactivos
- Muestre la fecha y hora actual

## Conceptos Fundamentales de JavaScript

### 1. Tipos de Datos Básicos

#### Números
```javascript
const precio = 45000;           // Entero
const porcentaje = 3.14;        // Decimal
const negativo = -100;          // Número negativo
```

#### Booleanos
```javascript
const esVisible = true;         // Verdadero
const estaOculto = false;       // Falso
```

#### Cadenas (Strings)
```javascript
const moneda = "USD";           // Comillas dobles
const mensaje = 'Hola mundo';   // Comillas simples
const precio = `$${45000}`;     // Template literals
```

### 2. Variables

#### Declaración de Variables
```javascript
// let: Para variables que pueden cambiar
let monedaActual = "USD";
monedaActual = "EUR";  // Puede cambiar

// const: Para variables que NO cambian
const intervalo = 3000;
// intervalo = 5000;  // Error: no se puede cambiar
```

### 3. Estructuras de Datos

#### Objetos
```javascript
// Los objetos son pares clave-valor
const usuario = {
    nombre: "Juan",
    edad: 25,
    ciudad: "Madrid"
};

// Acceder a propiedades
console.log(usuario.nombre);    // "Juan"
console.log(usuario["edad"]);   // 25
```

#### Arrays (Matrices)
```javascript
// Arrays son colecciones ordenadas
const monedas = ["USD", "EUR", "GBP"];
const precios = [45000, 38000, 35000];

// Acceder a elementos (índice empieza en 0)
console.log(monedas[0]);        // "USD"
console.log(precios[1]);        // 38000
```

### 4. Funciones

#### Declaración de Funciones
```javascript
// Función básica
function saludar() {
    console.log("¡Hola!");
}

// Función con parámetros
function sumar(a, b) {
    return a + b;
}

// Función con parámetros y uso
function formatearPrecio(precio, moneda) {
    return `${moneda} $${precio}`;
}

// Llamar funciones
saludar();                      // ¡Hola!
const resultado = sumar(5, 3);  // 8
const precioFormateado = formatearPrecio(45000, "USD");
```

### 5. Condicionales

#### Sentencias if/else
```javascript
function evaluarPrecio(precio) {
    if (precio > 50000) {
        console.log("Precio alto");
    } else if (precio > 30000) {
        console.log("Precio medio");
    } else {
        console.log("Precio bajo");
    }
}
```

## El DOM (Modelo de Objetos del Documento)

### ¿Qué es el DOM?
El DOM es una interfaz de programación que:
- Representa la estructura de una página web como un árbol de objetos
- Permite a JavaScript acceder y manipular elementos HTML
- Cada elemento HTML se convierte en un "nodo" del árbol

### Estructura del DOM
```
document
  └── html
      ├── head
      │   ├── title
      │   └── meta
      └── body
          ├── header
          ├── main
          └── footer
```

### Acceso a Elementos del DOM

#### Seleccionar Elementos
```javascript
// Por ID
const elemento = document.getElementById("precio");

// Por clase
const elementos = document.getElementsByClassName("mi-clase");

// Por etiqueta
const parrafos = document.getElementsByTagName("p");

// Selectores CSS (más flexible)
const elemento = document.querySelector("#precio");
const elementos = document.querySelectorAll(".mi-clase");
```

#### Manipular Contenido
```javascript
// Cambiar texto
const precioElemento = document.getElementById("precio");
precioElemento.textContent = "USD $45,000";

// Cambiar HTML
precioElemento.innerHTML = "<strong>USD $45,000</strong>";

// Cambiar estilos
precioElemento.style.color = "green";
precioElemento.style.display = "none";
```

## Eventos y Escuchadores (Event Listeners)

### ¿Qué son los Eventos?
Los eventos son acciones que ocurren en la página web:
- Clics del mouse
- Pulsaciones de teclas
- Cambios en formularios
- Carga de la página

### Escuchadores de Eventos
```javascript
// Escuchar clic en un botón
const boton = document.getElementById("actualizar");
boton.addEventListener("click", function() {
    console.log("¡Botón clickeado!");
});

// Escuchar cambio en un selector
const selector = document.getElementById("moneda");
selector.addEventListener("change", function() {
    console.log("Moneda cambiada a:", this.value);
});
```

## Programación Asíncrona y Fetch API

### ¿Qué es Fetch?
Fetch es una API moderna de JavaScript para:
- Realizar peticiones HTTP desde el navegador
- Obtener datos de servidores externos
- Manejar respuestas de forma asíncrona

### Sintaxis Básica
```javascript
// Función asíncrona para obtener datos
async function obtenerDatos() {
    try {
        const respuesta = await fetch("https://api.ejemplo.com/datos");
        const datos = await respuesta.json();
        console.log(datos);
    } catch (error) {
        console.error("Error:", error);
    }
}
```

## Construcción del Proyecto: Paso a Paso

### 1. Estructura HTML

#### HTML Base (index.html)
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tracker de Precio Bitcoin</title>
    <link rel="stylesheet" href="style.css">
    <script src="index.js" defer></script>
</head>
<body>
    <h1>Precio Actual de Bitcoin</h1>
    <p>Fecha y Hora: <span id="fecha-hora"></span></p>
    <p>El precio actual de Bitcoin es: <span id="precio"></span></p>
    
    <select id="selector-moneda">
        <option value="USD">USD</option>
        <option value="EUR">EUR</option>
        <option value="GBP">GBP</option>
    </select>
    
    <button id="boton-actualizar">Actualizar Precio</button>
    <button id="boton-alternar">Alternar Visibilidad</button>
</body>
</html>
```

### 2. Implementación JavaScript

#### Configuración Inicial
```javascript
// Variable global para la moneda actual
let monedaActual = "USD";

// Función para actualizar fecha y hora
function actualizarFechaHora() {
    const elementoFecha = document.getElementById("fecha-hora");
    const fechaActual = new Date().toLocaleString();
    elementoFecha.textContent = fechaActual;
}
```

#### Función para Obtener Precio de Bitcoin
```javascript
async function obtenerPrecioBitcoin() {
    // Construir URL de la API con la moneda actual
    const urlApi = `https://api.coinbase.com/v2/prices/BTC-${monedaActual}/spot`;
    
    try {
        // Hacer petición a la API
        const respuesta = await fetch(urlApi);
        const datos = await respuesta.json();
        
        // Extraer precio de la respuesta
        const precio = datos.data.amount;
        
        // Actualizar el precio en la página
        document.getElementById("precio").textContent = `${monedaActual} $${precio}`;
        
    } catch (error) {
        console.error("Error al obtener precio:", error);
        document.getElementById("precio").textContent = "Error al cargar precio";
    }
}
```

#### Función para Alternar Visibilidad
```javascript
function alternarVisibilidadPrecio() {
    const elementoPrecio = document.getElementById("precio");
    
    if (elementoPrecio.style.display === "none") {
        elementoPrecio.style.display = "inline";
    } else {
        elementoPrecio.style.display = "none";
    }
}
```

#### Configuración de Eventos
```javascript
// Esperar a que el DOM se cargue completamente
document.addEventListener("DOMContentLoaded", function() {
    // Obtener referencias a elementos
    const selectorMoneda = document.getElementById("selector-moneda");
    const botonActualizar = document.getElementById("boton-actualizar");
    const botonAlternar = document.getElementById("boton-alternar");
    
    // Configurar evento para cambio de moneda
    selectorMoneda.addEventListener("change", function() {
        monedaActual = this.value;
        obtenerPrecioBitcoin();
    });
    
    // Configurar evento para botón de actualizar
    botonActualizar.addEventListener("click", obtenerPrecioBitcoin);
    
    // Configurar evento para botón de alternar
    botonAlternar.addEventListener("click", alternarVisibilidadPrecio);
    
    // Configurar intervalos para actualizaciones automáticas
    setInterval(obtenerPrecioBitcoin, 3000);  // Actualizar precio cada 3 segundos
    setInterval(actualizarFechaHora, 1000);   // Actualizar fecha cada segundo
    
    // Llamadas iniciales
    obtenerPrecioBitcoin();
    actualizarFechaHora();
});
```

### 3. Estilos CSS (style.css)
```css
body {
    font-family: Arial, sans-serif;
    max-width: 800px;
    margin: 0 auto;
    padding: 20px;
    background-color: #f5f5f5;
}

h1 {
    color: #333;
    text-align: center;
    margin-bottom: 30px;
}

p {
    font-size: 18px;
    margin: 15px 0;
}

#precio {
    color: #2196F3;
    font-weight: bold;
    font-size: 24px;
}

select, button {
    padding: 10px 15px;
    margin: 10px 5px;
    font-size: 16px;
    border: 1px solid #ddd;
    border-radius: 5px;
    cursor: pointer;
}

button {
    background-color: #2196F3;
    color: white;
    transition: background-color 0.3s;
}

button:hover {
    background-color: #1976D2;
}

#fecha-hora {
    color: #666;
    font-style: italic;
}
```

## Conceptos Avanzados

### 1. Manejo de Errores
```javascript
async function obtenerPrecioBitcoin() {
    try {
        const respuesta = await fetch(urlApi);
        
        if (!respuesta.ok) {
            throw new Error(`Error HTTP: ${respuesta.status}`);
        }
        
        const datos = await respuesta.json();
        actualizarPrecio(datos.data.amount);
        
    } catch (error) {
        console.error("Error específico:", error);
        mostrarError("No se pudo obtener el precio");
    }
}
```

### 2. Formateo de Números
```javascript
function formatearPrecio(precio, moneda) {
    return new Intl.NumberFormat('es-ES', {
        style: 'currency',
        currency: moneda,
        minimumFractionDigits: 0,
        maximumFractionDigits: 0
    }).format(precio);
}
```

### 3. Almacenamiento Local
```javascript
// Guardar configuración del usuario
function guardarConfiguracion() {
    localStorage.setItem('monedaPreferida', monedaActual);
}

// Cargar configuración guardada
function cargarConfiguracion() {
    const monedaGuardada = localStorage.getItem('monedaPreferida');
    if (monedaGuardada) {
        monedaActual = monedaGuardada;
        document.getElementById('selector-moneda').value = monedaGuardada;
    }
}
```

## Mejores Prácticas

### 1. Organización del Código
```javascript
// Agrupar funciones relacionadas
const BitcoinTracker = {
    // Propiedades
    monedaActual: "USD",
    intervaloActualizacion: 3000,
    
    // Métodos
    init: function() {
        this.configurarEventos();
        this.iniciarActualizaciones();
    },
    
    configurarEventos: function() {
        // Código de configuración de eventos
    },
    
    obtenerPrecio: async function() {
        // Código para obtener precio
    }
};
```

### 2. Manejo de Errores Robusto
```javascript
function manejarError(error, contexto) {
    console.error(`Error en ${contexto}:`, error);
    
    // Mostrar mensaje amigable al usuario
    const elementoError = document.getElementById('mensaje-error');
    elementoError.textContent = 'Ha ocurrido un error. Intenta nuevamente.';
    elementoError.style.display = 'block';
    
    // Ocultar mensaje después de unos segundos
    setTimeout(() => {
        elementoError.style.display = 'none';
    }, 5000);
}
```

### 3. Optimización de Rendimiento
```javascript
// Evitar múltiples consultas al DOM
const elementos = {
    precio: document.getElementById('precio'),
    fecha: document.getElementById('fecha-hora'),
    selector: document.getElementById('selector-moneda')
};

// Usar elementos cacheados
elementos.precio.textContent = nuevoPrecio;
```

## Depuración y Herramientas

### 1. Console de Desarrollador
```javascript
// Diferentes tipos de logs
console.log('Información general');
console.error('Error importante');
console.warn('Advertencia');
console.table(datos);  // Mostrar datos en tabla
```

### 2. Herramientas del Navegador
- **Consola**: Para ver errores y probar código
- **Red**: Para inspeccionar llamadas API
- **Elementos**: Para ver cambios en el DOM
- **Sources**: Para depurar paso a paso

### 3. Técnicas de Depuración
```javascript
// Puntos de interrupción en código
function obtenerPrecio() {
    debugger;  // Pausa aquí
    // resto del código
}

// Verificar valores
console.log('Moneda actual:', monedaActual);
console.log('Respuesta API:', datos);
```

## Ejercicio Práctico: Expande la Aplicación

### Funcionalidades Adicionales a Implementar:

1. **Historial de Precios**
   - Mantener un array de precios históricos
   - Mostrar cambios porcentuales

2. **Múltiples Criptomonedas**
   - Añadir Ethereum, Litecoin, etc.
   - Crear una tabla comparativa

3. **Alertas de Precio**
   - Permitir al usuario configurar alertas
   - Mostrar notificaciones cuando se alcance un precio objetivo

4. **Gráfico Simple**
   - Crear un gráfico de líneas básico
   - Mostrar tendencias de precio

### Código Base para Historial:
```javascript
// Array para almacenar historial
let historialPrecios = [];

function agregarAlHistorial(precio) {
    const entrada = {
        precio: precio,
        fecha: new Date(),
        moneda: monedaActual
    };
    
    historialPrecios.push(entrada);
    
    // Mantener solo últimos 20 registros
    if (historialPrecios.length > 20) {
        historialPrecios.shift();
    }
}
```

## Recursos Adicionales

### Documentación y Guías
- [MDN JavaScript Guide](https://developer.mozilla.org/es/docs/Web/JavaScript/Guide)
- [JavaScript.info](https://javascript.info/)
- [Fetch API Documentation](https://developer.mozilla.org/es/docs/Web/API/Fetch_API)

### APIs para Practicar
- Coinbase API (precios de criptomonedas)
- OpenWeatherMap API (clima)
- JSONPlaceholder (datos de prueba)
- REST Countries API (información de países)

### Herramientas de Desarrollo
- Chrome DevTools
- Firefox Developer Tools
- Visual Studio Code con extensiones JavaScript

## Conceptos para Explorar Después

### 1. Frameworks y Librerías
- React.js para aplicaciones más complejas
- Vue.js para desarrollo progresivo
- Node.js para backend con JavaScript

### 2. Conceptos Avanzados
- Promises y async/await
- Modules (import/export)
- Programación funcional
- Patrones de diseño

### 3. Herramientas de Desarrollo
- npm (Node Package Manager)
- Webpack para bundling
- Babel para compatibilidad
- ESLint para código limpio

## Consejos para el Éxito

### 1. Práctica Constante
- Construye proyectos pequeños regularmente
- Experimenta con diferentes APIs
- Recrea funcionalidades que ves en otras webs

### 2. Depuración Efectiva
- Usa console.log generosamente
- Aprende a usar las herramientas de desarrollo
- Lee mensajes de error cuidadosamente

### 3. Construcción de Buenas Prácticas
- Escribe código limpio y comentado
- Organiza tu código en funciones pequeñas
- Maneja errores apropiadamente
- Prueba tu código en diferentes navegadores

### 4. Comunidad y Aprendizaje
- Participa en foros de desarrollo
- Lee código de otros desarrolladores
- Contribuye a proyectos open source
- Mantente actualizado con nuevas características

## Proyectos de Práctica Sugeridos

1. **Calculadora Interactiva**
   - Botones para números y operaciones
   - Pantalla para mostrar resultados
   - Manejo de errores

2. **Lista de Tareas (Todo List)**
   - Agregar, editar y eliminar tareas
   - Marcar tareas como completadas
   - Filtrar por estado

3. **Generador de Citas**
   - Obtener citas de una API
   - Botón para nueva cita
   - Compartir en redes sociales

4. **Aplicación del Clima**
   - Buscar clima por ciudad
   - Mostrar información detallada
   - Pronóstico de varios días

## Recuerda

JavaScript es un lenguaje poderoso y versátil. No te desanimes si al principio parece complejo - todos los desarrolladores han pasado por el mismo proceso de aprendizaje. La clave está en:

- **Práctica regular**: Escribe código todos los días
- **Construir proyectos**: Aprende haciendo, no solo leyendo
- **Experimentar**: No tengas miedo de probar cosas nuevas
- **Pedir ayuda**: La comunidad de desarrolladores es muy colaborativa

Con JavaScript, has dado el paso más importante hacia convertirte en un desarrollador web completo. ¡Ahora tienes las herramientas para crear aplicaciones web interactivas y dinámicas!

¡Felicidades por llegar hasta aquí! El próximo paso es seguir construyendo proyectos y explorando las infinitas posibilidades que JavaScript te ofrece. 🚀

---

*Traducido y adaptado del contenido original de PlebDevs* 