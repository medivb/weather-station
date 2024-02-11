# weather-station
A weather station measuring ambient light, motion, temperature, humidity and air quality using an ESP32 via ESPHome on Home Assistant

I started this project just to get some feeling for what I could do with ESP Home and some cheap sensors from the various Chinese webshops. So far I am really impressed with all the possibilities and although I am not a programmer, scripting stuff through ESP Home, statistics and template sensors in Home Assistant got me very far.

<i>Many thanks to Pieter Brinkman (https://www.pieterbrinkman.com/) who's air quality project was very useful to get the particle sensor up and running the right way.</i>

<b>Shopping list:</b>

<ul>
<li><i>Microcontroller:</i> ESP32 WROOM</li>
<li><i>ESP32 devboard:</i> https://share.temu.com/P1NwJmbdvAA</li>
<li><i>Display:</i> LCD1602 16x2 character with backlight controller (https://a.aliexpress.com/_EuZUkjX)</li>
<li><i>LCD bezel:</i> 3D print yourself or order printed via https://www.knutselaar.eu/Store/witte-bezel-voor-16x2-lcd-display.html</li>
<li><i>Particle sensor:</i> PMS5003 (https://a.aliexpress.com/_ExSZs4h)</li>
<li><i>Temperature & Humidity sensor:</i> Pick any AM2302 compatible one. They are available for 1 or 2 euros anywhere.</li>
<li><i>Light sensor:</i> I used one I had laying around as part of I kit I once ordered. This one has its limitatioons, but I could work around them.</li>
<li><i>Motion sensor:</i> Also one I had left over from a kit. Just works as on/off and is really sensitive, but after some tweaking I got it to work fine.</li>
<li><i>Case:</i> I went for an outdoor one from Temu that has some space for growth (https://share.temu.com/LRVdbrZLvCA)</li>
</ul>

<b>Requirements:</b>

I live close to a major airport and planes pass over my house at relatively altitude low every day. On top of that, a large steel factory is located just a few kilometers away, so I wanted to start doing some measurements of my own to see how bad the pollution level really is and how it fluctiuates with the weather and/or other conditions. I want to place the unit outside, but inderneath some shelter so it is not going to be exposed to direct rain and/or sun, but it will be exposed to a wide level of temperatures and humidity levels. I wanted everything to communicate locally on my own wifi and not be dependent on external services.

<b>Design decisions & execution:</b>

I decided to use a housing that is big enough en well-ventilated enough to also allow me to place the particle sensor inside of it and allow for maybe some future expansion. The Temu case does all that; it has openings at the bottom so outside air can move through it freely, but is sealed at the top and sides from any moisture dripping in. Condensation might happen and we will see how long everything keeps working, but for now I am really happy with this setup. I have used whatever I had laying around (the ESP32, light, temp and motion sensors) and ordered the rest from AliXpress and Temu (see shopping list). I think the whole setup cost me < 30 euros. 


I really liked the devboard that actually includes a wide-range (5V to 16V) power supply conncetion. Since I am powering the ESP, display, particle sensor and all the other sensors, I needed some power. Now I am using an old laptop power supply that does the trick really well and it plugs into the board directly. 


After creating the configuration in ESP Home and connecting all the sensors, I used a hot-glue gun to fix everything in place and it has been working fine like that for a few weeks now.

<b>Interpretation of the data:</b>

This was actually the challenging part. I started out making some template sensors to calculate the AQI according to the US standard. After completing this, I learned there actually is a EU standard, so decided to implement that instead. The EU AQI standard actually works with 6 levels and dictates how each type of measurement has to be intepreted in order to fall in one of these 6 levels. This is described quite well on https://ecmwf-projects.github.io/copernicus-training-cams/proc-aq-index.html. First step is to create a 24 hour mean statistics sensor for each of the relevant sensors. In my case, those are the PM2.5 and PM10.0 sensors. The sepcifications for those are included in the repository. These go into your configuration, after which you need to restart HA. The 24 mean is used as input for the intepretation. First you calculate them individually (rules are different), done by the PM_2_5_AQI_template_sensor and PM_10_AQI_template_sensor in the repository, which you can use as input for a helper. Last step is to calculate the average between these two, which will be your actual AQI. This is done by the AQI_template_sensor. I also create a template sensor that reflects the Air Pollution Level accrding to the EU standard. I display this also on the LCD of the weather station, together with the date, time, temperature and humidity.

The rest of the sensors is pretty straightforward and I left a lot of comments in the weather-station.yaml ESP Home configuration file.

If you have any questions, please let me know so I can improve on my documentation and writing skills and add whatever you feel is missing.

<b>Some pictures of the end-result:</b>

![front_1](https://github.com/medivb/weather-station/assets/69627753/41cf4feb-eddb-4490-b146-fc5cb5153432)

![front_2](https://github.com/medivb/weather-station/assets/69627753/15386e27-6fc1-4854-9d12-c0c66c3a80d2)

![inside_1](https://github.com/medivb/weather-station/assets/69627753/09536cd3-aae3-4772-be67-7c10c9cd57ed)






