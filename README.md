# build-react-aws-s3
buildspec file to build a React app to an S3 bucket

Add the AWS CodeBuild service specifying the buildspec.yml file. If you have more than one environment that goes to a different bucket or needs a different configuration, you can use multiple files (e.g. `buildspec-dev.yml`, `buildspec-dev.yml`) and specify the file name in the CodeBuild service for that environment.

Make sure your IAM Service Role has the right permissions to write in your bucket and that your S3 bucket has the right permissions to your Build Service and Public permissions to show your content.

S3 bucket permissions example:
~~~
{
    "Version": "2008-10-17",
    "Id": <id>,
    "Statement": [
        {
            "Sid": "build-allow",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::<your-iam-role-with-s3-permissions>"
            },
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::<your-s3-bucket>"
        },
        {
            "Sid": "public-allow",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::<your-s3-bucket>"
        }
    ]
}
~~~

### Serve your files with CloudFront

+ Create a CloudFront Distribution and add your S3 bucke as its Origin.
+ If using React Router, redirect your Errors Response in the CloudFront service, from Status Code 403 to 200, pointing to the `/index.html` file


