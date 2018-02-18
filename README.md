# Dojour Event Feed Plugin
A simple jQuery plugin for adding a [Dojour](https://dojour.us) event feed to your website.

## Installation
### Step 1 - Include jQuery
You may already have this included, but if add this to the <head> of your html:

```html
<script type="text/javascript" src='https://code.jquery.com/jquery-2.1.3.min.js'></script>
```

Or find the most up-to-date version [here](https://code.jquery.com/jquery/)

### Step 2 - Include Dojour Javascript and CSS files
Also add this to the <head> of your html underneath the jQuery snippet.

```html
<script type="text/javascript" src='https://dojour.us/assets/js/dojour.min.js'></script>
<link rel="stylesheet" href="https://dojour.us/assets/css/dojour-plugin.css">
```

DO NOT download this file. Keep the source url pointing to dojour.us. This will make sure we can make changes for you as our api evolves.

### Step 3 - Customizing the plugin
Create a div with the id ‘dojour-feed’ and add the following line to a javascript file or in a script tag.
At minimum, provide the username for dojour event feed you want to display.

```html
<div id='dojour-feed'></div>
<script>
	$('#dojour-feed').dojour({
		username:'yourUserName'
	});
</script>
```

This will call the Dojour API and return any upcoming events you’ve added, starred, or reposted.

Here’s a simple example of all these step together:
```html
<html>
	<head>
		<script type="text/javascript" src='https://code.jquery.com/jquery-2.1.3.min.js'></script>
		<script type="text/javascript" src='https://dojour.us/assets/js/dojour.min.js'></script>
		<link rel="stylesheet" href="https://dojour.us/assets/css/dojour-plugin.css">
	</head>
	<body>

		<div id='dojour-feed'></div>

		<script>
			$('#dojour-feed').dojour({
				username:'yourusername'
			});
		</script>
	</body>
</html>
```

The <body> will be converted into something like this:

```html
<div id='dojour-feed'>
	<div class='dojour-event-list'>
		...template with Event 1...
		...template with Event 2…
		...template with Event 3…
		...template with Event 4...
	</div>
	<button id="dojour-more">More</button>
</div>
```

## Customization

### Templating
You can override the plugin's default template by creating your own like so:

```html
<script id="event-list-template" type="text/template">
	<div>
		<h1>
			<a href="{{event_url}}">{{title}}</a>
		</h1>
		<h2>{{day_abrv}} {{date}} {{month_abrv}}: {{duration}}</h2>
		<img src="{{photo}}" />
		<p>{{description}}<p>
	</div>
</script>

<div id='dojour-feed'></div>

<script>
      jQuery('#dojour-feed').dojour({
          username:'your_username',
          template: jQuery('#event-list-template').html()
      });
</script>
```

Here is a complete list of variables you can use in your templates.

Variable | Description  
--- |---
{{title}} | The title of the event
{{photo}} | The url to the event image
{{photo_300}} | url to image resized to max 300px wide
{{photo_100}} | url to image resized to max 100px wide
{{description}} | The event description text.
{{description_truncate}} | The first 500 characters of event description text.
{{event_url}} | The url back the event page on Dojour.us
{{detail_url}} | The url to display an individual event page in this plugin. Adds ?event={{event_id}}
{{google_calendar_add_url}} | A url for the user to add the event to their google calendar
{{location_title}} | The name of the event’s location
{{location_address}} | the full address of the location
{{start_time}} | The next showings start time (ex 10:00pm)
{{end_time}} | The next showings end time, if exists (ex 2:00am)
{{door_time}} | The next showings door time, if exists (ex 9:00pm)
{{duration}} | The start, end, and door times as provided. If only the start time (10:00 PM), if end time provided (10:00 PM - 2:00 AM), if door time provided (10:00 PM - 2:00 AM, doors at 9:00 PM)
{{call_to_action_url}} | Either a Dojour registration url,  external url, or False
{{call_to_action_text}} | Either a Dojour user provided button text, or default button text
{{date}} | The number (1-31) representing the day of the month for the next upcoming occurrence of the event
{{day}} | The named day of the week (Sunday-Saturday)
{{day_abrv}} | The day abbreviated to three characters (Sun-Sat)
{{month}} | The named month (January-December)
{{month_abrv}} | The month abbreviated to three characters (Jan-Dec)
{{month_num}} | The month number (1-12)
{{year}} | The year (ex 1999)
{{year_abrv}} | The year abbreviated (ex 99)
{{end_date}} | If range of dates, the number (1-31) representing the day of the month for last event date.
{{end_day}} | If range of dates, the named day of the week (Sunday-Saturday)
{{end_day_abrv}} | If range of dates, The day abbreviated to three characters (Sun-Sat)
{{end_month}} | If range of dates and a different month than the start month, the named month (January-December)
{{end_month_abrv}} | If range of dates and a different month than the start month, the month abbreviated to three characters (Jan-Dec)
{{end_month_num}} | If range of dates and a different month than the start month, the month number (1-12)
{{end_year}} | If range of dates and a different year than the start year, the year (ex 1999)
{{end_year_abrv}} | If range of dates and a different year than the start year, the year abbreviated (ex 99)
{{if ...}} | Use any of the keys above to conditionally add hide_class (see above). Use to add class or attributes to elements. EX: ```<button class=”{{if call_to_action_url}}”>{{call_to_action_text}}</button>```

### Additional Settings
Option | Default | Description
--- | --- | ---
template | See Above | The template to use for the event list. Feel free to add HTML, see the table above for more reference.
detail_template | See Above | The template for event detail page.
subcalendar_id | false | Set a subcalendar id or array of ids to only show events from that subcalendar
event_id | false | Only show dates for a single event id or an array of ids
exclude_event_id | false | Exclude a single event id or an array of ids from a list
distinct | true | Set to false to list each date of an event as seperate.
page_size | 10 | The amount of events that should be fetched with each page (clicking the more button adds another page)
loading_text |‘Loading...’ |The text that appears while the events are being fetched from https://dojour.us
more_button_text | ‘More’ | The text of the button used to fetch more events
hide_class | ‘dojour-hide’ | The class to use for conditionally hiding elements in template (see templating below)
interpolate_start | ‘{{’ | The opening syntax for templating variables. You’ll only need this is you’re already using another templating language like django on your site!
interpolate_end | ‘}}' | The closing syntax for templating variables.


### Callback Function
If you need to fire more javascript after the feed has been built, add a callback function like so.
```javascript
$('#dojour-feed').dojour({
	username:'yourUserName'
}, function(){
	//put your logic here
alert('Finished!');
});
```
