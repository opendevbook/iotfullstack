# 2 Arduino IDE

![](../assets/images/arduino1.png)

## Lab1: Arduino Upload data

- Copy 4 screen from your pc and thingsboard account in Word

  1.1 Build and uplod to thingsboard

![](../assets/images/arduino2.png)
2.2 Check Thingsboard Dashboard in Device name "muict_esp32"

![](../assets/images/tb_db_device8.png)

3.3 Check Attribues in Device Infomations

![](../assets/images/tb_db_device9.png)

3.4 Check Telemetry Data

![](../assets/images/tb_db_device10.png)

## Lab2: Cli command line

2.1 Command Line Authentication to get authen token  
To authenticate with ThingsBoard’s REST API from the command line using curl, you need to use the login endpoint to get a JWT (JSON Web Token) token. Here’s how to do it:

```
curl -X POST -H "Content-Type: application/json" \
-d '{"username":"YOUR_USERNAME","password":"YOUR_PASSWORD"}' \
http://YOUR_THINGSBOARD_SERVER/api/auth/login

```

![](../assets/images/cli1.png)

2.2 Use command line to send data to thingsboard account

[https://thingsboard.io/docs/user-guide/telemetry/](https://thingsboard.io/docs/user-guide/telemetry/)

![](../assets/images/cli2.png)

Get access_token first from Dashboard by click [copy access token] will save in clipboard

![](../assets/images/tb_db_device14.png)

1 upload data by HTTP API

```
$ export THINGSBOARD_HOST_NAME=demo.thingsboard.io
$ export ACCESS_TOKEN=<from device information>
$ mosquitto_pub -d -q 1 -h "$THINGSBOARD_HOST_NAME" -p "1883" -t "v1/devices/me/telemetry" -u "$ACCESS_TOKEN" -m {"temperature":30}
```

![](../assets/images/cli3.png)

after install mqtt client package, Let try it again

![](../assets/images/cli4.png)

![](../assets/images/tb_db_device11.png)

!!! note "Basic Linux Shell variable set/get"

    - export VARIABLE=value
    - $VARIABLE  get value
    - need install mosquitto_pub in linux (ubuntu/centos/) first
    - $ACCESS_TOKEN - device access token.

2 upload data by HTTP POST

- URL /api/v1/{ACCESS_TOKEN}/telemetry

```
echo $ACCESS_TOKEN
curl -v -X POST --data "{"temperature":42,"humidity":73}" https://demo.thingsboard.io/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
```

![](../assets/images/tb_db_device12.png)

Ensure that you're connecting to ThingsBoard over HTTPS, especially if the ThingsBoard instance is set to require secure connections.

!!! note "Basic curl command"

    The curl command you provided is almost correct, but it has a syntax issue with the JSON payload. You should escape the quotes around the JSON payload or use single quotes around the entire JSON body.

![](../assets/images/tb_db_device13.png)

!!! note "if Error, here Explanation:"

    **if Error, here Explanation:**
    ![](../assets/images/cli6.png)
    [https://thingsboard.io/docs/reference/http-api/](https://thingsboard.io/docs/reference/http-api/)

3 Use mqtt client App. Example [https://mqttx.app/](https://mqttx.app/)

- Download and install application:

![](../assets/images/mqttx1.png)

- Create connection:

![](../assets/images/mqttx2.png)

- Connection Info:

![](../assets/images/mqttx3.png)

- publish Data: v1/devices/me/telemetry:

![](../assets/images/mqttx4.png)

![](../assets/images/mqttx5.png)

- Check Data again:

![](../assets/images/mqttx6.png)

!!! info "Summary"

    Remember topic  "v1/devices/me/telemetry"  use for public data to thingsboard iot Gateway by mqtt protocol
