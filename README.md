# DrrrAPI
This is Unofficial DRRR.com API documentation compiled by Atagait#Magic/GOD.

### Background
This API documentaiton *may not be complete*. Anything listed here I've come across through my own efforts, or by stumbling across it in other repositories. I'll reserve a section of this for other projects at the end.

For the purposes of brevity, I will not be including the full URI for each endpoint.
The Base URI is "https://drrr.com"

I will also be separating certain endpoints into two sections, **Session** and **No Session** since some endpoints have different returns depending on if you have logged in or not. This basically only applies to the index page.

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

# Lounge API Endpoints
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
#OBJECTS
## User Object
```
{
  name:(user name),
  id:(user unique ID),
  icon:(user icon),
  device:(desktop/tablet/mobile/bot),
  loginedAt:(login event timestamp),
  player:(boolean - i don't know what this is for),
  role:(string - i don't know what this is for),
  visible:(boolean - i don't know what this is for),
  alive:(boolean - i don't know what this is for)
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
}
```
