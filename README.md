# Principles for making things for the web #

This a list-in-progress of things I try to keep in mind when working on web projects.  Some are matters of logic, others are matters of personal taste.

I manage to abide by some of these things some of the time.

## Design/UI ##

* The best way to make something durable and flexible is not to get too fancy in the first place.  You only have to fix the web to the extent that you break it first.

* Make sure states of your app that you want to be shareable have unique URLs.  Use hashes if necessary.  Assume that someone will copy the URL directly from the address bar, not from your "Share this!" widget.

* You need a very good reason to have more than two fonts per page.

* Don't use tiny font sizes, and be generous with line spacing for body text.

* Don't make tiny click targets.  Assume that someone will be mashing that "X" icon with a fingertip, not a cursor.

* Redundancy is a useful design technique.  Labels+icons, color+width, etc.

* Embrace vertical scrolling, don't break it.  Scroll-based animation is annoying.  Stop turning the web into a popup book.

* Don't rely heavily on hovers to make something interesting.  Even if most of them have a mouse, requiring your users to go on a scavenger hunt is obnoxious.

* If an input is going to be numeric, use the "number" input type so mobile devices can show the appropriate keyboard.

* Use loading indicators for XHR requests, even if they're likely to be very fast.  You never know how slow or broken it might be for a user.  They should know if something is missing.

* If someone's going to have to scroll, make it clear that there's more below the fold.

* Don't break browser zoom (things like full-window maps are an exception).

* Try not to rely heavily on audio (this includes video with voiceover).  Lots of users will be in public, and even if they have headphones, requiring them to get them out and/or put them on is a big barrier.

* Text should be text, not text in an image.

* Make it clear that clickable things are clickable.  They should have `cursor: pointer`, have hover states, and if they're text, they should be distinct from other text.

* Avoid lightbox modals if possible.

* Don't make your location-based app *require* a user's location.  Be prepared for them to say no.  More generally, if you have a very personalized app, think about what interesting things you can show someone who doesn't want to get personal.

* Don't give someone 20 equally interesting things to do right off the bat.  Give them a more focused presentation upfront before turning them loose.

* Small multiples reflow easily.

* Limit the maximum width of text blocks to something like 900px.  Anything wider becomes hard to scan.

* Don't use Flash.

## Graphics/Charts ##

* Assume that people won't read the instructions.

* Put legends as close as possible to the chart content they describe.

* Label your axes and show your units. 

* Get live data into your visualization early.  If you can't, use historical data or something else a little bit representative.  Visualizing random test data will lead you astray.

* When showing the change in a value over time, you need a good reason not to use an area chart or line chart.

* When comparing a value for multiple categories, you need a good reason not to use a bar or column chart.

* When comparing two variables across a set of data, you need a good reason not to use a scatterplot.

* When showing a distribution, you need a good reason not to use a histogram/distribution curve.

* Don't make 3D charts.

* Don't muck around with the y-axis.  Be mindful of scale.

* Don't expect people to tell subtle differences in scale for bubble size or opacity.

* Don't use more than three or four colors in a categorical scheme.  If you have more categories than that, you probably need to use something besides color to differentiate.

