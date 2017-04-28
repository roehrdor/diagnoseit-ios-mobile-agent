# IOS Mobile Agent
## Description
The mobile Agent refers to an iOS application which is being monitored and allows the developer to sample the following metrics:

| Categorization | Metric | Sampling |
| ------ | ------ | ------ |
| System based | CPU usage | Interval based |
| System based | Hard disk usage | Interval based |
| System based | RAM usage | Interval based |
| System based | Battery power | Interval based |
| System based | WLAN active | Begin and end of remote calls |
| Network | Network connection | Begin and end of remote calls |
| Network | Network provider | Begin and end of remote calls |
| Network | SSID | Begin and end of remote calls |
| GPS | Position (longitude, latitude) | Begin and end of remote calls |
| Remote call | Response Code | Begin and end of remote calls |
| Application level | Timeout | Begin and end of remote calls |

## How to use
### Installation
Clone the repository into your applications folder and include the bridging header into your project settings.
If you already have a bridging header in your application make sure to add the following 
```c
#import <Foundation/Foundation.h>
#include "nativeDiagnostics.h"
```

### Use case and remote call creation and handling
The code below shows a small example of how the Agent can be used to track execution and remote calls of the running application
```swift
// start a root use case
let root = Agent.getInstance().startUseCase(name: "root-use-case")

// start a child use case
let login = Agent.getInstance().startUseCase(name: "login", root: root)

// start a remotecall
let rc = Agent.getInstance().startRemotecall(name: "login-remote", parent: login, url: "" ,httpMethod: "GET", timeout: 4.0, request: &request)

// .. perform
Agent.getInstance().closeRemoteCall(span: rc, responseCode: response.1, timeout: response.2)

// closing the root forces all children to be closed as well
Agent.getInstance().closeUseCase(span: root)
```

### Sampling Frequency Adjustment
The interval based metrics are sampled by default every 5 seconds. This sampling frequency can be adjusted by the developer regarding his needs. 
```swift
Agent.getInstance().changeTimerIntervall(seconds: 2)
```

By default 100 sampled values can be stored. The size of the buffer is constant and should be adjusted depending on frequency used and the duration of the use cases. 
