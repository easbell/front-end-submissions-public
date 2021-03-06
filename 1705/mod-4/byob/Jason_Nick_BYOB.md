# BYOB Submission Form

> Nick Svetnicka & Jason Lucas

[Project Spec](http://frontend.turing.io/projects/build-your-own-backend.html)

------

## Basics

#### Link to the GitHub Repository for the Project
[BYOB](https://github.com/EndlessHypnosis/build-your-own-backend)

#### Link to the Deployed Application
[Heroku](https://build-your-own-backend-brewery.herokuapp.com/)

#### Link to latest build on Circle CI
[Circle CI](https://circleci.com/gh/EndlessHypnosis/build-your-own-backend/57)

## Completion

#### Were you able to complete the base functionality?

* Documented all available endpoints and their usage in the README?

> YES

* Seeded a database with at least 2 tables and 1 relationship?

> YES

* Had at least 10 endpoints that returned responses with appropriate status codes?

> YES

* Secured at least 4 endpoints with JWTs?

> YES

* Enforced a linter and wrote code that conformed to it?

> YES

* Wrote tests for both happy and sad paths for each endpoint?

> YES

* Setup automatic deployments with CircleCI to a production app on Heroku?

> YES

# Code Quality

#### Link to a specific block of your code on GitHub that you are proud of
[happy code](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/server.js#L304-L333)

> This is the PUT endpoint for breweries. There's nothing terribly special about it, but we were proud of the fact that all edge cases were accounted for. The error checking is very complete and sanitizes the input for completeness before updating the record. We also made sure to delete the jwt token before sending the record to postgreSQL to update.

#### Link to a specific block of your code on GitHub that you feel not great about
[sad code](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/server.js#L221-L236)

> We discussed the fact that both of our DELETE endpoints could be refactored to use one common function for handling the request validation, the knex delete, and finally building the response. The challenge we ran into was that this refactor should really entail the refactoring of all common endpoints with this approach, which we couldn't find the time for.

#### Attach a screenshot or paste the output from your terminal of the result of your test-suite running.

[test suite](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/public/assets/ss_testing.png)

![test suite](https://raw.githubusercontent.com/EndlessHypnosis/build-your-own-backend/master/public/assets/ss_testing.png)

#### Attach a screenshot or paste the output from your terminal of the result of your linter running.

[linter output](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/public/assets/ss_linter.png)

![eslint](https://raw.githubusercontent.com/EndlessHypnosis/build-your-own-backend/master/public/assets/ss_linter.png)

#### Attach a screenshot of your CircleCI build passing

[circleCI build](http://github.com/EndlessHypnosis/build-your-own-backend/blob/master/public/assets/ss_circle_ci.png)

![circle ci](https://raw.githubusercontent.com/EndlessHypnosis/build-your-own-backend/master/public/assets/ss_circle_ci.png)

-----

#### Please feel free to ask any other questions or make any other statements below!

> N/A

-----


# Instructor Feedback (Brittany)

The following set of points are distributed at the discretion of the instructor.

### Documentation

**10 points**:  The README includes documentation for all available endpoints and how to use them. Instructor can easily follow the documentation for using the API.

### Feature Completion

**60 points**: (60 possible points) Developer has implemented all 10 endpoints, 4 are secured via JWTs and one is a custom endpoint that filters data based on query params. The database is seeded with at least two tables and one relationship.

### Testing & Linting & Error Handling

**34 points**: (40 possible points) Project has a running test suite that covers all happy and sad paths for the appropriate endpoints. Error handling is informative and helpful for the end-user. The project has a linting configuration that passes with no errors.

* Your error handling is nice and thorough, but it's also quite repetitive. I'd break out error handling like [this](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/server.js#L308-L318) into a helper function so it can be re-used among endpoints.

* You should be able to add variables like [this]() into a 'globals' object in your eslint config instead of doing these line-by-line comments to turn those warnings/errors off.

* I'd rather you set the environment as an environment variable in your test script of your package.json rather than [hardcoding](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/test/endpoints.spec.js#L11) it directly in the file. It just maintains the consistency and familiarity of seeing `process.env.NODE_ENV || 'fallback'`. There's also a possibility that a CI environment name might diverge from 'test' and you'd want to still be able to capture that with an environment variable.

* Missing a lot of .catches for [these](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/test/endpoints.spec.js#L65) [.thens](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/test/endpoints.spec.js#L57)

* Curious about the strategy [here](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/test/endpoints.spec.js#L68-L76) and why you're making these GET requests in a `beforeEach ` block. Probably not a huge deal in this scenario, but doing this much before every single test would significantly slow down the runner.

* [These](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/test/endpoints.spec.js#L90-L97) assertions could be simplified by simply grabbing a complete mock beer object and checking if the array contains it or not.

* Would be good to test the [error message](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/test/endpoints.spec.js#L165-L173) that comes back here as well.

### JavaScript Style

**25 points**: (40 possible points) Application is thoughtfully put together with some duplication and no major bugs. Developer can speak to choices made in the code and knows what every line of code is doing. Some improvements can be made with naming conventions to improve readability.

* A simpler way to do this [check](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/server.js#L35-L50) is by assigning the token variable right away and falling back through each possible option. If none of the values stick, it will just be undefined. e.g.:

```js
let token = request.headers.authorization || request.body.token || request.query.token;
```

* If you're going to leave comments in your code, you should make sure they accurately reflect what's happening. [This](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/server.js#L76) is checking for `appName` and `email`, not just appName. Your code should generally be written in an English-enough way that you don't need comments like this.

* Using 'it' in a [variable name](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/server.js#L82) is a poor naming convention. On it's own, without any context, I can't decipher what 'it' represents. I'd rename this to `emailBelongsToTuring`. That reads a little more like English when you're doing conditionals and provides some extra context about what the value represents.

* [This](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/server.js#L85) conditional is a little difficult to read. Could you use a `.contains()` on the string instead?

* Is the Abv specific to an actual [user](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/server.js#L102)? Or just a trait of each beer that you can filter on? Prefixing a variable name with 'user' when it's not a value that's actually tied to a user is a little bizarre. 'MyDatabase' is also a poopy variable name.

* This [conditional](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/server.js#L105) could be combined with an `&&` to avoid some nestiness.

* I know filtering by more than one query param wasn't a part of the spec, no points off for this, but just FYI you could make this [more](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/server.js#L102-L112) flexible and dynamic by doing blah blah blah. I bet there are a lot of beer characteristics people would be interested in filtering on, rather than just ABV.

* I don't necessarily agree with returning a 404 [here](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/server.js#L116). A 404 means nothing exists at that particular endpoint, and usually signifies that someone entered an incorrect URL. In this case, the endpoint is correct, and a resource *does* happen to exist there, it's just an empty array.

* Again, make sure your comments accurately reflect the [functionality](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/db/seeds/development/breweries.js#L6-L7). You don't want to be coopy and pasting examples and leaving evidents/artifacts behind.

* Missing a .catch for this [.then](https://github.com/EndlessHypnosis/build-your-own-backend/blob/master/db/seeds/development/breweries.js#L34-L43)


### Workflow

**15 points**: Developer(s) make many small, atomic commits that document the evolution of the application but sometimes contain irrelevant changesets and inconsistent commit messages. Developer(s) use git branches and pull requests when applicable to incorporate changes into the application, and are not pushing fresh changes directly to master. Pull requests may contain little or no code review. There may be slight instances where the developer(s) have committed source code that should be .gitignored. There may be some instances of “dead” or commented-out code and debugger statements like console.log that need to be cleaned up.

* Nice use of issues and pull requests, but would like to see some actual code review and conversation around the PRs -- do you both understand all the logic that's being added? Are you both equally responsible for it now?

* Similar inconsistencies in commit message formats and lots of areas where commits can be squashed, though you didn't learn about that until after this project was due.

## Project is worth 170 points

## To get a 3 on this project, you need to score 125 points or higher
## To get a 4 on this project, you need to score 145 points or higher

# Final Score: 134 / 170
