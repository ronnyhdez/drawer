# drawer

My notes to describe new things that I learn or things that I do and tend to
forget.

## Workflow

 - Include the Rmd or md file
 - load docmaker package
 - To render all documents and publish in just one step: `make_all_docs(T)`
 - If there is a md file at the moment I need to copy this one to `docs/`
 
**Remeber** I have two bash_aliases for building and deploying docs from
terminal instead from package function. This is useful for when I'm 
creating `.md` documents directly:

 - mkb = mkdocs build --config-file=mkdocs.yml
 - ghd = mkdocs gh-deploy --strict --force

## Things to remember

This are not blogposts. Notes here are mean to be raw unedited notes that I
usually take quickly while working solving things related to programming.

If there is material for a blogpost, I can use the information to create and
edit a new document in this [repo](https://github.com/ronnyhdez/blog)

## References

 - [Matherial theme](https://squidfunk.github.io/mkdocs-material/)
 - Favicon by Freepick <a href="https://www.flaticon.com/free-icons/brain" title="brain icons">Brain icons created by Freepik - Flaticon</a>
