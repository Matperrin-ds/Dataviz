<!DOCTYPE html>
<style>



</style>
<body>
<script src="https://d3js.org/d3.v4.min.js"></script>
<script>


var margin = {top: 20, right: 30, bottom: 30, left: 40},
    width = 960 - margin.left - margin.right,
    height = 500 - margin.top - margin.bottom;

var y = d3.scaleLinear()
    .rangeRound([height, 0])
		.nice();

var x = d3.scaleBand()
    .rangeRound([0, width])
    .paddingInner(0.05)
    .align(0.1)
    
var z = d3.scaleOrdinal(d3.schemeCategory20)

  // Date format https://bl.ocks.org/zanarmstrong/ca0adb7e426c12c06a95
  var parseTime  = d3.timeParse("%b %Y")
  
  
    // Load stocks data
  // Ex: 0: {symbol: "MSFT", date: "Jan 2000", price: "39.81"}
  d3.csv('https://raw.githubusercontent.com/LyonDataViz/MOS5.5-Dataviz/master/data/stocks.csv', function(error, raw) {
    
    var symbols = [];
    var data = []
    
    // Data pre-processing
    raw.forEach(function(d, i) {
      
      if(symbols.indexOf(d.symbol) < 0) {
        symbols.push(d.symbol)
        data[symbols.indexOf(d.symbol)] = [];
      }
      
      // String to INT
      d.value = +d.price;     
 
      // Parsing time
      d.date = parseTime(d.date)
      data[symbols.indexOf(d.symbol)].push(d);
    });
    
    var data_nest = d3.nest()
    	.key(function(d) { return d.date.getFullYear(); })
    	.key(function(d) { return d.symbol; })
    	.rollup(function(v) { return d3.sum(v, function(d) { return d.price; }); })
			.entries(raw);
    
    var years = data_nest.map(function(d) { return d.key; })
    
    // d3-stack format https://github.com/d3/d3-shape/blob/master/README.md#stack_keys
    // {MSFT: 356.07999999999987, AMZN: 527.17, IBM: 1162.97, AAPL: 260.98}
    var data_stack = []
    
    data_nest.forEach(function(d, i) {
      d.values = d.values.map(function(e) { return e.value; })
      var t ={}
      symbols.forEach(function(e, i) {
        t[e] = d.values[i]
      })
      t.year = d.key;
      data_stack.push(t)
    })
    
    console.log(data_stack)
    
    var layers = d3.stack().keys(symbols)(data_stack);

    var max = d3.max(layers[layers.length-1], function(d) { return d[1]; });
    
    y.domain([0, max]);
    x.domain(years);
    
    console.log(data_stack)
    
    var svg = d3.select("body").append("svg")
        .attr("width", width + margin.left + margin.right)
        .attr("height", height + margin.top + margin.bottom)
      .append("g")
        .attr("transform", "translate(" + margin.left + "," + margin.top + ")");

    svg.append("g").selectAll("g")
          .data(layers)
      .enter().append("g")
        .style("fill", function(d) { return z(d.key); })	
        .selectAll("rect")
      .data(function(d) {  return d; })
        .enter().append("rect")
          .attr("x", function(d, i) { return x(d.data.year); })
          .attr("y", function(d) { return y(d[1]); })
          .attr("height", function(d) { return y(d[0]) - y(d[1]); })
          .attr("width", x.bandwidth());
    
    svg.append("g")
      .attr("class", "x axis")
      .attr("transform", "translate(0," + height + ")")
      .call(d3.axisBottom(x))
    
    svg.append("g")
      .attr("class", "y axis")
      .attr("transform", "translate(" + (0) + ", 0)")
      .call(d3.axisLeft().scale(y))
    
    })
  
</script>tegories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
