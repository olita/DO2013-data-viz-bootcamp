## Exercise 2

`python -m SimpleHTTPServer` 

We want to go from [here](http://0.0.0.0:8000/d3example/), to [here](http://0.0.0.0:8000/histogram/index.html).

1. In their example, the data is generated from an Irwil-Hall distribution (if you remember what a probability distribution is from statistics class).  Basically it's a list of numbers between 0 < 1 that looks like this:

    <pre>
    0.38913265748415143
	0.3833155121654272
	0.356871722638607
	0.5699867873452604
	0.5243100179592147
	0.4786530909128487
	0.669452527211979
	...
    </pre>

2. Now, let's look at our MARCHASH data:

    ```json
    {
      "fields": [
      [ "005", "19980622213320.3" ], 
      [ "008", "860530s1986  onc     001   eng d" ], 
      [ "020", " ", " ", [ [ "a", "0195405080" ], [ "c", "$29.95" ] ] ], 
      [ "040", " ", " ", [ [ "a", "CaOTU" ], [ "b", "eng" ], [ "c", "CaOONL" ], [ "d", "WaOLN" ], [ "d", "CaOTR" ] ] ], 
      [ "043", " ", " ", [ [ "a", "n-cn-on" ] ] ], 
      [ "050", " ", "0", [ [ "a", "FC3097.7" ], [ "b", ".D46 1986" ] ] ], 
      [ "082", "0", "0", [ [ "a", "971.3/54119" ] ] ], 
      [ "100", "1", " ", [ [ "a", "Dendy, William," ], [ "d", "1948-" ] ] ], 
      [ "245", "1", "0", [ [ "a", "Toronto observed :" ], [ "b", "its architecture, patrons, and history /" ], [ "c", "William Dendy & William Kilbourn ; photographs by Bruce Litteljohn & William Dendy." ] ] ], 
      [ "260", " ", " ", [ [ "a", "Toronto [Ont.] :" ], [ "b", "Oxford University Press," ], [ "c", "1986." ] ] ], 
      [ "300", " ", " ", [ [ "a", "327 p. :" ], [ "b", "ill." ] ] ], 
      [ "500", " ", " ", [ [ "a", "Includes index." ] ] ], 
      [ "650", " ", "0", [ [ "a", "Architecture" ], [ "z", "Ontario" ], [ "z", "Toronto" ], [ "x", "History." ] ] ], 
      [ "650", " ", "0", [ [ "a", "Architects and patrons" ], [ "z", "Ontario" ], [ "z", "Toronto" ], [ "x", "History." ] ] ], 
      [ "650", " ", "0", [ [ "a", "Historic buildings" ], [ "z", "Ontario" ], [ "z", "Toronto." ] ] ], 
      [ "651", " ", "0", [ [ "a", "Toronto (Ont.)" ], [ "x", "Buildings, structures, etc." ], [ "x", "History." ] ] ], 
      [ "700", "1", " ", [ [ "a", "Kilbourn, William," ], [ "d", "1926-1995." ] ] ], 
      [ "700", "1", " ", [ [ "a", "Litteljohn, Bruce M.," ], [ "d", "1935-" ] ] ], 
      [ "907", " ", " ", [ [ "a", ".b10259892" ], [ "b", "main6" ], [ "c", "-" ] ] ], 
      [ "902", " ", " ", [ [ "a", "130224" ] ] ], 
      [ "998", " ", " ", [ [ "b", "4" ], [ "c", "991001" ], [ "d", "m" ], [ "e", "b" ], [ "f", "-" ], [ "g", "0" ] ] ], 
      [ "935", " ", " ", [ [ "a", "AAC-6982" ] ] ], 
      [ "910", " ", " ", [ [ "d", "jf" ] ] ], 
      [ "945", " ", " ", [ [ "l", "main6" ] ] ], 
      [ "945", " ", " ", [ [ "l", "main6" ] ] ], 
      [ "945", " ", " ", [ [ "l", "main6" ] ]
      ]
      ], "leader": "01158cam a2200337 a 4500", "type": "marc-hash", "version": [ 1, 0 ]
    }
    ```

    This looks _kind_ of like a MARC record, but with a lot of extra punctuation because it's stored in JSON, something d3 can read.
    
    If we wanted to select the year of publication for aggregating in our histogram, what would we use?

	```json
    {
      "fields": [
      [ "260", " ", " ", [ [ "a", "Toronto [Ont.] :" ], [ "b", "Oxford University Press," ], [ "c", "1986." ] ] ], 
      ]
    }
	```
	
3. Let's learn by reading code! What do you think this does?

	```javascript
    var x = d3.scale.linear()
        .domain([min, max])
        .range([0, width]);
	```

4. How about this?
	And this?

	```javascript
    var bar = svg.selectAll(".bar")
        .data(data)
        .enter().append("g")
        .attr("class", "bar")
        .attr("transform", function(d) { return "translate(" + x(d.x) + "," + y(d.y) + ")"; });

    bar.append("rect")
        .attr("x", 1)
        .attr("width", width / (max - min) - 2)
        .attr("height", function(d) { return height - y(d.y); });

    bar.append("text")
        .attr("dy", ".75em")
        .attr("y", 6)
        .attr("x", (width / (max - min)) / 2)
        .attr("text-anchor", "middle")
        .text(function(d) { return formatCount(d.y); });
	```

	What about this?

    ```javascript
    var values = _(data).map(function(record) {
      var field260 = _(record.fields).find(function(field) { return field[0] == "260" })
      var fieldc = _(_(field260).last()).find(function(field) { return field[0] == "c" })
      return _(fieldc).last();
    })
	```

	And this?

    ```javascript
    values = _(values).map(function(value) { return value.match(/\d{4}/)[0] })
    ```

5. Now that we have (at least kinda, hopefully) an idea of how to create a histogram, let's identify some things we'll need to do with our dataset to get the visualization we want.

	How must we modify our data to get this visualization to work?
	Hint: We'll have to ensure our values are __data-centric__

	Why might we have problems using a much larger dataset?
	Hint: We have to process __all__ the data, not just the bits that we're visualizing

	What aspects of a specific data format might make visualization easier?
	Hint: Think about how we select specific pieces of data

6. Now, let's make this happen!

## Resources

If you want to learn more, check out:
    
- [d3 Gallery](https://github.com/mbostock/d3/wiki/Gallery)
- [d3 Tutorial](http://alignedleft.com/tutorials/d3/)
- [vogievetsky](http://vogievetsky.github.io/IntroD3/#1)
- [Theory stuff](http://www.amazon.ca/Designing-Data-Visualizations-Noah-Iliinsky/dp/1449312284/)
- [@Sarah0s - Access 2012](http://www.youtube.com/watch?v=U5gAHdlobsM)
