---
layout: post
title:      "The One with the First Project: CLI Best Psych Books Gem"
date:       2020-10-06 16:48:42 +0000
permalink:  the_one_with_the_first_project_cli_best_psych_books_gem
---

After four months of diving headfirst into Flatiron's Self-Paced Software Engineering program, it's time to embrace the first project of building a Command Line Interface (CLI) ruby gem. The last two weeks have felt like the most daunting; sometimes making me question my decision to tackle this career. However challenging, it has also felt the most rewarding to integrate all that learning and see a physical representation that concepts stuck. You will get through it!!

When first reading the resources and requirements lesson, it can seem like a dump truck completely unloaded with no start line in sight. My biggest recommendation is to plan and address only one thing at a time. Everything else will follow when it's time. 

Now let's dive into the process!

# -Planning-
No lie, this was a crucial step, if not THE most crucial. It allowed me to slow down and not get caught up in the actual coding expectations, decreasing my overall anxiety. The main goal was to come up with an idea and create a flow chart of how to provide a solution to a user's problem.

**Steps for Planning:**
1. Create an idea, including user story and flow chart
2. Ensure website resource can be scrapped (cannot stress this enough!!!)
3. Plan classes


Early on I had many different ideas such as scraping Disney World's marathon races, or finding pumpkin patches in the New England area (it's fall here right now!), or just keep it simple with finding best psychology books. I should have known that Disney wouldn't allow me to scrape their website. How I found this out was by using the tools Flatiron provided.

