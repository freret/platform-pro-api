# Platform Pro API
Data delivery for the Platform Pro iOS app. Processes MTA protobuf messages and returns them as JSON objects.

## Big thanks
to [Jon Thornton](https://github.com/jonthornton) and his [MTAPI repo](https://github.com/jonthornton/MTAPI) on which all of this work is based.

I made assorted changes to the flask configuration and JSON output formats. The biggest difference between this repo and Jon's is the support for Service Alerts; I merged the MTA Mercury protobuf schema with the legacy MTA schema to support both realtime arrivals and alerts from the same server. I've left the `.proto` files here in case you need to recompile. Full disclosure: these schemas do not play nicely together out of the box, and will almost certainly require changes before successfully compiling

## Local setup

MTAPI is a Flask app designed to run under Python 3.3+.

1. Create a `settings.cfg` file with newline-separated `<key>=<value>` pairs as described in [settings](#settings)
2. Set up your environment and install dependencies.  
`$ python3 -m venv .venv`  
`$ source .venv/bin/activate`  
`$ python3 -m pip install -r requirements.txt`
3. Run the server  
`$ python app.py`

If your configuration is named something other than `settings.cfg`, set the `MTAPI_SETTINGS` env variable to your configuration path.

This app makes use of Python threads. If running under uWSGI include the --enable-threads flag.

## Making protobuf changes
is a huge pain in the ass. FWIW, I used `protobuf==3.19.3` and `libprotoc==3.21.9` to compile the `.proto` files. Best of luck.

## Settings

- **MTA_KEY** (required)  
The API key provided at hhttps://api.mta.info/#/signup
*default: None*

- **STATIONS_FILE** (required)  
Path to the JSON file containing station information.  
*default: None*

- **CROSS_ORIGIN**    
Add [CORS](http://enable-cors.org/) headers to the HTTP output.  
*default: "&#42;" when in debug mode, None otherwise*

- **MAX_TRAINS**  
Limits the number of trains that will be listed for each station.  
*default: 10*

- **MAX_MINUTES**  
Limits how far in advance train information will be listed.  
*default: 30*

- **CACHE_SECONDS**  
How frequently the app will request fresh data from the MTA API.  
*default: 60*

- **THREADED**  
Enable background data refresh. This will prevent requests from hanging while new data is retreived from the MTA API.  
*default: True*

- **DEBUG**  
Standard Flask option. Will enabled enhanced logging and wildcard CORS headers.  
*default: False*

## License

Available under the [MIT License](https://opensource.org/license/mit/)