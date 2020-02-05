<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
  <meta http-equiv="Content-Style-Type" content="text/css">
  <title></title>
  <meta name="Generator" content="Cocoa HTML Writer">
  <meta name="CocoaVersion" content="1671.4">
  <style type="text/css">
    p.p1 {margin: 0.0px 0.0px 0.0px 0.0px; font: 17.0px Courier; color: #eae96f; -webkit-text-stroke: #eae96f; background-color: #2e2e2e}
    span.s1 {font-kerning: none}
  </style>
</head>
<body>
<p class="p1"><span class="s1">&lt;!DOCTYPE html&gt;</span></p>
<p class="p1"><span class="s1">&lt;html&gt;</span></p>
<p class="p1"><span class="s1">&lt;body&gt;</span></p>
<p class="p1"><span class="s1">&lt;h1&gt;Hello World&lt;/h1&gt;</span></p>
<p class="p1"><span class="s1">&lt;p&gt;I'm hosted with GitHub Pages.&lt;/p&gt;</span></p>
<p class="p1"><span class="s1">&lt;/body&gt;</span></p>
<p class="p1"><span class="s1">&lt;/html&gt;</span></p>
</body>
</html>
    
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
