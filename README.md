Contentful Action Example
=====

An example application for how you can integrate content migrations in your continuous delivery pipeline using [Contentful's GitHub Action](https://github.com/contentful-userland/contentful-action).

What is this about?
=====

This example contains a folder of Migration Scripts. Whenever a new script is added to this repo, it's evaluated against a Contentful space. If the migration is added via pull request, a new environment is created on Contentful and the migration is run against that. If a new script is added to master (either directly or via merging a PR) then a new environment is created, migrations are run against that environment and then the alias for master is updated to point to that new environment.

You can read our [conceptual guide](https://www.contentful.com/developers/docs/concepts/deployment-pipeline/) on how to utilize Contentful Environments inside your continuous delivery pipeline.

You can further expand this example by integrating other GitHub actions, such as testing and deployment options.

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

Since we're using GitHub Actions, we'll be able to use the existing [Contentful-Action](https://github.com/contentful-userland/contentful-action) repo. Just add the following code into your github workflow.

```yml
    - name: Contentful Migration
      id: migrate
      uses: contentful-userland/contentful-action@main
      with:
        # delete_feature: true
        # set_alias: true
        # master_pattern: "main-[YY]-[MM]-[DD]-[hh]-[mm]"
        # feature_pattern: "sandbox-[branch]"
        # version_field: versionCounter
        # version_content_type: environmentVersion
        # migrations_dir: contentful/migrations
        space_id: ${{ secrets.SPACE_ID }}
        management_api_key: ${{ secrets.MANAGEMENT_API_KEY }}
      # env:
        # LOG_LEVEL: verbose
```
You can view the [main.yml](.github/workflows/main.yml) for example of a full working configuration.

![Screenshot of GitHub Secret Panel](images/Secrets.png)

You'll also need to add your `SPACE_ID` and `MANAGEMENT_API_KEY` in the secrets tab of the settings on your repository. You can optionally add a `MIGRATIONS_DIR` secret if you're storing your migration scripts somewhere besides the `migrations` folder.

License
=======

Copyright (c) 2020 Contentful GmbH. Code released under the MIT license.
