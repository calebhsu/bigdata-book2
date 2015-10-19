# UI

Pick one question class and build an exploratory visualization interface for it.
The question class you pick must have at least three variables that can be changed.

## How many business with more than X reviews have a rating of Y stars in Z state?

<div style="border:1px grey solid; padding:5px;">
    <div><h5>Reviews</h5>
        <input id="arg1" type="number" value="50"/>
    </div>
    <div><h5>Stars</h5>
        <input id="arg2" type="number" value="3"/>
    </div>
    <div><h5>State</h5>
        <input id="arg3" type="text" value="AZ"/>
    </div>    
    <div style="margin:20px;">
        <button id="viz">Vizualize</button>
    </div>
</div>


<div class="myviz" style="width:100%; height:500px; border: 1px black solid; padding: 5px;">
Data is not loaded yet
</div>

{% script %}
items = 'not loaded yet'

$.get('http://bigdatahci2015.github.io/data/yelp/yelp_academic_dataset_business.5000.json.lines.txt')
    .success(function(data){        
        var lines = data.trim().split('\n')

        // convert text lines to json arrays and save them in `items`
        items = _.map(lines, JSON.parse)

        console.log('number of items loaded:', items.length)

        console.log('first item', items[0])
     })
     .error(function(e){
         console.error(e)
     })

function viz(arg1, arg2, arg3){    

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect x="9"   \
                         width="${d.width}" \
                         height="23"    \
                         style="fill:${d.color};    \
                                stroke-width:0; \
                                stroke:rgb(0,0,0)" />   \
                    <text x="10" y="20">${d.label}</text> \
                    </g>'

    
    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {        
        return d[1].length * 1.5
    }

    function computeY(d, i) {
        return i * 26
    }

    function computeColor(d, i) {
        return 'orange'
    }

    function computeLabel(d, i) {
        return d[0] + ": "+ d[1].length
    }

    // Filter by sate
    var state = _.filter(items, function(d) {
        return d.state == arg3
    })
    
    // Filter by star rating (>=)
    var star = _.filter(state, function(d) {
        return d.stars >= arg2
    })
    
    // Filter by review count
    var rev = _.filter(star, function(d) {
        return d.review_count >= arg1
    })
    
    rev = _.groupBy(rev, 'city')
    rev = _.pairs(rev)

    rev = _.sortBy(rev, function(d){ 
        return d[1].length
    }).reverse() 

    rev = _.take(rev, 20)

    var viz = _.map(rev, function(d, i){                
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

    $('.myviz').html('<svg width="100%" height="100%">' + result + '</svg>')
}

$('button#viz').click(function(){    
    var arg1 = $('input#arg1').val()
    var arg2 = $('input#arg2').val()    
    var arg3 = $('input#arg3').val()    
    viz(arg1, arg2, arg3)
})  

{% endscript %}

# Authors

This UI is developed by
* [Caleb Hsu](https://github.com/calebhsu/)
* [Andrew Linenfelser](https://github.com/Linenfelser)
* [Zhili Yang](https://github.com/zhya215)
* [Andrey Shprengel](https://github.com/AndreyShprengel)
* [Andrew Berumen](https://github.com/anbe6083)