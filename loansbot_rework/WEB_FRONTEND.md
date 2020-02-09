# Web Frontend

The web front-end intends to be a little bit closer to modern frameworks, while
still being trivially runnable without a pipeline. That means no package
manager, no bundler, no webpack. There will also continue to be no
guarranteed support except for the current latest stable Chrome, with PR's
accepted for the latest stable Firefox and latest stable Microsoft Edge.

This uses React as described [here](https://medium.com/@chrislewisdev/react-without-npm-babel-or-webpack-1e9a6049714).

The front-end is served directly from Nginx and corresponds to everything not
in /api/, except for /deploy.php.

- Continuous deployment via a GitHub webhook to /deploy.php
  - Secured through [this approach](https://developer.github.com/webhooks/securing/)
- Automated testing (using MochaJS) via GitHub Actions
  - Testing will be fairly limited compare to the backend, but enough
    to be useful
