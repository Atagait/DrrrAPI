# DrrrAPI
This is Unofficial DRRR.com API documentation compiled by Atagait#Magic/GOD.

### Background
This API documentaiton *may not be complete*. Anything listed here I've come across through my own efforts, or by stumbling across it in other repositories. I'll reserve a section of this for other projects at the end.

For the purposes of brevity, I will not be including the full URI for each endpoint.
The Base URI is "https://drrr.com"

I will also be separating certain endpoints into two sections, **Session** and **No Session** since some endpoints have different returns depending on if you have logged in or not. This basically only applies to the index page.

**Important note!**
When creating string representation of boolean values, some languages will capitalize them (they return "True" not "true").
Some POST endpoints on Drrr.com that accept boolean values (I.E create_room) **REQUIRE** the boolean values to be lowercase.
If something seems to not be working, check your post requests to see if something is capitalized where it shouldn't be.

# Index API Endpoints
## GET /?api=json
**No Session**
```
{
  Authorization:(what will be set as your drrr-session-1 cookie which is used to track your session),
  aps: [
   {
    name:"Direct",
    ap:"direct" 
   }
  ],
  username_max_length:10,
  languages:
  {
    (all the languages the site supports, defines the language section shown at the top of the lounge)
  },
  default_language:"en-US",
  icons:[
    (all the available icons on the site)
  ],
  token:(what you post to the index to log in)
}
```
**Session**
```
{
  redirect:"lounge",
  message:"Already logined",
  authorization:(your drrr-session-1 cookie which is used to track your session)
}
```
## POST /
```
{
  name:(The name you want),
  icon:(The icon you want),
  token:(The token provided from the earlier endpoints),
  login:"ENTER",
  language:(The language you want)
}
```

# Lounge API Endpoint
## GET /lounge/?api=json
```
{
  redirect:"room",
  create_room_url:"//drrr.com/create_room/?api=json",
  active_user:(number of users in rooms),
  rooms:[
    (room objects)
  ],
  profile:(current user object)
}
```

# Create Room Endpoint
## GET /create_room/?api=json
```
{
  max_length:10,
  user_min:2,
  user_max:20,
  max_rooms:500,
  language:en-US,
  language:
  [
    (list of supported languages)
  ]
}
```

## POST /create_room/
```
{
  name:(room name),
  description:(room description),
  limit:(max number of users in the room),
  language:(room language section),
  adult:(boolean - is it an adult room),
  music:(boolean - can music be played),
  submit:"Create Room"
}
```

## GET /room/?api=json
```
{
  profile:(current user object),
  room:(current room object)
}
```

## GET /room/?api=json&update=TIMESTAMP
```
{
  profile:(current user object),
  room:(current room object)
}
```
update is a filter which will only return events that have taken place after the provided TIMESTAMP
It should be noted that if the timestamp is in the future, the request will attempt to wait until an event happens at that time.


# OBJECTS
## User Object
```
{
  name:(user name),
  id:(user unique ID),
  icon:(user icon),
  device:(desktop/tablet/mobile/bot),
  loginedAt:(login event timestamp),
  player:(boolean - flags if you are playing Werewolf in a game room),
  role:(string - what your role/job is in Werewolf),
  visible:(boolean - unsure as of now, see issue #1),
  alive:(boolean - flags if you are alive or dead in Werewolf)
}
```

## Room Object
```
{
  name:(room name),
  description:(room description),
  limit:(max population),
  total:(total no. of users),
  language:(room language section),
  roomId:(room unique ID),
  id:(same as roomId),
  since:(room creation timestamp),
  music:(boolean),
  staticRoom:(boolean),
  hiddenRoom:(boolean),
  gameRoom:(boolean),
  adultRoom:(boolean),
  users:
  [
    (user objects)
  ]
  //The following objects are only displayed by the /room endpoints
  user:(currnt user object),
  profile:(current user object),
  is_game_host:(boolean),
  //Now playing - used for audio share
  np:{
    name:(string),
    url:(string),
    playURL:(string, not sure how this differs from URL),
    shareURL:(string, not sure how this differs from URL),
    source:"direct"(string, haven't seen any others),
    direct:(boolean) (haven't seen it false),
    info:{
      album:(string, haven't seen it used),
      pic:(string, haven't seen it used),
      file_size:"0"(string, haven't seen it used),
      file_br:"0"(string, haven't seen it used),
    }
    time:(timestamp of when audio was posted)
  },
  talks:[
    (array of message objects)
  ]
  
}
```

## Message Objects
They are sorted by message type
### Message
```
{
  id:(string, message unique ID),
  time:(number, message timestamp),
  type:"message",
  from:(author user object),
  message:(string, message content)
}
```

### Me
```
{
  id:(string, message unique ID),
  time:(number, message timestamp),
  type:"me",
  from:(author user object, {1}),
  message:"{1} {2}",
  content:(string, message content, replaces {2})
}
```

### Leave
```
{
  id:(string, message unique ID),
  time:(number, message timestamp),
  type:"leave",
  user:(user who left, {1}),
  message:"{1} logged out."
}
```

### Roll
```
{
  id:(string, message unique ID),
  time:(number, message timestamp),
  type:"roll",
  from:(user who typed /roll, {1}),
  to:(user who was rolled {2}),
  message:"{1} has rolled @{2}"
}
```

### Join
```
{
  id:(string, message unique ID),
  time:(number, message timestamp),
  type:"join",
  user:(user who joined, {1}),
  message:"{1} logged in.."
}
```
