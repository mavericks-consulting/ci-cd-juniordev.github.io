# To get started, you need:

* A Github account

* The repo: https://github.com/mavericks-consulting/ci-cd-workshop-junior-dev
  - Fork it!

* Follow the README to setup the repo locally

# Getting started with Circle CI

* Change job1 to run lint
  - Add a lint command to your `package.json`
  - Change the `job1` stage to run the `lint` command
  
* Add a job to run tests
  - Add a command in your `package.json` to run unit tests, like this:
  ```;
  "scripts": {
    "start": "nodemon src/index.js",
    "test": "jest ."
  },
  ```
  - Ensure tests pass locally
  - Add a stage to run unit tests on the CI
  - This stage depends on the previous stage
  - Alternatively, copy the following into your `config.yml`:
    - Under `jobs`, add the following after the first job:
    ```
      test:
        docker:
          - image: circleci/node:10.15.3
        steps:
          - checkout
          - run: npm install && npm test
    ```
     - Under `workflows`, add the following after the `lint` job:
    ```
      version: 2
      build_and_test:
        jobs:
          - lint
          - test:
              requires:
                - lint
    ```
     - To add the deployment job you need the following:
      1) Create a Heroku App:
      ``` heroku create APP_NAME ```
      2) Fetch your Heroku API_KEY and APP_NAME from profile settings
      3) Add the environment variables: HEROKU_API_KEY and HEROKU_APP_NAME to Circle CI: Refer to https://circleci.com/docs/2.0/env-vars/#setting-an-environment-variable-in-a-project
      4) Use the below mentioned for deploying to Heroku:
      ```
      git push https://heroku:$HEROKU_API_KEY@git.heroku.com/$HEROKU_APP_NAME.git master
      ```

