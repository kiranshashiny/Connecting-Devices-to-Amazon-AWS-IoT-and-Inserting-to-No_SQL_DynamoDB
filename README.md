# Connecting-Devices-to-Amazon's-AWS-IoT-MQTT-Broker-and-Inserting-data-to-NoSQL_DynamoDB
This blog is about how to connect a MQTT client to Amazon IoT, and saving data in Amazon's NoSQL DynamoDB

Here, I will describe how we can create a simple ready made AWS IoT node.js application that sends data from an application
to the Amazon AWS IoT console. This data is now going to be inserted to the Amazon's NoSQL Dynamo DB into their table.

This integration from Amazon IOT Core to Amazon Dynamo DB requires a few steps that is described here.

The data being used here is a simple counter that is generated on my laptop, and published using MQTT to Amazon IoT console ( or the Amazon's MQTT Broker) 

One can imagine this data can be anything from any sensor, like Room air pressure, humidity sensor, or tilt sensor, motion sensor published to the Amazon's IoT Console.

Since the focus of this blog is about integration from the Amazon's MQTT IOT Broker to the NoSQL Dynamo DB, I will defer the details of the sensor and just publish a simple integer counter.

In order to set up the device, follow instructions listed in this Developers Guide. This will configure the device, setup certificates, rules, policies etc.a

https://us-west-2.console.aws.amazon.com/iot/home?region=us-west-2#/connIntro

Steps:

Create a Free-Tier Amazon account that gives the user multiple applications to use for-free ( at least for the first year) and also limited usage for free after 1 year.

![screen shot 2018-01-28 at 2 06 51 pm](https://user-images.githubusercontent.com/14288989/35480476-cac3932a-0434-11e8-88e5-6fcc33d36631.png)

Create a device ->Download the connection Kit, Choose Platform ( and in this case, I have chosen Linux OS/X, and used Node.js AWS IoT device SDK ) and modify the code to send 
corresponding JSON code to the Amazon IoT Broker.
Download the aws sdk kit.

![screen shot 2018-01-27 at 3 38 41 pm](https://user-images.githubusercontent.com/14288989/35470954-84bc7660-0378-11e8-9770-fdbf80c660f1.png)
![screen shot 2018-01-27 at 3 39 10 pm](https://user-images.githubusercontent.com/14288989/35470953-84840352-0378-11e8-8da7-c5181e07ad79.png)
![screen shot 2018-01-27 at 3 39 32 pm](https://user-images.githubusercontent.com/14288989/35470952-844ca330-0378-11e8-9cd5-a818d415014c.png)
![screen shot 2018-01-27 at 3 39 45 pm](https://user-images.githubusercontent.com/14288989/35470951-841277a0-0378-11e8-90c4-c766a147107e.png)
![screen shot 2018-01-27 at 3 40 16 pm](https://user-images.githubusercontent.com/14288989/35470950-83d6be90-0378-11e8-8903-af9d7f31e8a0.png)

Make appropriate changes to code to publish just the number in JSON format. (This number down the road can be anything like CPU temperature or a device's parameters)

The default client publishes to topic called "topic_2".


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


Now run the code.
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

Test
Check that you are receiving messages.

Click on Test 

Subscribe to the topic topic_2

![screen shot 2018-01-27 at 3 43 39 pm](https://user-images.githubusercontent.com/14288989/35471039-0221dab8-037a-11e8-8442-c0be4f7c101d.png)

![screen shot 2018-01-27 at 3 50 05 pm](https://user-images.githubusercontent.com/14288989/35471038-01e8f914-037a-11e8-87ee-6a667e3f857f.png)

![screen shot 2018-01-27 at 3 50 18 pm](https://user-images.githubusercontent.com/14288989/35471037-01b0d1a6-037a-11e8-8e19-a95ed64d497f.png)

Now that you are receiving data:

Let's add the Database Rules to insert to Amazon Dynamo DB


## Amazon Dynamo DB configuration and setting up

Click on Act -> Create a Rule

Follow the official page for formal detailed explanation of setting up the database.

https://docs.aws.amazon.com/iot/latest/developerguide/iot-ddb-rule.html


Here are my snapshots.

![screen shot 2018-01-27 at 3 54 53 pm](https://user-images.githubusercontent.com/14288989/35471163-c34a8e8c-037b-11e8-8204-1d5e6854b206.png)
![screen shot 2018-01-27 at 3 55 09 pm](https://user-images.githubusercontent.com/14288989/35471162-c3121a8e-037b-11e8-9896-4f44d03137e0.png)
![screen shot 2018-01-27 at 3 55 22 pm](https://user-images.githubusercontent.com/14288989/35471161-c2d9acb2-037b-11e8-9020-da0ff266d28a.png)
![screen shot 2018-01-27 at 3 55 33 pm](https://user-images.githubusercontent.com/14288989/35471159-c2a25fc8-037b-11e8-9b71-8c35d0c81637.png)
![screen shot 2018-01-27 at 3 56 16 pm](https://user-images.githubusercontent.com/14288989/35471158-c268229a-037b-11e8-8833-13cdf2d9f491.png)
![screen shot 2018-01-27 at 3 56 29 pm](https://user-images.githubusercontent.com/14288989/35471157-c2307c3c-037b-11e8-857e-7a2a33c8f9e7.png)
![screen shot 2018-01-27 at 3 56 58 pm](https://user-images.githubusercontent.com/14288989/35471156-c1f9370e-037b-11e8-88c9-f17c5e656fd9.png)
![screen shot 2018-01-27 at 3 57 13 pm](https://user-images.githubusercontent.com/14288989/35471155-c1c17990-037b-11e8-9390-b61fab0d6e81.png)
![screen shot 2018-01-27 at 3 57 31 pm](https://user-images.githubusercontent.com/14288989/35471154-c188bb0a-037b-11e8-8cb4-34e735a27f80.png)
![screen shot 2018-01-27 at 4 02 41 pm](https://user-images.githubusercontent.com/14288989/35471153-c150510c-037b-11e8-9706-ff98102bf3e2.png)

Database tables setup:
This final configuration is important, I have set up the id and temperature as the primary key and the sort key respectively.


![screen shot 2018-01-28 at 1 58 49 pm](https://user-images.githubusercontent.com/14288989/35480413-71a5ceee-0433-11e8-95c5-21b6cb4673de.png)



This is the snapshot of my AWS Dynamo DB console showing the number( Just imagine it is some temperature from a sensor on a microcontroller) being sent from the client being inserted to Dynamo DB Tables.

![screen shot 2018-01-28 at 1 38 37 pm](https://user-images.githubusercontent.com/14288989/35480415-7439fee6-0433-11e8-9d0f-b931ab8ac454.png)


If you are going to try this - Enjoy building it, and Good Luck.
