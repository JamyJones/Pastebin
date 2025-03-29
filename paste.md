## Linking CSS in Your GitHub Pages Project <br>
---<br>
To link a CSS file in your GitHub Pages project, you need to ensure that your CSS file is correctly referenced in your HTML files. Here’s how to do it step by step:

1. **Organize Your Files**  
   Make sure your CSS file is in the correct directory. A common practice is to create a folder named `css` in your project directory and place your CSS file there. For example:
   ```
   /my-github-page
   ├── index.html
   └── css
       └── styles.css
   ```

2. **Linking the CSS File in HTML**  
   In your HTML file (e.g., `index.html`), you need to use the `<link>` tag within the `<head>` section to link your CSS file. Here’s how to do it:
   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>My GitHub Page</title>
       <link rel="stylesheet" href="css/styles.css">
   </head>
   <body>
       <h1>Welcome to My GitHub Page</h1>
   </body>
   </html>
   ```
   - **Explanation of the `<link>` tag**:
     - `rel="stylesheet"`: This attribute specifies the relationship between the current document and the linked resource. In this case, it indicates that the linked file is a stylesheet.
     - `href="css/styles.css"`: This attribute specifies the path to the CSS file. Make sure the path is correct relative to the HTML file.

3. **Check Your GitHub Pages Settings**  
   Ensure that your GitHub Pages is set up correctly. Go to your repository settings, scroll down to the "GitHub Pages" section, and make sure the source branch is set (usually `main` or `master`).

4. **Commit and Push Your Changes**  
   After making these changes, commit and push your updates to your GitHub repository. Your CSS should now be linked and applied to your HTML page when you view it on GitHub Pages.

---  
Example:  
If your CSS file is named `styles.css` and is located in a folder named `css`, the link in your HTML should look like this:
```html
<link rel="stylesheet" href="css/styles.css">
```
This will apply the styles defined in `styles.css` to your HTML document.

---  
## References:  
## https://pages.github.com/  
## https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link