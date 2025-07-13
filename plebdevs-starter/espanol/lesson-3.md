# Configurar Git y GitHub: La Base de un Desarrollador

## Introducción
En esta lección, estableceremos una de las bases más importantes de tu viaje de desarrollo: el control de versiones con Git y GitHub. Este conocimiento te permitirá rastrear tu código, respaldarlo en la nube y comenzar a construir tu portafolio de desarrollador.

## Prerrequisitos
- Visual Studio Code instalado
- Conceptos básicos de Terminal/Línea de Comandos
- Cuenta de GitHub (la crearemos en esta lección)

## Objetivos de Aprendizaje
- Comprender qué son Git y GitHub y por qué son esenciales
- Configurar Git localmente y conectarlo a GitHub
- Aprender comandos básicos de Git y flujo de trabajo
- Crear tu primer repositorio y commit
- Establecer buenos hábitos de Git para tu viaje de desarrollador

## ¿Qué es Git y GitHub?

### Git: Tu Control de Versiones Local
Git es un sistema de control de versiones que ayuda a realizar un seguimiento de los cambios en el código a lo largo del tiempo. Permite que varias personas trabajen en el mismo proyecto sin sobrescribir el trabajo de los demás.

**Características principales:**
- Rastrea cambios en tu código
- Previene sobrescribir accidentalmente tu trabajo
- Permite colaboración segura entre desarrolladores
- Funciona localmente en tu máquina

### GitHub: Tu Código en la Nube
GitHub es una plataforma basada en la web que utiliza Git para almacenar y compartir código. Proporciona funciones de colaboración adicionales como:
- Seguimiento de incidencias
- Solicitudes de incorporación de cambios (Pull Requests)
- Herramientas de gestión de proyectos
- Capacidades de revisión de código

## ¿Por qué usar GitHub?

### 1. Construcción de Portafolio
- Actúa como tu "prueba de trabajo" como desarrollador
- Muestra tu actividad de codificación a través de gráficos de contribución
- Demuestra tu consistencia y dedicación
- Sirve como una vitrina pública de tus proyectos

### 2. Colaboración y Aprendizaje
- Realiza fácilmente un seguimiento de las actualizaciones de tus propios proyectos
- Colabora con otros desarrolladores en proyectos
- Fácil acceso a todo el código fuente abierto del mundo
- Abre tu código para recibir comentarios o contribuciones

### 3. Seguridad y Acceso al Código
- Copia de seguridad de todos tus proyectos de código en la nube de forma gratuita
- Accede a tus proyectos desde cualquier lugar
- Nunca pierdas tu trabajo por problemas de computadora

## Terminología Esencial de GitHub

| Término | Definición |
|---------|------------|
| **Repository (Repositorio)** | La carpeta que contiene todos los archivos de código, por lo general, se refiere a todo el proyecto |
| **Commit** | Una confirmación es un cambio guardado o una adición a un repositorio que puede ser local o remoto |
| **Staging (Preparación)** | El ensayo significa que has marcado un número de confirmaciones para enviarlas al repositorio remoto |
| **Push** | Una inserción es el acto de enviar tus confirmaciones preconfiguradas locales a tu repositorio de GitHub alojado |
| **Branch (Rama)** | Una rama es una versión del repositorio que difiere del proyecto de trabajo principal |
| **Pull Request/PR** | Una solicitud de incorporación de cambios es una solicitud para que una colección de cambios realizados se combine en la rama principal del repositorio |
| **Clone** | La clonación consiste simplemente en extraer una copia local de un repositorio remoto |
| **Fork** | La bifurcación consiste en tomar una copia de un repositorio remoto que ya existe y alojarla en tu propio github |

## Configuración Práctica

### Instalar Git Localmente/Conectarse a GitHub

