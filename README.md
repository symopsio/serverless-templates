# serverless-templates

Sym's serverless integrations let you use Sym workflows to manage resources that the Sym platform does not directly integrate with. Sym invokes your custom function w/the appropriate metadata and then your function implementation updates your backend systems. Our starter templates use the [Serverless Framework](https://serverless.com) to manage function creation and deployment.

## Generating a project

Use the `--template-url` parameter to `sls create` to specify one of our custom templates for your project:

### Typescript

```
$ sls create --name <your-project-name> --template-url https://github.com/symopsio/serverless-templates/tree/master/sym-aws-typescript-approvals
$ cd <your-project-name>
$ npm install
```

### Python

```
$ sls create --name <your-project-name> --template-url https://github.com/symopsio/serverless-templates/tree/master/sym-aws-python-approvals
$ cd <your-project-name>
$ npm install
```

Note: the python template includes the `dockerizePip` option set to `true`, but if you don't have Docker installed locally you can change this to `false`.

## Implementing your custom handler

Your handler needs to provide implementations of approval and expiration functions. 
* Approvals happen when a requester is approved (either by another user or automatically) for a given resource. Approvals receive an [Approval](https://github.com/symopsio/types/blob/master/docs/index.md#sym.messages.Approval) event.
* Expirations happen when the time limit on an approval is reached. Expirations receive an [Expiration](https://github.com/symopsio/types/blob/master/docs/index.md#sym.messages.Expiration) event.

### protobufs

Approvals and Expirations are defined as [protobufs](https://developers.google.com/protocol-buffers/) in the shared [symopsio/types](https://github.com/symopsio/types) repo. 

## Test data

The [test](test) folder includes example approval and expiration events that you can use for testing your lambdas.

## AWS Integration

### IAM Role

The `serverless.yml` config for AWS templates includes the definition of an IAM Role that Sym will use in order to invoke your functions. 

### Sym Configuration

Sym will need the following configuration items in order for your function to get invoked.

Function Names support any of the formats defined for the [FunctionName](https://docs.aws.amazon.com/lambda/latest/dg/API_Invoke.html#API_Invoke_RequestSyntax) parameter of the Lambda `Invoke` API.

* `ApprovalFunctionName`: The name of the approve Lambda function
* `ExpirationFunctionName`: The name the expiration Lambda function
* `RoleARN`: The ARN of the IAM Role that Sym assumes in order to invoke the function

## Get in touch

Please reach out to info@symops.io with any questions on this example or help getting started.
