# Connecting-Devices-to-Amazon-IoT-MQTT-Broker-and-NoSQL_DynamoDB
This blog is about how to connect a MQTT client to Amazon IoT, and saving data in Amazon's NoSQL DynamoDB

Here, I will describe how we can create a simple ready made AWS IoT node.js application that sends data from an application
to the Amazon AWS IoT console. This data is now going to be inserted to the Amazon's NoSQL Dynamo DB into their table.

This integration from Amazon IOT Core to Amazon Dynamo DB requires a few steps that is described here.

The data being used here is a simple counter that is generated on my laptop, and published using MQTT to Amazon IoT console ( or the Amazon's MQTT Broker) 

One can imagine this data can be anything from any sensor, like Room air pressure, humidity sensor, or tilt sensor, motion sensor published to the Amazon's IoT Console.

Since the focus of this blog is about integration from the Amazon's MQTT IOT Broker to the NoSQL Dynamo DB, I will defer the details of the sensor and just publish a simple integer counter.

Steps:

Create a Free-Tier Amazon account that gives the user multiple applications to use for-free ( at least for the first year) and also limited usage for free after 1 year.

Create a device ->Download the connection Kit, Choose Platform ( and in this case, I have chosen Linux OS/X, and used Node.js AWS IoT device SDK ) and modify the code to send 
corresponding JSON code to the Amazon IoT Broker.
Download the aws sdk kit.


Make approrpriate changes to code to publish just the number in JSON format. (This number down the road can be anything like CPU temperature or a device's parameters)

		./start.sh 
		
		Running pub/sub sample application...
		Connecting in Code
		connect
		Publishing topic_2 to Amazon IoT Broker an Incremented Number : 1
		{"mode1Process":1,"id":1,"temperature":1,"serialnumber":1}
		Publishing topic_2 to Amazon IoT Broker an Incremented Number : 2
		{"mode1Process":2,"id":2,"temperature":2,"serialnumber":2}
		Publishing topic_2 to Amazon IoT Broker an Incremented Number : 3
		{"mode1Process":3,"id":3,"temperature":3,"serialnumber":3}
		Publishing topic_2 to Amazon IoT Broker an Incremented Number : 4
		{"mode1Process":4,"id":4,"temperature":4,"serialnumber":4}


Modified Code in node_modules/aws-iot-device-sdk/examples/device-example.js

      if (args.testMode === 1) {
        console.log ("Publishing topic_2 to Amazon IoT Broker an Incremented Number : " + count);
         device.publish('topic_2', JSON.stringify({
            mode1Process: count,
            id: count,
            temperature: count,  // This is what we are interested in.
            serialnumber : count
         }));
        // print it to console so we know what is being published.
        console.log (JSON.stringify({
            mode1Process: count,
            id: count,
            temperature: count,
            serialnumber : count
         }));
      } else {
         device.publish('topic_1', JSON.stringify({
         mode2Process: count
         }));
      }
   }, Math.max(args.delay, minimumDelay)); // clip to minimum


![screen shot 2018-01-27 at 1 11 32 pm](https://user-images.githubusercontent.com/14288989/35469953-e123b782-0365-11e8-9ed2-8765d4c067c8.png)
![screen shot 2018-01-27 at 1 24 06 pm](https://user-images.githubusercontent.com/14288989/35469948-e00eae24-0365-11e8-8738-06c30c325b1c.png)
![screen shot 2018-01-27 at 1 13 21 pm](https://user-images.githubusercontent.com/14288989/35469949-e046c142-0365-11e8-83e1-428ed62cee17.png)
![screen shot 2018-01-27 at 1 12 54 pm](https://user-images.githubusercontent.com/14288989/35469950-e07e1d04-0365-11e8-818b-639af85dc470.png)
![screen shot 2018-01-27 at 1 12 21 pm](https://user-images.githubusercontent.com/14288989/35469951-e0b4fad6-0365-11e8-83b6-d0b3fcd2191b.png)
![screen shot 2018-01-27 at 1 12 00 pm](https://user-images.githubusercontent.com/14288989/35469952-e0ec2a06-0365-11e8-9bb2-4d7b42bda757.png)


--
