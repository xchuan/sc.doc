# SES邮件服务

例子：/Users/Code/aws-ses

安装：yarn add @aws-sdk/client-ses

例子：https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ses

需要在sesClient里面配置AWS所属地区，同时需要如果是sandbox的条件下只能给验证过的邮箱发送邮件，正式环境可以对外发送邮件；

文件名：sendmail.js
```js
import { SendEmailCommand } from "@aws-sdk/client-ses";
import { sesClient } from "./libs/sesClient.js";

const createSendEmailCommand = (toAddress, fromAddress) => {
  return new SendEmailCommand({
    Destination: {
      /* required */
      CcAddresses: [
        /* more items */
      ],
      ToAddresses: [
        toAddress,
        /* more To-email addresses */
      ],
    },
    Message: {
      /* required */
      Body: {
        /* required */
        Html: {
          Charset: "UTF-8",
          Data: "HTML_FORMAT_BODY",
        },
        Text: {
          Charset: "UTF-8",
          Data: "TEXT_FORMAT_BODY",
        },
      },
      Subject: {
        Charset: "UTF-8",
        Data: "EMAIL_SUBJECT",
      },
    },
    Source: fromAddress,
    ReplyToAddresses: [
      /* more items */
    ],
  });
};

const run = async () => {
  const sendEmailCommand = createSendEmailCommand(
    "xcnbanba@hotmail.com",
    "noreply@xs-table.com",
  );

  try {
    return await sesClient.send(sendEmailCommand);
  } catch (e) {
    console.error("Failed to send email.");
    return e;
  }
};

export { run };
```

入口文件test.js
```js
import { run } from "./sendmail.js";
const result = await run();
console.log(result);
```

测试路径
/Users/Code/aws-ses/4.js

运行命令
node 4.js

<https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/ses-examples-sending-email.html>