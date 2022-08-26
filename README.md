![Mosquitto Logo](https://mosquitto.org/images/mosquitto-text-side-28.png 'Mosquitto')

# Simple Mosquitto broker

This is a simple [Mosquitto](https://mosquitto.org) broker to to quickly initialize projects requiring an MQTT broker. The config file is in the folder `config/mosquitto.conf`.

By default we activated the log and data persistance (respectively in `log` and `data` folder).
The authentication can be activated if needed.

# How to use

To start the container, just :

```
docker-compose up -d
```

The Mosquitto broker is now available on localhost. You can test it easily:

| In first shell:

```
docker-compose exec mosquitto mosquitto_sub -h localhost -p 1883 -t "topic/one"
```

| In a second shell:

```
docker-compose exec mosquitto mosquitto_pub -h localhost -p 1883 -t "topic/one" -m "hello world"
```

# Enabling authentication

In the config file, just set `allow_anonymous false` in the `Authentication` part and then restart the container.
The default user is `admin/password`, `user/password`.

**You always have to restart if you want the modification to be taken in account:**

```
docker-compose restart
```

| In first shell:

```
docker-compose exec mosquitto mosquitto_sub -h localhost -p 1883 -t "topic/one" -u admin -P password
```

| In a second shell:

```
docker-compose exec mosquitto mosquitto_pub -h localhost -p 1883 -t "topic/one" -m "hello world" -u user -P password
```

## Change user password / create a new user:

```
docker-compose exec mosquitto mosquitto_passwd -b /mosquitto/config/password.txt user password
```

## Delete user:

```
docker-compose exec mosquitto mosquitto_passwd -D /mosquitto/config/password.txt user
```

## Access container shell

```
docker-compose exec mosquitto sh
```

## Convert the plain password file to encrypt password file

```
docker-compose exec mosquitto mosquitto_passwd -U </path/to/password.txt>
```
