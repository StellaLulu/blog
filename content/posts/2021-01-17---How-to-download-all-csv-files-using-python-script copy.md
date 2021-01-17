---
title: How to download all .csv files using python script
date: "2021-01-17T22:40:32.169Z"
template: "post"
draft: false
slug: "How-to-download-all-csv-files-using-python-script"
category: "Python"
tags:
  - "Web Crawling"
  - "Project"
description: "An Essay on Typography by Eric Gill takes the reader back to the year 1930. The year when a conflict between two worlds came to its term. The machines of the industrial world finally took over the handicrafts."
socialImage: "/media/blog/2021-01-17_01.png"
---

![](/media/blog/2021-01-17_01.png)

### Motivation

To download the Historical Daily Records from Meteorological Service Singapore

```jsx
import requests, os, bs4

mm = ["%.2d" % i for i in range(13)]  #['00', '01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12']

for i in mm:
    url ="http://www.weather.gov.sg/files/dailydata/DAILYDATA_S91_2020{}.csv".format(i)
    print(url)
```

```jsx
http://www.weather.gov.sg/files/dailydata/DAILYDATA_S91_202000.csv
http://www.weather.gov.sg/files/dailydata/DAILYDATA_S91_202001.csv
```

```jsx
import os
import requests
import re

def download_file():
    ''' Downloads file from the url and save it as filename '''
    mm = ["%.2d" % i for i in range(13)]  #['00', '01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12']
    year = ["%.2d" % i for i in range(2000,2021)]  #['00', '01', '02', '03', '04', '05', '06', '07', '08', '09', '10', '11', '12']
    print(year)
    
    
    parent_dir = os.getcwd() # Parent Directory path
    directory = 'yishun_weather_2000_2020' # Directory 
    path = os.path.join(parent_dir, directory)

    try:
        os.mkdir(path)
    except OSError:
        print ("The folder exists %s" % path)
    else:
        print ("Successfully created the directory %s " % path)
    
    for y in year:
        for i in mm:
            url ="http://www.weather.gov.sg/files/dailydata/DAILYDATA_S91_{0}{1}.csv".format(y,i)
            # print(url) http://www.weather.gov.sg/files/dailydata/DAILYDATA_S91_202000.csv

            filename = (re.search("([A-Z])\w+.csv", url)).group(0)

            # check if file already exists
            if not os.path.isfile(path + "/" + filename):
                response = requests.get(url)
                print('Downloaded file {}'.format(filename))
                # Check if the response is ok (200)
                if response.status_code == 200:
                    # Open file and write the content
                    with open(path + "/" + filename, 'wb') as file:
                        # A chunk of 128 bytes
                        for chunk in response:
                            file.write(chunk)

            else:
                print('File exists {}'.format(filename))

download_file()
```