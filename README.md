# ajaxWithRealTimeDeviceReading

##What's ajax

>Ajax (also AJAX; /ˈeɪdʒæks/; short for asynchronous JavaScript and XML)[1][2][3] is a set of web development techniques using many web >technologies on the client-side to create asynchronous Web applications. With Ajax, web applications can send data to and retrieve from a >server asynchronously (in the background) without interfering with the display and behavior of the existing page. By decoupling the data >interchange layer from the presentation layer, Ajax allows for web pages, and by extension web applications, to change content >dynamically without the need to reload the entire page.[4] In practice, modern implementations commonly substitute JSON for XML due to >the advantages of being native to JavaScript.[5]

>Ajax is not a technology, but a group of technologies. HTML and CSS can be used in combination to mark up and style information. The DOM >is accessed with JavaScript to dynamically display – and allow the user to interact with – the information presented. JavaScript and the >XMLHttpRequest object provide a method for exchanging data asynchronously between browser and server to avoid full page reloads.

Above is from Wikipedia, it means Ajax is more like an idea instead of a hard core technique, which means the chanlleng of Ajax is how to orgaized techniques such as HTML, Javascript/jQuery together.

To make things clear, I work on 2 streams, data flow and control method.

###First data flow
Ajax visit certain address/URL and bring back data to user. Then user use javascript to porcess this data in order to show these data properly.

In order to visit URL, jQuery give us methods such as **get()**, **post()** and **ajax()**. Based on the API of **jQuery.ajax( [settings ] )**, we should set up the datatype of the target address such as JSON, then if the visit success, then we can process then at the success method.

Target URL and ajax datatype settings should match up, otherwise the return data will be hard to manage. Normally lots of public API provides JSON based API. In our case, we just create a **HttpRespon** at Django framework,

'''python 
@ views.py
from django.http import HttpResponse
def realtime_data_api(request):
	if request.method == 'GET':
		rows = getRealdate()
		return HttpResponse(json.dumps(rows),content_type="application/json")
    
    
@ urls.py
url(r'^realtime/$', disply_1.views.realtime_data_api, name='realtime'), #add this line in urls.py setting
'''
this funtion


