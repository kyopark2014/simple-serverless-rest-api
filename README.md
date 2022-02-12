# simple-serverless-rest-api
여기에서는 API Gateway와 Lambda로 구성된 매우 간단한 Rest-api 기반의 서버리스 아키텍처를 설명하고자 합니다. 

API Gateway는 http/https를 사용하는 브라우저 또는 application이 서버와 접속할 수 있는 public endpoint의 기능을 제공합니다. 
사용자가 endpoint로 request을 보내면, API Gateway는 이러한 요청들을 serialize하여 Lambda로 전달하여 원하는 오퍼레이션을 수행 할 수 있습니다.

<img width="551" alt="image" src="https://user-images.githubusercontent.com/52392004/153715629-b20750dd-ca4e-43bd-93f9-f51dc94f725c.png">


## Lambda 설정 

1. AWS 콘솔  에서 AWS Lambda 서비스로 이동합니다.
https://ap-northeast-2.console.aws.amazon.com/lambda/home?region=ap-northeast-2#/functions

2. [Create function] 버튼을 클릭하여 함수 생성을 시작합니다.

<img width="1054" alt="image" src="https://user-images.githubusercontent.com/52392004/153702060-2a086a91-90bc-4368-bb0d-e12edf0bc478.png">

3. [Function name] 을 입력하고 [Runtime] 은 Node.js 을 선택합니다.

<img width="1286" alt="image" src="https://user-images.githubusercontent.com/52392004/153712281-a287d82d-4943-49c0-9f01-8b0921e63c46.png">

4.  [Create function] 버튼을 클릭하여 Lambda 함수를 생성을 완료합니다.

<img width="1290" alt="image" src="https://user-images.githubusercontent.com/52392004/153712330-62b5e554-15be-448e-bde0-34780978697e.png">

아래와 같이 기본 코드가 설정되어 있습니다.

<img width="1285" alt="image" src="https://user-images.githubusercontent.com/52392004/153712352-bd646c3e-5123-483f-af3e-62c575510f81.png">



## API Gateway 생성

Amazon API Gateway는 REST 및 WebSocketAPI 등을 생성, 배포, 유지 관리 할 수 있는 AWS 서비스로 모든 규모의 API 를 개발자가 손쉽게 구성할 수 있도록 해줍니다.


