## Coding Standards

### General Guidelines

- Don't abbreviate variable names. For example use `knowledge_libary` or `knowledgeLibrary`, and not `kl`.
- No trailing spaces.
- Spaces not tabs.
- Try to have a consistent approach with what you see in the repository. If you don't know where an example of something similar is, just ask.
- Don't add an external dependency without reviewing it with another developer.
- Avoid comments in code. If the code is not clear enough without them, then it's not written simple enough. It's okay to comment out code if you think you might be putting back at some point.


### Git

- Always start your branch from `staging`. For example `git checkout -b card-123` when on the `staging` branch.
- All Pull Requests are made against `staging`. We only want to see the changes that your branch made.
- Always append the branch number to the commit message. We have a git hook that will do that for you.
- We prefer pulls to be rebase. In other words `git pull --rebase`. This keeps commit history in tack. You can make this automatic for all pulls by using the command `git config --global pull.rebase true`.


### Backend

- Four spaces per indent
- Always end lists, dictionaries, tuples with an ending comma. It prevents dumb conflicts.
- Imports are always alphabetical
- Imports are grouped by (stdlib, third-party python, django, django-contrib, third-party django, project-imports, module-imports)
- If you have more than about four imports use a multiline import format (parentheses).
- Direct imports always appear before module imports (`import sys` would come before `from sys import os`)
- Direct imports are prefered since you don't risk name clashing
- `as` imports should be avoided if possible
- Imports not at the top of the page should definitely be avoided if possible. It's not always possible due to circular dependencies.
- Most of the codebase is double quote for strings but my preference is single quotes because it's less visual noise. We will convert it at some point.
- The exception to the single quotes preferred rule is when we're actually using sentences like in `help_text`.
- Always run the lint checker before committing. There is a pre-commit git hook you should use.


### Django

- Warning: this code base was inherited. We're cleaning it up but when it comes to following the standards below it's hit and miss. It was originally developed by a group that wasn't familiar with idiomatic Django.
- `CharField` should never have `null=True`. We store blanks in all cases instead.
- Admin registers at the bottom of the module and should also be alphabetical.
- Always push code to the lowest level possible. In other words if you have a function in the serializer that might also be needed for an email, push that code to the model.
- Use `raw_id_fields` in the admin for large lists like `creator`.
- Merge migrations that are required due to merges into `develop` should only be made and committed into `develop`, not your feature branch. Releases may not be in the same order when we release to production.
- If doing data migrations that are expensive, consider instead to do it as a management command. Realize that this will be a communication issue with the other developers or their database will be outdated.
- Always include choices within the model. This helps a lot when it comes to using them elsewhere. You can see many examples of this.
- Use `related_name` as much as possible. This allows us to use syntax like `client_category.summary_pages.all()` vs `client_category.summary_page_set.all()`
- API registers should be alphabetical.
- Serializers should strictly be stuff from the model. Calculated fields that are read-only are fine. If you need to have extra information from another model we handle this in the viewset.


### Frontend

- Two spaces per indent
- Alphabetize dependency injection items. The first group is builtins, the second group is everything else.
- Prefer non-binding or one-way binding scope variables over two-way binding.
- Prefer smaller manageable components over monolithic controllers. (Note, we didn't do a good job of this historically.)
- Bind scope to children components but update via function callbacks. No chance for circular / multiple updates.
- Controller events should accept `$event` as their first parameter. For example when binding to `ng-click`.
- Except for rare exceptions we don't use Angular's event system. That means we don't use `broadcast`, `on`, `emit`, `listen`. If you feel like you need this, you probably have an architecture problem.
- Let Angular manage the DOM. Don't use jQuery for DOM manipulation.
- Use `lodash` extensively. Manipulating things like arrays or objects with pure javascript is often not cross-browser supported.
- Use the `_initialize` function to set local variables in a controller.
- Use local variables that are calculated once, vs referencing functions in the template. Angular has a tendency to call a function four or five times. It's really inefficient.
- Use `ng-if` when possible, instead of `ng-show`.
- Use underscore functions `_whatever` to indicate the function is only used internally to the controller and not referenced from the template or activated from the template.
- Before making changes to the `lib` area of code review it with another developer. It has the possibility to impact a lot of core functionality.
- Always push code to the lowest level possible. If logic can go in the model then put it there, but only if it's data logic. (Note, we didn't do a good job of this historically.)
- Always use Angular's wrappers around common browser javascript. For example use `$window` instead of `window`.

