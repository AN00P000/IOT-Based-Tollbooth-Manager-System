* Vehicle arriving: when a vehicle approaches the toll booth, its RFID tag is 
detected by the RFID reader
 * Data processing : Arduino processes the RFID data, verifies the vehicle’s identity, and 
calculates the toll amount based on predefined rates.
 * Transaction Processing : Upon successful verification, the toll amount is deducted from 
the vehicle owner’s account or collected in the cash.
* Gate controller : Arduino commands the toll gate barrier to open, allowing the vehicles to 
pass through.
* Data Transmission : Transaction details, including vehicle ID , toll amount, and timestamp, 
are sent to the centralised server for record-keeping and analysis.
* Remote Monitoring : Administrators can access real-time data through the web interface, 
monitor traffic flow, revenue, and system status, and generate reports for analysis and 
decision-making.
