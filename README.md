# BCC Developer Docs Deploy

A GitHub Action that converts Markdown documentation to a BCC documentation site and deploys it to the BCC developer portal [Site](https://developer.bcc.no)

## Updating the GitHub Action

The `action.yml` file is a [composite](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action) GitHub Action. It is used by workflows in other repositories like this:

```yml
steps:
    - name: Build documentation site
        uses: bcc-code/bcc-developer-docs-deploy@v1
        with:
            title: Documentation Guide
            description: Information on how to set up documentation for BCC projects
```

You can see a full workflow in action [in this repository](./.github/workflows/build-and-deploy-documentation.yml), as it is used here as well to build the documentation.

The "version" of the Action (the `@v1` part) is just a tag that is added to a commit on this repository. When updating the `action.yml` file, there are two ways to propagate this update to all the other repositories.

### 1. Create a new tag

This is the easiest approach. Use this if there are any breaking changes to the action, such as renaming an argument. Create a new tag with a comment like this:

```sh
git tag v2
```

This creates a `v2` tag.

Then push the tag to GitHub (and any non-pushed commits) flag to `git push`:

```sh
git push origin v2
```

After doing this, all the workflows in this and other repositories need to be updated to use that `v2` tag. This enables gradual adoption, but the downside is of course a potential burden of having to upgrade a lot of repositories.

### 2. Republish or delete an existing tag

By deleting and republishing the last tag, any future workflow will use the updated version without having to update all the other workflows. This is a **dangerous** action though, as an error in the configuration can lead to all repositories breaking, and you're deleting the tag from the server forever. **Only use this for backwards compatible changes**.

First delete the tag locally:

```sh
git tag -d v1
```

Then delete it on GitHub:

```sh
git push --delete origin v1
```

The run the two commands under option 1 to create and push this tag. Note that any workflows running between deleting the old tag and pushing the new tag will fail.
