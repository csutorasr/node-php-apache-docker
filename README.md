# node-php-docker
This is an docker image for php backend services with node to compile frontend.

# Usage

Install PHPUnit with the image and your node modules you test with (e.g. Karma). Run the test with the commands.

# Configuration

## Bitbucket pipelines

You should add `karma-phantomjs2-launcher` as a development dependency in your app and run angular tests with that. For me polyfills were needed. I imported them in the `src/test.ts` in the angular-cli project.

### Example project:

The angular code is in the `/angular` folder. Composer project directory is `/`.

 `bitbucket-pipelines.yml`:

```yaml
image: csutorasr/node-php

pipelines:
  branches:
    master:
      - step:
          caches:
            - composer
            - node
            - node-angular
          script:
            - npm run composer:install
            - npm run composer:test:linux
            - npm run angular:install
            - npm run angular:test

definitions:
  caches:
    node-angular: angular/node_modules
```

`package.json`: (needed scripts)

```json
    "composer:install": "composer install",
    "composer:test:linux": "cd vendor/bin && ./phpunit --bootstrap ../autoload.php ../../tests",
    "angular:install": "cd angular && npm install",
    "angular:test": "cd angular && npm run test:single",
```