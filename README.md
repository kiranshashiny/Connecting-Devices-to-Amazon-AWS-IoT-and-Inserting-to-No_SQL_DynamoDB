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


![screen shot 2018-01-27 at 3 38 23 pm](https://user-images.githubusercontent.com/14288989/35470956-852ec15c-0378-11e8-8172-a34db2dba6fd.png)
![screen shot 2018-01-27 at 3 38 29 pm](https://user-images.githubusercontent.com/14288989/35470955-84f45bc0-0378-11e8-9809-d6f774ddb7fb.png)
![screen shot 2018-01-27 at 3 38 41 pm](https://user-images.githubusercontent.com/14288989/35470954-84bc7660-0378-11e8-9770-fdbf80c660f1.png)
![screen shot 2018-01-27 at 3 39 10 pm](https://user-images.githubusercontent.com/14288989/35470953-84840352-0378-11e8-8da7-c5181e07ad79.png)
![screen shot 2018-01-27 at 3 39 32 pm](https://user-images.githubusercontent.com/14288989/35470952-844ca330-0378-11e8-9cd5-a818d415014c.png)
![screen shot 2018-01-27 at 3 39 45 pm](https://user-images.githubusercontent.com/14288989/35470951-841277a0-0378-11e8-90c4-c766a147107e.png)
![screen shot 2018-01-27 at 3 40 16 pm](https://user-images.githubusercontent.com/14288989/35470950-83d6be90-0378-11e8-8903-af9d7f31e8a0.png)


Test
Check that you are receiving messages.


