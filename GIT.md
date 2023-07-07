
## Conventional Git Commits

This project uses conventional Git commits to keep the commit history readable and organized. Conventional Git commits follow a specific format: `<type>(<scope>): <description>`.

The available types for commits are:

- `feat`: A new feature.
- `fix`: A bug fix.
- `docs`: Changes to documentation.
- `style`: Changes to code style (no code changes).
- `refactor`: Code changes that neither fixes a bug nor adds a feature.
- `perf`: Code changes that improve performance.
- `test`: Adding or updating tests.
- `chore`: Changes to the build process, development tools, or other miscellaneous tasks.

The optional scope can indicate the area of the code affected by the commit.

For example, a conventional commit might look like this:  
  
`feat(login): add ability to remember login credentials`  


## Pull Requests

When creating a pull request, please follow these guidelines:

1. Use a clear and descriptive title.
2. Include a description of the changes made and any relevant context.
3. Ensure that all tests pass and the code is properly formatted and documented.
4. Assign at least one reviewer.
5. Wait for approval before merging the pull request.
  
For example,  
```  
# Description
[Describe what changes this pull request makes and why it should be merged.]

# Checklist
- [ ] I have read the contributing guidelines and this PR complies with them
- [ ] I have tested this PR locally and ensured that it works as expected
- [ ] I have added unit tests for the changes made in this PR
- [ ] I have updated the documentation to reflect any changes made in this PR
- [ ] I have assigned a reviewer to this PR

# Related Issues
[If there are any related issues or pull requests, link them here.]
```  