---
layout: default
---
<script src="//code.jquery.com/jquery.js"></script>
<style>

.node {
  stroke: #fff;
  stroke-width: 1.5px;
}

.link {
  stroke: #999;
  stroke-opacity: .6;
}

</style>


# @fgreve

Welcome to my personal webpage! My name is Fernando Greve, I am an economist and engineer from Chile. I work as a **Research Economist** at the [Productivity Commission of Chile (CNP)](http://www.comisiondeproductividad.cl/) 


## Recent Posts/Proyects

<ul class="well">
  {% for post in site.categories.blog limit:1 %}

      <h2>
        <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
      </h2>

      <span class="text-warning">{{ post.date | date: "%b %-d, %Y" }}</span>

      <p>{{ post.content | strip_html | truncatewords:75}}</p>
      
      <a href="{{ post.url | prepend: site.baseurl }}">Read more...</a><br>

  {% endfor %}
</ul>


><div style="text-align: right"><i> 
"Hay que defenderse de esa incapacidad a veces tan nacional<br>
y tan sudamericana, para desnudar la raiz de las cosas y verlas<br>  
y comprenderlas antes de cubrirlas con la bruma de los objetivos<br> 
con que se descargan nuestros prejuicios y pasiones."<br></i>  
--Jorge Millas, Idea de la Individualidad.</div>


<div id='d3div'></div>

<script src="//d3js.org/d3.v3.min.js"></script>
<script>

var width = $("#d3div").width(),
    height = 500;

var color = d3.scale.category20();

var force = d3.layout.force()
    .charge(-120)
    .linkDistance(30)
    .size([width, height]);

var svg = d3.select("#d3div").append("svg")
    .attr("width", width)
    .attr("height", height);

d3.json("../../../../scripts/miserables.json", function(error, graph) {
  if (error) throw error;

  force
      .nodes(graph.nodes)
      .links(graph.links)
      .start();

  var link = svg.selectAll(".link")
      .data(graph.links)
    .enter().append("line")
      .attr("class", "link")
      .style("stroke-width", function(d) { return Math.sqrt(d.value); });

  var node = svg.selectAll(".node")
      .data(graph.nodes)
    .enter().append("circle")
      .attr("class", "node")
      .attr("r", 5)
      .style("fill", function(d) { return color(d.group); })
      .call(force.drag);

  node.append("title")
      .text(function(d) { return d.name; });

  force.on("tick", function() {
    link.attr("x1", function(d) { return d.source.x; })
        .attr("y1", function(d) { return d.source.y; })
        .attr("x2", function(d) { return d.target.x; })
        .attr("y2", function(d) { return d.target.y; });

    node.attr("cx", function(d) { return d.x; })
        .attr("cy", function(d) { return d.y; });
  });
});

</script>