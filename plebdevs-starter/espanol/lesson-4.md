# HTML: Construyendo la Estructura de la Web

## Introducción
En esta lección, aprenderemos HTML (HyperText Markup Language), el lenguaje fundamental para crear páginas web. HTML es como el esqueleto de una casa: proporciona la estructura básica sobre la cual todo lo demás se construye.

## Prerrequisitos
- Visual Studio Code instalado
- Git y GitHub configurados
- Conocimiento básico de archivos y carpetas
- Motivación para crear tu primera página web

## Objetivos de Aprendizaje
- Comprender qué es HTML y por qué es fundamental
- Aprender la estructura básica de un documento HTML
- Dominar las etiquetas HTML esenciales
- Crear tu primera página web funcional
- Entender el concepto de HTML semántico
- Establecer buenas prácticas de desarrollo web

## ¿Qué es HTML?

### Definición
HTML (HyperText Markup Language) es el lenguaje de marcado estándar para crear páginas web. Es la base de toda la web y define la estructura y el contenido de las páginas.

**Características principales:**
- Es un lenguaje de marcado, no de programación
- Utiliza etiquetas para estructurar el contenido
- Los navegadores interpretan HTML para mostrar las páginas
- Es el fundamento sobre el cual se construyen CSS y JavaScript

### La Analogía de la Construcción
Cuando construyes una página web, piénsalo como construir una casa:
- **HTML**: El armazón y la estructura (paredes, habitaciones, diseño)
- **CSS**: Los elementos de diseño (pintura, decoraciones, estilo)
- **JavaScript**: La funcionalidad (plomería, electricidad, partes móviles)

## Estructura Básica de HTML

### Plantilla HTML Básica
Toda página web comienza con una plantilla básica:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tu Título de Página</title>
</head>
<body>
    <!-- Tu contenido va aquí -->
</body>
</html>
```

### Comprendiendo las Partes
- `<!DOCTYPE html>`: Define el tipo de documento como HTML5
- `<html>`: El elemento raíz que encierra todo el contenido HTML
- `<head>`: Contiene metadatos sobre el documento
- `<title>`: Establece el título de la página
- `<body>`: Contiene todo el contenido visible de la página

## Elementos HTML Esenciales

### 1. Encabezados
HTML tiene seis niveles de encabezados:
```html
<h1>Título Principal</h1>
<h2>Subtítulo</h2>
<h3>Encabezado de Sección</h3>
<h4>Subsección</h4>
<h5>Encabezado Menor</h5>
<h6>Encabezado Más Pequeño</h6>
```

### 2. Párrafos
```html
<p>Este es un párrafo de texto. Puede contener tanto texto como necesites.</p>
```

### 3. Imágenes
```html
<img src="ruta-a-imagen.jpg" alt="Descripción de la imagen" width="300">
```

### 4. Enlaces
```html
<a href="https://ejemplo.com">Haz clic aquí</a>
```

## Atributos HTML
Los atributos proporcionan información adicional o modifican elementos HTML:

```html
<etiqueta atributo="valor">Contenido</etiqueta>
```

**Atributos comunes:**
- `src`: Ruta de origen para imágenes
- `href`: Destino para enlaces
- `alt`: Texto alternativo para imágenes
- `class`: Nombres de clase CSS
- `id`: Identificador único
- `width`: Ancho del elemento

## Construyendo tu Primera Página Web

### Ejemplo Práctico
Vamos a crear una página personal paso a paso:

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Primera Página Web</title>
</head>
<body>
    <h1>¡Bienvenido a Mi Primera Página Web!</h1>
    <p>Este es un proyecto simple para empezar con HTML.</p>
    
    <h2>Sobre Mí</h2>
    <p>Hola, soy un principiante aprendiendo desarrollo web. ¡Esta es mi primera página web!</p>
    
    <h2>Una Imagen Divertida</h2>
    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTPDkcqjFiI0DCMU_VGSk-sTo0_0Bovwofb7hy1vzRm6g&s" 
         alt="Una imagen divertida" width="200">
</body>
</html>
```

## El Div y HTML Semántico

### Comprendiendo `<div>`
- Elemento contenedor utilizado para agrupar elementos HTML
- Se utiliza normalmente para estilizar o maquetar con CSS
- Ejemplo: `<div class="header">Contenido del header aquí</div>`

### ¿Qué es el HTML Semántico?
Los elementos HTML semánticos describen claramente su significado de forma legible por humanos y máquinas. Ayudan al:
- SEO (optimización para motores de búsqueda)
- Accesibilidad
- Mantenimiento del código

### Ejemplos de Etiquetas HTML Semánticas
```html
<header>Contenido introductorio o enlaces de navegación</header>
<nav>Enlaces de navegación</nav>
<main>Contenido principal</main>
<article>Contenido independiente y autocontenido</article>
<section>Define secciones en un documento</section>
<footer>Pie de página de un documento o sección</footer>
```

### Sustituyendo lo No Semántico por lo Semántico
En lugar de utilizar `<div>` no semánticos en todas partes, utiliza etiquetas semánticas donde corresponda:

**Antes:**
```html
<div class="header">
    <div class="nav">Enlaces</div>
</div>
<div class="content">
    <div class="article">Contenido</div>
</div>
<div class="footer">Pie de página</div>
```

