# iam-scrapbook
A collection of IAM policies we use

Although AWS does not support a checkbox solution to require users to log in with multi-factor authentication (MFA), you can force users to implement MFA by applying an IAM policy with a conditional statement that will deny all granted permissions until a user configures MFA with a mobile application and logs in with an MFA token. You can find the policy here. For more information on how the policy works, see this blog post. This guide assumes that the appropriate user IDs already have the Force MFA IAM policy attached to them.
IMPORTANT: Always be careful when assigning the Force MFA policy to IDs with access keys. For example, if access keys are hardcoded into an application, that application will lose its permissions when Force MFA is enabled.

MFA with the AWS Console

To configure MFA in the AWS console: 
1)  Log in to AWS account

2)  Click Services > IAM

3)  Click Users > find your user and click the ID

4)  Click the Security credentials tab

5)  Click edit button next to Assigned MFA device (it should say No)

6)  Select the virtual MFA device radio button and click Next step

7)  Make sure to install an MFA application on your mobile device (e.g., Authy or Google Authenticator). Click Next Step

8)  You should see a QR code. With your MFA application add an account (directions on this vary)

9)  Scan QR code with your mobile device

10) In the mobile app, find the changing code associated with the account you just created and enter it into the AWS console (you will need to use 2 codes to synchronize it)

11) Your MFA is now activated. You will need to logout of AWS and back in using your MFA code in order to assume permissions


Using MFA from the CLI

!! AWS CLI Minimum Required Version
Your AWS CLI needs to be up to date for best results when working with the CLI with MFA enabled. You can check your version as follows:
$ aws --version
aws-cli/1.11.36 Python/2.7.10 Darwin/16.3.0 botocore/1.4.93

If you are on anything lower than 1.11, you must update your CLI. See AWS' documentation for details on how to do so according to the package type you have.

!! As noted below, using MFA from the CLI is slightly more complex. There may be certain use cases that require special steps to perform with MFA enabled. For instance, to build one of our Cloud Formation stacks, an additional flag was needed for 'aws cloudformation create-stack' (--role-arn). But, please make sure that you are able to install and run the session script as described below first of all. Ensure that you can authenticate with MFA, and then execute arbitrary AWS CLI commands. If, after doing so, you encounter issues with procedures that used to work for you before MFA was enabled, contact someone on the Operations team for assistance.
 
Using your MFA code from the CLI is a bit more complicated. Trek10 has written a script that greatly simplifies this process. You can find the Windows and Mac scripts here. Below are the steps to use MFA from the CLI using Trek10’s session script: 
1) From Mac or Windows, navigate to your terminal and enter aws configure

2) Enter your AWS credentials (Access Key ID, Secret Access Key) and region

3) Navigate to your credentials file in the .aws directory (on Windows, it’s in the directory %userprofile%\.aws)

4) You will see something like:
[default]
aws_access_key_id = AKIA...
aws_secret_access_key = lWK...

5) Add the directory with the session script to your path environment variable.

6) Now run the session script and your AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY and AWS_SESSION_TOKEN will be set
  a) On Mac run: . session.sh <mfa token> <label> (ex: . session.sh 123456 default) Don’t forget the period before session.sh.
  b) Update? - When I run session.sh it asks for the code as part of the script so I typed - <  . session.sh default >  Then I get prompted for the MFA code.  It seemed to work then.
  c) On Windows: session <mfa token> <label> (ex: session 123456 default)

7) This session will be valid for 12 hours
