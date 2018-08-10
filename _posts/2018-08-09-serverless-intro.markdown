---
id: 66
title: Process Migration
date: 2015-09-28T04:53:38+00:00
author: Ashutosh Chaudhary
layout: post
guid: http://codeasashu.tk/blog/?p=66
permalink: /process-migration/
dsq_thread_id:
  - 4196426863
views:
  - 54
sociallikes:
  - 1
categories:
  - M.Tech
  - Mobile Computing
tags:
  - mobile computing
  - mtech
---

# Serverless setup with AWS

Setting up a completely automated serverless setup on AWS can be a very hefty task. It will require a lot of efforts, configuring various services, keeping a note of what to link where, IAM roles etc. This not only requires a lot of research but consumes a lot of time as well.

Serverless solves all the problem of configurations, linking and creating roles and permissions, letting you focus on business logic and getting things done in few keystrokes. 

In this tutorial, I will be creating a sample "hello world" program in *Lambda* (NodeJS), using *API Gateway* which will output the results on browser.  In the next tutorial, I will help setting up dynamodb so you can have a datastore. We can also use RDS.

## Prerequisites

1. An AWS account
2. Any local machine (I use linux)

## Setting up AWS

Setting up AWS is the easy part. You just have to goto [console.aws.amazon.com](http://console.aws.amazon.com) and signup for an account. You have to link a credit card for verification. Its free for an year so you won't be charged and test out things for sake of this tutorial.

Once account setup is done, go to IAM section and create a new user, making the access type `programmatic` (NOTE: it is going to provide you with access key id and secret which will be used later on). 

Next screen will take you to add permission to this user, where you can create a new group or choose from any existing one. Click *Create Group*  to begin, which will bring a popup asking for name and roles. Since you are new, create a new group with any name of your choice. **Select Administrator access** as role for this user since it will be easiest route to go. Hit *Create Group* button. 

Move on to next step *Review* where you can tally if user have the administrator access and programmatic authentication type. Finally, hit *Create User* which will suffice and complete the process. 

You will be presented with the accesskey and secret which you should copy down somewhere safe. There is also a *download* option to download them in csv.
> ETA: 5 mins

## Setting up Serverless

We are going to need following AWS entities to get things rolling:

* Lambda (for processing)
* API Gateway (for API)
* CloudWatch (for logging)
* IAM roles (for permissions)
  
This will be enough to get a presentable state of our demo up and running in minutes. Lets get things going.

All serverless need is a config files which will determine how to configure things on AWS. 
To do so, First, it needs to know AWS account and region so it can use it to deploy applications. 

### 1. Setting up local aws credentials

**Manual Way**

1. Create a directory .aws in your home directory (~/.aws on *nix, C:\Users\USER_NAME\\.aws). 
2. Create config file in .aws with following contents
   ```
   [default]
   output = json
   region = us-west-2
    ```
3. Create credentials file in .aws with following contents
   ```
   [default]
   aws_access_key_id = <your-access-key-id>
   aws_secret_access_key = <your-secret-access-key>
    ```

    Read More: https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/setup-credentials.html

** Automated Way**

1. Download and set AWS CLI on your machine. [Instructions here](https://docs.aws.amazon.com/lambda/latest/dg/setup-awscli.html)
2. run `aws configure` and answer the prompt questions. 

### Creating Serverless configuration file

1. Create a project directory and enter it
2. Create a file called `serverless.yml`
3. Enter following content:

```yml
service: serverless-helloworld
frameworkVersion: ">=1.1.0 <2.0.0"

provider:
  name: aws
  runtime: nodejs8.10
  environment:
    SERVICE_NAME: ${self:service}
    WORLD: 'world'
  
functions:
  helloworld:
    handler: hello/index.hello
    events:
      - http:
          path: hello
          method: get
          cors: true
```

Although, pretty much self explanatory, let me summarize this for you.

`provider` key tells serverless what aws providers are we going to use and which versions of them, such as lambda runner, dynamodb etc. This is kind of versioning aspect of the 

This is a good place to set **environment variables** too. For instance, we are setting up *SERVICE_NAME*, *WORLD* as variables, which can be accessed inside our lambda node functions as `process.env.SERVICE_NAME`, `process.env.WORLD`.

Serverless supports *event per function* kinda configuration. Hence, we write the lambda handlers and the events on which it is going to respond.
Various types of events are supported such as API Gateway, S3, SQS, CloudWatch etc. Since we are going to process as soon as we get an API request, we will be using API gateway event. 

We can respond to same kind of process in response to various kind of events. For ex- You may want to write logs if file is uploaded to s3, but also when certain API request is being made, same lambda handler can be used to handle both the events. For now, we will only respond to API event(HTTP).

Hence, the basic configuration may look like:

```yml
...

functions:
  <custom-name>:
    handler: <file-location-without-js-suffix>.<method-name>
    events:
      - <event-name>:
          <event-configurations>
      - <another-event-name>:
          <event-configurations>
```

Serverless configures events 

```yml
- http:
    path: hello
    method: get
    cors: true
```
This tells the event type will be http and values provides the configurations, such as for which path the event should be triggered etc. You got this gist.

> ETA: 10-15 mins

### Writing actual code

Lets again look at the configuration, specifically this part:

```yml
  helloworld:
    handler: hello/index.hello
    events:
      - http:
          path: hello
          method: get
          cors: true
```

This can be translated to:
> Create a file in the `hello` directory (relative to config file), called `index.js` which will have the function `hello` to handle the request and will provide the response.

Lets write some code.

NOTE: Since we are using node 8.9.0 (check config again), we can use ES6 features.

```js
// index.js

module.exports = (event, context, callback) => {
  // Getting query string parameters. It looks something like {a: 'b', c: 1}
  const params = event.queryStringParameters ? event.queryStringParameters : {}
  const body = 'Hello ' + process.env.WORLD
  const responseType = {success: 200, client_error: 400, server_error: 500};

  // Success response
  callback(null, {
    statusCode: responseType.success,
    body: JSON.stringify({body, params})
  });

  // Or Error response like
  /*
  //let error = new TypeError('some error')
  callback(error, {
    statusCode: responseType.client_error,
    body: JSON.stringify({ error: 'Any error message' }),
  });
  */
}
```

Thats it. You are nearly ready to deploy all this. Just execute the following command and hit `Enter` key to deploy.

```sh
$ serverless deploy

# or

$ sls deploy
```

Before making ourselves much happier and comfortable, lets first see what are we getting as input in our lambda handler and what and how are we outputting.

Starting with the first line:

```js
module.exports = (event, context, callback) => {
  // shorthand for:
  // module.exports = function (event, context, callback) {
  // ...
```

We get three parameters to lambda handler (for nodejs): 

1. event: Provides event sources and variables attached to any of [AWS Event Sources](https://docs.aws.amazon.com/lambda/latest/dg/invoking-lambda-function.html)
2. context: This provides lambda context so that you can extract out any piece of information related to this function such as configurations, runtime operations etc. [Read more](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-prog-model-context.html)
3. callback: An optional parameter, which will be called after event loop becomes empty. In layman terms, this is the place which will be called "finally" and this is where your response should be.

Lets dive straightaway into the code now.

Getting query parameters of an Gateway request is quite easy. You can get them using `event.queryStringParameters`. `event` also provides other necessary info related to requests such as headers. Here is how a sample event object looks like:

```json
{
  "path": "/test/hello",
  "headers": {
    "Accept": "application/json;q=0.9,image/webp,*/*;q=0.8",
    "Accept-Encoding": "gzip, deflate, lzma, sdch, br",
    "Accept-Language": "en-US,en;q=0.8",
    ...
  },
  "pathParameters": {
    "proxy": "hello"
  },
  "requestContext": {
    "accountId": "123456789012",
    "resourceId": "us4z18",
    "stage": "dev",
    "requestId": "41b45ea3-70b5-11e6-b7bd-69b5aaebc7d9",
    "identity": {
      ...
      "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/52.0.2743.82 Safari/537.36 OPR/39.0.2256.48",
      "user": ""
    },
    "resourcePath": "/{proxy+}",
    "httpMethod": "GET",
    "apiId": "xxxx"
  },
  "resource": "/{proxy+}",
  "httpMethod": "GET",
  "queryStringParameters": {
    "a": "b"
  }
  ...
}
```

Setting response body is also quite easy. You just have to pass it as **second parameter** of `callback` function. This is because the **First parameter** is reserved for `Error` and should be `null` if you want to indicate a success response.

I think its very straighforward. Moving on...

### Deploying services

Like I've said, deployment is easy. Just execute `serverless deploy` or `sls deploy` command and serverless will create the necessary cloudformation stack, upload the package to s3 and cloudformation will take care of creating the necessary architecture. You can sit and relax and have a coffee instead. 

It hardly took 1 hour to get things up and running and having a response, which might've took you more if you're planning to go manual way.

Hope you've liked my efforts. Will keep posting new articles and more on architectures and orchestration. Comment below if you need any help.