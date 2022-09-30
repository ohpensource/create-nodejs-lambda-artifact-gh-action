# create-nodejs-lambda-artifact-gh-action

Repository containing Ohpen's Github Action to package nodejs applications.

- [create-nodejs-lambda-artifact-gh-action](#create-nodejs-lambda-artifact-gh-action)
  - [code-of-conduct](#code-of-conduct)
  - [github-action](#github-action)

## code-of-conduct

Go crazy on the pull requests :) ! The only requirements are:

> - Use _conventional-commits_.
> - Include _jira-tickets_ in your commits.
> - Create/Update the documentation of the use case you are creating, improving or fixing. **[Boy scout](https://biratkirat.medium.com/step-8-the-boy-scout-rule-robert-c-martin-uncle-bob-9ac839778385) rules apply**. That means, for example, if you fix an already existing workflow, please include the necessary documentation to help everybody. The rule of thumb is: _leave the place (just a little bit)better than when you came_.

### github-action

This action executes a script defined in your `package.json` on the specified folder and uploads it to an s3 bucket. The (required) inputs are:

- _region_: aws region name.
- _access-key_: user access key to be used.
- _secret-key_: user secret key to be used.
- _account_: AWS account id.
- _role-name_: Role to assume before uploading lambda package to s3.
- _version_: Service version that the package(artifact) represents.
- _service-name_: Name of the service that the package represents.
- _project-name_: Name of the project. Will suffix the s3 artifact key name.
- _project-folder_: Folder where the lambda package.json is located.
- _script-name_: Name of the script to execute to pack your code.
- _bucket-name_: Bucket where lambda package will be uploaded.

Here is an example:

```yaml
name: CI
on:
  pull_request:
    branches: ["main"]
jobs:
  upload-lambda-artifact:
    needs: [build]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: ohpensource/create-nodejs-lambda-artifact-gh-action@0.0.0.1
        name: Create and upload lambda artifact
        with:
          region: $REGION
          access-key: ${{ secrets.COR_AWS_ACCESS_KEY_ID }}
          secret-key: ${{ secrets.COR_AWS_SECRET_ACCESS_KEY }}
          account: $ARTIFACTS_AWS_ACCOUNT_ID
          role-name: $COR_CICD_AUTOMATION_ROLE_NAME
          version: $GITHUB_HEAD_REF
          service-name: $SERVICE_NAME
          project-name: "custom-lambda-auth"
          project-folder: "src/custom-lambda-auth"
          script-name: "pack"
          bucket-name: $ARTIFACTS_BUCKET_NAME
```
