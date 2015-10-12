# Pokemon

Create interactive visualizations for the Pokemon data. Complete the script
so that when a user clicks on each menu button, there will be a different
bar chart shown in the Viz block.

You will be reusing the code you wrote for generating SVG bar charts a couple weeks
ago. The main challenge is to figure out how to connect your visualization code
with the front-end code (event handlers ... etc).

## Menu

<button id="viz-horizontal">Attack (Horizontal Bars)</button>
<button id="viz-vertical">Attack (Vertical Bars)</button>
<button id="viz-attack-defense">Attack vs. Defense</button>
<button id="viz-speed-defense">Speed vs. Defense</button>
<button id="viz-horizontal-sorted">Attack (sorted from low to high)</button>
<button id="viz-horizontal-sorted-desc">Attack (sorted from high to low)</button>
<button id="viz-attack-speed">Attack (width) vs. Speed (color)</button>

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


function vizAsHorizontalBars(){

    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d.Attack
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i)
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
$('button#viz-horizontal').click(vizAsHorizontalBars)


function vizAsVerticalBars(){

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect x="${d.x}"  \
                         y="${d.y}"  \
                         width="20" \
                         height="${d.height}"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return i * 20
    }

    function computeHeight(d, i) {
        return d.Attack
    }

    function computeY(d, i) {
        return 0
    }

    function computeColor(d, i) {
        return 'red'
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    height: computeHeight(d, i),
                    color: computeColor(d, i)
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
$('button#viz-vertical').click(vizAsVerticalBars)


function attackVsDefense(){

    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
                        <rect  \
                             x="-${d.attack}"  \
                             width="${d.attack}"  \
                             height="20"  \
                             style="fill:${d.colorAtk};  \
                                    stroke-width:1;  \
                                    stroke:rgb(0,0,0)" />  \
                        <rect  \
                             x="0"  \
                             width="${d.defense}"  \
                             height="20"  \
                             style="fill:${d.colorDef};  \
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

    function computeAttack(d, i) {
        return d.Attack
    }

    function computeDefense(d, i) {
        return d.Defense
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeAttackColor(d, i) {
        return 'red'
    }

    function computeDefenseColor(d, i) {
        return 'blue'
    }

    function computeLabel(d, i) {
        return d.Name
    }

    var viz = _.map(pokemonData, function(d, i){
        return {
            x: computeX(d, i),
            y: computeY(d, i),
            attack: computeAttack(d, i),
            defense: computeDefense(d, i),
            colorAtk: computeAttackColor(d, i),
            colorDef: computeDefenseColor(d, i),
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
$('button#viz-attack-defense').click(attackVsDefense)


function speedVsDefense(){

    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
                        <rect  \
                             x="-${d.speed}"  \
                             width="${d.speed}"  \
                             height="20"  \
                             style="fill:${d.colorSpd};  \
                                    stroke-width:1;  \
                                    stroke:rgb(0,0,0)" />  \
                        <rect  \
                             x="0"  \
                             width="${d.defense}"  \
                             height="20"  \
                             style="fill:${d.colorDef};  \
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

    function computeSpeed(d, i) {
        return d.Speed
    }

    function computeDefense(d, i) {
        return d.Defense
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeSpeedColor(d, i) {
        return 'green'
    }

    function computeDefenseColor(d, i) {
        return 'blue'
    }

    function computeLabel(d, i) {
        return d.Name
    }

    var viz = _.map(pokemonData, function(d, i){
        return {
            x: computeX(d, i),
            y: computeY(d, i),
            speed: computeSpeed(d, i),
            defense: computeDefense(d, i),
            colorSpd: computeSpeedColor(d, i),
            colorDef: computeDefenseColor(d, i),
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
$('button#viz-speed-defense').click(speedVsDefense)


function attackAscending(){

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
        return d.Attack
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

    var sortedData = _.sortBy(pokemonData, function(d) {
        return d.Attack
    })

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
$('button#viz-horizontal-sorted').click(attackAscending)


function attackDescending(){

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
        return d.Attack
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

    var sortedData = _.sortBy(pokemonData, function(d) {
        return d.Attack
    }).reverse()

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
$('button#viz-horizontal-sorted-desc').click(attackDescending)


function attackSpeedColor(){

    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:1; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(${d.width + 10} 15)">  \
                        ${d.label}  \
                    </text>  \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d.Attack
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        var maxSpd = _.max(_.pluck(pokemonData, 'Speed'))
        var red = (d.Speed / maxSpd) * 255
        return 'rgb(' + Math.round(red) + ',0,0)'
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
$('button#viz-attack-speed').click(attackSpeedColor)

{% endscript %}
