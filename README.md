# Demo AWS lambda function


## Create a function

Write a simple function, for example a python script that says hello:

```py
def lambda_handler(event, context):
    print("Hello World")
```

Create a file `HelloWorld.py` with the function above.


### Create a zip

Create a zip file that contains the function, which is necessary to upload to AWS:

```sh
$ zip HelloWorld.zip HelloWorld.py
  adding: HelloWorld.py (stored 0%)
```

### Create the function

Create the function by using the AWS CLI:

```sh
aws lambda create-function \
--function-name HelloWorld \
--runtime python3.6 \
--role arn:aws:iam::048251220134:role/demo-aws-cloudwatch-lambda-role \
--handler HelloWorld.lambda_handler \
--zip-file fileb://HelloWorld.zip
```


### Invoke

Invoke:

```sh
aws lambda invoke \
--function-name HelloWorld output.txt
```

Verify the output text:

```sh
cat output.txt
```

### Troubleshooting: bad handler

If you get this reply:

  * {"errorMessage": "Bad handler 'lambda_handler'"}

Then you can see that 'lambda_handler' is wrong because it is missing the file name.

  * The handler must have the file name e.g. "HelloWord.lambda_handler".

Update the handler name:

```sh
aws lambda update-function-configuration \
--function-name HelloWorld \
--handler HelloWorld.lambda_handler
```

### Troubleshooing: ResourceConflictException

If you get this:

  * An error occurred (ResourceConflictException) when calling the CreateFunction operation: Function already exist: HelloWorld

Then you probably need to update, not create:

```sh
aws lambda update-function-code \
--function-name HelloWorld \
--zip-file fileb://HelloWorld.zip
```

## Thanks & Credits

Links:

* <a href="https://aws.amazon.com/blogs/compute/building-a-dynamic-dns-for-route-53-using-cloudwatch-events-and-lambda/">"Building a dynamic DNS for Route 53" - AWS blog post</a>

* <a href="https://markhneedham.com/blog/2017/04/02/aws-lambda-programatically-create-a-python-hello-world-function/">"AWS Lambda: Programatically create a Python 'Hello World' function" - blog post - by Mark Needham</a>

* <a href="https://spin.atomicobject.com/2016/04/28/route-53-hosted-zone-managment/">"Route 53 hosted zone management" - Atomic Object blog post - By Justin Kulesza</a>




