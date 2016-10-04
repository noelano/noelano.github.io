---
layout:  post
title:  "Map of gmail archive in R"
date:  2016-10-04 15:20:42 +0100
categories:  Visualisation
---
This post gives an example of creating maps in R. 

The source data is going to be my gmail archive, not something that seems to be immediately relevant for a map-based project.
The idea is to take the origin IP adresses from each mail, convert to a geolocation and put together a plot showing where all the mails I've received have originated.

The starting point is to extract the features that are required from the email data. The archive is downloaded in a .mbox format and to use it in R this a .csv file will instead be generated. A simple python script does this

{% highlight python %}
import mailbox
import csv

mbox = mailbox.mbox('All mail Including Spam and Trash.mbox')

writer = csv.writer(open("Mail_Archive.csv", "w"))
writer.writerow(["Subject", "From", "Date", "Labels", "Importance", "Received", "Thread-Topic", "Has-Attach"])

for message in mbox:
	try:
		writer.writerow([message['subject'], message['from'], message['date'], message['X-Gmail-Labels'],\
		message['Importance'], message['Received-SPF'], message['Thread-Topic'], message['X-MS-Has-Attach']])
	except (UnicodeDecodeError, UnicodeEncodeError):
		print('Unicode error!')
{% endhighlight %}


The next step then was to load into R and extract the IP addresses from this data.

{% highlight r %}
dat1 <- data.frame(t(matrix(
+     unlist(strsplit(as.vector(raw$Received), split = "client-ip=")), 
+     ncol = length(raw$Received), nrow = 2)))

write.csv(file='ip_addresses.csv', x=dat1$X2)
{% endhighlight %}

A python script obtains addresses from this file by querying 

Next step, start creating the map! First, we want to get a count on the occurrence of each location

{% highlight r %}
counts <- count(locations)

world <- map_data("world")
p <- ggplot()
p <- p + geom_polygon(data = world, aes(x=long, y=lat, group = group), color = "steelblue4", fill = "royalblue4") + 
	theme(panel.background = element_rect(color = "midnightblue", fill = "midnightblue")) + theme(line = element_blank(), 
	axis.title = element_blank(), axis.ticks = element_blank(), 
	axis.text = element_blank())

p <- p + geom_point(data = counts, aes(x=Longitude, y=Latitude, size = freq), color = "red2")
{% endhighlight %}

To zoom in on the relevant map location:

{% highlight r %}
p <- p + coord_cartesian(xlim = c(-150, 50), ylim = c(-40, 80))
{% endhighlight %}

To give an illustration of the paths, lines will be added from each location to the recipients (ie me) locations (Dublin)

{% highlight r %}
for(i in 1:nrow(counts)){
	p <- p + geom_line(data = counts[c(16,i),], aes(x=Longitude, y=Latitude, scale = 0.2, alpha = 0.5), color = "lightgoldenrod2")
} + theme(legend.position = "none")
{% endhighlight %}

All that's left is to add the title and voila, the map is complete!

{% highlight r %}
p <- p + annotate("text", label="Incoming Mails", x = -100, y = -30, color = "white", size = 8
{% endhighlight %}
![R map](/images/map_v2.png)
