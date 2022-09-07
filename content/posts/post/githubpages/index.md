---
title: "Github Pages"
date: 2022-09-07T03:53:47Z
lastmod: 2022-09-07T03:53:47Z
draft: false
tags: ["github", "pages"]
categories: ["English"]
author: "Daxesh Panchal"

autoCollapseToc: true
# contentCopyright: '<a href="https://github.com/gohugoio/hugoBasicExample" rel="noopener" target="_blank">See origin</a>'

---
## How to Deploy OpenAPI specs using Github Pages
   ### Enable github Pages
   * First step is to enable gh-pages for your repository
   * Next Select Pages from Settings menu of your repository
   * Select visibility Public/Private based on who you wants to share your API specs
   * Next Select `Deploy from branch` option in source
   * Next select branch and directory where your API specs is available.
   * Optionally you can select Theme and Custom domain.
   * When you will specify custom domain gh-pages will be deploy using custom domain instead of `wattpad.github.io/repo-name`
  ### Add entrypoint page to the gh-pages directory
  * Create index.html page in the same directory where you have openAPI specs file
  * Copy this content to index.html file
 ``` 
<html>
<head>
    <script src="https://unpkg.com/swagger-ui-dist@4.13.0/swagger-ui-bundle.js"></script>
    <link rel="stylesheet" type="text/css" href="https://unpkg.com/swagger-ui-dist@4.13.0/swagger-ui.css"/>
    <title>Password-strength API</title>
</head>
<body>
<div id="swagger-ui"></div>
<script>
    window.onload = function () {
        const ui = SwaggerUIBundle({
            url: "openapi.yaml",
            dom_id: '#swagger-ui',
            deepLinking: true,
            presets: [
                SwaggerUIBundle.presets.apis,
                SwaggerUIBundle.SwaggerUIStandalonePreset
            ],
            plugins: [
                SwaggerUIBundle.plugins.DownloadUrl
            ],
        })
        window.ui = ui
    }
</script>
</body>
</html>
```
* commit and merge changes to the branch which is used to deploy Gh-Pages
* By default API specs should be available on `wattpad.github.io/repo-name` if you have not used custom domain
* Ex: `https://wattpad.github.io/password-strenth`
* For enabling live test you can specify correct server url in your API specs for Example for Password-strength service. `https://github.com/Wattpad/password-strength/blob/main/docs/openapi.yaml#L7`