**Después:**
```html
<header>
    <nav>Enlaces</nav>
</header>
<main>
    <article>Contenido</article>
</main>
<footer>Pie de página</footer>
```

## Ejemplo Completo con HTML Semántico

```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Primera Página Web</title>
</head>
<body>
    <!-- Sección del header -->
    <header>
        <h1>¡Bienvenido a Mi Primera Página Web!</h1>
        <p>Este es un proyecto simple para empezar con HTML y CSS.</p>
    </header>
    
    <!-- Sección del contenido principal -->
    <main>
        <section>
            <h2>Sobre Mí</h2>
            <p>Hola, soy un principiante aprendiendo desarrollo web. ¡Esta es mi primera página web!</p>
        </section>
        
        <section>
            <h2>Una Imagen Divertida</h2>
            <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTPDkcqjFiI0DCMU_VGSk-sTo0_0Bovwofb7hy1vzRm6g&s"
                 alt="Una imagen divertida" width="200">
        </section>
    </main>
    
    <!-- Sección del footer -->
    <footer>
        <p>&copy; 2024 Tu Nombre. Todos los derechos reservados.</p>
    </footer>
</body>
</html>
```

## Mejores Prácticas

### 1. Estructura del Código
- Usa indentación adecuada
- Mantén el código organizado y legible
- Utiliza elementos semánticos cuando sea posible
- Incluye todos los elementos requeridos

### 2. Contenido
- Usa niveles de encabezado apropiados (comienza con `h1`)
- Escribe texto alternativo descriptivo para imágenes
- Mantén el contenido significativo y organizado
- Usa comentarios para explicar secciones complejas

### 3. Accesibilidad
- Utiliza elementos HTML semánticos
- Proporciona texto alternativo para imágenes
- Mantén una estructura lógica de encabezados
- Asegúrate de que el contenido tenga sentido cuando se lee linealmente

## Problemas Comunes y Soluciones

### Problema: Las Imágenes No Cargan
```html
<!-- Incorrecto -->
<img src="imagen.jpg">

<!-- Correcto -->
<img src="./imagenes/imagen.jpg" alt="Descripción">
```

### Problema: Los Enlaces No Funcionan
```html
<!-- Incorrecto -->
<a>Haz clic aquí</a>

<!-- Correcto -->
<a href="https://ejemplo.com">Haz clic aquí</a>
```

### Problema: Estructura HTML Incorrecta
```html
<!-- Incorrecto -->
<h1>Título</h1>
<h3>Subtítulo</h3>

<!-- Correcto -->
<h1>Título</h1>
<h2>Subtítulo</h2>
```

## Ejercicio Práctico: Crea tu Página de Perfil

### Paso a Paso
1. **Crea un nuevo archivo** llamado `perfil.html`
2. **Añade la estructura básica** HTML
3. **Incluye información sobre ti:**
   - Tu nombre en un `<h1>`
   - Una sección "Sobre Mí" con `<h2>` y `<p>`
   - Una lista de tus intereses
   - Una imagen (opcional)
4. **Usa HTML semántico** apropiado
5. **Añade un footer** con información de contacto

### Plantilla de Inicio
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Perfil - [Tu Nombre]</title>
</head>
<body>
    <header>
        <h1>[Tu Nombre]</h1>
        <p>Futuro PlebDev en formación</p>
    </header>
    
    <main>
        <!-- Añade tu contenido aquí -->
    </main>
    
    <footer>
        <p>Contacto: [tu-email@ejemplo.com]</p>
    </footer>
</body>
</html>
```

## Consejos para el Éxito

### 1. Practica Regularmente
- Crea diferentes tipos de páginas
- Experimenta con diferentes elementos
- Mantén tu código limpio y organizado

### 2. Enfócate en la Semántica
- Piensa en el significado del contenido
- Usa las etiquetas apropiadas para cada tipo de contenido
- Prioriza la accesibilidad y usabilidad

### 3. Construye Buenos Hábitos
- Siempre incluye texto alternativo para imágenes
- Usa estructura de encabezados lógica
- Mantén tu código indentado y comentado

## Recursos Adicionales
- [Documentación de HTML - MDN](https://developer.mozilla.org/es/docs/Web/HTML)
- [Guía de HTML5 Doctor](http://html5doctor.com/)
- [Tutorial de HTML - W3Schools](https://www.w3schools.com/html/)

## Próximos Pasos

1. **Domina HTML**
   - Crea varias páginas web
   - Experimenta con diferentes elementos
   - Practica HTML semántico

2. **Prepárate para CSS**
   - Piensa en cómo quieres que se vea tu página
   - Considera qué estilos querrás añadir
   - Planifica la estructura de tu diseño

3. **Construye tu Portafolio**
   - Sube todos tus proyectos HTML a GitHub
   - Documenta tu progreso
   - Comparte tu trabajo con la comunidad

## Recuerda
HTML es la base del desarrollo web. Tómate el tiempo para entender bien estos conceptos básicos, ya que servirán como los bloques de construcción para todo lo demás que aprenderás. Cada página web que ves comenzó con HTML, y ahora tú también puedes crear las tuyas.

¡Feliz codificación! 🚀

---

*Traducido y adaptado del contenido original de PlebDevs* 