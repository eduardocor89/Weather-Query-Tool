
import requests

API_ROOT = 'https://www.metaweather.com'
API_LOCATION = '/api/location/search/?query='
API_WEATHER = '/api/location/'  # + woeid

def fetch_location(query):
    return requests.get(API_ROOT + API_LOCATION + query).json()

def fetch_weather(woeid):
    return requests.get(API_ROOT + API_WEATHER + str(woeid)).json()

def display_weather(weather):
    print(f"Weather for {weather['title']}:")
    sunset = weather['sun_set'][12:16]
    for forecast in weather['consolidated_weather']:
        weather_state = forecast['weather_state_name']
        date = forecast['applicable_date']
        high = int(forecast['max_temp']) * 5/9 + 32     
        low = int(forecast['min_temp']) * 5/9 + 32

        print(f"Mostly {weather_state}, on {date}")
        print(f"With a high of {int(high)} and a low of {int(low)}")
        print(f"With the sunset coming around {sunset} tonight\n\n")


def disambiguate_locations(locations):
    print("Ambiguous location! Did you mean:")
    for loc in locations:
        print(f"\t* {loc['title']}")

def weather_dialog():
    try:
        where = ''
        while not where:
            where = input("Where in the world are you? ")
        locations = fetch_location(where)
        if len(locations) == 0:
            print("I don't know where that is.")
        elif len(locations) > 1:
            disambiguate_locations(locations)
        else:
            woeid = locations[0]['woeid']
            display_weather(fetch_weather(woeid))
    except requests.exceptions.ConnectionError:
        print("...Connection lost!")
        print("Let's try again.")
        weather_dialog()

if __name__ == '__main__':
    while True:
        weather_dialog()
