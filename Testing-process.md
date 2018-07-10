Once you have submitted a PR and it has been reviewed and approved, it is ready for testing! It will be moved to the "Test ready" column in our [Zenhub delivery pipeline board](https://github.com/openfoodfoundation/openfoodnetwork/#boards?repos=6257856).
<br/>PRs are ordered by priority, the upper one will be the first one to test.

### 1- Staging a PR
In our distributed team, we are using various staging servers. For now in Australia, France, Iberica and UK.
<br/>The next available developer who has access to a staging server takes the top priority PR and stages it, add as a comment the URL of the staging server he deployed on + add the label of which staging it is on.
<br/>This should be done regularly for the delivery pipeline to be fluid. At least developers able to stage should check once a day and stage one PR if there are some. If a developer can pair and work synchrone with a tester, the process will be even more efficient.

### 2- Seeds data
While provisioning your server you will need to load sample data, which are located [here](https://github.com/openfoodfoundation/openfoodnetwork/blob/master/lib/tasks/dev.rake).
This makes the testing task much lighter! We are building a comprehensive set of seeds data that is aiming to cover the various possible user cases (WIP).
<br/>**NOTE:** As a tester or developer, if you are missing some seeds data for your tests to be easy and smooth, please create a GH issue to add them. That way we can enrich step by step our common set of sample data.

### 3- Testing outcomes
The next tester available looks at the "Test ready" column and assign her/himself on the PR they start testing.
<br/>Please follow the [testing guidelines](https://github.com/openfoodfoundation/openfoodnetwork/wiki/Feature-Testing-Handbook) to ensure top quality tests.
- If it's all good, the tester comments the PR with link to the testing notes and moves the PR to the column "Ready to go" where an accredited developer can take it and merge.
- It there are some remaining issues, the tester sums them up in the PR with link to testing notes and moves back the PR to the "In dev" column for the developer to continue the work.
