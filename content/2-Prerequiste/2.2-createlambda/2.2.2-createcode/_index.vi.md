---
title : "Tạo Code"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2.2.2 </b> "
---

{{% notice note %}}
Bạn cần cài đặt sẵn Golang compiler trên máy để thực hiện phần này
{{% /notice %}}
#### Viết code Golang cho Lambda Function
1. Tạo 1 thư mục tên **s3-upload**
  + Mở terminal ở thư mục hiện tại ghi ```go mod init s3-upload```
  + Vô trong thư mục tạo file ``main.go``
  + Nhập đoạn code sau file **main.go**
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
Ở dòng 22 trong file **main.go** thay **bucketName** bằng tên bucket của bạn
{{% /notice %}} 
  + Mở terminal lên và ghi ``go mod tidy``. Nó sẽ giúp bạn download các package chưa có

#### Build code

Giờ ta sẽ tạo 1 file để build code

##### **Windows**

Tạo file **build.bat** tại thư mục có file **main.go**
Điền đoạn code này vô file đó:
```
set GOOS=linux
set GOARCH=arm64
go build -tags lambda.norpc -o bootstrap main.go
winrar a -afzip myfunc.zip .\bootstrap
```
##### **Linux**
Tạo file **build.sh** tại thư mục có file **main.go**
Điền đoạn code này vô file đó:
```sh
#!/bin/bash

GOOS=linux GOARCH=arm64 go build -tags lambda.norpc -o bootstrap main.go
zip myfunc.zip bootstrap
```

#### Upload code lên Lambda
1. Truy cập giao diện Truy cập [giao diện quản trị dịch vụ Lambda](console.aws.amazon.com/lambda/home)
  + Chọn Function **s3-upload**
  + Tại mục **Code**
    + Chọn **Upload from**
      + Chọn **.zip file**
![Code](/images/2.prerequisite/001-createcode.png)

  + Upload file **myfunc.zip** tại thư mục bạn vừa build code

##### **AWS CLI**
  + Hoặc nếu trên máy bạn đã có sẵn **AWS CLI** bạn có thể chạy:
  ```aws lambda update-function-code --function-name s3-upload --zip-file fileb://myfunc.zip```