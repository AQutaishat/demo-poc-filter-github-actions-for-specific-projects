[![project1_test](https://github.com/cr2product/sandbox-digital-test-workflow/actions/workflows/project1_test.yml/badge.svg)](https://github.com/cr2product/sandbox-digital-test-workflow/actions/workflows/project1_test.yml)
## Description

This project is POC for applying **unit testing on two projects** based on the location of changed files and preventing merging based on the unit testing results.

It also implements **Linting** only for changed files.

## Folder structure

The solution includes two different angular projects in the folders **(project1, project2)**. The CI actions for unit testing and Linting are in the folder **(.github/workflows)**

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/b2511f4389857a1b9af33dafdad55edbf66e3686dc3522ee.png)

## Filtering Unit Testing

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/a1bb2bc7ac61a54ea9b2b4b25af8890698dbf11d70b6a4d9.png)

In the CI action file _(.github/workflows/**project1\_test.yml**)_, we define this action to run on each code push or pull request creation.

But we use filters in this Yaml to filter those events for changed files only in (project1) folder.

```plaintext
  push:
    paths:
      - 'project1/**'
  pull_request:
    paths:
      - 'project1/**'
```

In addition, we define (project1) to be the **default folder** for running commands. 

After that, we use this job as **(branch status checks)** to prevent merging unless this action is successful.

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/1f6a9b4e4b7c5d5f5f2ee1da76f7c43fd6b438c46b0f1ae1.png)

#### Duplicate for project2

We also have a similar file for (project2) that will execute a unit test on (project2) only if a file in (project2) has changed using the action event filter.

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/5630daab78a8eb38353c1d81be6f28f486123e124116f46a.png)

#### Implement recommended adhoc

 I did implement a recommended Adhoc from GitHub documentation by creating a duplicated file for **(project1\_test.yml**) with the **same workflow and job names** but the job will do nothing. This is to prevent blocking project2 changes from being merged if project2 unit testing succeeded. They say not executing project1 unit test action will make the action state (pending) which will prevent the merge. Check the following link

[Handling skipped but required checks (https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/troubleshooting-required-status-checks#handling-skipped-but-required-checks)](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/troubleshooting-required-status-checks#handling-skipped-but-required-checks)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/d8447350db19358ebab32e0e9c196cb603c92533fbbf4d9d.png)

#### Linting only on changed files

Check the action in (.github/workflows/lint\_Changed\_Files.yml), it runs the script defined in (prject1/ packages.json) which is:

```plaintext
....
"scripts":
 "lint:script": "eslint -c eslintrc.js $(git diff --name-only --diff-filter=ACMRTUXB origin/master | grep  -E \"(.js$|.ts$|.tsx$)\")"
```

Check the article

[https://gist.github.com/seeliang/0f0de424d1cdc4541c338f4ee93b7e6a](https://gist.github.com/seeliang/0f0de424d1cdc4541c338f4ee93b7e6a)

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/77beb90061c545f84b97013ba5291b1fd2c8b5d016da1edf.png)

#### Adding building badge

Another extra, we can add an **action status badge** to our (readme.md) file by copying the text provided by the action and pasting it into the readme.md file

![](https://33333.cdn.cke-cs.com/kSW7V9NHUXugvhoQeFaf/images/06e796e029b0d44124d420151cad61767e0aab8f23174a8b.png)
