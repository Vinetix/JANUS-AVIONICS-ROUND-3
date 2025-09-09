FEATURES:-

  Connect/Disconnect Arduino via browser using Web Serial API.
  Live 2D sensor plot & 3D gps plot 
  CSV Data Format: expects sensor,lat,lon,alt (from Arduino)

HOW IT WORKS:-

  Initialize Plots:-
  	2D plot for sensor data (lines + markers)
  	3D scatter plot for GPS coordinates (x=lon, y=lat, z=alt)

  Connect to Arduino:- 
  	User selects serial port
  	Browser opens the port at baud rate 9600
  	Incoming data is decoded via TextDecoderStream

  Process Incoming data & Plot Graphs:-
  	Data is read line by line in CSV format
  	Values are appended to arrays: sensorData, latData, lonData, altData
        Plots are updated in real-time using Plotly.update()
  Disconnect:-
       Stops reading loop
  	Closes serial port safely

USAGE INSTRUCTIONS:-
Arduino Setup:-
	Upload code that sends sensor + GPS data in CSV format:-
		sensor_value,latitude,longitude,altitude
        Baud rate: 9600

Open HTML file in Chrome

Connect & Visualize
	Click Connect to Arduino → select port
	Live sensor and GPS data will appear on the plots

Click Disconnect to stop reading