## API Description
### Feature
This API is used to send notification or marketing SMS messages to a group of users.
Mobile numbers must be all Chinese numbers or overseas numbers. A maximum of 200 mobile numbers can be submitted at a time, and the length of each message must be not more than 450 words. This API is also available for one-to-one SMS messages.

### URL Example
`https://yun.tim.qq.com/v5/tlssmssvr/sendmultisms2?sdkappid=xxxxx&random=xxxx`    
 **Note**: Replace `xxxxx` in the field `sdkappid=xxxxx` with the sdkappid you applied for on Tencent Cloud, and replace `xxxx` in the field `random=xxxx` with a random number.

## Request Parameters
```json
{
    "ext": "",
    "extend": "",
    "params": [
        "Verification code",
        "1234",
        "4"
    ],
    "sig": "be66bb4aeb54701ed0637d0996a0b75111d5b8eda9b3a71bdc579a3d26f3edfb",
    "sign": "Tencent Cloud",
    "tel": [
        {
            "mobile": "13788888888",
            "nationcode": "86"
        },
        {
            "mobile": "13788888889",
            "nationcode": "86"
        }
    ],
    "time": 1457336869,
    "tpl_id": 19
}
```

| Parameter | Required | Type | Description |
|--------|------|--------|-----------------------------------------------------------------------|
| ext | No | String | User's session content (optional). The Tencent server returns it as is. You can leave it empty if it is not needed. |
| extend | No | String | Extended SMS code which is valid only when it is a pure numeral string. It is not enabled by default. [Contact SMS Helper](https://cloud.tencent.com/document/product/382/3773) to enable it. |
| params | Yes | Array | Template parameters. If the template has no parameters, leave it empty. |
| sig | Yes | String | App credential. For more information on the calculation, please see the following. |
| sign | No | String | SMS signature. To use the default signature, leave this field with the default value. |
| tel | Yes | Array | An array of mobile numbers to which SMS messages are sent in bulk. A maximum of 200 mobile numbers are supported for a request for sending bulk SMS messages. |
| time | Yes | Number | The time to initiate the request. It is a unix timestamp (in sec). A failure message is returned if the time difference between the unix timestamp and the system time is greater than 10 minutes. |
| tpl_id | Yes | Number | The template ID approved on the console |


- Parameters of the array element `tel`:

| Parameter | Required | Type | Description |
|------------|------|--------|----------|
| mobile | Yes | String | Mobile number |
| nationcode | Yes | String | Country code |
**Notes**:
1. The approved template ID needs to be entered in the "tpl_id" field. Based on the above request parameters, the issued content is:
"[Tencent Cloud] Your verification code is 1234. This verification code is valid for 4 minutes. If you are not using our service, ignore this message."
If you have multiple SMS signatures, place the needed SMS signature in the "sign" field.
For example, if you have two signatures, "[Tencent Technology]" and "[Tencent Cloud]", and you want to send an SMS message with "[Tencent Cloud]", the "sign" field can be: "Tencent Cloud".
2. The "sig" field is generated according to the formula sha256(appkey=$appkey&random=$random&time=$time&mobile=$mobile).
The pseudo codes are as follows:
```
string strMobile = "13788888888,13788888889"; //The content of the "mobile" field of "tel" is separated by ","
string strAppKey = "5f03a35d00ee52a21327ab048186a2c4"; //The appkey for the sdkappid, which must be kept confidential
string strRand = "7226249334"; //The value of the "random" field in the URL
string strTime = "1457336869"; //The unix timestamp
string sig = sha256(appkey=5f03a35d00ee52a21327ab048186a2c4&random=7226249334&time=1457336869&mobile=13788888888,13788888889)
           = be66bb4aeb54701ed0637d0996a0b75111d5b8eda9b3a71bdc579a3d26f3edfb;
```
## Response Parameters
```json
{
    "result": 0,
    "errmsg": "OK",
    "ext": "",
    "detail": [
        {
            "errmsg": "OK",
            "fee": 1,
            "mobile": "13788888888",
            "nationcode": "86",
            "result": 0,
            "sid": "xxxxxxx"
        },
        {
            "errmsg": "OK",
            "fee": 1,
            "mobile": "13788888889",
            "nationcode": "86",
            "result": 0,
            "sid": "xxxxxxx"
        }
    ]
}
```

| Parameter | Required | Type | Description |
|--------|------|--------|-----------------------------------------------|
| result | Yes | Number | Error code. 0: Successful (basis for billing). Other values: Failed |
| errmsg | Yes | String | Error message. The specific error message when the "result" is not 0 |
| ext | No | String | User's session content. The Tencent server returns it as is. |
| detail | Yes | Array | Result details |


- Parameters of the array element `detail`:

| Parameter | Required | Type | Description |
|------------|------|--------|------------------------------------------|
| result | Yes | Number | Error code. 0: Successful (basis for billing). Other values: Failed. For more information, please see [Error Codes](https://cloud.tencent.com/document/product/382/3771). |
| errmsg | Yes | String | Error message. The specific error message when the "result" is not 0 |
| fee | No | Number | Number of SMS messages billed. [About Billing](https://cloud.tencent.com/document/product/382/9556#.E7.9F.AD.E4.BF.A1.E5.86.85.E5.AE.B9.E9.95.BF.E5.BA.A6.E8.AE.A1.E7.AE.97.E8.A7.84.E5.88.99) |
| mobile | Yes | String | Mobile number |
| nationcode | Yes | String | Country code |
| sid | No | String | Delivery ID, indicating an SMS delivery record |




