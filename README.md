# gmail-multipart-body-maker
Simple utility for creating gmail multipart message-body

(Based on https://github.com/EmilTholin/gmail-api-create-message-body)

## Example usage

```js
const request = require('request-promise-native')
const fs      = require('fs')
const path    = require('path')

const createBody = require('./lib')

const catBase64 = fs.readFileSync(path.join(__dirname, 'cat.png')).toString('base64')
const dogBase64 = fs.readFileSync(path.join(__dirname, 'dog.jpg')).toString('base64')

const body = createBody({
  headers: {
    To: 'receiver@gmail.com',
    From: 'sender@gmail.com',
    Subject: 'This was rad, brother.'
  },
  textHtml: 'Thanks for last time, <b>buddy.</b>',
  textPlain: 'Thanks for last time, *buddy.*',
  threadId: '1536195a8ad6a354',
  attachments: [
    {
      filename: 'dog.jpg',
      content: dogBase64,
      encoding: 'base64',
      type: 'image/jpeg'
    },
    {
      filename: 'cat.jpg',
      content: catBase64,
      encoding: 'base64',
      type: 'image/jpeg'      
    }
  ]
})

const responseString = await request.post({
  url: 'https://www.googleapis.com/upload/gmail/v1/users/me/messages/send?uploadType=multipart',
  headers: {
    'Authorization': `OAuth ${token}`,
    'Content-Type': 'multipart/related; boundary="foo_bar_baz"'
  },
  body: body
})

return JSON.parse(responseString)
```

## Licence
MIT
