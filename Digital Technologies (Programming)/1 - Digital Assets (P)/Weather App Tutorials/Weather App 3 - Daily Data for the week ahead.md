# Weather App Part 3 - Daily Data for the Week Ahead
Now we should have the current information showing up, and also allowing the user to select from a few different cities.
Next, we want to show the data for the week ahead!
Let's double check out assignment requirements:
>Your weather app should be able to display the following information:
>- The user should be able to select from at least three different locations
>- The current weather for a location
>	- This can be just the current specifics (temperature, with current weather), or you might like to include the upcoming hourly forecast
>- The predicted weather for the next week
>	- This must include at least the high and low temperatures, along with the general weather prediction (rainy, sunny, etc.)

Looking at this, we just need to include the High, Low and weather prediction; so that's not too bad! We are of course, also going to have to access the Datetime for each day to get the day of the week and the date.

Let's check our API docs and see how we can break out the Daily Data from the API response:
```json
"daily":[
      {
         "dt":1684951200,
         "sunrise":1684926645,
         "sunset":1684977332,
         "moonrise":1684941060,
         "moonset":1684905480,
         "moon_phase":0.16,
         "summary":"Expect a day of partly cloudy with rain",
         "temp":{
            "day":299.03,
            "min":290.69,
            "max":300.35,
            "night":291.45,
            "eve":297.51,
            "morn":292.55
         },
         "feels_like":{
            "day":299.21,
            "night":291.37,
            "eve":297.86,
            "morn":292.87
         },
         "pressure":1016,
         "humidity":59,
         "dew_point":290.48,
         "wind_speed":3.98,
         "wind_deg":76,
         "wind_gust":8.92,
         "weather":[
            {
               "id":500,
               "main":"Rain",
               "description":"light rain",
               "icon":"10d"
            }
         ],
         "clouds":92,
         "pop":0.47,
         "rain":0.15,
         "uvi":9.23
      },
      ...
   ],
```

You can see here, that inside of "daily" is a list. Inside of each list item (one item per day) is all of the data for that specific day. The data we're going to need to draw from each day is:
- `["dt"] `- The Datetime
- `["temp]["min"]` - The minimum temperature
- `["temp"]["max"]` - The maximum temperature
- `["weather][0]["description"]` - The weather description 
	- NB: The 0 is important because `["weather"]` has a list inside it, so we need to make sure we access the "zeroth" item in the list. (I don't know the use-case for having multiple items in that list is)

Basically, the plan is we're going to loop through the "Daily" list, and then for each loop, we'll show the user the details I've just pointed out above.
Of course, there are SO MANY more useful things to show the user that are also provided in the data, but that's for you to decide how to include! Personally, I like Sunrise/Sunset, Probably of Precipitation (chance of rain) and humidity!

## Actually implementing it!
Let's start off by adding a section on our UI to actually add the information. I'm going to use a Group, and I'll give it the tag "dailies". 
Basically, when the `check_weather` function runs, I'm going to add a bunch of rows to this group!

```python
#Add this to the bottom of your window code!
dpg.add_spacer(height=10)
    dpg.add_separator()
    dpg.add_spacer(height=10)

    dpg.add_text("This Week's Forecast")
    with dpg.group(tag="dailies"):
        pass
```

Next, we need to edit our `check_weather` function. Let's add this to the bottom of our function:
```python
for day in data["daily"]:
    with dpg.group(parent="dailies", horizontal=True):
		dt = datetime.fromtimestamp(day['dt'])
		dpg.add_text(dt.strftime("%a %d/%m"))
		
		dpg.add_text(f"Min: {day['temp']['min']}°C", indent=100)
		dpg.add_text(f"Max: {day['temp']['max']}°C", indent=200)
		dpg.add_text(f"{day['weather'][0]['description'].capitalize()}", indent=300)
```

This is going to let us loop through each day's worth of data. For each day, we will:
- Create a new horizontal group, and parent it to the "dailies" group we made in our GUI window earlier
- Convert the datetime from the Epoch Timestamp into a datetime object
- Render a string from the datetime object. In this case, my string it "Shorthand day of the week, day/ month" - the day I'm writing this, it would show up as "Thu 07/05". Then add that as a text object.
- Add a text object for the Minimum Temp
- Add a text object for the Maximum Temp
- Add a text object for the weather description (capitalised)

You may notice that on each of those "add_text" functions, I've included an "indent", going up by 100 pixels each time. This is going to allow each of those to be rendered at even spacing.

Let's see how it looks!
![[Weather Part 3a.gif]]
HMMMMMMMMM, that's not right!
It works great for the first run, but when I try to then check another city, it just adds the new data onto the end of the existing data. We need to delete the previous data!

Add this line of code above the start of our FOR loop, so it will delete any contents of the "dailies" group before adding the data we want:
```python
dpg.delete_item("dailies", children_only=True)
```

Let's try again:
![[Weather Part 3b.gif]]

Looks good!

TECHNICALLY, this meets all the requirements of the assignment, however - it kind of sucks.
You can improve this ALOT. The way I have implemented the GUI here is not particularly appealing, nor is it particularly easy to find the data that you want as a user. 

Take the Wireframe you should have designed by now, and try to make the program create the app that you designed!