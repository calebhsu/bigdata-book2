# Input

<input id="show" type="text" value="hello"/> <button id="show">Show text</button>

<input id="setcolor" type="text" value="green"/> <button id="setcolor">Set Background Color</button>

<input id="setheight" type="text" value="200"/> <button id="setheight">Set Height</button>

## Bars (1)

<div style="border:1px grey solid; padding:5px;">
Number: <input id="bars1-number" type="text" value="5"/>
<button id="bars1">Show Bars (1)</button>
</div>

## Bars (2)

<div style="border:1px grey solid; padding:5px;">
Number: <input id="bars2-number" type="text" value="5"/>
Color:  <input id="bars2-color" type="text" value="red"/>
<button id="bars2">Show Bars (2)</button>
</div>

## Bars (3)

<div style="border:1px grey solid; padding:5px;">
Number: <input id="bars3-number" type="text" value="5"/>
Color:  <input id="bars3-color" type="text" value="red"/>
Height:  <input id="bars3-height" type="text" value="50"/>
<button id="bars3">Show Bars (3)</button>
</div>


## Viz

<div class="myviz" style="width:100%; height:100px; border: 1px black solid;">
</div>


{% script %}

$('button#show').click(function(){    
    var value = $('input#show').val()    
    console.log(value)
    $('.myviz').html(value)
})

$('button#setcolor').click(function(){
    // Set the background color of the viz window to the specified color    
    var value = $('input#setcolor').val() 
    $('.myviz').css('background-color', value)
})

// Event handler for "Set Height" button to set the height of the
// viz window to the specified value
$('button#setheight').click(function(){
    var value = parseInt($('input#setheight').val()) 
    $('.myviz').height(value)
})

// Event handler for "Show Bars (1)" to display a specified number of
// vertical bars
$('button#bars1').click(function(){   
    var svg = "<svg>"
    for (i = 0; i < parseInt($('input#bars1-number').val()); i++) { 
        svg += "<rect height='50' width='10' x='" + i * 20 + "'/>" 
    }
    svg += "</svg>"
    $('.myviz').html(svg)    
})

// Event handler for "Show Bars (2)" to display a specified number of
// vertical bars in the specified color
$('button#bars2').click(function(){   
    var color = $('input#bars2-color').val()
    var svg = "<svg style='fill:" + color + ";'>"
    for (i = 0; i < parseInt($('input#bars2-number').val()); i++) { 
        svg += "<rect height='50' width='10' x='" + i * 20 + "'/>" 
    }
    svg += "</svg>"
    $('.myviz').html(svg)    
})

// Event handler for "Show Bars (3)" to display a specified number of
// vertical bars in the specified color at the specified height
$('button#bars3').click(function(){   
    var color = $('input#bars3-color').val()
    var height = $('input#bars3-height').val()
    
    var svg = "<svg style='fill:" + color + ";'>"
    for (i = 0; i < parseInt($('input#bars3-number').val()); i++) { 
        svg += "<rect height='" + height + "' width='10' x='" + i * 20 + "'/>" 
    }
    svg += "</svg>"
    $('.myviz').html(svg)    
})

{% endscript %}
