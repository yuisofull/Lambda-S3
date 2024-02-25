---
title : "Create Code"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2.2.2 </b> "
---

{{% notice note %}}
You need to have the Golang compiler installed on your machine to complete this part.
{{% /notice %}}

#### Writing Golang code for Lambda Function

1. Create a directory named **s3-upload**.
   - Open a terminal in the current directory and run ```go mod init s3-upload```.
   - Inside the directory, create a file named ``main.go``.
   - Copy the following code into the **main.go** file:

```go
package main

import (
	"bytes"
	"context"
	"encoding/json"
	"fmt"
	"github.com/aws/aws-lambda-go/events"
	"github.com/aws/aws-lambda-go/lambda"
	"github.com/aws/aws-sdk-go-v2/aws"
	"github.com/aws/aws-sdk-go-v2/config"
	"github.com/aws/aws-sdk-go-v2/service/s3"
	"github.com/grokify/go-awslambda"
	"io"
	"net/http"
	"path/filepath"
	"strings"
	"time"
)

var (
	bucketName = "bucket-for-lambda-55555" //change to your bucket name
	region     = "ap-southeast-1"
)

type customStruct struct {
	Content       string `json:"content,omitempty"`
	FileName      string `json:"fileName,omitempty"`
	FileExtension string `json:"fileExtension,omitempty"`
	Link          string `json:"link,omitempty"`
}

func GetFileFromAPIGatewayProxyRequest(req events.APIGatewayProxyRequest) ([]byte, string, error) {
	r, err := awslambda.NewReaderMultipart(req)
	if err != nil {
		return []byte{}, "", err
	}

	part, err := r.NextPart()
	if err != nil {
		return []byte{}, "", err

	}

	content, err := io.ReadAll(part)
	if err != nil {
		return []byte{}, "", err
	}

	return content, part.FileName(), nil
}

func fileNameWithoutExtSliceNotation(fileName string) string {
	return fileName[:len(fileName)-len(filepath.Ext(fileName))]
}

func Upload(request events.APIGatewayProxyRequest, cfg aws.Config) (image string, err error) {
	content, fileName, err := GetFileFromAPIGatewayProxyRequest(request)
	if err != nil {
		return
	}

	client := s3.NewFromConfig(cfg)

	fileExt := filepath.Ext(fileName)
	fileName = fmt.Sprintf("%s-%v%s", fileNameWithoutExtSliceNotation(fileName), time.Now().UnixNano(), fileExt)
	fileName = strings.Replace(fileName, " ", "-", -1)

	data := &s3.PutObjectInput{
		Bucket: &bucketName,
		Key:    &fileName,
		Body:   bytes.NewReader(content),
	}

	_, err = client.PutObject(context.TODO(), data)
	if err != nil {
		return
	}

	image = fmt.Sprintf("https://%s.s3.%s.amazonaws.com/%s", bucketName, region, fileName)

	return
}

func create(req events.APIGatewayProxyRequest) (res events.APIGatewayProxyResponse, err error) {
	cfg, err := config.LoadDefaultConfig(context.TODO())
	if err != nil {
		return events.APIGatewayProxyResponse{
			StatusCode: http.StatusInternalServerError,
			Headers: map[string]string{
				"Content-Type":                "application/json",
				"Access-Control-Allow-Origin": "*",
			},
			Body: "Error while retrieving AWS credentials",
		}, nil
	}

	image, err := Upload(req, cfg)
	if err != nil {
		return events.APIGatewayProxyResponse{
			StatusCode: http.StatusInternalServerError,
			Headers: map[string]string{
				"Content-Type":                "application/json",
				"Access-Control-Allow-Origin": "*",
			},
			Body: err.Error(),
		}, nil
	}
	custom := customStruct{
		Link: image,
	}
	customBytes, err := json.Marshal(custom)
	if err != nil {
		return res, err
	}

	return events.APIGatewayProxyResponse{
		StatusCode: 200,
		Headers: map[string]string{
			"Content-Type":                "application/json",
			"Access-Control-Allow-Origin": "*",
		},
		Body: string(customBytes),
	}, nil
}

func main() {
	lambda.Start(create)
}

```
{{% notice warning %}}
  At line 22 replace the placeholder values with your actual bucket name.
{{% /notice %}}
   - Open a terminal and run ``go mod tidy`` to download any missing packages.

#### Build the code

Now we will create a file to build the code.

##### **Windows**

Create a file named **build.bat** in the directory where **main.go** is located.
Add the following code to the file:

```bat
set GOOS=linux
set GOARCH=arm64
go build -tags lambda.norpc -o bootstrap main.go
winrar a -afzip myfunc.zip .\bootstrap
```

##### **Linux**

Create a file named **build.sh** in the directory where **main.go** is located.
Add the following code to the file:

```sh
#!/bin/bash

GOOS=linux GOARCH=arm64 go build -tags lambda.norpc -o bootstrap main.go
zip myfunc.zip bootstrap
```

#### Upload the code to Lambda

1. Access the [Lambda service management interface](console.aws.amazon.com/lambda/home)
   - Select the **s3-upload** function.
   - Under **Code** tab:
     - Select **Upload from**.
       - Choose **.zip file**.

![Code](/images/2.prerequisite/001-createcode.png)

   - Upload the **myfunc.zip** file from the directory where you built the code.

##### **Using AWS CLI**

   - Alternatively, if you have the AWS CLI installed on your machine, you can run:

   ```bash
   aws lambda update-function-code --function-name s3-upload --zip-file fileb://myfunc.zip
   ```