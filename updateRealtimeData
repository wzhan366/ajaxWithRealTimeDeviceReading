
function updateRealtimeData() {
        $.ajax({                                      
            url: '/realtime',                             
            dataType: 'json',
            type: 'GET',
            success: function (data) {
              // return data['articles']
                setTimeout(updateRealtimeData, 5000);
                console.log(data)
                $(document).ready($('#myDynamicTable').html(addTable(data)))
                var firstColumnHeader = $('#myDynamicTable thead th:first-child');
                firstColumnHeader.css('background', '#FCD116');
            } 
        });
    }


function addTable(data) {
    var myTableDiv = document.createElement("myDynamicTable");
    var table = document.createElement('TABLE');
    table.border = '1';

    var tableBody = document.createElement('TBODY');
    table.appendChild(tableBody);

    var data_len = data.length
    var table_head = ['Device Name', 'Temperature', 'Humidity', 'Brightness']

    console.log(data)
    var entry_len = table_head.length

    var tr = document.createElement('TR');
    tableBody.appendChild(tr);
    for (var j = 0; j < entry_len; j++) {
        var td = document.createElement('TD');
        td.bold
        td.appendChild(document.createTextNode(table_head[j]));
        tr.appendChild(td);
    }

    for (var i = 0; i < data_len; i++) {
        var tr = document.createElement('TR');
        tableBody.appendChild(tr);
        var entry = [data[i][0], data[i][1], data[i][2], data[i][3]]
        for (var j = 0; j < entry_len; j++) {
            var td = document.createElement('TD');
            td.appendChild(document.createTextNode(entry[j]));
            tr.appendChild(td);
        }
    }
    return myTableDiv.appendChild(table);
};



$(document).ready(updateOrders);

$( document ).ajaxError(function( event, request, settings ) {
  $( "#msg" ).append( "<li>Error requesting page " + settings.url + "</li>" );
});
