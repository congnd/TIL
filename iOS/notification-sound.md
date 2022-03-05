### Custom push notification sound

When sending the payload for the notifition to APNS, we need to specify the file of the sound we want to be played 
when the notification is delivered to the device. The file must be added to your app's bundle and its format should be `.caf`.

We can convert from sound files to `.caf` easily with the following command:

```
afconvert -f caff -d aacl@22050 -c 1 1.mp3 1.caf
```

The payload looks like this:

```
{
    aps =     
    {
        alert = "notification message";
        sound = "1.caf";
    };
}
```

### If you use Firebase Cloud Messaging

```
npm install firebase-admin
```

```js
var admin = require("firebase-admin");

var serviceAccount = require("/path/to/serviceaccount.json");

admin.initializeApp({
    credential: admin.credential.cert(serviceAccount)
});

var payload = {
    notification: {
        title: "This is a Notification",
        body: "This is the body of the notification message.",
        sound: "1.caf"
    }
};

var options = {
    priority: "high",
    timeToLive: 60 * 60 *24
};

admin.messaging().sendToTopic("user_xxx", payload, options)
.then(function(response) {
    console.log("Successfully sent message:", response);
})
.catch(function(error) {
    console.log("Error sending message:", error);
});
```

### Sounds source

https://audiojungle.net/tags/notification%20sound

Ref: https://stackoverflow.com/a/37536837/3867033
