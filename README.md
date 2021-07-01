# ML Engineering Docs

Documentation for useful tools and best practices for real-world ML engineering

> Disclaimer: These docs are based on my personal experiences and suggestions from friends and colleagues. I do not claim to know everything. If you have suggestions/modification, please open a new issue and/or open a PR if you want to contribute.


Preview dark color scheme

<script> const toggleDarkMode = document.querySelector('.js-toggle-dark-mode'); jtd.addEvent(toggleDarkMode, 'click', function(){ if (jtd.getTheme() === 'dark') { jtd.setTheme('light'); toggleDarkMode.textContent = 'Preview dark color scheme'; } else { jtd.setTheme('dark'); toggleDarkMode.textContent = 'Return to the light side'; } }); </script>


## Metadocs

### How to get started with `just-the-docs` theme?

1. Create a new public repo with a `README.md`.
2. In the repo, go to `Settings -> Pages` and enable Github Pages. ([reference](https://guides.github.com/features/pages/))
3. Add a new file `_config.yml` and add the following line to it:
    ```yaml
    remote_theme: pmarsceill/just-the-docs
    ```

    That's it! You have the template ready.

### How to test my site locally?

See the [docs](https://pmarsceill.github.io/just-the-docs/#local-installation-use-the-gem-based-theme) on official JTD page.


