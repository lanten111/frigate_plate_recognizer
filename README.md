# Frigate Classifier

Identify license plates via [Plate Recognizer](https://guides.platerecognizer.com/) and adds them as sublabels to [blakeblackshear/frigate](https://github.com/blakeblackshear/frigate)

### Setup

Create a `config.yml` file in your docker volume with the following contents:

```yml
frigate:
  frigate_url: http://127.0.0.1:5000
  mqtt_server: 127.0.0.1
  mqtt_auth: false
  mqtt_username: username
  mqtt_password: password
  main_topic: frigate
  camera:
    - driveway_camera
plate_recognizer:
  api_key: xxxxxxxxxx
  regions: 
    - us-ca
logger_level: INFO
```

Update your frigate url, mqtt server settings. If you are using mqtt authentication, update the username and password. Update the camera name(s) to match the camera name in your frigate config. Add your Plate Recognizer API key and region(s).

You'll need to make an account (free) [here](https://app.platerecognizer.com/accounts) and get an API key. You get up to 2,500 lookups per month for free.


### Running

```bash
docker run -v /path/to/config:/config -e TZ=America/New_York -it --rm --name frigate_plate_recognizer lmerza/frigate_plate_recognizer:latest
```

or using docker-compose:

```yml
services:
  frigate_plate_recognizer:
    image: lmerza/frigate_plate_recognizer:latest
    container_name: frigate_plate_recognizer
    volumes:
      - /path/to/config:/config
    restart: unless-stopped
    environment:
      - TZ=America/New_York
```

https://hub.docker.com/r/lmerza/frigate_plate_recognizer

### Debugging

set `logger_level` in your config to `DEBUG` to see more logging information:

```yml
logger_level: DEBUG
```

Logs will be in `/config/frigate_plate_recognizer.log`