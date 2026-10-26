# QA GitHub Actions Demo

[![02 API Tests (Postman)](https://github.com/SKumladze/qa-github-actions-demo/actions/workflows/02-api-tests.yml/badge.svg)](https://github.com/SKumladze/qa-github-actions-demo/actions/workflows/02-api-tests.yml)

Small, beginner-friendly examples of **GitHub Actions** for QA: running **Postman API tests automatically** on every push, on a schedule, or with one click.

The tests call [JSONPlaceholder](https://jsonplaceholder.typicode.com), a free fake API made for practice.

## What is GitHub Actions?

A robot that GitHub gives you for free. You write a recipe (a `.yml` file in `.github/workflows/`) that says:

| Word in the file | Meaning |
|---|---|
| `on` | **When** to run: on push, on a pull request, on a timer, or by clicking a button |
| `jobs` | **What** to run. Each job gets a fresh, clean computer (`runs-on: ubuntu-latest`) |
| `steps` | The commands, run one by one, top to bottom |
| `run` | A terminal command, e.g. `npm run test:api` |
| `uses` | A ready-made step someone else wrote, e.g. `actions/checkout` downloads the repo |
| `if: always()` | Run this step even if an earlier step failed (handy for saving reports) |

If any step fails, the whole run is marked with a red ❌. If all pass, it gets a green ✅.

## The examples

| File | What it shows | How to start it |
|---|---|---|
| [`01-hello-world.yml`](.github/workflows/01-hello-world.yml) | The smallest workflow: print text, use an input, check out the code | Actions tab, **Run workflow** |
| [`02-api-tests.yml`](.github/workflows/02-api-tests.yml) | Runs the Postman collection with Newman and saves an HTML report | Automatic on push / pull request, daily at 06:00 UTC, or manual |
| [`03-failing-test-demo.yml`](.github/workflows/03-failing-test-demo.yml) | A test that fails **on purpose**, to practise reading a red run | Actions tab, **Run workflow** |

## The Postman tests

[`postman/jsonplaceholder.postman_collection.json`](postman/jsonplaceholder.postman_collection.json) has 4 requests and 11 checks:

1. **GET /users/1**: status 200, fast response, correct id, name and email present
2. **GET /posts?userId=1**: returns exactly 10 posts, all from user 1
3. **POST /posts**: status 201, new post gets an id, title comes back unchanged
4. **GET /posts/99999**: a missing post returns 404 (negative test)

The URL lives in an **environment** file ([`postman/test.postman_environment.json`](postman/test.postman_environment.json)) as `{{baseUrl}}`, so the same tests could run against a different server just by changing one value.

You can import both files into the Postman app (**Import** button) and run them there too.

## Exercises

1. Go to **Actions**, run **01 Hello World** with your own name, and find it in the log.
2. Run **03 Failing Test Demo**. Open the red run, find which assertion failed and why. Download the report from the **Artifacts** section at the bottom of the run page.
3. Fix the failing test: edit [`postman/failing-on-purpose.postman_collection.json`](postman/failing-on-purpose.postman_collection.json) on GitHub (pencil icon), commit, and run it again until it is green.
4. Add a new check to the main collection, for example "user 1 lives in the city Gwenborough" (`address.city`). Commit it and watch **02 API Tests** start by itself.
5. Break a test on a new branch and open a **pull request**. See how the red ❌ shows up right on the pull request.

## Run it on your own computer

Needs [Node.js](https://nodejs.org).

```bash
npm install
npm run test:api     # all should pass
npm run test:fail    # one should fail
```

The HTML reports are written to the `reports/` folder.
