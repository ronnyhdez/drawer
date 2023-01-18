# Drawer

A place to keep all my fast-taken notes about R, python, bash and other dev stuff.

## Objective

**Things here are just notes. These are not blogposts.**

Notes here are mean to be raw unedited notes that I
usually take quickly while working solving things related to programming.

If there is material for a blogpost, I can use the information to create and
edit a new document in this [repo](https://github.com/ronnyhdez/blog)

# Workflows

There are two workflows: from RStudio and using .Rmd files or usin nvim.

## From RStudio and .Rmd

 - Include the Rmd or md file
 - Load `docmaker` package
 - To render all documents and publish in just one step: `make_all_docs(T)`
 
## With nvim and .md files
 
 - Open terminal and type `d` (alias to move to drawer repo)
 - Type `w` and the name of the file to be written. (If it's new, remember to include the .md) 
 - If there is a md file at the moment I need to copy this one to `docs/`
 - Run `mkb` (alias for mkdocs build --config-file=mkdocs.yml)
 - When done with the changes, run `ghd` (alias for mkdocs gh-deploy --strict --force)

## Things to remember


## References

 - [Matherial theme](https://squidfunk.github.io/mkdocs-material/)
 - Favicon by Freepick <a href="https://www.flaticon.com/free-icons/brain" title="brain icons">Brain icons created by Freepik - Flaticon</a>
