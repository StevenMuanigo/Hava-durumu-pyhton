# Hava-durumu-pyhton
Bu komut satırı aracı, OpenWeatherMap API kullanarak seçtiğiniz şehrin anlık hava durumu bilgisini getirir. Kullanıcıdan şehir adı alınır, sıcaklık, nem ve açıklama Türkçe olarak görüntülenir

Özellikler

 Gerçek zamanlı hava durumu verisi (API tabanlı)

 Türkçe açıklama desteği

 Komut satırından şehir adı argümanı alma (argparse)

Sıcaklık, açıklama ve nem bilgisi görüntüleme

Gereksinimler

Python 3.8+

requests kütüphanesi

pip install requests

 API Anahtarı Alma

https://openweathermap.org/api
 adresine gidin.

Ücretsiz bir hesap oluşturun.

API key (appid) oluşturun.

Kod içinde aşağıdaki satırı kendi anahtarınızla değiştirin:

api_key = 'YOUR_API_KEY'

Kod:

import requests
import json
import argparse

def get_weather(api_key, city):
    # Base URL for OpenWeatherMap API
    base_url = "http://api.openweathermap.org/data/2.5/weather"
    # Parameters for API request
    params = {
        'q': city,
        'appid': api_key,
        'units': 'metric',  # to get temperature in Celsius
        'lang': 'tr'        # Turkish language for description
    }
    try:
        response = requests.get(base_url, params=params)
        response.raise_for_status()
        data = response.json()
        return data
    except requests.exceptions.HTTPError as http_err:
        print(f"HTTP error occurred: {http_err}")
    except Exception as err:
        print(f"Other error occurred: {err}")
    return None

def parse_weather_data(data):
    # Extracting the required information from the API response
    weather = data['weather'][0]['description']
    temp = data['main']['temp']
    humidity = data['main']['humidity']
    return weather, temp, humidity

def main():
    # Setting up argument parser to accept city name from command line
    parser = argparse.ArgumentParser(description='Hava Durumu CLI Uygulaması')
    parser.add_argument('city', type=str, help='Hava durumu sorgulamak istediğiniz şehir adı')
    args = parser.parse_args()

    # Your OpenWeatherMap API Key
    api_key = 'YOUR_API_KEY'  # Replace 'YOUR_API_KEY' with your actual API key

    # Fetch weather data
    weather_data = get_weather(api_key, args.city)

    if weather_data:
        # Parse weather data
        weather, temp, humidity = parse_weather_data(weather_data)

        # Display weather data
        print(f"\n{args.city} için hava durumu:")
        print(f"Sıcaklık: {temp} °C")
        print(f"Açıklama: {weather.capitalize()}")
        print(f"Nem: {humidity}%")
    else:
        print("Hava durumu bilgileri alınamadı.")

if __name__ == '__main__':
    main()


