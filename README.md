# README #

To connect to Telco via SMPP (sending & receiving SMS) we will use kannel (kannel.org) this is an opensource SMPP gateway used widely in SMS connectivity. 

How it works
1. We create kannel image (this image download kannel and configures then exposes two services i.e bearer & smsbox)
2. Create a configuration file that defines all the connectivity details including Telco IP, smpp username & smpp password
3. Deploy the image

- This creates 2 services, bearerbox and smsbox, bearerbox needs to connect to SMSbox which connects to the telco.  
- Bearerbox exposes several API endpoints listed below

1. health check and status endpoint (GET /status)
2. Send SMS endpoint (GET) - We shall use this endpoint to send SMS to the connected telco
