# How to Read a Sync Message

TODO

# How to Read an Interaction Message

outerMessage.message.interactionType:

```
    public enum INTERACTIONS
    {
        LOOK = 0,         // Mouse pointer enter
        LOOK_END = 1,     // Mouse pointer exit
        SHOW = 2,         // Show model or model pack
        HIDE = 3,         // Hide model or model pack
        GRAB = 4,         // Start moving model or model pack
        DROP = 5,         // Stop moving model or model pack
        CHANGE_SCENE = 6, // Unused as of mid-2021
        SLICE_OBJECT = 7, // Unused as of late 2020
        LOCK = 8,         // Lock model or model pack
        UNLOCK = 9,       // Unlock model or model pack
        LINE = 10,        // Begin drawing stroke
        LINE_END = 11,    // End drawing stroke
        SHOW_MENU = 12,   // Show VR menu
        HIDE_MENU = 13,   // Hide VR menu

        SETTING_TAB = 14, // Open settings tab
        PEOPLE_TAB = 15,  // Open people tab
        INTERACTION_TAB = 16, // Open interaction tab
        CREATE_TAB = 17,  // Open create tab
    }
```

# How to Read a Draw Message

TODO

# How to read capture files (`data` or `data.json`):

```
[
  {
    "session_id": 999,
    "client_id": 998,
    "type": "sync",
    "message": {
      "clientId": 998,
      "entityId": 720,
      "entityType": 0,
      "scaleFactor": 1.0000001192092896,
      "rot": {
        "x": -0.0349637046456337,
        "y": 0.6177959442138672,
        "z": -0.027513714507222176,
        "w": -0.7850788235664368
      },
      "pos": {
        "x": 20.264863967895508,
        "y": 1.911105751991272,
        "z": -6.410734176635742
      }
    },
    "ts": 1643908918801,
    "seq": -3,
    "capture_id": "999_1643908918804"
  },
  {
    "session_id": 999,
    "client_id": 997,
    "type": "interaction",
    "message": {
      "sourceEntity_id": 1,
      "targetEntity_id": 0,
      "interactionType": 15
    },
    "ts": 1643908925922,
    "seq": 7118,
    "capture_id": "999_1643908918804"
  },
  ...
]
```
