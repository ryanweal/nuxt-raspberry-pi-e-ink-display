# Using Nuxt to power Raspberry Pi e-ink displays



## Who am I?

Ryan Weal

Software Developer

Kafei Interactive Inc.



### Idea behind this project

- Create a low-power display that can hang on a wall
- Minimal battery recharging (months?)
- Fetch data from the network
- Re-use concepts I already know (web stuff)
- Have fun (not work project)



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



### Hats?

- We also want to look at what pins they will use
- Some pins support multiple hats, some do not
- Many will rely on the GPIO connector, some do not



### Displays

- Waveshare is what I used (cheap), 2.13"
- Does it have drivers? Yes, and...
  - also has open-source drivers/examples
- supports partial or full updates
- has a fast refresh rate (important)
- what pins will it be using?



### Power

- Made sure to get a supported power plug option
- Got the largest battery I could find
  - Used a specialized vendor that recommended it
  - Suggested one that does not go to sleep
- Invested in a "Sleepy Pi 2" (arduino)
  - No ACPI support on Pi, so this will do
  - what pins will it be using?



### Networking

- Would be really cool to use cellular modem!
- 2G is the only good power option, maybe 3G
- HSPA+ and LTE cards cost lots and need extrnal power
- Let's use Wifi
- Someday we could try Bluetooth



## Order the things

- 3 Raspberry Pi variations
- 4 SD cards
- 2 chargers
- 2 Waveshare 2.13" displays
- 1 Sleepy Pi 2
- 1 USB keyboard - to setup network



## Software Recipes

- Pi-Hole
- SSH console
- The Nuxt Thing



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



#### How to fetch the data?

- Puppeteer has been my favorite tool lately
- Fetch and parse the data on a schedule
- Only write the data if it is valid
- Leave the data there for now



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

- This was sorta hard to figure out



#### Putting it all together:



### What about Sleepy Pi 2?

- An Arduino board, an enterly different computer!
- Sets a wake/sleep schedule
- Recpies are provided
- Seems like I needed the GUI to run, which means running on a desktop...
- I do not want to setup a desktop pi just for this!
- Oh there is a connector cable option, forgot to buy
- Oh the newer version is CLI-only but... it is running so who cares!
- I will revisit this later