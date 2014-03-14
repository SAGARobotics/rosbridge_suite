[![Build Status](https://api.travis-ci.org/RobotWebTools/rosbridge_suite.png)](https://travis-ci.org/RobotWebTools/rosbridge_suite)

Rosbridge provides a JSON interface to ROS, allowing any client to send JSON to
publish or subscribe to ROS topics, call ROS services, and more. Rosbridge
supports a variety of transport layers, including WebSockets.

### Packages

 * rosbridge_suite is a [ROS
   meta-package](http://www.ros.org/wiki/catkin/conceptual_overview#Metapackages_and_the_Elimination_of_Stacks),
   simply including all the rosbridge packages.

 * rosbridge_library contains the Python API that receives JSON-formatted
   strings as input and controls ROS publishers/subscribers/service calls
   according to the content of the JSON strings.

 * rosbridge_server contains a WebSocket server implementation that exposes the
   rosbridge_library.

 * rosapi provides service calls for getting meta-information related to ROS
   like topic lists as well as interacting with the Parameter Server.

### Clients

A rosbridge client is a program that communicates with rosbridge using its JSON
API. Rosbridge clients include:

 * [roslibjs](https://github.com/robotwebtools/roslibjs) - A JavaScript API, which
   communicates with rosbridge over WebSockets.

### Resources

 * [Documentation](http://www.ros.org/wiki/rosbridge_suite)
 * [Rosbridge Protocol Specification](ROSBRIDGE_PROTOCOL.md)
 * [Coding Style Guide](http://www.ros.org/wiki/PyStyleGuide)
 * [Contributors](https://github.com/RobotWebTools/rosbridge_suite/graphs/contributors)
 * [License (BSD)](http://opensource.org/licenses/BSD-2-Clause)

### Restricted rosbridge for unsafe web environments
This branch contains a groovy-devel based version of the rosbridge suite that has some safety features implemented that check that as respective action (subcription, publish, call service) is actually allowed before it is executed. This prevents access to topics that shouldn't be accessed (e.g. image stream for privacy concerns). This is implemented via 'rosparam' parameter _whitelists_ and _blacklists_. 
If all whitelists and blacklists are empty (the default), then the behaviour of the bridge is the same as before, i.e. everything is "bridged". But if a whitelist is not empty, then only topics/services named in the list will be "bridged" all others requests are ignored by this rosbridge implementation. If a blacklist is not empty, then all topics/services listed are explicitly ignored (only makes sense, when whitelist is empty).

The following lists exist and can be configured:
* `advertisements_whitelist`: e.g. set to `[/tf2_web_republisher/goal, /tf2_web_republisher/cancel]`, default `[]`
* `advertisements_blacklist`: default `[]`
* `service_whitelist`: e.g. `[/rosapi/get_param]`, default `[]`
* `service_blacklist`: default `[]`
* `subscription_whitelist`: e.g. `[/amcl_pose, /mileage, /tf2_web_republisher/status, /tf2_web_republisher/feedback, /tf2_web_republisher/result, /map]`, default `[]`
* `subscription_blacklist`: default `[]`

Here's an exemplary YAML file corresponding to the above:
```
advertisements_whitelist: [/tf2_web_republisher/goal, /tf2_web_republisher/cancel]
service_whitelist: [/rosapi/get_param]
subscription_whitelist: [/amcl_pose, /mileage, /tf2_web_republisher/status, /tf2_web_republisher/feedback, /tf2_web_republisher/result, /map]
```

See https://github.com/strands-project/rosbridge_suite/blob/readonly_capabilities/rosbridge_server/launch/rosbridge_websocket_safe.launch for an example launch file that sets the parameters.

This is part of the [STRANDS project](http://www.strands-project.eu/)
