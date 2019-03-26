# Spaghetti API

This API is built on [Flask](http://flask.pocoo.org/) and connects to a mongoDB storage attached to this app. The purpose is receiving HTML POST messages from Actility every time a LoRaWAN packet has been received. Spaghetti API stores all the relevant data and prepares them to be fetched with other apps connecting with REST API.

## Run locally  

1. Install [Python](http://docs.python-guide.org/en/latest/starting/installation/)
1. Install Setuptools and pip (see guide above)
1. Install Virtualenv (acconplish this by running `pip install virtualenv`)
1. Run `virtualenv venv`
1. Run `source venv/bin/activate` on Mac OS X/Linux or`venv\Scripts\activate.bat` on windows
1. Run `pip install -r requirements.txt`
1. Run `python app.py`
1. Visit [http://localhost:3000](http://localhost:3000)

## Run in the cloud  

1. Install the [cf CLI](https://github.com/cloudfoundry/cli#downloads)
1. Run `cf push my-python-app -m 128M --random-route`
1. Visit the given URL


##Description of the API  
###Data input  
The app route `/sc_lpn` handles the data input from Actility Thingpark with HTTP POST in the format given by Actility.

App route `/gateway` can handle the input of new gateways to be stored in the database, when used with HTTP POST.  
Arguments:  
`id`: gateway EUI as a string  
`lat`: latitude of the gateway  
`lon`: longitude of the gateway

App route `/import` can import from a backup point-by-point. All the point information has to be given as arguments. The function is not tested yet, for this purpose, a script needs to be written that transforms the .json backup into a series of POST requests.


##Data output  

App route `/gateway` with HTTP GET returns all the gateways within a certain radius around the center point.
Arguments:  
`lat`: latitude of the center point  
`lon`: longitude of the center point  
`radius`: radius in km, output will contain all gtws within this radius.  

App route `/json` returns a .json file for download, containing the current point database. Might take some CPU...

App route `/query` is filtering the database for certain criteria and only returns the points of interest.  
Arguments:  
`delpoint`+timestamp deletes a specific point  
`track`: returns only a specific track number. Default: 20  
`start`: String with start time. Default: 1 year ago  
`end`: String with end time. Default: now  
`hdop`: maximum GPS HDOP. Default: 500  
`sf`: Spreading factor. Default: 7  
`txpow`: Transmission power. Default: 0  
`device`: Filter for transmissions of a specific device EUI only. Default: All devices  

The additional app routes `/freeboard/devices` and `/freeboard/dbmonitor`were created to push statistics to a [Freeboard](freeboard.io).
  

