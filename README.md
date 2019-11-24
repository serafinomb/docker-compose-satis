### Run
- Update `satis.json` as needed.
- Run `docker-compose -p satis up -d`
- Visit http://127.0.0.1:8080.

### Update package info
`docker-compose -p satis run satis vendor/composer/satis/bin/satis build satis.json web`

### AWS CodeCommit IAM configuration
Step 1-8
https://docs.aws.amazon.com/codecommit/latest/userguide/setting-up-ssh-unixes.html#setting-up-ssh-unixes-keys

Ideally you would have an IAM user for the package manager and another for your projects.

### Usage in a project
- Update your project's composer.json as follows:
```json
{
    ...
    "repositories": [{
        "type": "composer",
        "url": "http://packages.example.org"
    }],
    ...
}
``` 
- Use Composer as usual.

Mind that the repository download will still happen from CodeCommit. After updating your composer.json file make sure that your SSH public key is configured on AWS and that your .ssh/config file contains the CodeCommit repository host (as seen in the AWS CodeCommit IAM configuration guide). If everything is correctly configured, you'll be able to authenticate to CodeCommit.

### Deploy on AWS
You can find a `buildspec.yml` file that will help you deploy Satis on AWS.
This file will take care of building and updating your packages, the artifact
should be deployed on a S3 bucket with support for Web Hosting (see
https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html)

You'll need to:
- Create a CodeCommit repository to host this code
- Create a CodePipeline to build and deploy it
- Create an S3 bucket and configure it to host a static website
- Attach to the CodePipeline service role the "AWSCodeCommitReadOnly" policy

This should be enough to deploy your Satis instance.

You can choose whether to run the CodePipeline periodically or everytime one of your packages (hosted on CodeCommit) get updated. You'll need to use CloudWatch Events for either solutions.