1. AWS 콘솔  에서 Amazon API Gateway 서비스로 이동합니다.
   https://ap-northeast-2.console.aws.amazon.com/apigateway/main/apis?region=ap-northeast-2#
   
   ![image](https://user-images.githubusercontent.com/52392004/153605555-29f9f780-3fa0-4b77-9769-36aef28d88bb.png)


2. 우측 상단의 [Create API] 를 클릭하고 [REST API] 옵션의 [Build] 버튼을 선택합니다.
  ![image](https://user-images.githubusercontent.com/52392004/153605698-9493c454-a51e-44ec-a5dd-e43a7b7b2906.png)

3. API 생성 화면에서 Create new API 에는 [New API] 를 선택하고 하단 Settings 의 [API name] 에는 serverless-app-api 를 입력합니다. [Endpoint Type] 은 Regional 을 선택합니다. API 트래픽의 오리진에 따라 Edge, Regional, Private 등의 옵션  을 제공하고 있습니다. [Create API] 를 클릭하여 API 를 생성합니다.

<img width="845" alt="image" src="https://user-images.githubusercontent.com/52392004/153711793-8ef37c45-3ebb-4b0a-8113-4158083edd0d.png">


Reginal API : current region
Edge Optimized APIs : CloudFront Network
Private API : accessible only from VPC


4. API 생성이 완료되면 [Resources] 메뉴 상단의 [Actions] 버튼을 드롭 다운 한 뒤 [Create Resources] 옵션을 선택합니다. 

<img width="695" alt="image" src="https://user-images.githubusercontent.com/52392004/153713845-60f15fc2-e173-4c63-b169-e015dfb881b4.png">


5. [New Child Resource]에서 [Resource Name]을 입력하고 [Create Resource]를 선택합니다.

<img width="972" alt="image" src="https://user-images.githubusercontent.com/52392004/153713987-29db83d2-7bb0-4217-bb0c-b01c92c4ff18.png">


6. API 생성이 완료되면 [Resources] 메뉴 상단의 [Actions] 버튼을 드롭 다운 한 뒤 [Create Method] 옵션을 선택합니다. 

<img width="418" alt="image" src="https://user-images.githubusercontent.com/52392004/153714119-75ec561e-7f07-483a-ac69-787e2a735d77.png">


생성된 빈 드롭 다운 메뉴에서는 [POST] 을 선택한 뒤 체크 버튼을 클릭합니다.

<img width="416" alt="image" src="https://user-images.githubusercontent.com/52392004/153714163-32b2411e-0dc4-46b3-bbd6-e2bd5f80ecbe.png">



7. / - POST - Setup 화면이 나타납니다. [Ingegration type] 은 Lambda Function 을 선택하고 [Lambda Region] 은 ap-northeast-2 를 선택합니다. [Lambda Function] 에는 미리 만든 lambda-filesharing-upload 를 선택합니다. [Save] 를 선택하여 API 메소드 생성을 완료합니다.

<img width="992" alt="image" src="https://user-images.githubusercontent.com/52392004/153714636-1f563e26-5f87-40c2-9da2-01418f1b440b.png">



8. Add Permission to Lambda Function 팝업이 나타나면 [OK] 를 선택합니다.

<img width="850" alt="image" src="https://user-images.githubusercontent.com/52392004/153715046-a386c2e5-a0c1-47a1-a694-80037e5ddd9a.png">

이후 아래와 같이 생성된 API 를 확인 할 수 있습니다.



<img width="872" alt="image" src="https://user-images.githubusercontent.com/52392004/153715013-6f3b326c-b54d-43cf-b8e5-96f5ee90520f.png">


9. 생성한 API 를 배포해줘야 합니다. [Resources] 메뉴 상단의 [Actions] 버튼을 드롭다운 한 뒤 [Deploy API] 를 클릭합니다.


<img width="595" alt="image" src="https://user-images.githubusercontent.com/52392004/153714369-89c54cab-229f-47ec-bfe2-188bc83f586f.png">



10. [Deploy stage] 는 [New Stage] 를 선택하고 [Stage name*] 에는 dev 를 입력한 뒤 [Deploy] 버튼을 클릭합니다.

<img width="592" alt="image" src="https://user-images.githubusercontent.com/52392004/153713143-f2b2e6d4-fbbe-4c37-ba0a-ff03e42481c7.png">


11. 아래와 같이 [Stages] - [dev]를 선택한후, invoke URL을 확인합니다.
https://xeps4yi0g0.execute-api.ap-northeast-2.amazonaws.com/dev

<img width="1404" alt="image" src="https://user-images.githubusercontent.com/52392004/153714415-4d6d19ef-f0e6-4c78-85ea-cb6620b19f18.png">


12. 아래와 같이 curl 명령으로 api-gateway와 lambda가 잘 연결되었음을 확인 할 수 있습니다. 

```c
$ curl -i https://xeps4yi0g0.execute-api.ap-northeast-2.amazonaws.com/dev/upload -X POST
HTTP/2 200
date: Sat, 12 Feb 2022 14:03:37 GMT
content-type: application/json
content-length: 50
x-amzn-requestid: 233dc7bb-0662-4dea-a68c-65dbc913f668
x-amz-apigw-id: Nbqo8HH_IE0FtIw=
x-amzn-trace-id: Root=1-6207be39-02e1750519543a464645f254;Sampled=0

{"statusCode":200,"body":"\"Hello from Lambda!\""}
```

### Reference
https://catalog.us-east-1.prod.workshops.aws/v2/workshops/05e3e1f9-5d5a-4cc5-9899-df114def68e7/ko-KR/lab1/step3


https://catalog.us-east-1.prod.workshops.aws/v2/workshops/05e3e1f9-5d5a-4cc5-9899-df114def68e7/ko-KR/lab2/step4




