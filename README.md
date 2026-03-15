import requests
import os
from twilio.rest import Client
app_key=os.environ["APP_KEY"]                                                                
account_sid=os.environ["ACC_SID"]
auth_token=os.environ["AUTH_TOKEN"]
from_num=os.environ["FROM_NUMBER"]
to_num=os.environ["TO_NUMBER"]

base_url = "https://api.openweathermap.org/data/2.5/forecast"
parameters={
          "lat":"place latitude",
           "lon":"place longitude",
            "appid":app_key,
             "cnt":4
}

response = requests.get(url=base_url, params=parameters)
response.raise_for_status()

weather_data = response.json()
# weather=weather_data["list"][0]["weather"][0]["id"]
will_rain=False
for hour_data in weather_data["list"]:
        condition=(hour_data["weather"][0]["id"])
        if int(condition)<=700:
             will_rain=True
if will_rain:
    client = Client(account_sid, auth_token)
    message = client.messages.create(
        from_=from_num,
        body="bring the umbrella",
        to=to_num
    )

    print(message.sid)
