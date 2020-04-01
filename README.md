# Mission Files 
### Version 1.0.0
## A mission file is a configuration for a robot controlling software , it defines the execution order and additional properties for example waypoints.
### The mission file is composed of a header, a body and a end.

  
### Header 
#### The Header contains some configuration for example the name.
#### The Header is always written in YAML.


   `HEADER`  
    `mission:true` This is a mission file
    `version:1.0.0` Version of the misson file
    `mf_version:1.0.0` Version of this readme definition
    `name: "Example"`      
    `author: "Example"`     
    `license: "none"`  
    `supported-devices:`    
    ` - 'example-robot'`    
    ` - 'another example robot'`    
    `type:waypoints`   
#### Example Files for each type are stored in the examples folder. 
    Types :     
      - waypoints | The Body Defines a list of waypoints with speed height longitude and latitude     
      - script | The Body defines a python script to execute
      - _plugin_name_/_body-type_    
      - ai | The Body defines a way to get input from a neural network and where to send the robots output
      - steps | The Body defines steps to execute for example `{"steps":[{"type":"fly","velocity":[0,0,0]}]}`
      - events-steps | advanced version of the steps parameter allows the device to react do events for example {"events":[{name:"battery-low",run:"example.steps"}]} 
#### Events that you should send if you are a developer of a robot controlling software that uses this file format
- battery-low
- collision-with-obj
- turned-on
- manual-off        
 
#### Optional Header objects are :
  - `ignorerrors:true` (default : false) The robot ignores all errors in execution
  - `simulate:true`(default : false)  The controlling software executes the mission file in a simulation not on a real device.
  - `devicecheck:false`(default : true) Ignores The supported-devices section of the Header
### Plugins
#### Plugins are defined in the Header the section is marked with a `PLUGINS` at the start.
    `PLUGINS`
    `example_plugin:1.0.0`
    `https://example.com/example_plugin.mfplugin`
### Body
#### The Body section is marked with a `BODY`.     
##### Type 1
    `BODY`     
    `url:example.com/example.steps` The Url where the real Body is stored.     
##### Type 2
    `BODY`    
    `{}`  
##### Type 3
    `BODY`
    `file:example.steps`
#### Here are some lines for your BODY that might be usefull (in waypoint files) :
    `{"type":"sleep","time":"4s"}`
### End
#### The end is a Json block that is executed after the body
### Error
#### The error section is a Yaml block that defines what to do if the connection is lost or something bad happens
  `connection-lost:"example.mission"`
#### Exceptions that you should send if you are a developer of a robot controlling software that uses this file format
   `connection-lost` The connection to the device was lost
   `collision` The device (for example a drone) collided with something.
   `manual-off` The device was turned of by a human
   `battery-off` The battery failed or is empty
## .step
### Step files contain only a Body and a Error section, they cant be run so they have to be called by a mission file in the body or end part.
`{"run":"example.com/example.steps"}` | `{"run":"example.steps"}`

