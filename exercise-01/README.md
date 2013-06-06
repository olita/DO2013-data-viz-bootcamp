## Exercise 1

`python -m SimpleHTTPServer` 

We want to go from [here](http://0.0.0.0:8000/d3example/), to [here](http://0.0.0.0:8000/pie-chart/final.html).

1. Let's look at their dataset first:

    <pre>
    age,population
    <5,2704659
    5-13,4499890
    14-17,2159981
    18-24,3853788
    25-44,14106543
    45-64,8819342
    ≥65,612463
    </pre>

2. Now, let's look at the first few lines of our dataset:

    <pre>
    identifier;identifier_citation;title;description;contributor_author;date_created;date;date_issued_ymd;date_issued_ym;date_issued_y;type;format;format2;relation;relation_isformatof;assignment_envelope;relation_ispartof;relation_ispartofseries;relation_ispartofseries2;relation_ispartofseries3;relation_ispartofseries4;subject;subject_01;subject_02;;;;;
    ASC00001;1974-002 / 317;Mrs. Roy Abrahamson : author;Portrait;;2003-12-03;;1966-10-26;;;;;;;;;Toronto Telegram fonds, F0433;Toronto Telegram;;;;;;;;;;;
    ASC00002;1974-002 / 111;Isaacs Gallery : Polychrome construction show;Image of Joyce Wieland standing on a bench looking down at an art installation;Kennedy;2010-06-30;10 March 1965;1965-03-13;;;;1 photograph : b&w negative ; 35mm;3200 dpi .tif and jpeg;One neg (#) scanned out of 29 in assignment.;;;Toronto Telegram fonds, F0433;Toronto Telegram;;;;;;;;;;
    ASC00003;1974-002 / 111;Isaacs Gallery : Donald Judd constructions;Image of artwork in the gallery;;2003-12-03;;1965-03-26;;;;;;;;;Toronto Telegram fonds, F0433;Toronto Telegram;;;;;;;;;;;
    ASC00004;1974-002 / 111;Isaacs Gallery : Tony Uquhart exhibition;Image of a group of men in conversation with a sculpture in the foreground;;2003-12-03;;1965-05-14;;;;;;;;;Toronto Telegram fonds, F0433;Toronto Telegram;;;;;;;;;;;
    ASC00005;1974-002 / 111;Isaacs Gallery : Harold Town and Arthur Handy;Image of man [Arthur Handy] standing in the middle of the gallery;;2003-12-03;;1966-01-29;;;;;;;;;Toronto Telegram fonds, F0433;Toronto Telegram;;;;;;;;;;;
    </pre>

    Pretty ugly, eh? 
    
    If we wanted to show the number of items in a given collection, which column could we use?

3. Now, let's learn by reading code! What do you think this does?

    <pre>
    var width = 960,
        height = 500,
        radius = Math.min(width, height) / 2;
    </pre>

4. How about this?

    ```javascript
    var color = d3.scale.ordinal()
        .range(["#98abc5", "#8a89a6", "#7b6888", "#6b486b", "#a05d56", "#d0743c", "#ff8c00"]);
    ```

    and this?

    ```javascript
    var arc = d3.svg.arc()
        .outerRadius(radius - 10)
        .innerRadius(0);

    var pie = d3.layout.pie()
              .sort(null)
              .value(function(d) { return d.population; });
    ```

    and this?

    ```javascript
    var svg = d3.select("body").append("svg")
        .attr("width", width)
        .attr("height", height)
        .append("g")
        .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");
    ```

    and this?

    ```javascript
    d3.csv("data/data.csv", function(error, data) {

      data.forEach(function(d) {
        d.population = +d.population;
    });
    ```

    finally, this?

    ```javascript
    var g = svg.selectAll(".arc")
        .data(pie(data))
        .enter().append("g")
        .attr("class", "arc");

    g.append("path")
        .attr("d", arc)
        .style("fill", function(d) { return color(d.data.age); });

    g.append("text")
        .attr("transform", function(d) { return "translate(" + arc.centroid(d) + ")"; })
        .attr("dy", ".35em")
        .style("text-anchor", "middle")
        .text(function(d) { return d.data.age; });
    ```

5. Now that we have (at least kinda, hopefully) an idea of how to create a pie chart, let's identify some things we'll need to do with our dataset to get the visualization we want; the number of numbers in each collection.

   - What do we need plug in from our dataset?
   - Do we need to create any new values?
    Hint: We'll need to figure out a way to get the uniq values in a given column
    Hint: We'll need to count those how many their are for each uniq value
    Hint: You should totally use underscore!

6. Now, let's make this happen!

## Resources

If you want to learn more, check out:
    
- [d3 Gallery](https://github.com/mbostock/d3/wiki/Gallery)
- [d3 Tutorial](http://alignedleft.com/tutorials/d3/)
- [vogievetsky](http://vogievetsky.github.io/IntroD3/#1)
- [Theory stuff](http://www.amazon.ca/Designing-Data-Visualizations-Noah-Iliinsky/dp/1449312284/)
- [@Sarah0s - Access 2012](http://www.youtube.com/watch?v=U5gAHdlobsM)
  

We will probably what a better data format to work with; json. BOET.
