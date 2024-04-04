# View defender for cloud alerts in Event Hub

## In Azure portal

Navigate to Event Hubs instance > Process data <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/c5fc2fb5-4b88-4384-a802-56b6ba5332f4)

Click `start` in `Process your Event Hub data using Stream Analytics Query Language` <br>

Click Create here <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/c537c4ef-f1b9-4d05-bcab-3b9edc18ecf7)


Run the query here <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/109766bb-3c48-4385-91a3-af7210b72082)


You can review the events here <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/f54814a6-9661-4de7-84c2-dfabf5de37ba)


Sample defender for cloud alerts <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/b43dcbe2-5d91-4508-8de8-fe966466c318)


## Use [Releases · paolosalvatori/ServiceBusExplorer (github.com)](https://github.com/paolosalvatori/ServiceBusExplorer)

Navigate to `Event Hubs namespace > Shared acess policies` <br>

Click `RootManageSharedAccessPolicy`, copy `Connection string-primary key` to clipboard, we need it later. <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/71db03a6-c32f-4102-82a2-e103f8536604)


Download `ServiceBusExplorer` from [Release 5.0.17 · paolosalvatori/ServiceBusExplorer (github.com)](https://github.com/paolosalvatori/ServiceBusExplorer/releases/tag/5.0.17)

Launch `ServiceBusExplorer`, click `File > connect` <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/5bca279c-3e85-4795-82b3-b39f2c33d52d)


Paste `Connection string-primary key` in `Connection settings` <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/63410538-437a-44ad-b33d-50108a2815e8)


Right click `Default` consumer group > create consumer group listener <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/abbcb9bb-9b9a-4dfc-b5c2-09d194e2386e)


Here you can set the time range and start scanning the events <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/366a0e97-45dc-43c7-afef-0f497eaff63a)

![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/6522054b-5100-487d-98f2-99686c84b9d7)

We can then find the events (defender for cloud alerts) here <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/a8c77ff9-d76a-4ed5-b42f-23b6b1a4a48b)

We can also save all events <br>
![image](https://github.com/guguji666666/GJS-MDC-Tips/assets/96930989/9641f482-f04c-4dca-abf5-a42a9939e53a)


