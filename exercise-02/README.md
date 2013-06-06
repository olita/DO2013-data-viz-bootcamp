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
	
	We have to identify the exact __location of__ the specific bit of data that we're interested in.
	
3. Let's learn by reading code! What do you think this does?

    ```javascript
    var width = 960,
        height = 500,
        radius = Math.min(width, height) / 2;
	```

4. How about this?

    ```javascript
    var color = d3.scale.ordinal()
        .range(["#98abc5", "#8a89a6", "#7b6888", "#6b486b", "#a05d56", "#d0743c", "#ff8c00"]);
    ```

5. Now that we have (at least kinda, hopefully) an idea of how to create a pie chart, let's identify some things we'll need to do with our dataset to get the visualization we want; the number of numbers in each collection.

    What do we need plug in from our dataset?
    Do we need to create any new values?
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
