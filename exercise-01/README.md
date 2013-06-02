## Exercise 1

1. Let's look at our dataset first

    d3Data.csv

2. We will probably what a better data format to work with. In this case we'll use json.

    d3Data.json


<pre>

 //need variable for uniq relation_ispartofseries column values
  //need variable for count of relation_ispartofseries uniq column values
  var convert_data = function(data) {
    var converted = [];

    data.forEach(function(d) {
      var record;

      if(record = _.find(converted, function(obj) { return obj.series_name == d.relation_ispartofseries })) {
        record.num++;
        record = null;
      } else {
        converted.push({
          series_name: d.relation_ispartofseries,
          num:         1
        });
      }
    });

    return converted;
  };

  var width = 960,
      height = 650,
      radius = Math.min(width, height) / 2;

  //maybe we should select some random colours
  var color = d3.scale.ordinal()
  .range(["grey", "steelblue"]);

  var arc = d3.svg.arc()
      .outerRadius(radius)
      .innerRadius(0);

  var pie = d3.layout.pie()
      .sort(null)
      //need to grab uniq values column 'relation_ispartofseries', then count
      .value(function(d) { return d.num; });

  var svg = d3.select("body").append("svg")
      .attr("width", width)
      .attr("height", height)
    .append("g")
      .attr("transform", "translate(" + width / 2 + "," + height / 2 + ")");

  d3.json("../data/d3Data.json", function(error, data) {
    data = convert_data(data);

    data.forEach(function(d) {
      d.num = +d.num;
    });

    //need uniq values of 'relation_ispartofseries'
    var g = svg.selectAll(".arc")
        .data(pie(data))
        .enter().append("g")
        .attr("class", "arc")
        .on("mouseover", function(d) {
          $("#series_name").html(d.data.series_name || "[No series name]");
          $("#num").html(d.data.num);
        });

    g.append("path")
        .attr("d", arc)
        .style("fill", function(d) { return color(d.data.series_name); });
  });
  
</pre>
