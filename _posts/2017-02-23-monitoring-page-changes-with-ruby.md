---
title: Monitoring Page Changes with Ruby
layout: post
---

Recently, I've found myself checking the Federal Business Opportunities website [fbo.gov](fbo.gov) for updates on a regular basis. For a while, I was using the [VisualPing](https://visualping.io) service to automate my change monitoring. Here's why and how I switched from VisualPing to a custom ruby script.

The main reason I stopped using VisualPing has nothing to do with how well the service works. My only issue is that I want to monitor for page changes on a very high frequency basis -- several times per hour rather than once per day. That would cost more than I'm willing to pay.

My overall process for monitoring an FBO page for changes was:

- Identify what to monitor
- Write a web scraping script in ruby
- Create a cron job to run the script automatically

In this post, I'll go over what I did and why, with a focus on how the ruby script and cron jobs are set up. I won't cover things like installing gems or managing ruby versions. The current version of this script is available on GitHub [here](https://github.com/joseph-turk/fbo-page-monitor).

## Identify What to Monitor

While VisualPing provides monitoring of an entire page and prevents a detailed report of what changed, this is overkill for me. I just want to know if an update has been posted for the solicitation I'm responding to. So I am taking a more targeted approach to monitoring the page.

The updates to a solicitation on the FBO website are provided in a list at the top left of the main content.

![Update list screenshot](../../../../img/blog/monitoring-page-changes-with-ruby/update-list.jpg "Update list screenshot")

By inspecting this element in Chrome Dev Tools, I found that it's an `ul` element with an `id` of `sb_related_notices`.

![Dev tools screenshot](../../../../img/blog/monitoring-page-changes-with-ruby/update-list-element.jpg "Dev tools screenshot")

This is what my script will monitor.

## Write the Scraper

Before I get into the details of what my requirements were and how I implemented them, here's the full, final version of the script.

{% highlight ruby linenos %}
require "open-uri"
require "nokogiri"
require "terminal-notifier"

# Set FBO page URL and number of known updates (including Complete View list item)
url = "https://www.fbo.gov/spg/DON/USMC/Contracts_Office_CTQ8/M67854-17-R-7601A/listing.html"
known_updates = 5 # Set number of updates we already know about
updates = []

# Create Nokogiri objects
page = Nokogiri::HTML(open(url))
solicitation_name = page.css('.agency-header h2').text
update_list = page.css('ul#sb_related_notices li')

# Add all list items in updates ul element to updates array
update_list.each do |update|
  updates.push(update.text)
end

if updates.length > known_updates # If there are more updates than we know about, it was updated
  # Send notification with details of update
  TerminalNotifier.notify(updates.last,
                          :title => "#{solicitation_name} Updated",
                          :sound => "Glass")

  # Log time and details of update
  puts Time.now.strftime("%d/%m/%Y %H:%M")
  puts "The #{solicitation_name} solicitation was updated. Here are the details:"
  puts updates.last
  puts "----------------------------------"
elsif updates.length < known_updates # If there are fewer updates than we know about, something went wrong
  # Send notification that something went wrong
  TerminalNotifier.notify("Something went wrong. Manually check FBO.",
                          :title => "#{solicitation_name} Error",
                          :sound => "Basso")

  # Log that something went wrong
  puts Time.now.strftime("%d/%m/%Y %H:%M")
  puts "Status Check for #{solicitation_name}. Something went wrong. Manually check FBO"
  puts "----------------------------------"
else # If there is the same number of updates that we know about
  # Log time of check and that nothing was changed
  puts Time.now.strftime("%d/%m/%Y %H:%M")
  puts "Status Check for #{solicitation_name}"
  puts "No updates since you last checked."
  puts "----------------------------------"
end
{% endhighlight %}

Here are the requirements that I started with for monitoring the page:

- It should send me a push notification if an update was made
- It should send me a push notification if it fails for some reason
- It should log activity, even if nothing changed
- I should be able to reuse it for future solicitations with minimal effort

To satisfy these requirements, I used the [Nokogiri](http://www.nokogiri.org/) gem for scraping the content and the [Terminal-Notifier](https://github.com/julienXX/terminal-notifier) gem to display macOS notifications.

After requiring those gems and ruby's built-in OpenURI module, I set the URL that I'll be checking and the number of updates for the current state of the solicitation, so the script has a value to check against for updates. I also create an empty array that will store the update list items.

{% highlight ruby linenos %}
# Set FBO page URL and number of known updates (including Complete View list item)
url = "https://www.fbo.gov/spg/DON/USMC/Contracts_Office_CTQ8/M67854-17-R-7601A/listing.html"
known_updates = 5 # Set number of updates we already know about
updates = []
{% endhighlight %}

Next, I create a Nokogiri object for the page. This object then lets me search for the items I want to monitor on the page using Nokogiri's CSS syntax.

{% highlight ruby linenos %}
# Create Nokogiri objects
page = Nokogiri::HTML(open(url))
solicitation_name = page.css('.agency-header h2').text
update_list = page.css('ul#sb_related_notices li')
{% endhighlight %}

The `'ul#sb_related_notices li'` query will return the list of updates shown above, and the `'.agency-header h2'` is where the title of the solicitation appears on the page. Using the page's solicitation title will let me show that in the notification without having to update it for each new solicitation I want to monitor.

Next, I loop over the `update_list` that I got from the Nokogiri query and push just the text of the update into the `updates` array.

{% highlight ruby linenos %}
# Add all list items in updates ul element to updates array
update_list.each do |update|
  updates.push(update.text)
end
{% endhighlight %}

At this point, I have all the information I need to tell whether the solicitation was updated. All I need to do is check how many updates were found against the value set at the top of the script and respond with notifications as appropriate.

My first conditional check is for whether there is an actual update. This would be if the number I got back is greater than what I set initially.

{% highlight ruby linenos %}
if updates.length > known_updates
{% endhighlight %}

In that case, I want to generate a push notification. I also set a custom notification sound for both the update and error notifications so I can tell them apart without looking at the screen.

{% highlight ruby linenos %}
TerminalNotifier.notify(updates.last,
                        :title => "#{solicitation_name} Updated",
                        :sound => "Glass")
{% endhighlight %}

I also want to log the date and time that the check was performed and the last update that was posted.

{% highlight ruby linenos %}
puts Time.now.strftime("%d/%m/%Y %H:%M")
puts "The #{solicitation_name} solicitation was updated. Here are the details:"
puts updates.last
puts "----------------------------------"
{% endhighlight %}

The other two conditional checks are built similarly, with the exception being that I don't send a push notification if nothing has changed and there was no error. Also, in either case, I don't send or log the last update, since there is nothing new to show.

## Create a Cron Job

Having the script on its own doesn't do much to make the monitoring process easier. Opening a terminal and typing `ruby scraper.rb` isn't any easier than hitting the refresh button in Chrome. To fully automate my monitoring process, I created a cron job to run the ruby script automatically every 15 minutes.

To create a cron job, you need to add an entry to the crontab. Because I've never fully come around on VIM, I opened the crontab for editing from a terminal with the Nano editor using the command `EDITOR=nano crontab -e`.

I then added the following line to the crontab. (Make sure to scroll right to see the full entry.)

```
*/15 * * * * PATH=$HOME/.rbenv/bin:$HOME/.rbenv/shims:$PATH ruby ~/Development/scrape-fbo/scraper.rb >> ~/Development/scrape-fbo/log.out
```

Cron jobs are created using the following format in the crontab.

```
* * * * * *
| | | | | |
| | | | | +-- Year
| | | | +---- Day of the Week
| | | +------ Month of the Year
| | +-------- Day of the Month
| +---------- Hour
+------------ Minute
```

I used `*/15` in the Minute place to signify "every 15 minutes."

One challenge with running a cron job is that cron operates at a low level in the system. When you open a terminal, you have a lot more settings in place because of your profile settings. Because I use RVM to manage ruby installs on my local machine, I added the following to my crontab entry to make sure cron uses the preferred version rather than the system version of ruby.

```
PATH=$HOME/.rbenv/bin:$HOME/.rbenv/shims:$PATH
```

Next, I needed to tell the cron job to actually run the script. This points to where I have the script saved on my local machine.

```
ruby ~/Development/scrape-fbo/scraper.rb
```

All that's left to do, then is send the output of the script to a log file. I sent it to a file named `log.out` in the same directory the script is stored in.

```
>> ~/Development/scrape-fbo/log.out
```

To save and close the crontab from Nano, press control+x, then y, then enter.

## Considerations

At this point, the script-plus-cron job combination is a functional page monitoring setup for me. There are a couple things to consider with the way I've implemented this, though:

- The notifications are macOS-specific. Work would need to be done to port this over to Windows or Linux.
- The cron job will only run while my MacBook Pro is awake. This is fine for me since I tend to keep it open while I'm working to run Spotify, send iMessages, and use Google Hangouts for conference calls.

These considerations could be handled in a future iteration of the monitoring script, but for now, this version is working for me just fine.
