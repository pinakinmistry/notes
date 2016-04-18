# D3 (FEM):

var svg = d3.select('#append svg');
svg.append('circle')
	.attr({
	cx: 100,
	cy: 100,
	r: 10,
	fill: '#989829'
	})

d3.select('#select').select('h2')
	.style('background-color', '#dedede');

d3.select('#select svg')
	.append('rect')
	.attr({
	x: 100, y: 100,
	width: 200,height: 10, fill: '#222222'
	})



## Binding API
- select()
- selectAll()
- append()
- data()
- enter()
- exit()
- transition()


var data = [1, 2, 3, 4];
var divs = d3.select('#select .right') 		//select containing element
			 .selectAll('div.items')		//select all items that don't exist yet
			 .data(data);					//bind them with data

divs.enter()
	.append('div').classed('item', true)
	.style({
		width: '20px',
		height: function (d) { return d + 'px' },
		margin: '15px',
		float: 'left',
		'background-color': '#eeffee'
	});

//updated data. exit will work on removed data items
data = [1, 2];
divs = d3.select('#select .right')
		 .selectAll('div.items')
		 .data(data);
divs.exit().style({
	'background-color': 'red'
})

//updated data. transition will get applied while updating dom elements
data = [5, 6, 7, 8];
divs = d3.select('#select .right')
		 .selectAll('div.item')
		 .data(data)
divs.transition().delay(50).duration(1000)
	.style({
		height: function (d) { return d; }
	})

//on dom event
divs.style(...)
	.on('click', function (d, i) {
		d3.select(this)
		  .style('backgound', '#ffdedf')
		  .text(i);
	});

## SVG Circles

var svg = d3.select('svg');
var data = tributary.pics.data.children;
var g = svg.append('g')
			. attr('transform', 'translate(0, -10)');

var circles = g.selectAll('circle')	
			   .data(data);
//data binding
//circles don't exist yet (soles without bodies)

circles.enter()
//for all missing circles, append them
	.append('circle')
	.attr({
		cx: function (d, i) { return i + 70; },
		cy: function (d, i) { return d.data.score; },
		r: 6
	});

## Non binding API

### linear scale
var svg = d3.select('svg');

var data = tributary.pics.data.children
	.sort(function (a, b) {
		return a.data.score - b.data.score;
	})

var maxScore = d3.max(data, function (d) { return d.data.score; })

var yScale = d3.scale.linear()
	.domain([0, maxScore])
	.range([500, 0])


var g = svg.append('g')
	.attr('transform', 'translate(0, -10)')

var circles = g.selectAll('circle')
	.data(data)

circles.enter()
	.append('circle')
	.style({
		cx: function (d, i) { return 49 + i + 15; },
		cy: function (d, i) { return yScale(d.data.score); }
		r: 5
	})
	.on('mouseover', function (d) {
		console.log(d.data.score);
	});

### categorical scale
//bar chart
var svg = d3.select('svg');
var data = tributary.pics.data.children;
var maxScore = d3.max(data, function (d) { return d.data.score; });
var cHeight = 400;

var yScale = d3.scale.linear()
	.domain([0, maxScore])
	.range([0, cHeight]);

var xScale = d3.scale.ordinal()
	.domain(d3.range(data.length))
	.rangeBands([0, 500], 0.5);

var g = svg.append('g')
	.attr('transform', 'translate(10, 100)');
var bars = g.selectAll('rect')
	.data(data);

bars.enter()
	.append('rect')
	.attr({
		x: function (d, i) { return xScale(i); },
		y: function (d, i) { return ch - yScale(d.data.score); },
		width: xScale.rangeBand(),
		height: function (d, i) { return yScale(d.data.score); }
	})

### axis generator

var scale = d3.scale.linear()
	.domain([20, 80])
	.range([0, 400]);

var axis = d3.svg.axis()
	.scale(scale)
	.orient('bottom')
	.ticks(6)
	.tickValues([20, 50, 80])	//tick these specifically

