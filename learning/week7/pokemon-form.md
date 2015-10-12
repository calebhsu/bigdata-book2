# Pokemon Form

In the previous page, the selection of which attributes to visualize were
hard coded. Let's make our viz more interactive and flexible by providing
forms for users to select which attributes to visualize dynamically.

## Menu

### One attribute

<div style="border:1px grey solid; padding:5px;">
Attribute (e.g., Attack): <input id="pokemon-attribute-name" type="text" value="Attack"/>
<button id="viz-horizontal">Horizontal Bars</button>
</div>

### Comparing two attributes

<div style="border:1px grey solid; padding:5px;">
Attribute 1: <input id="pokemon-attribute-name-1" type="text" value="Attack"/>
Attribute 2: <input id="pokemon-attribute-name-2" type="text" value="Attack"/>
<button id="viz-compare">Compare Side-by-Side</button>
</div>

### One attribute with sorting

<div style="border:1px grey solid; padding:5px;">
Attribute: <input id="pokemon-sorted-attribute-name" type="text" value="Attack"/>
<button id="viz-horizontal-sorted-asc">Ascending</button>
<button id="viz-horizontal-sorted-desc">Descending</button>
</div>

### Three attributes

<div style="border:1px grey solid; padding:5px;">
Width 1: <input id="pokemon-three-width1" type="text" value="Attack"/>
Width 2: <input id="pokemon-three-width2" type="text" value="Defense"/>
Color: <input id="pokemon-three-color" type="text" value="Speed"/>
<button id="viz-three">Viz Three Attributes</button>
</div>

## Viz

<div class="myviz" style="width:100%; height:500px; border: 1px black solid;">
Data is not loaded yet
</div>

{% script %}
// pokemonData is a global variable
pokemonData = 'not loaded yet'

$.get('/data/pokemon-small.json')
 .success(function(data){
     console.log('data loaded', data)
     $('.myviz').html('number of records loaded: ' + data.length)
     pokemonData = data          
 })


function vizAsHorizontalBars(attributeName){

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:1; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(${d.width + 10} 15)">  \
                        ${d.label}: ${d.width}  \
                    </text>  \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {        
        return i * 20 + d[$('input#pokemon-attribute-name').val()]
    }

    function computeY(d, i) {
        return i * 25
    }

    function computeColor(d, i) {
        return 'red'
    }

    function computeLabel(d, i) {
        return d.Name
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}
$('button#viz-horizontal').click(function(){
    var attributeName = $('input#pokemon-attribute-name').val()  
    vizAsHorizontalBars(attributeName)
})  


function vizSideBySide(attributeName1, attributeName2){
    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
                        <rect  \
                             x="-${d.a1}"  \
                             width="${d.a1}"  \
                             height="20"  \
                             style="fill:${d.colorA1};  \
                                    stroke-width:1;  \
                                    stroke:rgb(0,0,0)" />  \
                        <rect  \
                             x="0"  \
                             width="${d.a2}"  \
                             height="20"  \
                             style="fill:${d.colorA2};  \
                                    stroke-width:1;  \
                                    stroke:rgb(0,0,0)" />  \
                        <text transform="translate(0 15)">  \
                            ${d.label}  \
                        </text>  \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeAttrib1(d, i) {
        return d[$('input#pokemon-attribute-name-1').val()]
    }

    function computeAttrib2(d, i) {
        return d[$('input#pokemon-attribute-name-2').val()]
    }

    function computeY(d, i) {
        return i * 25
    }

    function computeAttrib1Color(d, i) {
        return 'red'
    }

    function computeAttrib2Color(d, i) {
        return 'blue'
    }

    function computeLabel(d, i) {
        return d.Name
    }

    var viz = _.map(pokemonData, function(d, i){
        return {
            x: computeX(d, i),
            y: computeY(d, i),
            a1: computeAttrib1(d, i),
            a2: computeAttrib2(d, i),
            colorA1: computeAttrib1Color(d, i),
            colorA2: computeAttrib2Color(d, i),
            label: computeLabel(d, i) 
        }
    })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}
