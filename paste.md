## Linking CSS in a GitHub Pages Project <br>
---<br>
When working with a GitHub Pages project, you can link your CSS files in various ways. If your CSS directory is in the root of your project and you have a `pages` directory containing your HTML files, you can use both relative paths and raw file URLs to link your CSS. 

### Using Relative Paths
Relative paths are the most common way to link CSS files. For example, if your project structure looks like this:

```
/my-project
  ├── index.html
  ├── pages
  │   └── about.html
  └── css
      └── styles.css
```

In your `about.html`, you would link the CSS like this:

```html
<link rel="stylesheet" href="../css/styles.css">
```

This tells the browser to go up one directory (from `pages` to `my-project`) and then into the `css` directory to find `styles.css`.

### Using Raw File URLs
You can also use a raw file URL to link your CSS. This method is less common but can be useful in certain situations. To use a raw file URL, you need to get the raw link to your CSS file from GitHub. Here’s how to do it:

1. Navigate to your CSS file in your GitHub repository.
2. Click on the file to view it.
3. Click on the "Raw" button to view the raw file.
4. Copy the URL from the address bar.

The URL will look something like this:

```
https://raw.githubusercontent.com/username/repository/branch/css/styles.css
```

You can then link to this CSS file in your HTML like this:

```html
<link rel="stylesheet" href="https://raw.githubusercontent.com/username/repository/branch/css/styles.css">
```

### Considerations
- **Caching**: Using raw URLs may lead to caching issues, as browsers might cache the raw file. If you update your CSS, you may need to clear your cache or use versioning in the URL.
- **Performance**: Relative paths are generally preferred for performance reasons, as they are faster and more reliable than fetching files from raw URLs.

### Example
Here’s a simple example of how you might structure your HTML file to include the CSS:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My GitHub Page</title>
    <link rel="stylesheet" href="../css/styles.css"> <!-- Using relative path -->
    <!-- or -->
    <link rel="stylesheet" href="https://raw.githubusercontent.com/username/repository/branch/css/styles.css"> <!-- Using raw URL -->
</head>
<body>
    <h1>Welcome to My GitHub Page</h1>
</body>
</html>
```

### Conclusion
Both relative paths and raw file URLs can be used to link CSS files in your GitHub Pages project. However, relative paths are generally the preferred method due to their reliability and performance.

## References
## https://docs.github.com/en/pages/getting-started-with-github-pages
## https://docs.github.com/en/github/managing-files-in-a-repository/viewing-and-editing-files#viewing-a-file