var g = svg.append('g')
axis(g);
g.attr('transform', 'translate(50, 50)')l
g.selectAll('path')
	.style({fill: 'none', stroke: '#000'})
g.selectAll('line')
	.style({stroke: '#000'});

### line generator

var data = [0, 10, 30, 20, 50];
var x = d3.scale.ordinal()
	.domain(d3.range(data.length))
	.rangeBands([10, 450]);

var y = d3.scale.linear()
	.domain(d3.extent(data))
	.range([150, 10]);

var line = d3.svg.line()
	.x(function (d, i) { return x(i); })
	.y(function (d, i) { return y(d); });

svg.append('path')
	.attr('d', line(data))
	.style({
		fill: 'none',
		stroke: '#000'
	});


//line chart with bar chart
var svg = d3.select('svg');
var data = tributary.pics.data.children;
var maxScore = d3.max(data, function (d) { return d.data.score; });
var cHeight = 400;

var yScale = d3.scale.linear()
	.domain([0, maxScore])
	.range([0, cHeight]);

var xScale = d3.scale.ordinal()
	.domain(d3.range(data.length))
	.rangeBands([0, 500], 0.5);

//line
var yScaleLine = d3.scale.linear()
	.domain([0, maxScore])
	.range([0, cHeight]);

var line = d3.svg.line()
	.x(function (d, i) { return xScale(i); })
	.y(function (d, i) { return yScaleLine(d.data.score); });

var g = svg.append('g')
	.attr('transform', 'translate(10, 100)');
var bars = g.selectAll('rect')
	.data(data);

bars.enter()
	.append('rect')
	.attr({
		x: function (d, i) { return xScale(i); },
		y: function (d, i) { return ch - yScale(d.data.score); },
		width: xScale.rangeBand(),
		height: function (d, i) { return yScale(d.data.score); }
	});

g.append('path')
	.attr('d', line(data))
	.style({
		fill: 'none',
		stroke: '#000'
	});

### brush generator

var scale = d3.scale.linear()
  .domain([20, 30])
  .range([10, 300])

var brush = d3.svg.brush()
brush.x(scale)
brush.extent([22, 28])

var g = svg.append("g")
brush(g)
g.attr("transform", "translate(50, 100)")
g.selectAll("rect").attr("height", 30)
g.selectAll(".background")
  .style({fill: "#4B9E9E", visibility: "visible"})
g.selectAll(".extent")
  .style({fill: "#78C5C5", visibility: "visible"})
g.selectAll(".resize rect")
  .style({fill: "#276C86", visibility: "visible"})

var rects = g.selectAll('rect.events')
	.data(data);

rects.enter()
	.append('rect').classed('events', true)
	.attr({
		x: function (d) { return scale(d.data.created); },
		y: 0,
		width: 1,
		height: 30
	})
	.style('pointer-events', 'none');

brush.on('brushend', function () {
	var extent = brush.extent();
	var filtered = data.filter(function (d) {
		return d.data.created > extent[0] && d.data.created < extent[1];
	});
	g.selectAll('rect.events')
	 .style({
	 	stroke: '#000'
	 });
	g.selectAll('rect.events')
	 .data(filtered, function (d) { return d.data.id; })
	 .style({
	 	stroke: '#fff'
	 });
});

### histogram layout

layout: data transformer

var data = [1, 1, 1, 2, 2, 2, 5, 5, 3, 1, 2, 3, 4]
var hist = d3.layout.histogram()
.value(function(d) { return d })
.range([0, d3.max(data) ])
.bins(5);

var layout = hist(data);
console.log("histogram:", layout)

svg.selectAll("rect")
.data(layout)
.enter().append("rect")
.attr({
  x: function(d,i) {
    return 150 + i * 30
  },
  y: 50,
  width: 20,
  height: function(d,i) {
    return 20 * d.length
  }
})