* Be mindful of color blindness when picking combinations.  Use something like [Colorbrewer](http://colorbrewer2.org/) to pick your scales.

* Don't make slideshows/lists without a "view all" option.  Better yet, make it "view all" from the start with vertical scrolling instead of requiring a dozen clicks.

* Transitions are fun the first time and then usually annoying.  Try not to use them unless you're actually trying to show persistent objects in transition.

* Use [Leaflet](http://leafletjs.com/) for maps.

* Don't make population maps.

* For world maps, consider using a Robinson projection.  For US maps, consider using an Albers projection.  Whatever the map, be aware of the distortions of your chosen projection.

## HTML/JavaScript ##

* Specify a `charset`, presumably `utf-8`.

* Specify a doctype, presumably `<!DOCTYPE html>` (I don't know of a good reason to use any other type).

* Include descriptive social media `<meta>` tags.

* Put site scripts at the end of the `<body>` tag.

* If something is going to be rendered the same way every time with JavaScript, it should become static.

* Concatenate and minify scripts to minimize page size and number of requests.

* Use descriptive subfolders for resources (`css/`, `js/`, `images/`).

* Include version numbers in the filenames of JS libraries.

* Cache jQuery and D3 selectors that are going to be reused:  

```
$("div#sidebar").html("Don't do");
$("div#sidebar").html("this");

var $sidebar = $("div#sidebar");

$sidebar.html("Do this");
$sidebar.html("instead");
```

* CamelCase or lowercase?  Tabs or spaces?  Hyphens or underscores?  Picking a system and being consistent is more important than which you choose.

* Don't poll for updates constantly.  How up-to-the-second does the data actually need to be?

* Don't leave console debugging messages in your production code.

## Working with data ##

* Clean and transform your raw data stepwise.  Make it a repeatable process.  Use Makefiles or shell scripts if you can.

* Give data files descriptive names.  `geocoded-20131206.tsv`, not `data.tsv`.

* Link to your data sources in the final presentation.  Explain your methodology, and especially **explain the limits and shortcomings of your analysis**.

* Don't use really complex regular expressions if you can help it.  Use multiple steps instead, you'll be less likely to screw up.

* Round coordinates (quantize) and simplify geodata files to save space as appropriate.  You don't need data to the inch to make a world map.

* Store data as JSON or TSV.

* Work with simple text files when possible.  Avoid the overhead of a database unless your data really demands it.

* Make sure you know the way(s) that a dataset represents unavailable values.  It could be a blank space, it could be a dash, it could be an asterisk.  It could be multiple things.  Some places will use 999 or 99.999 to mean a number that's not available.

* Cache pages while you scrape them so that you don't have to start over if you were scraping the wrong thing.

* Beware the four C's of working with text data: character encoding, capitalization, curly quotes, and cwhitespace.

```
"Côte d'Ivoire" //damn it
"Cote d’Ivoire" //crap
"cote d'ivoire" //whoops
"Côte d'Ivoire " //what the?
"Ivory Coast" //COME ON
```

## Site setup ##

* Make things static whenever possible.  If they can't be static files, cache routes with something like [Varnish](https://www.varnish-cache.org/).

* Cool URLs don't change.

* Make URLs short, descriptive, and lowercase.

* Use `http://domain.com/`, not `www.domain.com` - but configure `www.` to redirect to the former.

* Flush POST data so it can't be resubmitted with a refresh.

* Try very hard not to rely directly on external APIs.  Use an in-between layer so that if the API goes down, your site is just stale instead of broken.

* Assume your page will be one of user's dozen open tabs.  Use short, descriptive page titles and a favicon.

* Minify images as much as possible, and use the appropriate format.  You don't need rich, lossless, giant files for tiny icons or thumbnails.

## Server config ##

* Don't store credentials in script files.  Use environment variables.

* Have a recovery plan, and **test it**.

* Keep detailed logs, and rotate the logfiles.

* Keep track of your server's dependencies and jobs such that you can quickly rebuild it from scratch.  Have a clean AMI or equivalent.

* If you use AWS, have at least one non-AWS fallback.

* Use a staging server.

* Don't rely on browser sniffing.  Detect support for relevant features instead.  And if you are doing too much of that, you probably got too fancy (see bullet #1).

* If you're using a framework, make sure verbose error messages are off in production.

## General process ##

* Be open about uncertainty around a project's eventual form.  Build with escape routes and next-best options in mind in case you hit a dead end or the data doesn't cooperate.

* Form a team around a project at the beginning.  Everyone who's going to be building something should be involved in the formative discussions.  Have them physically sit together as a team if possible.

* Build in sufficient testing time.  Guerrilla test early with people who aren't involved in the project, and don't give your testers a bunch of prefatory instructions before turning them loose (your real users won't get any).

* Don't spend too much time designing static mockups of unstatic things.  Go from paper to code as quickly as possible.  Graphics software is for graphics.

* Deciding how much to care about different types of users requires knowing your users in the first place.  Understand who they are, what they want, what browsers they use, what devices they use, etc.  Make that the starting point when deciding what level of resources to devote to addressing different scenarios.

* Set time aside to write good documentation.  Be specific about dependencies and installation.  Use real-world scenarios and variable names in your examples, no `foo` and `bar` gibberish.

## Some further reading ##

* [NPR Apps best practices](https://github.com/nprapps/bestpractices)  

* [ProPublica's news apps and data guides](https://github.com/propublica/guides/)

* [Lena Groeger's course materials on design](http://lenagroeger.github.io/design/)

* [Cool URIs don't change](http://www.w3.org/Provider/Style/URI.html)

* [When maps shouldn't be maps](http://www.ericson.net/content/2011/10/when-maps-shouldnt-be-maps/)

* [Shawn Allen's web how-to](https://github.com/shawnbot/web-howto/wiki)