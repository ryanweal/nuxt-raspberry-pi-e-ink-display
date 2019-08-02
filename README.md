# Using Nuxt to power Raspberry Pi e-ink displays



## Who am I?

Ryan Weal

Software Developer

Kafei Interactive Inc.

https://kafei.dev



### Idea behind this project

- Create a low-power display that can hang on a wall
- Minimal battery recharging (months?)
- Fetch data from the network
- Re-use concepts I already know (web stuff)
- Have fun (not a work project)



<section data-background-image="/images/IMG_20190215_101034586.jpg">
</section>



## Research phase

- What Raspberry Pi models are available?
- What is a "pi hat"?
  - two formats of these hats
- What kinds of displays are out there?
- How will we provide power?
- Networking?



### Which Pi?

- Decided to get all 3 variations that were available:
  - 3 "A" - newer processor stuff
  - 3 "B" - complete w/extra ports but most support
  - Zero W - lowest power, less RAM, etc.
- SD cards - get fast ones (and extras)

https://en.wikipedia.org/wiki/Raspberry_Pi



### Hats?

- We also want to look at what pins they will use
- Some pins support multiple hats, some do not
- Many will rely on the GPIO connector, some do not

https://pinout.xyz/boards



### Displays

- Waveshare is what I used (cheap), 2.13"
- Does it have drivers? Yes, and...
  - also has open-source drivers/examples
- supports partial or full updates
- has a fast refresh rate (important)
- what pins will it be using?

https://www.waveshare.com/product/2.13inch-e-paper-hat.htm



### Power

- Made sure to get a supported power plug option
- Got the largest battery I could find
  - Used a specialized vendor that recommended it
  - Suggested one that does not go to sleep
- Invested in a "Sleepy Pi 2" (arduino)
  - No ACPI support on Pi, so this will do
  - what pins will it be using?

  https://spellfoundry.com/sleepy-pi/sleepy-pi-2-getting-started/



### Networking

- Would be really cool to use cellular modem!
  - 2G is the only good power option, maybe 3G
  - HSPA+ and LTE cards cost lots and need extrnal power
  - The answer is NO, for now.
- Let's use Wifi
- Someday we could try Bluetooth



## Order the things

- 3 Raspberry Pi variations
- 4 SD cards
- 2 chargers
- 2 Waveshare 2.13" displays
- 1 Sleepy Pi 2
- 1 USB keyboard - to setup network
- USB power bank - 5V USB power



## Software Recipes

- Pi-Hole
- SSH console
- The Nuxt Thing

https://github.com/ryanweal/raspberry-pi-recipes



### Pi-Hole

- Let's create a PiHole (adblocker)
- Totally unrelated to our project!
- Easy to install
- Used model 3 "B" for this one since it has ethernet
- After install edited my router settings
  - Use pi-hole as DNS now
- Document all the steps!



### SSH console

- Used PaperTTY project
- Get the Pi on wifi
- Install...
- Pretty easy!
- Document all the steps again!



### The Nuxt Thing

- Fetch data from net, and display it!
- Clone the example repo
- Try running the test commands
- Test commands powered by Python
- Provide our own image? ...yes.
  - Needs to be 1-bit image though

https://github.com/ryanweal/papercards



#### How to fetch the data?

- Puppeteer has been my favorite tool lately
- Fetch and parse the data on a schedule
- Only write the data if it is valid
- Leave the data there for now
- Did you expect it to be easy to run on Pi?
  - Pro-tip: use built-in Chromium
  - Make sure to use correct puppeteer version to match!

  https://github.com/GoogleChrome/puppeteer/issues/550



#### How to render the data?

- We can build a nuxt app and target to screen size
- Lets us build on a normal machine using web tech
- Provide query parameters for getting the data
- Really this could be plain JS...
- ...but eventually we want to cycle through different formats
- Make sure to put date + timestamp on display



#### Get the data from nuxt into an image?

- Need a nuxt server running or a web server
- Decided to install nginx (easy to install and run)
- Did "npm run generate" to pre-render the app
- Nginx only needs to serve static files
- Less risky for getting hacked
- Use Puppeteer again to take a snapshot
  - Produces png file



#### Convert 256(?) color PNG to 1-bit BMP

- This was sorta hard to figure out...

`convert output/weather.png -depth 1  ppm:- | pnmdepth 1 | ppmtobmp >output/weather.bmp`



#### Putting it all together:

- puppeteer script to parse values from scrape
- pass the parse values to local webserver as parameters
- nuxt consumes the parameters, makes display
- puppeteer takes a snapshot
- shell script then converts output png to 1-bit bmp
- then we run the example python code to load it
- and we put this into cron as a script
  - or at startup, when we configure Sleepy Pi



#### Let it Run

- It kept going from February to mid-June!
- Not sure why it crashed...
  - Power outage?
  - New template for weather service?
  - Was not home to find out



<section data-background-image="/images/frozen-june30.jpg">
</section>



<section data-background-image="/images/unfrozen-june30.jpg">
</section>



#### Fixes itself...

- The clear command a couple times will completely wipe away the problems
- You can also just wait it out as it will clear on each update



### What about Sleepy Pi 2?

- Now that we know it is stable, we can do this part...

- It is an Arduino board, an enterly different computer!
- Sets a wake/sleep schedule
- Recpies are provided by vendor
- Had some trouble with needing local GUI
  - Install a desktop?
  - Get an external cable to flash it
  - Do more research to not need to deal with GUI



## Future considerations

- It would be nice to have a cellular version
- Need to experiment with Sleepy Pi 2 settings
- Smaller battery would fit better in most "lightboxes"
  - I am talking about a literal lightbox, these are a thing
- Consume different APIs for fresh data: MLB, NHL are good examples



## Thanks!

That's all for now...

Ryan Weal

https://kafei.dev