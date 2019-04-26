# SMSHub

## what?

SMSHub is an SMS Gateway application for Android phones you can use to add SMS functionality to your software.
It connects to a webpage to retrieve messages to be sent (in JSON format) at regular intervals. Also notify about delivery status and incoming messages. 

## how?

### Send SMSs

1- The application connects at regular intervals to a URL

```
POST https://yourcustomurl.com/send_api
    deviceId: 1
    action: SEND
```

2- It should read a JSON containing *message*, *number* and *id*, or an empty response if there is nothing to send
```
{ "message": "hola mundo!", "number": "3472664455", "id": "1" }
```

3- The app will send the SMS *message* to *number*

4- Once sent (or failed) the app will notify the status to the status URL
```
POST https://yourcustomurl.com/status_api
    deviceId: 1
    messageId: 1
    status: SENT
    action: STATUS
```

5- Once delivered the app will notify the status to the status URL

```
POST https://yourcustomurl.com/status_api
    deviceId: 1
    messageId: 1
    status: DELIVERED
    action: STATUS
```

Possible _status_ values are: SENT, FAILED, DELIVERED

### Receive SMSs

1- Each time a SMS is received the app will notify the received URL
```
POST https://yourcustomurl.com/received_api
    deviceId: 1
    number: 3472556699
    message: Hello man!
    action: RECEIVED
```


## why?

Commercial SMS APIs are (depending on the case) prohibitively expensive. 
Instead you can use your own phone line to send SMS with an Android phone as a gateway.

## settings

you can customize the next settings directly in the application

#### Send SMS:
+ *Enable sending*: whether the app should read from the API and send messages
+ *send URL*: messages will be parsed from this URL, you return a JSON containing *message*, *number* and *id*
+ *interval*: the app will check whether there is an incoming message for sending each specific interval in minutes
+ *status URL*: once a message is sent, status will be reported to this URL via GET parameters, *id* and *status* (SENT, FAILED, DELIVERED)

#### Receive SMS:
+ *received URL*: Message received will be posted here. If nothing is specified it will skip this action.