The [Repl Scraper Checker Tool](http://https://repl.it/@TheGingertonic/ScraperChecker#main.rb) and [Youtube video](http://https://www.youtube.com/watch?v=KwBMwZ89Hj8&list=PLc6AmvC5Zybybc-NjUUwQwTtUEXH4iB2s&index=2&t=0s) let you play around with scraping and to double check your website is capable of being scrapped. The video was super helpful in demonstrating how to use the tool. 

Since Disney was a dead end, I decided to scrape pumpkin patches. (**Hint:** You'll notice this is not what my gem ended up being. More on that later!) Once I found I could scrape names or titles, I made my decision on pumpkin patches. I highly suggest ensuring you can scrape into the first level somewhat easily. Otherwise, you might encounter the problem I did where ultimately, it was my website choice holding me up, which is not the focus of the project.

Lastly, after the idea and website resource were intact, the classes came somewhat easily. For the original idea, the classes were: CLI, Scraper, Farm. But when I changed the scraped website and idea in the 11th hour to Goodread's greatest psychology books, my classes changed to CLI, Scraper, and Book.

# -Building Gem-
For the actual building of the gem, the YouTube video mentioned above had a [Part 2](http://https://www.youtube.com/watch?v=TaRZ9Z8dK2s&list=PLc6AmvC5Zybybc-NjUUwQwTtUEXH4iB2s&index=4) and [Part 3](http://https://www.youtube.com/watch?v=VMAW3VjPUPw&list=PLc6AmvC5Zybybc-NjUUwQwTtUEXH4iB2s&index=5) that was very helpful. Stubbing out the CLI was key in this step and created a natural flow from one method to the next. Remember to frequently remind yourself of relationships:

* A Book object has many details such as an author, description, and rating
* Details belong to a book

The second key and point of the whole project was to set object attributes or methods equal to other objects rather than passed in strings. 
```
  def self.get_details
    PsychBooks::Scraper.scrape_book_details(book)
  end
```

Before I would get too far ahead, pry was my friend to ensure objects were collaborating effectively. Then my next step was always to run the program and let the terminal lead me in the right direction to fix any roadblocks. I challenged myself to give methods only one responsibility such as below:
```
  def valid_input?(input, data)
    input.to_i <= data.length && input.to_i > 0        #change input to integar and check validity
  end
```
Personally, I get a little overwhelmed if there's more than one thing happening inside a method. So if it can be refactored out, it allows me to clearly identify what happens where.
# -Clean Up-
As I was now at the end of the project and the CLI was working smoothly, I decided to take a peek at how difficult it would be to implement [Colorize](http://https://rubygems.org/gems/colorize/versions/0.8.1). Many of the book details, descriptions in particular, were somewhat difficult to read because nothing allowed separation of details (author, rating etc.) and text appeared as one run-on paragraph. After successfully getting Colorize to work,  I did a quick search to find how to add line spacing instead of using "puts ' ' ". These were originally all over my code BUT luckily, I found a post on easier implementation using regex. Here are some examples of using these two add-ons:

```
  def list_info_for(chosen_book)
    book = @books[chosen_book - 1] 
    PsychBooks::Scraper.scrape_book_details(book)   #set variable to array item of @books, minus 1 for correct index
    puts "\nHere are the details for #{book.title}:\n".green
    puts "\nAuthor:".yellow.bold + " #{book.author}\n".cyan
    puts "\nDescription:".yellow.bold + " #{book.description}\n".cyan
    puts "\nPage count:".yellow.bold + " #{book.page_count}\n".cyan
    puts "\nRating:".yellow.bold + " #{book.rating}\n".cyan
    menu
  end
```
Additionally, the book list kept printing with "(Hardcover)" or "(Paperback)" next to the book title since this was included in the selector tag. Back to Google, I figured out I could *chomp* specific words. Thank goodness these words all looked the same or it would have gotten very hazy. After another Google search, I learned it was possible to chain *chomp* so I could remove both words in one line.

Enter stage left the not so pretty side of this project for me personally.
# -Challenges-
The absolute most challenge aspects for me were:
* Gem setup
* Git commands
* Scraping website

It was quite unfortunate and very discouraging that the above were my greatest hurdles because my ruby collaborating object methods were intact for the most part. 

For the gem setup and git commands, I resourced the three parts of this [Youtube Live Build](http://https://www.youtube.com/watch?v=KwBMwZ89Hj8&list=PLc6AmvC5Zybybc-NjUUwQwTtUEXH4iB2s&index=2&t=0s). This massively helped with initial setup! A current student who just finished the project wrote a super helpful [blog](http://https://yani82.github.io/the_beginners_guide_to_creating_your_first_ruby_gem_cli) on git commands. Specifically, it provides a helpful step-by-step for each time you go to work on your project. Once I was using git commands so much because Learn IDE kept restarting, I can now happily say they are seared into my brain.

Initially, I attempted to scrape a pretty static New England Farms website to scrape pumpkin patches. I felt proud because even just getting the names of the farms was somewhat difficult and that should have been a sign. I attempted a couple times to scrape details such as activities provided at the farm for the fall season but still had complications. The reason being this particular site did not have a specific *h4*  and *li* for the information I needed. So rather than scrape the one *h4* line, it scrapped any *h4* tag, even with the parent tag. I even tried providing indexes to scrape just the one activities item. I made the mistake and assumed I would find the solution later. 

```
<ul>
<h4>Type of Attraction-Activity</h4><li>Farm</li><li>Free Hayride</li><li>Hayride - Weekends</li><li>Orchard</li><li>Pumpkin Patch</li><h4>Event -Festival Type</h4><li>Fall Festival</li><h4>Indoor-Outdoor</h4><li>Outdoor Attraction</li><h4>Facility Type </h4><li>Farm Store</li><h4>Admission Payments</h4><li>Free Festival</li>
```

Because I was stuck for a solid two days and spending 3+ hours on just this one aspect of scraping into the farm details, I tossed the idea completely. Yes, this might have been a crazy decision at this point but I reminded myself this was not the focus of the project and I just needed to get something finished that demonstrated I could implement ruby concepts. After I made the switch to Goodreads, I created a new repo and copied all the code over, changing classes and variables to reflect my new idea. I believe there would've been a way to merge with the new repo but honestly, anxiety and frustrations were high, making this way "seem" easier. 

# -Sigh of Relief-
This process felt like one of the most challenging but rewarding projects I've had to accomplish. While it makes me quite anxious for what's to come, I am provided a sigh of relief to see that yes, I am learning something! Again remember to take it one baby step at a time, even with just planning. Each large step can be broken down and a little tunnel vision wouldn't hurt in this instance. 

Trust me, at times I felt like I knew nothing and made the wrong decision. However, remind yourself of your strengths (mine is Googling to my heart's content!). Don't be afraid to reach out and take in the smallest accomplishments even if it is a syntax error. 

Now to figure out how to use gifs in a blog post...

Happy coding!!



