# cdcsdk-testing

## Setup S3 Repository access to download Yugabyte Connector


    mkdir -p ~/.aws
    touch ~/.aws/credentials


Copy credentials from [AWS Management
Console|https://aws.amazon.com/blogs/security/aws-single-sign-on-now-enables-command-line-interface-access-for-aws-accounts-using-corporate-credentials/]


Follow instructions to create a profile in `~/.aws/credentials`. Change the
profile to default. The file should look like:


    [default]
    aws_access_key_id=...
    aws_secret_access_key=...
    aws_session_token=...


**Note that these credentials and temporary and you may have to regularly pick
up new credentials**

## Integration Tests


    mvn integration-test
    # Run a specific integration test
    mvn integration-test -Dit.test=HttpIT -DfailIfNoTests=false

## Run Release Tests

As of now, we are targeting 3 different stages of automation:
1. YugabyteDB inside TestContainers
2. CDCSDK Server inside TestContainers
3. Assertion of S3 values and their cleanup

To run the tests, follow these steps:
* Make sure you have the required creds setup in your env variables
  * `export AWS_ACCESS_KEY_ID='<your-access-key-id>'`
  * `export AWS_SECRET_ACCESS_KEY='<your-secret-access-key>'`
* Start a YugabyteDB instance on you local IP
  * `./yugabyted start --listen $(hostname -i)` or `./yugabyted start --advertise_address $(hostname -i)` if you're using YugabyteDB version higher than 2.12
* Run `mvn clean integration-test -PreleaseTests` inside the `cdcsdk-testing` repo

* The below command will create a docker image of CDCSDK Server and run
integration tests in cdcsdk-testing
    * ```mvn integration-test -Drun.releaseTests```
