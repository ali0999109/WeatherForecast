# Overview this is a weatherForecast app utilizing open weather API to get weather data for the week. Its built using streamlit and graphs are made with plotly.







![image](https://github.com/ali0999109/WeatherForecast/assets/145396907/1c383f32-5531-4cf2-a0cd-ecbcc6afe09d)



---------------------------------------------------

![image](https://github.com/ali0999109/WeatherForecast/assets/145396907/7df2a7b5-1711-407f-8ebb-a14cce3e67d3)


---------------------------------------



![image](https://github.com/ali0999109/WeatherForecast/assets/145396907/f8e15dec-485e-4d55-83ed-889b2e7b612e)




# Code for the Frontend & Backend

```import streamlit as st
import plotly.express as px
from backend import get_data
st.title("Weather Forecast For The Week")
place = st.text_input("Place:")
days = st.slider("Forecast Days", min_value=1, max_value=5
                 ,help="select the number of days forecasted")

option = st.selectbox("Select data to view", ("Temperature", "Sky"))
st.subheader(f"{option} for the next {days} days in {place}")

if place:
    filtered_data= get_data(place,days)
    if option == "Temperature":
        temperatures = [dict["main"]["temp"] for dict in filtered_data]
        dates = [dict["dt_txt"] for dict in filtered_data]
        figure = px.line(x=dates,y=temperatures,labels={"x": "Date","y": "Temperature (C)"})
        st.plotly_chart(figure)

    if option == "Sky":
        images = {"Clear":"images/clear.png","Clouds":"images/cloud.png",
                  "Rain":"images/rain.png","Snow":"images/snow.png"}
        sky_conditions = [dict["weather"][0]["main"] for dict in filtered_data]
        image_paths = [images[condition] for condition in sky_conditions]

        st.image(image_paths,width=200)



# Backend


import requests

API_KEY = '569e47bd99e5e515ecf93ef55aa80165'

def get_data (place,forecast_days=None):
    url = f"http://api.openweathermap.org/data/2.5/forecast?q={place}&appid={API_KEY}"
    response = requests.get(url)
    data = response.json()
    filtered_data = data["list"]
    nr_values = 8 * forecast_days
    print (data)
    filtered_data = filtered_data[:nr_values]
    return filtered_data

if __name__ == '__main__':
    print(get_data(place="Tokyo",forecast_days=3))






