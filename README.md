# Southwest-Scraper

This is a scraper for Southwest Airlines Flight Information. It saves the scraped data into your private Supabase database (https://supabase.com/) for storage and use. It requires 6 dependancies - puppeteer-extra (to scrape the webpage), puppeteer-extra-plugin-stealth (additional stealth plugin to puppeteer-extra), ghost-cursor (to trick SW's bot checkers), random-useragent (same as ghost-cursor), Luxon (small date package), and supabase-JS (to load data into your database).

As of now, this is a 85% finished project. It still needs much refinement as SW still will detect the bot on occassion. There are a few things off the bat that would make this even better:

* Finding an alternative package to 'random-useragent' or updating the package (located here: https://github.com/skratchdot/random-useragent) from its source. Issue #12 on the package page goes into more details. 
* Fixing the random mouse movement warnings. Using ghost-cursor clashes with puppeteer-extra, but ghost-cursor has definetely helped with bot detection. Not sure how to fix currently
* Finding better solutions to SW bot detection methods. In particular, I have seen that restarting the tool at the point of failure will usually work. I suspect this has to do with the chrome profile information (currently puppetteer will create a new one with each program run) but I am not certain - particularly because each scrape opens a new browser after closing the previous one. 

# How it works

The tool will scrape up to the first 10 flights between one airport to another on any particular day. The data is organized like this: 

{
    departurePort: 'ATL',

    arrivalPort: 'BWI',
    
    date: 2022-04-01T00:00:00.000Z

    metadata: {
          prices: ["299","259","209"],
          arrTime: "3:45 PM",
          deptTime: "6:45 AM",
          duration: "7h 0m",
          numStops: "1",
          seatsLeft: [null,"4","4"],
          flightNums: ["100","1185"],
          planeChange: "DAL"
    }
 }

Notes: 
* numStops will consist of either a number (as a String) of stops OR the string 'Nonstop'.
* planeChange will consist of 'null' if there is no plane change OR an airport code like 'DAL'.
* The prices array will mimic the structure of the webpage, reading left to right. The price catagories are as follows: [Business Select, Anytime, Wanna Get Away]. The string 'Unavailable' will indicate when tickets for that seat were sold out.
* seatsLeft will either consist of a positive number which will indicate the number of seats left for that category of ticket or a -1. It is important to understand that -1 does not mean all seats were taken, but rather that there are many seats OR no seats. It's order is the same as the prices array (reading page left to right). Do NOT use seatsLeft to determine if seats exist (use the prices property). 

# How to use

To start, download the project and extract its contents. Open in your CMD or Terminal window and navigate to the project. Then install the dependencies using *npm install*.

Next, go to supabase.com, create an account, and create a project. Create a new table called 'Flights' with columns named like so -> 
* departurePort -> set to text
* arrivalPort -> set to text
* date -> set to date
* metadata -> set to jsonb

Finally, access your supabase API credentials in the settings of the project. Copy/Paste the URL and the API key to the config.js file using a text editor. 

You can also change the date for which you want to scrape flight data from by adding months/days to the current date following the instructions in the config.js file.

# *WARNING & DISCLAIMER: DO NOT USE THE DATA YOUR SCRAPE FOR COMMERCIAL PURPOSES OR TO MAKE MONEY IN ANY WAY. THE DEVELOPER DOES NOT ATTAIN ANY RESPONSIBILTY FOR YOUR USE OF THE PROGRAM.
