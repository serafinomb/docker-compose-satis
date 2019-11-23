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
