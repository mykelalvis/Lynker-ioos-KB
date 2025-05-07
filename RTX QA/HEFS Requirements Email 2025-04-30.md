---
aliases: 
date created: 2025-05-01T07:50:35.000
date modified: 2025-05-01T09:39:06.879
tags: 
---

Guys,

  We need to figure out what is going on here and why we are not getting consistent data. Can we please get you both on this work with WRDS? What is the magic about all the data being 7 days old?   
  

---------- Forwarded message ---------  
From: **Gautam Sood - NOAA Federal** <[gautam.sood@noaa.gov](mailto:gautam.sood@noaa.gov)>  
Date: Wed, Apr 30, 2025 at 2:22 PM  
Subject: HEFS requirements  
To: David Moring - NOAA Affiliate <[david.moring@noaa.gov](mailto:david.moring@noaa.gov)>, Noah Karpinski - NOAA Affiliate <[noah.karpinski@noaa.gov](mailto:noah.karpinski@noaa.gov)>  
Cc: Fernando Salas - NOAA Federal <[Fernando.Salas@noaa.gov](mailto:Fernando.Salas@noaa.gov)>, Jason Whitehead - NOAA Affiliate <[jason.whitehead@noaa.gov](mailto:jason.whitehead@noaa.gov)>, Derrica Campbell - NOAA Affiliate <[derrica.campbell@noaa.gov](mailto:derrica.campbell@noaa.gov)>  

  
  

Good afternoon,

  

With our continued data availability issue with the HEFS API, I'd like to stop all work on the other HEFS requirements (last ensemble, quantiles) and shift 100% focus on resolving this issue.  

  

I've gone through the HEFS bucket and have compiled a full list of all location_ids we should have data for.  I've also taking the full list of official HEFS sites and wrote a python program that ran a query for each one and grabbed the date of the last forecast we have in the API (see below).  It seems the newest forecast we have in the API is April 23rd (7 days old).  I've also confirmed that all RFC's data feeds are up to date (I see data from April 29/30 in the bucket)

  

At the minimum, we need to have the data feed/ingest fully working for the full list of official HEFS sites (3111 locations).  My program only queried for 2295 locations (ran out of time before I could finish it) but I'll finish it up in the morning.  I can share my program once I complete it.  It essentially queries the service for all the locations within 1.5-2 minutes

  

The full list of locations I'm using as a reference is this spreadsheet I've shared previously:

[https://docs.google.com/spreadsheets/d/1-zSWzXV-vQrc-oRUf0gFETIefYyw-wOgfbTPOvE_HjA/edit?usp=sharing](https://docs.google.com/spreadsheets/d/1-zSWzXV-vQrc-oRUf0gFETIefYyw-wOgfbTPOvE_HjA/edit?usp=sharing)

  

thanks!

Gautam

---

[[David Moring]]'s reply

Team,  
  
How this started: the original requirements stated that the RFCs would submit properly valid, structured and standardized XML data to the API via the REST interface.  
What happened:  
1. We built a small standing script as a bridge between S3 submission and RFC XML submission via REST, relying on the original requirements.  
2. RFCs submitted data that varied from the original requirements in structure and content.  
3. We responded by expanding the script to accommodate those variances -- that script is now over 740 lines -- and we still have missing data  
  
The Fix:  
1.  We expand the original requirements to include the ELT functions in loading the data from the RFCs  
2.  We do this in a planned and structured manner, not in the break/fix mode that we have been doing in the past.  
  
Meeting these additional requirements will take time, all that can be said at this point is that it will be days, not hours.   
  
Here is the current development plan:  
Refactoring the 740+ line ingest script as follows:  
4. Breaking the ingest pipeline into discrete stages  
5. Adding Input/Output data validation of each stage  
6. Implement detailed structured and leveled logging  
7. Implement a quarantine mechanism  
  
I will publish the schedule as soon as the technical and data requirements are better understood.

---

Also from [[David Moring]]
To receive HEFS Loader code status:  
go to VLAB: My Account -> Email notifications-> select "For any event on the selected projects only.."  
click HEFS API project.  
  
You will be notified of progress on code commits.