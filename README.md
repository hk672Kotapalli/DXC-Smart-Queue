# DXC-Smart-Queue
**Business Value:** Our goal is to make it safer to use any public facility. Rather than predicting and restricting, we incentivize people (with a gaming experience) to look out for each other.
 
**Technical Innovation:** [Algorithmic mechanism design](https://en.wikipedia.org/wiki/Algorithmic_mechanism_design) is an under-used technique. We used it to reward those who willingly social distance.

**Business implementation:** The application uses Microsoft Azure’s native cloud services and scale. The roadmap to production requires customizing the application to a specific set of trains and times, deploying the system, and testing it.

[![Watch the video](https://img.youtube.com/vi/__JuQAPp798/maxresdefault.jpg)](https://www.youtube.com/watch?v=__JuQAPp798&feature=youtu.be)

## Overview
People respond a lot better when you make them part of the solution, not the problem. The transit system is complex and keeping it both useful and safe is difficult. What if instead of imposing travel restrictions, you gave people incentives to look out for each other? [Algorithmic mechanism design](https://en.wikipedia.org/wiki/Algorithmic_mechanism_design) lets you align the interests of each person with the common good. We built this technique into a system that we call the Smart Queue. The Smart Queue monitors the complex transit network and offers each rider real-time rewards for willingly keeping a safe social distance.

Choose where you want to be and when you want to be there. The Smart Queue estimates the social responsibility of each option, assigns points, and shows you the results. Choose whichever option you like, but the Smart Queue will gently nudge if more responsible choices are available. The better your choices, the more reward points you earn. You can check on the rewards you’ve earned at any time. The application notifies you of upcoming reservations or when, despite your best efforts, you might be entering an overcrowded zone.


## The App Demo
[The app](https://github.com/dxc-technology/DXC-Smart-Queue/tree/master/Code/App/app_code) is written using the Flutter framework. We created two versions of the app. One runs natively on Android mobile devices, the other runs natively on iOS mobile devices.

### Install the App (Android)
From your Android device:
1. **Allow installation of third-party apps.** Go to Menu > Settings > Security > and check Unknown Sources to allow your phone to install apps from sources other than the Google Play Store
1. **Download the APK file.** From the browser, download the APK: https://link.APK
1. Go to the downloaded APK file, tap it, then hit Install

### Search for a queue
1. From the main screen select _Trains_ from the bottom navigation bar.
1. Tap _From_ text field. Search for and select _Darien_.
1. Tap _To_ text field. Search for and select _Grand Central_.
1. Select today's date and a time one hour in the future.
1. Tap _Search Trains_.

### Make reservations
1. Search for a queue
1. Select any available option
1. From the _details_ screen, select the _Book_ option

### Check reservations
1. From the main screen, tap _Bookings_ in the bottom navigation bar.

### Complete a reservation
1. Check reservations
1. Select a reservation
1. Tap _Complete Reservation_

### Cancel a reservation
1. Check reservations
1. Select a reservation
1. Tap _Cancel Reservation_

### Miss a reservation
Normally, the application will automatically mark your reservation as missed if you do not show up to the location at the scheduled time. The following instructions allow you to simulate this experience
1. Make a reservation
1. From the _hamburger menu_ select _simulate missed reservation_.

### Check your points
1. From the _main_ screen tap on _profile_
1. Select [option]

## The Design
[The app](https://github.com/dxc-technology/DXC-Smart-Queue/tree/master/Code/App/app_code) uses the Flutter framework to run natively on Android and iOS. The app communicates with the Smart Queue, which is implemented as Python/Django app running in Azure. The data pipeline pulls real-time information from the MTA and updates the Azure PostgreSQL Database—which feeds the Smart Queue. Each component is designed to scale separately using Azure's native cloud services.

![](Documentation/Technical/architecture-diagram.png)

### The Smart Queue
The [Smart Queue](https://github.com/dxc-technology/DXC-Smart-Queue/blob/master/Code/SmartQueue/smartqueue.py) is written in Python and deployed as a Django app running in a Microsoft Azure Web App. The Smart Queue reads queue schedules (every 15 minutes) from data lake and updates itself.
The Smart Queue monitors the occupants of each train, the capacity of each station, and the capacity of each queue. When in demo deployment, readings on the occupancy of each train is simulated. But when deployed in production, the Smart Queue can be plugged into live train occupant updates. The Smart Queue assigns points based on remaining occupant capacity. The lower the remaining capacity, the lower the points. The Smart Queue takes into account queues that overlap at different locations and queues that lose capacity because the arriving train is already full.


### The Data Lake
The Smart Queue reads [Queue Schedules]( https://github.com/dxc-technology/DXC-Smart-Queue/blob/master/Data/Schema_Resource_Locations_Queues.json) from the Data Lake. The Data Lake is a PostgreSQL database running in a Microsoft Azure for PostgreSQL Database cloud service. The Queue Schedule specifies waiting queues for each train and platform. This data is updated on a regular basis from a running data pipeline.

### The Data Pipeline
The [data pipeline](https://github.com/dxc-technology/DXC-Smart-Queue/tree/master/Code/Pipeline) is written in Python and deployed as a Django app running in a Microsoft Azure Web App. The data pipeline reads raw MTA data from [select MTA APIs]( https://github.com/dxc-technology/DXC-Smart-Queue/blob/master/Documentation/Technical/apiList_backend.txt), converts the raw data into a [queue schedule]( https://github.com/dxc-technology/DXC-Smart-Queue/blob/master/Data/Schema_Resource_Locations_Queues.json) and writes the queue schedule to the data lake. By default, the data pipeline runs every 15 minutes.

## Supporting Documents
Links to important documents
