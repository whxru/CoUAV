# MAVC Message
MAVC message is a JSON string essentially, it is designed to implement the communication between Raspberry Pi 3 and the monitor. The message is sent between programs mainly through UDP protocol.

##  Type

There are some values predefined as follows to identify the type of MAVC message:

| Name              | Value | Description                              |
| ----------------- | ----- | ---------------------------------------- |
| MAVC_REQ_CID      | 0     | Request the Connection ID                |
| MAVC_CID          | 1     | Response to the ask of Connection ID     |
| MAVC_STAT         | 2     | Status information of the drone          |
| MAVC_SET_GEOFENCE | 3     | Set the geofence of drone                |
| MAVC_ACTION       | 4     | Actions to be performed                  |
| MAVC_ARRIVED      | 5     | Tell the monitor that the drone has arrived at the target |
| MAVC_DONE         | 6     | Close the connection between RPi and monitor |

### Action Type

There are some values predefined as follows to identify the type of action in MAVC_ACTION message:

| Name                   | Value | Description                              |
| ---------------------- | ----- | ---------------------------------------- |
| ACTION_ARM_AND_TAKEOFF | 0     | Ask drone to arm and takeoff             |
| ACTION_GO_TO           | 1     | Ask drone to fly to next target specified by latitude and longitude |
| ACTION_GO_BY           | 2     | Ask drone to fly to next target specified by the distance in both North and East directions |
| ACTION_LAND            | 3     | Ask drone to land at current or a specific position |



## Format

The format of MAVC message should follow the example below:

```python
[
    {
    	"Header": "MAVCluster_Drone",   # Or "MAVCluster_Monitor"
    	"Type": MAVC_REQ_CID,           # Values pre-defined
    },
	
    # Type = MAVC_REQ_CID
    {
        "Lat": 38.13546,                # Latitude of home
        "Lon": -113.546874              # Longitude of home
    }

    # Type = MAVC_CID
    {
        "CID" : 1
    }

    # Type = MAVC_STAT
    {
        "CID" : 2,
        "Armed": True,      # Is armed or not
        "Mode": "Guided",   # Flight mode that the drone currently in
        "Lat" : 38.13421,   # Latitude
        "Lon" : -114.31341, # Longitude
        "Alt" : 4           # Altitude(meters)
    }
    
    # Type = MAVC_SET_GEOFENCE
    {
    	"Radius": 50,       # Radius of the border
	"Lat": 38.131465,   # Latitude of the center
	"Lon": -114.23546   # Longitude of the center
    }
    
    # Type = MAVC_ACTION
    {
    	"Action_type": ACTION_ARM_AND_TAKEOFF,
    	"CID": 3,
    	"Alt": 5,   	  
    	"Step": 0,	    # This value can only be 0
    	"Sync": False,	    # Whether synchronize all of the drones after reaching the altitude
    }
    
    # Type = MAVC_ACTION
    {
        "Action_type": ACTION_GO_TO,
        "CID": 3,
        "Lat": 38.11523,
        "Lon": -118.53556,
        "Alt": 5,
        "Time": 3,          # Time limit(seconds)
        "Step": 1,          # Which step this target at in the drone"s mission
        "Sync": True,       # Whether synchronize all of the drones after reaching the target
    },...  # The ellipsis indicates that there can be more than one action in this format in a single MAVC message

    # Type = MAVC_ACTION
    {
        "Action_type": ACTION_GO_BY,
        "CID": 3,
        "N": 3,             # Distance in North direction(meters)
        "E": 5,             # Distance in East direction(meters)
        "Alt": 5,           
        "Time": 3,          
        "Step": 2,           
        "Sync": False,      
    },...
    
    # Type = MAVC_ACTION
    {
    	"Action_type": ACTION_WAIT,
	"CID": 3,
	"Time": 5,
	"Step": 5
    },...
    
    # Type = MAVC_ACTION
    {
    	"Action_type": ACTION_LAND,
	"CID": 3,
	"Lat": 38.11564,   # Latitude of position to land at, current position should be zero
	"Lon": -115.5687,  # Longitude of position to land at, current position should be zero
	"Step": 10,        # This should be the last step
	"Sync": True
    }
    
    # Type = MAVC_ARRIVED
    {
        "CID": 3,
        "Step": 1 
    }
    
    # Type = MAVC_DONE
    {
        "CID": 4
    }
]
```
