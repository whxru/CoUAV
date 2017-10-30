# MAVC Message
MAVC Message is a JSON string essentially, it is an simple protocol designed to implement the communication between Raspberry Pi 3 and the Monitor. The message is sent between programs mainly through UDP protocol.

##  Type

There are some values predefined as follows to identify the type of MAVC message:

| Name                 | Value | Description                              |
| -------------------- | ----- | ---------------------------------------- |
| MAVC_REQ_CID         | 0     | Request the Connection ID                |
| MAVC_CID             | 1     | Response to the ask of Connection ID     |
| MAVC_REQ_STAT        | 2     | Ask for the state of drone(s)            |
| MAVC_STAT            | 3     | Report the state of drone                |
| MAVC_ARM_AND_TAKEOFF | 4     | Ask drone to arm and takeoff             |
| MAVC_GO_TO           | 5     | Ask drone to fly to next target specified by latitude and longitude |
| MAVC_GO_BY           | 6     | Ask drone to fly to next target specified by the distance in both North and East directions |

## Format

The format of MAVC message should follow the example below:

```python
[
    {
        'Header': 'MAVCluster_Drone',   # or 'MAVCluster_Monitor'
        'Type': MAVC_REQ_CID            # values defined later
    },
    # If the value of 'Type' begins with 'MAVC_REQ_',then the message should contain the information above only

    # Type = MAVC_CID
    {
        'CID' : 1
    }

    # Type = MAVC_STAT
    {
        'CID' : 2,
        'Armed': True,      # Is armed or not
        'Mode': 'Guided',   # Flight mode that the drone currently in
        'Lat' : 38.13421,   # Latitude
        'Lon' : -114.31341, # Longitude
        'Alt' : 4           # Altitude(meters)
    }
    
    # Type = MAVC_ARM_AND_TAKEOFF
    {
    	'CID': 3,
    	'Alt': 5		   # Altitude(meters)
    }
    
    # Type = MAVC_GO_TO
    {
        'CID': 3,
        'Lat': 38.11523,
        'Lon': -118.53556,
        'Alt': 5,
        'Time': 3           # Time limit(seconds)
    },...

    # Type = MAVC_GO_BY
    {
        'CID': 3,
        'N': 3,             # Distance in North direction(meters)
        'E': 5,             # Distance in East direction(meters)
        'Alt': 5,           # Altitude(meters)
        'Time': 3           # Time limit(seconds)
    },...
]
```