# ajaxWithRealTimeDeviceReading

##What's ajax

>Ajax (also AJAX; /ˈeɪdʒæks/; short for asynchronous JavaScript and XML)[1][2][3] is a set of web development techniques using many web >technologies on the client-side to create asynchronous Web applications. With Ajax, web applications can send data to and retrieve from a >server asynchronously (in the background) without interfering with the display and behavior of the existing page. By decoupling the data >interchange layer from the presentation layer, Ajax allows for web pages, and by extension web applications, to change content >dynamically without the need to reload the entire page.[4] In practice, modern implementations commonly substitute JSON for XML due to >the advantages of being native to JavaScript.[5]

>Ajax is not a technology, but a group of technologies. HTML and CSS can be used in combination to mark up and style information. The DOM >is accessed with JavaScript to dynamically display – and allow the user to interact with – the information presented. JavaScript and the >XMLHttpRequest object provide a method for exchanging data asynchronously between browser and server to avoid full page reloads.

Above is from Wikipedia, it means Ajax is more like an idea instead of a hard core technique, which means the chanlleng of Ajax is how to orgaized techniques such as HTML, Javascript/jQuery together.

To make things clear, I work on 2 streams, backend and front.

###Big picture
Ajax visit certain address/URL and bring back data to user. Then user use javascript to porcess this data in order to show these data properly.

###First, Backend

In order to visit URL, jQuery give us methods such as **get()**, **post()** and **ajax()**. Based on the API of **jQuery.ajax( [settings ] )**, we should set up the datatype of the target address such as JSON, then if the visit success, then we can process then at the success method.

Target URL and ajax datatype settings should match up, otherwise the return data will be hard to manage. Normally lots of public API provides JSON based API. In our case, we just create a **HttpRespon** at Django framework,

```python 
@ views.py
from django.http import HttpResponse
def realtime_data_api(request):
	if request.method == 'GET':
		rows = getRealdate()
		return HttpResponse(json.dumps(rows),content_type="application/json")
    
    
@ urls.py
url(r'^realtime/$', disply_1.views.realtime_data_api, name='realtime'), #add this line in urls.py setting
```

As the code shows, Django will dump data to URL [I'm an inline-style link with title](/realtime/ "/realtime/") using Json format.

###Second, Front
Since we alreay know the source and datatype, then the job is super easy for us to process at Front. Here's the code to do so.
####First JS code

```javascript 
function updateOrders() {
        $.ajax({                                      
            url: '/realtime',                             
            dataType: 'json',
            type: 'GET',
            success: function (data) {
                setTimeout(updateOrders, 5000);
                $(document).ready($('#myDynamicTable').html(addTable(data)))
                var firstColumnHeader = $('#myDynamicTable thead th:first-child');
                firstColumnHeader.css('background', '#FCD116');
            } 
        });
    }
```
To get a better understand of this code here're some tips:jQuery Syntax
```javascript 
The jQuery syntax is tailor-made for selecting HTML elements and performing some action on the element(s).

Basic syntax is: $(selector).action()

* A $ sign to define/access jQuery
* A (selector) to "query (or find)" HTML elements
* A jQuery action() to be performed on the element(s)
Examples:

$(this).hide() - hides the current element.

$("p").hide() - hides all <p> elements.

$(".test").hide() - hides all elements with class="test".

$("#test").hide() - hides the element with id="test".

Are you familiar with CSS selectors?

jQuery uses CSS syntax to select elements. You will learn more about the selector syntax in the next chapter of this tutorial.

The Document Ready Event

You might have noticed that all jQuery methods in our examples, are inside a document ready event:

$(document).ready(function(){

// jQuery methods go here...

});
This is to prevent any jQuery code from running before the document is finished loading (is ready).

It is good practice to wait for the document to be fully loaded and ready before working with it. This also allows you to have your JavaScript code before the body of your document, in the head section.

Here are some examples of actions that can fail if methods are run before the document is fully loaded:

* Trying to hide an element that is not created yet
* Trying to get the size of an image that is not loaded yet
Tip: The jQuery team has also created an even shorter method for the document ready event:

$(function(){

// jQuery methods go here...

});
Use the syntax you prefer. We think that the document ready event is easier to understand when reading the code.

```
Reading this makes our code super easy to understand, in our code, **setTimeout(updateOrders, 5000);** is the way to helpe us looping around this code. 
**$(document).ready($('#myDynamicTable').html(addTable(data)))**, this one has a **addTable()** which is the function draws the table for us. **$** direct our code to jQuery and get document object, when it's ready, we find the element id of myDynamicTable using **#**. After this we recall function **html** to finish this whole mission.

That's it. Data flows from URL to our page and formed by designed ways.

Another way is worth to mention is how to handle **js** at backend, how does **html** file work with **js** code. This is basic problem, the answer is as long as you put **html, JS** together then browser will finish the job for you, like how to layout and what to show, or how to interact with user.

At Django, we place all **JS** code at **/static** file, and **html** will load code from there, in oder to do so, we need to use tag **{% load static %}**, then get the specific **JS** using ** :
```javascript 
<script src="{% static "formsUpdate/updateTable.js" %} "></script>
```

I put the Django file structure below:
![image](https://cloud.githubusercontent.com/assets/13154388/23378545/7eb34966-fd01-11e6-97a4-9339cec4ace7.png)

Combine these file structure and the code I put here, then you could see the clear way to do so.

I also put the full **JS** code in this repo.



