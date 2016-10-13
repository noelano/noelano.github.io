---
layout:  post
title:  "Building a concept lattice in python"
date:  2016-10-13 20:00:00 +0100
categories:  Coding
---
This post gives an example of creating maps in R. The full code can be found on [Github][gh_link].

The source data is going to be my gmail archive, not something that seems to be immediately relevant for a map-based project.
The idea is to take the origin IP address from each mail, convert to a geolocation and put together a plot showing where all the mails I've received have originated.

### Formal Concept Analysis Background
\\
The starting point is to extract the features that are required from the email data. The archive is downloaded in a .mbox format and to use it in R, a .csv file will instead be generated. A simple python script does this:

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

This is simply loading the mbox object (Thanks to the mailbox library) which is a list of dictionary items. Iterating over this list, the relevant fields can be extracted and written to the csv file.

The next step then was to load into R and extract the IP addresses from this data.

Another detour into python takes the ip addresses and returns the geolocations.

### Creating the map
\\
Next step, start creating the map! First, we want to get a count on the occurrence of each location and set up the map background with ggplot.


[gh_link]: //github.com/noelano/R_Visualisations
