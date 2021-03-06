# About

This plugin provides a simple emailer.

![Travis build status](https://travis-ci.org/playframework/play-mailer.svg?branch=master)

## Setup

 1. Add a library dependency on play-mailer:

 ```scala
"com.typesafe.play" %% "play-mailer" % "{version}"
 ```
 See below for the version matrix.

 1. Add ```1500:play.api.libs.mailer.CommonsMailerPlugin``` to your ```conf/play.plugins```
 1. Configure the mailer in your ```conf/application.conf```:

 ```
smtp.host (mandatory)
smtp.port (defaults to 25)
smtp.ssl (defaults to no)
smtp.tls (defaults to no)
smtp.user (optional)
smtp.password (optional)
smtp.debug (defaults to no, to take effect you also need to set the log level to "DEBUG" for the application logger)
smtp.mock (defaults to no, will only log all the email properties instead of sending an email)
smtp.timeout (defaults to 60s)
smtp.connectiontimeout (defaults to 60s)
 ```

## Usage

### Java

```java
import play.libs.mailer.Email;
import play.libs.mailer.MailerPlugin;

Email email = new Email();
email.setSubject("Simple email");
email.setFrom("Mister FROM <from@email.com>");
email.addTo("Miss TO <to@email.com>");
// adds attachment
email.addAttachment("attachment.pdf", new File("/some/path/attachment.pdf"));
// adds inline attachment from byte array
email.addAttachment("data.txt", "data".getBytes(), "text/plain", "Simple data", EmailAttachment.INLINE);
// sends text, HTML or both...
email.setBodyText("A text message");
email.setBodyHtml("<html><body><p>An <b>html</b> message</p></body></html>");
MailerPlugin.send(email);
```

### Scala

```scala
import play.api.libs.mailer._

val email = Email(
  "Simple email",
  "Mister FROM <from@email.com>",
  Seq("Miss TO <to@email.com>"),
  // adds attachment
  attachments = Seq(
    AttachmentFile("attachment.pdf", new File("/some/path/attachment.pdf")),
    // adds inline attachment from byte array
    AttachmentData("data.txt", "data".getBytes, "text/plain", Some("Simple data"), Some(EmailAttachment.INLINE))
  ),
  // sends text, HTML or both...
  bodyText = Some("A text message"),
  bodyHtml = Some("<html><body><p>An <b>html</b> message</p></body></html>")
)
MailerPlugin.send(email)
```

`MailerPlugin.send()` method needs an implicit `play.api.Application` available to it.  If you do not have one available already from where you are trying to create the mailer you may want to add this line to get the current Application.

```scala
import play.api.Play.current
```

## Versionning

The Play Mailer plugin supports several different versions of Play.

| Plugin version      | Play version       | 
|---------------------|--------------------|
| 2.x                 | 2.3.x              |
| 3.x (milestone)     | 2.4.x              |

## Current Versions

For Play 2.3:

```scala
"com.typesafe.play" %% "play-mailer" % "2.4.0"
```

For Play 2.4 **milestone**:

```scala
"com.typesafe.play" %% "play-mailer" % "3.0.0-M1" // (not yet released)
```

## Licence

This software is licensed under the Apache 2 license, quoted below.

Copyright 2012 Typesafe (http://www.typesafe.com).

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this project except in compliance with the License. You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0.

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
