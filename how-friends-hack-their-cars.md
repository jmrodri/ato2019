# How Friends Hack Cars, and Stuff
#### Lori Wachter, Advance Auto Parts
#### Devin Shackle, Dragos
#### Garrett Bladow, Dragos

Data science at advance auto parts

Devon Shackle and Lori? And two other folks on 

Obd ii

Obd originated in the sixties 

These codes are DTCS. They are accessible through a port on the drivers side under the dash. 

Obd hacking first attempt 

Car Porsche? If you can get one 
Obd cable to raspberry pi to
Pyobd to jupyter to Notebook to data  (to is - >) 


Request all the commands the vehicle supports 
For c in connection.suppoerted_commands

Huge amounts of data in the car 

Garrett from dragos. Director of telemetry 

Raspberry pi to inter tubes to s3 buckets to elastic search to kibana 

Showed real time hrv cam bus 

Lambda function parses out the data and normalizes it. Then pumps it to elasticseaech 

Then kibana gauges 


Blake is in a Camaro outside. Data is federally mandated but it can vary wildly from one manufacturer  to another 

Q&a

How do you make sense of the data? 
Pyodb does a lot of the help in translating 

Can you write back to the car? 
Clearing codes. It's a rabbit hole going back. 

Whats the latency like? 
They used quarter second., can be lower if you want to do something specific. But quarter second is the lowest for getting all the data all the time 

Is there any security? 
Just physical access 

Is it possible to ddos the ecu? 
We don't know. 

News to reverse engineer the manufacturr protocols 


