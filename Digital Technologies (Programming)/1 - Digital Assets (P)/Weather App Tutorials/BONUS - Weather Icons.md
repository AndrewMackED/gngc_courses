Ok, great! We have the basics now, but have you ever seen a single weather application that doesn't have images? Probably not! Let's add some.

## How does OpenWeatherMap's images work?
The key thing here is to understand OWM's image codes. 
[Take a look here to understand the basics.](https://openweathermap.org/api/weather-conditions#How-to-get-icon-URL)

As you can see, they have a bunch of different general codes, and an extra character on them to determine whether they're day or night.

On google classroom you can find a zipped folder I prepared earlier with all the weather icons, along with an extra (000 - it's a blank)

Let's think about how to show the weather icon for the current weather. The reference for it in our code so far would be:
`data["current"]["weather"][0]["icon"]`

You can rework the code I'm about to share to easily work with icons for the upcoming daily weather too!

## How to load the images
This should be fairly familiar to you, it's fairly similar to how we loaded the images in your previous assignment. 

For this to work, you NEED to make sure your file organisation is accurate. My folder structure looks like this:
```
weather-app/
│
├── weather-app.py
│
├── fonts/
│   └── Roboto-Regular.ttf
│
└── weather_icons/
    ├── 000.png
    ├── 01d.png
    ├── 01n.png
    └── ... (remaining icons)
```

Ok, let's take a look at the code we're adding. This needs to go *beneath `dpg.create_context()`* and *before your window creation*

```python
def load_image(image_path, icon_code):
    width, height, channels, data = dpg.load_image(image_path)
    texture_tag = icon_code
    with dpg.texture_registry():
        dpg.add_static_texture(width, height, data, tag=texture_tag)
    return texture_tag

icon_codes = [ "000", "01d", "01n", "02d", "02n","03d", "03n","04d", "04n","09d", "09n","10d", "10n","11d", "11n", "13d", "13n", "50d", "50n"]

for i in icon_codes:
    image_path = folder_path + "\\weather_icons\\" + f"{i}.png"
    load_image(image_path, i)
```


Let's go through to code, but we're going to address the load_image function when we call it:
- First, we have our list of all the icon codes. We just need to set that up!
- Now, we iterate through our icon codes. For each one, we will:
	- Get the image path for it (adjust this if your folder structure is different to mine)
	- Run the `load_image` function
- The `load_image` function:
	- Runs the inbuilt load_image function to extract the width, height, channels and data from the image
	- Sets the `texture_tag` variable to be the `icon_code` (good for later)
	- Registers the texture into DPG's texture registry

## How to use the images
So, onto the fun bit! The first step, is to add an image to somewhere in your window.

```python
dpg.add_image(tag="current-weather-img", texture_tag="000", width=100, height=100)
```

The key notes here:
- I've set the tag for this one to be "current-weather-img". If you set up other images, just make sure the tags are unique
- the texture tag is the "blank" image that I made by default.
- 100x100px, probably a good size honestly, but feel free to go smaller or larger of course

### Setting the image based on the current weather
Now we have our image set up in our window, we just need to update it when we update our weather data. Add it to the `check_weather` function, it can go anywhere as long as it's after the `data = r.json()` line.
```python
dpg.configure_item("current-weather-img", texture_tag = data["current"]["weather"][0]["icon"])
```

Since it's updating an image, we need to use `configure_item`. We need to tell it the tag for the item we're updating, and then the property we're setting and what we're setting it to.