$('button#viz-compare').click(function(){    
    var attributeName1 = $('input#pokemon-attribute-name-1').val()  
    var attributeName2 = $('input#pokemon-attribute-name-2').val()  
    vizSideBySide(attributeName1, attributeName2)
})  


function vizAsSortedHorizontalBars(attributeName, sortDirection){
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:1; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(${d.width + 10} 15)">  \
                        ${d.label}: ${d.width}  \
                    </text>  \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d[$('input#pokemon-sorted-attribute-name').val()]
    }

    function computeY(d, i) {
        return i * 25
    }

    function computeColor(d, i) {
        return 'red'
    }

    function computeLabel(d, i) {
        return d.Name
    }

    if (sortDirection == 'asc'){
        sortedData = _.sortBy(pokemonData, function(d) {
            return d[$('input#pokemon-sorted-attribute-name').val()]
        })
    }
    else {
        sortedData = _.sortBy(pokemonData, function(d) {
            return d[$('input#pokemon-sorted-attribute-name').val()]
        }).reverse()
    }

    var viz = _.map(sortedData, function(d, i){
        return {
            x: computeX(d, i),
            y: computeY(d, i),
            width: computeWidth(d, i),
            color: computeColor(d, i),
            label: computeLabel(d, i) 
        }
     })  

    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}
$('button#viz-horizontal-sorted-asc').click(function(){    
    var attributeName = $('input#pokemon-sorted-attribute-name').val()
    var sortDirection = 'asc'
    vizAsSortedHorizontalBars(attributeName, sortDirection)
})
$('button#viz-horizontal-sorted-desc').click(function(){    
    var attributeName = $('input#pokemon-sorted-attribute-name').val()
    var sortDirection = 'desc'
    vizAsSortedHorizontalBars(attributeName, sortDirection)
})  


// TODO: complete the code below
// visualize three attributes, the first two attributes as side-by-side bar charts
// using bar widths to represent attribute values, and the third attribute's value
// is represented using the red color brightness
function vizThreeAttributes(attributeName1, attributeName2, attributeName3){
    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
                        <rect  \
                             x="-${d.w1}"  \
                             width="${d.w1}"  \
                             height="20"  \
                             style="fill:${d.color};  \
                                    stroke-width:1;  \
                                    stroke:rgb(0,0,0)" />  \
                        <rect  \
                             x="0"  \
                             width="${d.w2}"  \
                             height="20"  \
                             style="fill:${d.color};  \
                                    stroke-width:1;  \
                                    stroke:rgb(0,0,0)" />  \
                        <text transform="translate(0 15)">  \
                            ${d.label}  \
                        </text>  \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeW1(d, i) {
        return d[$('input#pokemon-three-width1').val()]
    }

    function computeW2(d, i) {
        return d[$('input#pokemon-three-width2').val()]
    }

    function computeY(d, i) {
        return i * 25
    }

    function computeColor(d, i) {
        var maxAttrib = _.max(_.pluck(pokemonData, $('input#pokemon-three-color').val()))
        var red = (d[$('input#pokemon-three-color').val()] / maxAttrib) * 255
        return 'rgb(' + Math.round(red) + ',0,0)'
        
    }

    function computeLabel(d, i) {
        return d.Name
    }

    var viz = _.map(pokemonData, function(d, i){
        return {
            x: computeX(d, i),
            y: computeY(d, i),
            w1: computeW1(d, i),
            w2: computeW2(d, i),
            color: computeColor(d, i),
            label: computeLabel(d, i) 
        }
    })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

$('button#viz-three').click(function(){    
    var attributeName1 = $('input#pokemon-three-width1').val()
    var attributeName2 = $('input#pokemon-three-width2').val()
    var attributeName3 = $('input#pokemon-three-color').val()
    vizThreeAttributes(attributeName1, attributeName2, attributeName3)
})  


{% endscript %}
