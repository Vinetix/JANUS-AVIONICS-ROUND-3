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
