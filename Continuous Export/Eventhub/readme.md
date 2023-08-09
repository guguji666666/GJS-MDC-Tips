# View MDFC alerts in event hub

## In Azure portal

Navigate to Event Hubs instance > Process data <br>![image-20230808162358800](https://guguimage.aceultraman.com/i/2023/08/08/quncfs-1.png)

Click `start` in `Process your Event Hub data using Stream Analytics Query Language` <br>

Click Create here <br>

![image-20230808162648112](https://guguimage.aceultraman.com/i/2023/08/08/qwd6tl-1.png)

Run the query here <br>

![image-20230808162721408](https://guguimage.aceultraman.com/i/2023/08/08/qwsug5-1.png)

You can review the events here <br>

![image-20230808162802547](https://guguimage.aceultraman.com/i/2023/08/08/qxa5rf-1.png)

Sample defender for cloud alerts <br>

![image-20230808162837827](https://guguimage.aceultraman.com/i/2023/08/08/qxhx27-1.png)

## Use [Releases · paolosalvatori/ServiceBusExplorer (github.com)](https://github.com/paolosalvatori/ServiceBusExplorer)

Navigate to `Event Hubs namespace > Shared acess policies` <br>

Click `RootManageSharedAccessPolicy`, copy `Connection string-primary key` to clipboard, we need it later.

![image-20230808163035897](https://guguimage.aceultraman.com/i/2023/08/08/qyo37t-1.png)

Download `ServiceBusExplorer` from [Release 5.0.17 · paolosalvatori/ServiceBusExplorer (github.com)](https://github.com/paolosalvatori/ServiceBusExplorer/releases/tag/5.0.17)

Launch `ServiceBusExplorer`, click `File > connect` <br>

![image-20230808163454966](https://guguimage.aceultraman.com/i/2023/08/08/r161wl-1.png)

Paste `Connection string-primary key` in `Connection settings`

![image-20230808163156057](https://guguimage.aceultraman.com/i/2023/08/08/qze3x3-1.png)

Right click `Default` consumer group > create consumer group listener

![image-20230808163630568](https://guguimage.aceultraman.com/i/2023/08/08/r27s4s-1.png)

Here you can set the time range and start scanning the events <br>

![image-20230808163902899](https://guguimage.aceultraman.com/i/2023/08/08/r3u1s2-1.png)

![image-20230808163940216](https://guguimage.aceultraman.com/i/2023/08/08/r428l1-1.png)

We can then find the events (defender for cloud alerts) here <br>

![image-20230808180147076](https://guguimage.aceultraman.com/i/2023/08/08/tsjy9k-1.png)
