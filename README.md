QUESTION 1-
Core Logic:-

   Read GPS NMEA sentence

   Parse relevant fields (time, lat, lon, alt)

   Convert latitude/longitude to decimal degrees

   Update flight state machine based on altitude

   Print telemetry (time, position, altitude, state)


1.) GPS Data Reading & Parsing:

     Input: NMEA sentences from GPS ($GNGGA and $GNRMC)

     Functionality:
        $GNGGA: extracts UTC time, latitude, longitude, altitude
 	$GNRMC: extracts UTC time, latitude, longitude

    Latitude/Longitude Conversion:
	NMEA format ddmm.mmmm → decimal degrees
	Handles hemisphere (N/S/E/W)

    Example:
	2836.8093,N → 28 + 36.8093/60 ≈ 28.6135° N
	07712.5418,E → 77 + 12.5418/60 ≈ 77.2090° E

    Time Conversion: UTC → IST (+5:30)

2.)Flight State Machine:

	State           	Trigger / Condition
	a.)IDLE	        	Waiting on ground; transitions to ASCENT if altitude > 5 m
	b.)ASCENT		CanSat is climbing; updates maxAltitude
	c.)APOGEE		Detected when altitude decreases after maximum; transitions to DESCENT
	d.)DESCENT	        CanSat descending; triggers PAYLOAD_DEPLOYED if altitude < 75% of max
	e.)PAYLOAD_DEPLOYED	Payload released; eventually transitions to LANDED
	f.)LANDED		Altitude < 5 m; final state

3.) Telemetry Output:
       ex - Time (IST): 05:31:15 | Lat: 28.613488 | Lon: 77.209030 | Alt: 1030.00 m | State: ASCENT

Question 2
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
