Contentful Action Example
=====

An example application for how you can integrating content migrations in your continuous delivery pipeline using the Contentful GitHub Actions.

What is this about?
=====

This example deploys a simple flask site integrated with Contentful onto Heroku utilizing GitHub Actions.

You can read our [conceptual guide](https://www.contentful.com/developers/docs/concepts/deployment-pipeline/) on how to utilize Contentful Environments inside your continuous delivery pipeline.

Getting started
=====

### Requirements

To use the continuous delivery pipeline of this project you need accounts for the following services:

- [Contentful](https://www.contentful.com)
- GitHub

### Setup

* Fork and clone this repository

#### The Contentful part (required)

* Create a new space using the Contentful CLI
  * `$ contentful space create --name "continuous delivery example"`
* Set the newly created space as default space for all further CLI operations
  * `$ contentful space use` (this will present you with a list of all available spaces – choose the one you just created)
* Import the provided content model (`./import/export.json`) into the newly created space
  * `$ contentful space import --content-file ./import/export.json`

#### The continuous delivery pipeline

Since we're using GitHub Actions, we'll be able to use the existing [Contentful-Action]() repo. Just add the following code into your github workflow.

```yml
    - name: Contentful Migration
      id: migrate
      uses: shy/contentful-action@master
      env: # Set the secret as an input
        SPACE_ID: ${{ secrets.SPACE_ID }}
        MANAGEMENT_API_KEY: ${{ secrets.MANAGEMENT_API_KEY }}
```
You can view the main.yml inside the worflow directory inside .github for example of a working confirguration. You'll also need to add your `SPACE_ID` and `MANAGEMENT_API_KEY` in the secrets tab of the settings on your repository.

License
=======

Copyright (c) 2019 Contentful GmbH. Code released under the MIT license. See [LICENSE](LICENSE) for further details.
