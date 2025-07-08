[![Watch the lesson video](https://img.shields.io/badge/Watch-Lesson%20Video-blue)](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-1-lesson-1.mp4)

*You can access the lesson video directly here:*
- [MP4 Format](https://plebdevs-bucket.nyc3.cdn.digitaloceanspaces.com/course-1-lesson-1.mp4)

# HTML & Bitcoin: Building Your First Bitcoin Blog

## Lesson Overview
Welcome to the first lesson of the PlebDevs Frontend Course! In this lesson, we'll dive into HTML (HyperText Markup Language) and learn how to build a basic Bitcoin blog. This is your foundation for web development and the first step in your journey to building Lightning applications.

## Prerequisites
- Text editor (Sublime Text, VS Code, or even Notepad)
- Web browser (Firefox, Chrome, Safari, etc.)
- Basic computer navigation skills

## Key Learning Objectives
- Understand what HTML is and how it works
- Learn HTML's box model and element structure
- Master common HTML elements and their attributes
- Build forms for user input
- Create a functional Bitcoin blog with styling
- Establish good HTML coding practices

## What is HTML?

### Definition
HTML is a markup language that instructs web browsers how to display a web page. It's not a programming language since it can't perform calculations - it simply tells browsers how to display images, text, and other content.

### How HTML Works
1. **HTML documents are hosted on servers**
2. **When you visit a website, your browser downloads the HTML document**
3. **The browser reads the HTML and displays the content according to the instructions**

### Basic HTML Structure
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>My Page Title</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <p>This is my first HTML page.</p>
    </body>
</html>
```

## Understanding the Box Model

### What is the Box Model?
HTML uses a box model to display content. It divides content into different boxes (called elements) and displays those boxes on the page.

### Basic Example
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
    </head>
    <body>
        <div class="greeting">
            Hello world!
        </div>
        <div class="info">
            This div illustrates HTML's box model
        </div>
    </body>
</html>
```

### Visualizing Boxes
To see the box model in action, add background colors:
```html
<div style="background-color: red">
    Hello world!
</div>
<div style="background-color: yellow">
    This div illustrates HTML's box model
</div>
```

## Block vs. Inline Elements

### Block Elements
- **Cover the full width of the page**
- **Force new lines before and after**
- **Examples**: `<div>`, `<p>`, `<h1>`, `<h2>`

### Inline Elements
- **Have variable width**
- **Multiple inline elements can be on the same line**
- **Examples**: `<span>`, `<a>`, `<img>`

### Example Comparison
```html
<!-- Block elements (stacked) -->
<div>Block element 1</div>
<div>Block element 2</div>

<!-- Inline elements (side by side) -->
<span>Inline element 1</span>
<span>Inline element 2</span>
```

## Common HTML Elements

### Document Structure
| Element | Purpose |
|---------|---------|
| `<!DOCTYPE html>` | Declares document type |
| `<html>` | Root element |
| `<head>` | Document metadata |
| `<body>` | Visible content |
| `<meta>` | Metadata (charset, viewport) |
| `<title>` | Page title (appears in browser tab) |

### Content Elements
| Element | Purpose | Type |
|---------|---------|------|
| `<div>` | Generic container | Block |
| `<span>` | Generic inline container | Inline |
| `<p>` | Paragraph | Block |
| `<h1>`, `<h2>` | Headers | Block |
| `<a>` | Links | Inline |
| `<img>` | Images | Inline |
| `<br>` | Line break | None |
| `<hr>` | Horizontal rule (line) | Block |

### Form Elements
| Element | Purpose | Type |
|---------|---------|------|
| `<form>` | Form container | Block |
| `<input>` | User input field | Inline |
| `<textarea>` | Multi-line text input | Inline |
| `<button>` | Clickable button | Inline |

## Element Attributes

### Common Attributes
- **`style`** - Change appearance (color, font, size)
- **`class`** - Categorization and CSS/JavaScript hooks
- **`id`** - Unique identifier for anchor links
- **`name`** - Pass form data to servers
- **`data-*`** - Custom attributes

### Attribute Examples
```html
<div class="word" style="background-color: red">Text</div>
<a href="https://google.com" id="google-link">Google</a>
<input name="username" type="text" placeholder="Enter username">
```

### Anchor Links
Create internal page navigation:
```html
<a href="#section1">Go to Section 1</a>
<p id="section1">This is section 1</p>
```

## Working with Forms

### Basic Form Structure
```html
<form action="/login/">
    <p><input name="username" type="text" placeholder="username"></p>
    <p><textarea name="message"></textarea></p>
    <p><button>Submit</button></p>
</form>
```

### Form Behavior
- **Action attribute**: Where to send form data
- **Name attributes**: Identify form fields
- **Submit button**: Triggers form submission
- **URL encoding**: Data appears as `?username=value&message=value`

### Important Note
Forms require server-side code or JavaScript to process data. Without it, forms will either do nothing or show errors.

## Building Your Bitcoin Blog

### Project Overview
We'll create a blog post about Bitcoin using the Bitcoin whitepaper content. This will include:
- Header with title and author info
- Abstract and introduction sections
- Comment form (non-functional)
- Basic CSS styling

### Step 1: Basic Structure
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Bitcoin Blog</title>
    </head>
    <body>
        <h1>Bitcoin: A Peer-to-Peer Electronic Cash System</h1>
        <div class="info">
            Satoshi Nakamoto<br>
            satoshin@gmx.com<br>
            www.bitcoin.org<br>
        </div>
    </body>
</html>
```

### Step 2: Add Content
```html
<p class="abstract">
    Abstract. A purely peer-to-peer version of electronic cash would allow online payments to be sent directly from one party to another without going through a financial institution. Digital signatures provide part of the solution, but the main benefits are lost if a trusted third party is still required to prevent double-spending. We propose a solution to the double-spending problem using a peer-to-peer network...
</p>

<h2 class="introduction">I. Introduction</h2>

<p>
    Commerce on the Internet has come to rely almost exclusively on financial institutions serving as trusted third parties to process electronic payments...
</p>
```

### Step 3: Add Comment Section
```html
<hr>
<h2 class="comments">Comments</h2>

<p>Name</p>
<p class="input_name">
    <input type="text" name="name">
</p>

<p>Comment</p>
<p>
    <textarea rows="8" name="comment" class="input_comment"></textarea>
</p>

<p>
    <button type="button" name="submit_comment">Submit</button>
</p>
```

### Step 4: Add Basic Styling
```html
<head>
    <meta charset="UTF-8">
    <style>
        * {
            box-sizing: border-box;
            font-size: 1.15em;
            font-family: Arial, sans-serif;
        }
        html {
            max-width: 70ch;
            padding: 3em 1em;
            margin: auto;
            line-height: 1.25;
        }
        h1 {
            font-size: 2em;
        }
        h2 {
            font-size: 1.5em;
        }
        input, textarea {
            width: 100%;
            height: 1.8em;
            border: 1px solid grey;
        }
        textarea {
            height: auto;
        }
        .info {
            text-align: center;
        }
    </style>
</head>
```

### Step 5: Add Navigation
```html
<nav>
    <a href="https://example.com/home.html">
        &larr; more articles
    </a>
</nav>
```

## CSS Fundamentals (Preview)

### What CSS Does
- **Cascading Style Sheets** make your HTML look beautiful
- **Separates content from presentation**
- **Enables responsive design**

### Key CSS Concepts
- **Selectors**: Target specific elements (`*`, `html`, `h1`, `.class`)
- **Properties**: What to change (`color`, `font-size`, `margin`)
- **Values**: How to change it (`red`, `2em`, `auto`)
- **Cascading**: Later rules override earlier ones

### Box Sizing
```css
* {
    box-sizing: border-box;
}
```
This ensures borders and padding are included in element width calculations.

## Best Practices

### 1. File Organization
- Use descriptive filenames (`blog.html`, not `page1.html`)
- Keep related files together
- Use lowercase for file names

### 2. Code Structure
- Proper indentation makes code readable
- Use meaningful class names
- Add comments for complex sections

### 3. Semantic HTML
- Use appropriate elements for their purpose
- `<h1>` for main headings, `<p>` for paragraphs
- `<nav>` for navigation, `<main>` for main content

### 4. Testing
- Test in multiple browsers
- Use "View Page Source" to learn from other sites
- Validate your HTML

## Common Issues and Solutions

### 1. Elements Not Displaying
- Check for unclosed tags
- Ensure proper nesting
- Verify file saved with `.html` extension

### 2. Styles Not Working
- Check for typos in CSS
- Ensure CSS is in `<head>` section
- Remember cascading order matters

### 3. Forms Not Working
- Forms need server-side processing
- Use `action` attribute to specify destination
- Include `name` attributes on inputs

## Hands-on Exercises

### Exercise 1: Personal Blog Post
1. Create a new HTML file
2. Write a blog post about why you're learning Bitcoin development
3. Include headers, paragraphs, and a comment form
4. Add basic styling

### Exercise 2: Navigation Practice
1. Create multiple HTML pages
2. Link them together with navigation
3. Use anchor links for internal navigation
4. Test all links work correctly

### Exercise 3: Form Experimentation
1. Create a contact form
2. Include different input types
3. Add placeholder text
4. Style the form elements

## Learning Resources

### Essential References
- **[W3Schools HTML Tutorial](https://www.w3schools.com/html/)** - Comprehensive HTML reference
- **[MDN Web Docs](https://developer.mozilla.org/en-US/docs/Web/HTML)** - Official documentation
- **[HTML Validator](https://validator.w3.org/)** - Check your HTML

### Search Tips
When you don't know how to do something:
1. Google: "How do I [what you want] in HTML?"
2. Include "w3schools" in your search
3. Use "View Page Source" on websites you like
4. Practice with online code editors

### Example Searches
- "w3schools checkboxes HTML"
- "how to make left arrow HTML"
- "HTML table tutorial"
- "responsive HTML forms"

## Next Steps

### Immediate Actions
1. **Complete the blog project** from this lesson
2. **Experiment with different HTML elements**
3. **Practice viewing source code** on websites
4. **Start building your own pages**

### Prepare for Next Lesson
In the next lesson, we'll dive deeper into CSS and learn how to:
- Style your HTML professionally
- Create responsive layouts
- Use CSS frameworks
- Make your Bitcoin blog look amazing

### Building Your Portfolio
- Save all your HTML files
- Create a simple portfolio page
- Link your projects together
- Share your work on GitHub

## Key Takeaways

1. **HTML is the foundation** of all web development
2. **Box model understanding** is crucial for layout
3. **Practice by building** real projects
4. **View source** to learn from others
5. **Google is your friend** for specific questions
6. **Start simple** and iterate

Remember: Every expert was once a beginner. The key is to start coding, make mistakes, learn from them, and keep building. Your Bitcoin blog is the first step in your journey to becoming a Lightning developer!

Happy coding! ⚡️

---

## Complete Blog Example

Here's the complete HTML for the Bitcoin blog we built in this lesson:

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Bitcoin Blog</title>
        <style>
            * {
                box-sizing: border-box;
                font-size: 1.15em;
                font-family: Arial, sans-serif;
            }
            html {
                max-width: 70ch;
                padding: 3em 1em;
                margin: auto;
                line-height: 1.25;
            }
            h1 {
                font-size: 2em;
            }
            h2 {
                font-size: 1.5em;
            }
            input, textarea {
                width: 100%;
                height: 1.8em;
                border: 1px solid grey;
            }
            textarea {
                height: auto;
            }
            .info {
                text-align: center;
            }
        </style>
    </head>
    <body>
        <nav>
            <a href="https://example.com/home.html">
                &larr; more articles
            </a>
        </nav>
        <h1>
            Bitcoin: A Peer-to-Peer Electronic Cash System
        </h1>
        <div class="info">
            Satoshi Nakamoto<br>
            satoshin@gmx.com<br>
            www.bitcoin.org<br>
        </div>
        <p class="abstract">
            Abstract. A purely peer-to-peer version of electronic cash would allow online payments to be sent directly from one party to another without going through a financial institution. Digital signatures provide part of the solution, but the main benefits are lost if a trusted third party is still required to prevent double-spending. We propose a solution to the double-spending problem using a peer-to-peer distributed timestamp server to generate computational proof of the chronological order of transactions. The system is secure as long as honest nodes collectively control more CPU power than any cooperating group of attacker nodes.
        </p>
        <h2 class="introduction">
            I. Introduction
        </h2>
        <p>
            Commerce on the Internet has come to rely almost exclusively on financial institutions serving as trusted third parties to process electronic payments. While the system works well enough for most transactions, it still suffers from the inherent weaknesses of the trust based model. Completely non-reversible transactions are not really possible, since financial institutions cannot avoid mediating disputes. The cost of mediation increases transaction costs, limiting the minimum practical transaction size and cutting off the possibility for small casual transactions, and there is a broader cost in the loss of ability to make non-reversible payments for non-reversible services. With the possibility of reversal, the need for trust spreads. Merchants must be wary of their customers, hassling them for more information than they would otherwise need. A certain percentage of fraud is accepted as unavoidable. These costs and payment uncertainties can be avoided in person by using physical currency, but no mechanism exists to make payments over a communications channel without a trusted party.
        </p>
        <p>
            What is needed is an electronic payment system based on cryptographic proof instead of trust, allowing any two willing parties to transact directly with each other without the need for a trusted third party. Transactions that are computationally impractical to reverse would protect sellers from fraud, and routine escrow mechanisms could easily be implemented to protect buyers. In this paper, we propose a solution to the double-spending problem using a peer-to-peer distributed timestamp server to generate computational proof of the chronological order of transactions. The system is secure as long as honest nodes collectively control more CPU power than any cooperating group of attacker nodes.
        </p>
        <hr>
        <h2 class="comments">
            Comments
        </h2>
        <p>
            Name
        </p>
        <p class="input_name">
            <input type="text" name="name">
        </p>
        <p>
            Comment
        </p>
        <p>
            <textarea rows="8" name="comment" class="input_comment"></textarea>
        </p>
        <p>
            <button type="button" name="submit_comment">Submit</button>
        </p>
    </body>
</html>
```

Save this as `blog.html` and open it in your browser to see your Bitcoin blog in action! 