#### Paso 1: Instalar Git
1. Visita [git-scm.com](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
2. Descarga la versión para tu sistema operativo
3. Ejecuta el instalador
4. Sigue las instrucciones de instalación

#### Paso 2: Configurar tu Identidad
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "TU_EMAIL@ejemplo.com"
```

#### Paso 3: Instalar GitHub CLI (Opcional pero Recomendado)
1. Visita [cli.github.com](https://cli.github.com/)
2. Instala GitHub CLI
3. Autentica con GitHub:
```bash
gh auth login
```

### Tu Primer Repositorio
1. Crea un nuevo repositorio en GitHub llamado "hola-mundo"
2. Inicializa Git localmente:
```bash
git init
git add .
git commit -m "Mi primer commit"
git remote add origin <url-de-tu-repositorio>
git push -u origin main
```

## Flujo de Trabajo Básico de Git

### Proceso para Enviar Código a GitHub
```bash
# 1. Realizar un cambio en un archivo en el editor de código
# 2. Cambios en la puesta en escena
git add .

# 3. Confirmar cambios con un mensaje
git commit -m "Escribe un mensaje sobre tu cambio aquí"

# 4. Enviar al repositorio remoto
git push
```

### Proceso para Extraer Código de GitHub
```bash
# SI tienes un repositorio en tu máquina local
git pull

# SI NO tienes repositorio
git clone https://github.com/nombre/repo.git
```

## Construyendo Buenos Hábitos

### Práctica Diaria de Git
- Haz que sea una meta empujar código todos los días
- Incluso cambios pequeños cuentan
- Usa tu gráfico de contribución de GitHub como motivación
- Rastrea tu progreso a lo largo del tiempo

### Mejores Prácticas
1. **Haz commits frecuentes** con mensajes claros
2. **Haz pull antes de empezar** a trabajar
3. **Haz push de tus cambios** cuando termines
4. **Mantén cada proyecto** en su propio repositorio
5. **Incluye archivos README** para explicar tus proyectos

## Problemas Comunes y Soluciones

### Error "No upstream branch"
Si ves este error al hacer push:
```bash
git push --set-upstream origin main
```

### Los Cambios No Se Muestran
1. Verifica si los cambios están preparados:
```bash
git status
```
2. Asegúrate de haber hecho commit:
```bash
git commit -m "Tu mensaje"
```
3. Verifica que hayas hecho push:
```bash
git push
```

### Verificar el Estado de tu Repositorio
```bash
git status    # Ver el estado actual
git log       # Ver historial de commits
git diff      # Ver cambios no confirmados
```

## Ejercicio Práctico: Inicia tu Viaje

### Paso a Paso
1. **Crea tu cuenta de GitHub** si aún no lo has hecho
2. **Configura Git localmente** usando los comandos que cubrimos
3. **Crea tu primer repositorio** llamado "hola-mundo"
4. **Haz tu primer commit** con un archivo HTML básico
5. **Haz push de tu código** a GitHub
6. **Establece el hábito** de hacer push de código diariamente

### Ejemplo de Archivo HTML para tu Primer Commit
```html
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mi Primer Proyecto</title>
</head>
<body>
    <h1>¡Hola Mundo!</h1>
    <p>Este es mi primer proyecto en GitHub.</p>
</body>
</html>
```

## Consejos para el Éxito

### 1. Consistencia es Clave
- Haz commits pequeños y frecuentes
- Escribe mensajes de commit descriptivos
- Mantén un flujo de trabajo regular

### 2. Organización
- Un repositorio por proyecto
- Nombres claros y descriptivos
- Estructura de carpetas lógica

### 3. Colaboración
- Aprende a leer código de otros
- Contribuye a proyectos de código abierto
- Pide feedback en tu código

## Recursos Adicionales
- [Documentación de GitHub](https://docs.github.com)
- [Documentación de Git](https://git-scm.com/doc)
- [Guía de inicio de GitHub](https://docs.github.com/en/get-started/getting-started-with-git/set-up-git)

## Próximos Pasos

1. **Practica el Flujo de Trabajo**
   - Crea y gestiona repositorios
   - Usa comandos básicos de Git
   - Establece una rutina diaria

2. **Construye tu Portafolio**
   - Haz push de todos tus proyectos
   - Mantén un README descriptivo
   - Muestra tu progreso consistentemente

3. **Prepárate para la Próxima Lección**
   - Mantén GitHub activo
   - Practica comandos básicos
   - Prepárate para aprender HTML y CSS

## Recuerda
GitHub puede actuar como tu prueba de trabajo. Como plebdev, puedes pensar en él como el portafolio de todo tu código, mostrando tu progreso, lo que has hecho y la cantidad de código que has estado sacando. Cada desarrollador experto comenzó donde estás ahora. La clave es la consistencia y la persistencia.

¡Feliz codificación! 🚀

---

*Traducido y adaptado del contenido original de PlebDevs* 