### creating table

var display = d3.select('#display');
var data = tributary.pics.data.children;

var table = display.append('table');
var rows = table.selectAll('tr.row')
	.data(data);

var rowsEnter = rows.enter()
	.append('tr')
	.classed('row', true)

rowsEnter.append('td')
	.text(function (d) { return d.data.score; })

rowsEnter.append('td')
	.append('img')
	.attr({
		src: function (d) { return d.data.thumbnail; }
	});

rowsEnter.append('td')
	.append('a')
	.attr({
		href: function (d) { return d.data.url; }
	})
	.text(function (d) { return  d.data.title; });

rowsEnter.append('td')
	.text(function (d) { return d.data.ups; });

rowsEnter.append('td')
	.text(function (d) { return d.data.downs; });


􏰥􏰮􏰋􏰆􏰁 􏰍􏰎􏰋􏰃􏰌􏰏􏰁 􏰊􏰅􏰇􏰑􏰋􏰐􏰃􏰆􏰁 􏰄􏰁 􏱈􏰃􏰪􏰁 􏰄􏰊􏰊􏰎􏰋􏰍􏰄􏰏􏰋􏰇􏰌􏰁 􏰏􏰇􏰁 􏰑􏰄􏰅􏰋􏰇􏰩􏰆􏰁 􏰮􏰃􏰄􏰎􏰏􏰮􏰍􏰄􏰅􏰃􏰁 􏰆􏰃􏰅􏰑􏰋􏰍􏰃􏰁 􏰊􏰅􏰇􏰑􏰋􏰐􏰃􏰅􏰆􏰁 􏰔􏰮􏰇􏰆􏰊􏰋􏰏􏰄􏰎􏰆􏱉􏰁 􏰍􏰎􏰋􏰌􏰋􏰍􏰆􏱉􏰁 􏰎􏰄􏰪􏰆􏱉􏰁 􏰃􏰏􏰍􏰒􏰘􏰁 􏰄􏰌􏰐􏰁 􏰋􏰆􏰁 􏰩􏰆􏰃􏰐􏰁􏰪􏰂􏰁􏰨􏰃􏰌􏰃􏰅􏰄􏰎􏰁􏰐􏰇􏰍􏰏􏰇􏰅􏰆􏰁􏰏􏰇􏰁􏰓􏰄􏱊􏰃􏰁􏰅􏰃􏰈􏰃􏰅􏰅􏰄􏰎􏰆􏰳􏰄􏰊􏰊􏰇􏰋􏰌􏰏􏰓􏰃􏰌􏰏􏰆􏰁􏰈􏰇􏰅􏰁􏰏􏰮􏰃􏰁􏰊􏰄􏰏􏰋􏰃􏰌􏰏􏰆􏰒
􏰠􏰑􏰃􏰅􏰁 􏰂􏰃􏰄􏰅􏰆􏰁 􏰆􏰋􏰌􏰍􏰃􏰁 􏰋􏰏􏰆􏰁 􏰋􏰌􏰍􏰃􏰊􏰏􏰋􏰇􏰌􏰁 􏰋􏰌􏰁 􏰂􏰃􏰄􏰅􏰁 􏱋􏱌􏱌􏱌􏱉􏰁 􏰏􏰮􏰃􏰁 􏰄􏰊􏰊􏰎􏰋􏰍􏰄􏰏􏰋􏰇􏰌􏰁 􏰌􏰇􏱈􏰁 􏰌􏰃􏰃􏰐􏰆􏰁 􏰏􏰇􏰏􏰄􏰎􏰁 􏰅􏰃􏰑􏰄􏰓􏰊􏰁 􏰏􏰇􏰁 􏰓􏰃􏰃􏰏􏰁 􏰍􏰩􏰅􏰅􏰃􏰌􏰏􏰁 􏰄􏰌􏰐􏰁 􏰈􏰩􏰏􏰩􏰅􏰃􏰁 􏰅􏰃􏱍􏰩􏰋􏰅􏰃􏰓􏰃􏰌􏰏􏰆􏰁 􏰎􏰋􏱊􏰃􏰁 􏰪􏰩􏰆􏰋􏰌􏰃􏰆􏰆􏰁 􏰃􏰉􏰊􏰄􏰌􏰆􏰋􏰇􏰌􏰁 􏰄􏰌􏰐􏰁 􏰃􏰌􏰮􏰄􏰌􏰍􏰃􏰓􏰃􏰌􏰏􏰆􏱉􏰁 􏰋􏰌􏰏􏰃􏰅􏰌􏰄􏰏􏰋􏰇􏰌􏰄􏰎􏰋􏱎􏰄􏰏􏰋􏰇􏰌􏱉􏰁 􏰓􏰇􏰪􏰋􏰎􏰋􏰏􏰂􏱉􏰁 􏰪􏰋􏰨􏰁 􏰐􏰄􏰏􏰄􏰁 􏰄􏰌􏰄􏰎􏰂􏰏􏰋􏰍􏰆􏰁 􏰪􏰃􏰆􏰋􏰐􏰃􏰆􏰁 􏰪􏰃􏰋􏰌􏰨􏰁 􏰊􏰃􏰅􏰈􏰇􏰅􏰓􏰄􏰌􏰏􏰁 􏰄􏰌􏰐􏰁 􏰆􏰍􏰄􏰎􏰃􏰄􏰪􏰎􏰃􏰁 􏰈􏰇􏰅􏰁 􏰈􏰩􏰏􏰩􏰅􏰃􏰁 􏰌􏰃􏰃􏰐􏰆􏰒􏰁 􏰝􏰇􏰅􏰁 􏰏􏰮􏰋􏰆􏰁 􏰏􏰇􏰁 􏰮􏰄􏰊􏰊􏰃􏰌􏱉􏰁 􏰏􏰮􏰃􏰁 􏰄􏰊􏰊􏰎􏰋􏰍􏰄􏰏􏰋􏰇􏰌􏰁 􏰌􏰃􏰃􏰐􏰆􏰁 􏰏􏰇􏰁 􏰪􏰃􏰁 􏰅􏰃􏰪􏰩􏰋􏰎􏰏􏰁 􏰩􏰆􏰋􏰌􏰨􏰁 􏰎􏰄􏰏􏰃􏰆􏰏􏰁 􏰏􏰃􏰍􏰮􏰌􏰇􏰎􏰇􏰨􏰂􏰁 􏰆􏰏􏰄􏰍􏱊􏰒􏰁 􏱏􏰃􏰁 􏰄􏰅􏰃􏰁 􏰅􏰃􏱈􏰅􏰋􏰏􏰋􏰌􏰨􏰁 􏰏􏰮􏰃􏰋􏰅􏰁 􏰄􏰊􏰊􏰎􏰋􏰍􏰄􏰏􏰋􏰇􏰌􏰁 􏰩􏰆􏰋􏰌􏰨􏰁 􏱐􏰥􏰣􏰦􏱑􏱉􏰁 􏱒􏰫􏰫􏰲􏱉􏰁 􏰫􏱒􏰫􏰫􏱉􏰁 􏰡􏰄􏰑􏰄􏰫􏰍􏰅􏰋􏰊􏰏􏱉􏰁 􏰧􏰌􏰨􏰩􏰎􏰄􏰅􏰡􏰫􏱉􏰁 􏰧􏰌􏰨􏰩􏰎􏰄􏰅􏱓􏰜􏰟􏱉􏰁 􏰥􏱈􏰋􏰏􏰏􏰃􏰅􏰁 􏰢􏰇􏰇􏰏􏰆􏰏􏰅􏰄􏰊􏰁 􏰄􏰌􏰐􏰁 􏰩􏰆􏰋􏰌􏰨􏰁 􏰓􏰇􏰐􏰃􏰅􏰌􏰁 􏰈􏰅􏰇􏰌􏰏􏰁 􏰃􏰌􏰐􏰁 􏰏􏰇􏰇􏰎􏰁 􏰍􏰮􏰄􏰋􏰌􏰁 􏰎􏰋􏱊􏰃􏰁 􏰭􏰩􏰎􏰊􏱉􏰁 􏰢􏰇􏱈􏰃􏰅􏱉􏰁 􏱔􏰯􏰣􏱉􏰁 􏰰􏰄􏰅􏰓􏰄􏱉􏰁 􏰡􏰄􏰆􏰓􏰋􏰌􏰃􏱉􏰁 􏰯􏰅􏰇􏰏􏰅􏰄􏰍􏰏􏰇􏰅􏱉􏰁􏰃􏰏􏰍􏰁􏰈􏰇􏰅􏰁􏰊􏰄􏰍􏱊􏰄􏰨􏰋􏰌􏰨􏰁􏰄􏰌􏰐􏰁􏰏􏰃􏰆􏰏􏰋􏰌􏰨􏰁􏰏􏰮􏰃􏰁􏰄􏰊􏰊􏰎􏰋􏰍􏰄􏰏􏰋􏰇􏰌􏰒􏰥􏰮􏰋􏰆􏰁 􏰍􏰎􏰋􏰃􏰌􏰏􏰁 􏰊􏰅􏰇􏰑􏰋􏰐􏰃􏰆􏰁 􏰄􏰁 􏱈􏰃􏰪􏰁 􏰄􏰊􏰊􏰎􏰋􏰍􏰄􏰏􏰋􏰇􏰌􏰁 􏰏􏰇􏰁 􏰑􏰄􏰅􏰋􏰇􏰩􏰆􏰁 􏰮􏰃􏰄􏰎􏰏􏰮􏰍􏰄􏰅􏰃􏰁 􏰆􏰃􏰅􏰑􏰋􏰍􏰃􏰁 􏰊􏰅􏰇􏰑􏰋􏰐􏰃􏰅􏰆􏰁 􏰔􏰮􏰇􏰆􏰊􏰋􏰏􏰄􏰎􏰆􏱉􏰁 􏰍􏰎􏰋􏰌􏰋􏰍􏰆􏱉􏰁 􏰎􏰄􏰪􏰆􏱉􏰁 􏰃􏰏􏰍􏰒􏰘􏰁 􏰄􏰌􏰐􏰁 􏰋􏰆􏰁 􏰩􏰆􏰃􏰐􏰁􏰪􏰂􏰁􏰨􏰃􏰌􏰃􏰅􏰄􏰎􏰁􏰐􏰇􏰍􏰏􏰇􏰅􏰆􏰁􏰏􏰇􏰁􏰓􏰄􏱊􏰃􏰁􏰅􏰃􏰈􏰃􏰅􏰅􏰄􏰎􏰆􏰳􏰄􏰊􏰊􏰇􏰋􏰌􏰏􏰓􏰃􏰌􏰏􏰆􏰁􏰈􏰇􏰅􏰁􏰏􏰮􏰃􏰁􏰊􏰄􏰏􏰋􏰃􏰌􏰏􏰆􏰒
􏰠􏰑􏰃􏰅􏰁 􏰂􏰃􏰄􏰅􏰆􏰁 􏰆􏰋􏰌􏰍􏰃􏰁 􏰋􏰏􏰆􏰁 􏰋􏰌􏰍􏰃􏰊􏰏􏰋􏰇􏰌􏰁 􏰋􏰌􏰁 􏰂􏰃􏰄􏰅􏰁 􏱋􏱌􏱌􏱌􏱉􏰁 􏰏􏰮􏰃􏰁 􏰄􏰊􏰊􏰎􏰋􏰍􏰄􏰏􏰋􏰇􏰌􏰁 􏰌􏰇􏱈􏰁 􏰌􏰃􏰃􏰐􏰆􏰁 􏰏􏰇􏰏􏰄􏰎􏰁 􏰅􏰃􏰑􏰄􏰓􏰊􏰁 􏰏􏰇􏰁 􏰓􏰃􏰃􏰏􏰁 􏰍􏰩􏰅􏰅􏰃􏰌􏰏􏰁 􏰄􏰌􏰐􏰁 􏰈􏰩􏰏􏰩􏰅􏰃􏰁 􏰅􏰃􏱍􏰩􏰋􏰅􏰃􏰓􏰃􏰌􏰏􏰆􏰁 􏰎􏰋􏱊􏰃􏰁 􏰪􏰩􏰆􏰋􏰌􏰃􏰆􏰆􏰁 􏰃􏰉􏰊􏰄􏰌􏰆􏰋􏰇􏰌􏰁 􏰄􏰌􏰐􏰁 􏰃􏰌􏰮􏰄􏰌􏰍􏰃􏰓􏰃􏰌􏰏􏰆􏱉􏰁 􏰋􏰌􏰏􏰃􏰅􏰌􏰄􏰏􏰋􏰇􏰌􏰄􏰎􏰋􏱎􏰄􏰏􏰋􏰇􏰌􏱉􏰁 􏰓􏰇􏰪􏰋􏰎􏰋􏰏􏰂􏱉􏰁 􏰪􏰋􏰨􏰁 􏰐􏰄􏰏􏰄􏰁 􏰄􏰌􏰄􏰎􏰂􏰏􏰋􏰍􏰆􏰁 􏰪􏰃􏰆􏰋􏰐􏰃􏰆􏰁 􏰪􏰃􏰋􏰌􏰨􏰁 􏰊􏰃􏰅􏰈􏰇􏰅􏰓􏰄􏰌􏰏􏰁 􏰄􏰌􏰐􏰁 􏰆􏰍􏰄􏰎􏰃􏰄􏰪􏰎􏰃􏰁 􏰈􏰇􏰅􏰁 􏰈􏰩􏰏􏰩􏰅􏰃􏰁 􏰌􏰃􏰃􏰐􏰆􏰒􏰁 􏰝􏰇􏰅􏰁 􏰏􏰮􏰋􏰆􏰁 􏰏􏰇􏰁 􏰮􏰄􏰊􏰊􏰃􏰌􏱉􏰁 􏰏􏰮􏰃􏰁 􏰄􏰊􏰊􏰎􏰋􏰍􏰄􏰏􏰋􏰇􏰌􏰁 􏰌􏰃􏰃􏰐􏰆􏰁 􏰏􏰇􏰁 􏰪􏰃􏰁 􏰅􏰃􏰪􏰩􏰋􏰎􏰏􏰁 􏰩􏰆􏰋􏰌􏰨􏰁 􏰎􏰄􏰏􏰃􏰆􏰏􏰁 􏰏􏰃􏰍􏰮􏰌􏰇􏰎􏰇􏰨􏰂􏰁 􏰆􏰏􏰄􏰍􏱊􏰒􏰁 􏱏􏰃􏰁 􏰄􏰅􏰃􏰁 􏰅􏰃􏱈􏰅􏰋􏰏􏰋􏰌􏰨􏰁 􏰏􏰮􏰃􏰋􏰅􏰁 􏰄􏰊􏰊􏰎􏰋􏰍􏰄􏰏􏰋􏰇􏰌􏰁 􏰩􏰆􏰋􏰌􏰨􏰁 􏱐􏰥􏰣􏰦􏱑􏱉􏰁 􏱒􏰫􏰫􏰲􏱉􏰁 􏰫􏱒􏰫􏰫􏱉􏰁 􏰡􏰄􏰑􏰄􏰫􏰍􏰅􏰋􏰊􏰏􏱉􏰁 􏰧􏰌􏰨􏰩􏰎􏰄􏰅􏰡􏰫􏱉􏰁 􏰧􏰌􏰨􏰩􏰎􏰄􏰅􏱓􏰜􏰟􏱉􏰁 􏰥􏱈􏰋􏰏􏰏􏰃􏰅􏰁 􏰢􏰇􏰇􏰏􏰆􏰏􏰅􏰄􏰊􏰁 􏰄􏰌􏰐􏰁 􏰩􏰆􏰋􏰌􏰨􏰁 􏰓􏰇􏰐􏰃􏰅􏰌􏰁 􏰈􏰅􏰇􏰌􏰏􏰁 􏰃􏰌􏰐􏰁 􏰏􏰇􏰇􏰎􏰁 􏰍􏰮􏰄􏰋􏰌􏰁 􏰎􏰋􏱊􏰃􏰁 􏰭􏰩􏰎􏰊􏱉􏰁 􏰢􏰇􏱈􏰃􏰅􏱉􏰁 􏱔􏰯􏰣􏱉􏰁 􏰰􏰄􏰅􏰓􏰄􏱉􏰁 􏰡􏰄􏰆􏰓􏰋􏰌􏰃􏱉􏰁 􏰯􏰅􏰇􏰏􏰅􏰄􏰍􏰏􏰇􏰅􏱉􏰁􏰃􏰏􏰍􏰁􏰈􏰇􏰅􏰁􏰊􏰄􏰍􏱊􏰄􏰨􏰋􏰌􏰨􏰁􏰄􏰌􏰐􏰁􏰏􏰃􏰆􏰏􏰋􏰌􏰨􏰁􏰏􏰮􏰃􏰁􏰄􏰊􏰊􏰎􏰋􏰍􏰄􏰏􏰋􏰇􏰌􏰒􏰥􏰮􏰋􏰆􏰁 􏰍􏰎􏰋􏰃􏰌􏰏􏰁 􏰊􏰅􏰇􏰑􏰋􏰐􏰃􏰆􏰁 􏰄􏰁 􏱈􏰃􏰪􏰁 􏰄􏰊􏰊􏰎􏰋􏰍􏰄􏰏􏰋􏰇􏰌􏰁 􏰏􏰇􏰁 􏰑􏰄􏰅􏰋􏰇􏰩􏰆􏰁 􏰮􏰃􏰄􏰎􏰏􏰮􏰍􏰄􏰅􏰃􏰁 􏰆􏰃􏰅􏰑􏰋􏰍􏰃􏰁 􏰊􏰅􏰇􏰑􏰋􏰐􏰃􏰅􏰆􏰁 􏰔􏰮􏰇􏰆􏰊􏰋􏰏􏰄􏰎􏰆􏱉􏰁 􏰍􏰎􏰋􏰌􏰋􏰍􏰆􏱉􏰁 􏰎􏰄􏰪􏰆􏱉􏰁 􏰃􏰏􏰍􏰒􏰘􏰁 􏰄􏰌􏰐􏰁 􏰋􏰆􏰁 􏰩􏰆􏰃􏰐􏰁􏰪􏰂􏰁􏰨􏰃􏰌􏰃􏰅􏰄􏰎􏰁􏰐􏰇􏰍􏰏􏰇􏰅􏰆􏰁􏰏􏰇􏰁􏰓􏰄􏱊􏰃􏰁􏰅􏰃􏰈􏰃􏰅􏰅􏰄􏰎􏰆􏰳􏰄􏰊􏰊􏰇􏰋􏰌􏰏􏰓􏰃􏰌􏰏􏰆􏰁􏰈􏰇􏰅􏰁􏰏􏰮􏰃􏰁􏰊􏰄􏰏􏰋􏰃􏰌􏰏􏰆


jd, resume word format, tools, trainings, emirates coaching, ankur's format