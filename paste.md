## Summary: Linking CSS in Your GitHub Pages Project Using a Raw File URL  
---  
In your GitHub Pages project, you can link your CSS file using a raw file URL provided by GitHub instead of using a relative path. However, while this approach works, it is generally not as ideal as using relative paths for the sake of maintainability, performance, and security.  

Below is an explanation of how you can achieve this, as well as a small discussion on its drawbacks. We'll walk through the concept, syntax, and an **example** implementation.  

---  
**Explanation**:  
### 1. Understanding the Raw File URL  
When you host files on a GitHub repository, GitHub allows public access to the raw content of files. This is done through GitHub's `https://raw.githubusercontent.com` domain. By constructing the appropriate raw file URL, you can directly link your CSS (or any other static file) in your project.  

The structure of the raw file URL is as follows:  
```
https://raw.githubusercontent.com/{username}/{repository}/{branch}/{file-path}
```

Where:  
- `{username}`: Your GitHub username.  
- `{repository}`: The repository name.  
- `{branch}`: The branch name (typically `main` or `master`).  
- `{file-path}`: The path to the CSS file in your repository (relative to the repository root).  

For example, if your CSS file is located in the `css` directory in the root of your repository, and your branch is `main`, the URL might look like this:  
```
https://raw.githubusercontent.com/myusername/myrepo/main/css/style.css
```

This raw URL can then be used to link the CSS in your HTML files.  

---

### 2. Using the Raw URL in Your HTML File  
You can include the raw file link in the `<link>` tag of your HTML file as follows:  

```html
<link rel="stylesheet" href="https://raw.githubusercontent.com/myusername/myrepo/main/css/style.css">
```

This works because the `href` attribute in the `<link>` tag accepts absolute URLs, the raw file URL included.  

However, **caution** must be taken as this method has some drawbacks:
- **Performance**: GitHub's raw file server does not use Content Delivery Networks (CDNs), meaning page load speeds may be slower compared to hosting the CSS directly on your GitHub Pages site or using a proper CDN.
- **Cache Control**: Browsers may not cache raw GitHub file links well, since GitHub doesn't optimize these files for frequent web use.
- **TLS Verification**: Some browsers or web proxies may block raw.githubusercontent.com links due to stricter security policies, as they are not optimized for production-grade hosting.

---

### 3. Recommended Alternative: Use Relative Paths  
Using relative paths is recommended because files hosted in your GitHub Pages project are accessible directly from your website's domain (e.g., `https://yourproject.github.io`). Here's the standard way to link your CSS without raw URLs:  

**Example Directory Structure**:
```plaintext
project/
├── css/
│   └── style.css
├── pages/
│   └── about.html
```

From the `about.html` file located in the `pages/` directory, you can refer to the `style.css` file using a **relative path**:

```html
<link rel="stylesheet" href="../css/style.css">
```

This ensures faster load times, better caching, and easier maintainability.

---

## Example of Linking CSS with Raw URL  
If you want to try the raw file method despite its drawbacks, here's an example:  

1. Assume the following project structure:  
```plaintext
project/
├── css/
│   └── style.css
├── pages/
│   └── about.html
```

2. Upload the repository to GitHub under the username `myusername` and repository name `myrepo`.  

3. Identify the raw GitHub file URL for `style.css`:  
```plaintext
https://raw.githubusercontent.com/myusername/myrepo/main/css/style.css
```

4. Use the following HTML in your `about.html` page (`pages/about.html`):  
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>About</title>
    <!-- Link CSS using raw.githubusercontent.com -->
    <link rel="stylesheet" href="https://raw.githubusercontent.com/myusername/myrepo/main/css/style.css">
</head>
<body>
    <h1>About Page</h1>
</body>
</html>
```

When accessing your GitHub Pages link for `about.html`, the browser will attempt to fetch the raw CSS file from the given URL.

---

## References: Only plain text references.  
1. https://docs.github.com/en/rest/overview/resources-in-the-rest-api#raw-content  
2. https://raw.githubusercontent.com  
3. https://pages.github.com/  

