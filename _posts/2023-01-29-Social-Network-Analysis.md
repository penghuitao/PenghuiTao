---
layout: page
title:  "Social Network Analysis"
subtitle: ""
date:   2023-01-29 15:01:21 +0530
categories: ["Network Analysis"]
---


<h2><strong>What is Social Network Analysis</strong></h2>
<img src="{{ '/assets/img/20240123/SNA1.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px;">
<p>Social Network Analysis (SNA) is a method for studying social structures and interactions using networks and graph theory. It characterizes network structure based on nodes (individual participants, people, or things in the network) and the relationships, edges, or links connecting them (relationships or interactions). Social structures visualized through social network analysis typically include social media networks, meme propagation, information flow, friendship and acquaintance networks, business networks, collaboration graphs, kinship relationships, disease spread, and sexual relationships. Individuals or organizations in social relationships are represented as points, and their relationships are represented as lines or edges.</p>

<p>Social network analysis is widely used in disciplines such as anthropology, biology, demography, communication, economics, geography, history, information science, organizational studies, and political science. Of course, some psychological research can also apply this method. For example, in the study of self-monitoring traits, Oh and Kilduff (2008) used individual networks to discover that differences in self-monitoring led to differences in individuals’ positions in acquaintance networks, with high self-monitors more likely to occupy direct or indirect brokerage positions.</p>

<h2><strong>R Implementation of Social Network Analysis</strong></h2>
<p>In R, the primary package for social network analysis is <code>igraph</code>. <b>In this article, I will demonstrate how to implement social network analysis in R using data from a friendship survey of 52 elementary school students in a class, obtained from a Chinese public database. You don't need to focus on specific numerical values, but you should note that social network analysis data appears in pairs.</b></p>

<h3><strong>1. Load Data</strong></h3>
<pre>
<code>
library(igraph) # Load the package
data <- read.csv(file.choose(), header=T) # Choose the data file
View(data) # View the data in R
str(data) # Understand the basic structure of the data
</code>
</pre>
<img src="{{ '/assets/img/20240123/SNA2.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>This data includes 4 variables and 290 observations. The first column contains the name of the first person, the second column contains the name of the second person, and the other columns contain grade and specialty. We only need to use the first two columns, so we create the data frame as follows:</p>
<pre>
<code>
y <- data.frame(data$first, data$second) # Extract the first two columns
</code>
</pre>

<h3><strong>2. Create the Social Network</strong></h3>
<pre>
<code>
net <- graph.data.frame(y, directed=T) # Create the social network, directed = TRUE means the relationships are directed
</code>
</pre>

<h3><strong>3. Basic Information of the Social Network</strong></h3>

<h4><strong>3.1 Check the Number of Nodes</strong></h4>
<pre>
<code>
V(net) # View the number of nodes
</code>
</pre>
<img src="{{ '/assets/img/20240123/SNA3.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>There are 52 nodes in total.</p>

<h4><strong>3.2 Check the Number of Edges</strong></h4>
<pre>
<code>
E(net) # View the number of edges
</code>
</pre>
<img src="{{ '/assets/img/20240123/SNA4.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>There are 290 edges in total.</p>

<h3><strong>4. Relationship Distribution</strong></h3>
<pre>
<code>
hist(V(net)$degree, col = 'green', 
     main = 'Histogram of Node Degree', 
     ylab = 'Frequency', 
     xlab = 'Degree of Vertices')
</code>
</pre>
<img src="{{ '/assets/img/20240123/SNA5.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>This will generate a histogram of node degrees. Most individuals have fewer than 10 relationships, but some have up to 70 relationships.</p>

<h3><strong>5. Visualize the Social Network</strong></h3>
<pre>
<code>
set.seed(222) # Set randomization for visualization
plot(net, vertex.color = 'red', 
     vertex.size = 2, 
     edge.arrow.size = 0.1, 
     vertex.label.cex = 0.8)
</code>
</pre>
<img src="{{ '/assets/img/20240123/SNA6.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>This will display the social network with nodes colored red, with small node sizes and small edge arrow sizes. This visualization is quite cluttered, so further adjustments are needed.</p>

<h3><strong>6. Highlight Degree and Layout</strong></h3>
<pre>
<code>
plot(net, 
     vertex.color = rainbow(52), 
     vertex.size = V(net)$degree * 0.4, 
     edge.arrow.size = 0.1, 
     layout = layout.fruchterman.reingold)
</code>
</pre>
<img src="{{ '/assets/img/20240123/SNA8.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>Here, nodes are colored according to a rainbow palette, and node sizes are proportional to their degree. The layout function adjusts the positioning of the nodes.</p>

<h3><strong>7. Visualize Hubs and Authorities in the Network</strong></h3>

<h4><strong>7.1 Compute Hub and Authority Scores</strong></h4>
<pre>
<code>
hs <- hub_score(net)$vector # Hub scores for outgoing relationships
as <- authority.score(net)$vector # Authority scores for incoming relationships
</code>
</pre>

<h4><strong>7.2 Create the Plots</strong></h4>
<pre>
<code>
par(mfrow = c(1, 2)) # Plot 1 row, 2 columns
set.seed(123) # Randomization
plot(net, 
     vertex.size = hs * 30, 
     main = 'Hubs', 
     vertex.color = rainbow(52), 
     edge.arrow.size = 0.1, 
     layout = layout.kamada.kawai)

plot(net, 
     vertex.size = as * 30, 
     main = 'Authorities', 
     vertex.color = rainbow(52), 
     edge.arrow.size = 0.1, 
     layout = layout.kamada.kawai)
</code>
</pre>
<img src="{{ '/assets/img/20240123/SNA9.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>These plots show the hub and authority scores for each node. Student CC is the most sociable, while student CA is the most popular.</p>

<h3><strong>8. Check Small Communities</strong></h3>
<pre>
<code>
net <- graph.data.frame(y, directed = F) # Set undirected relationships
cnet <- cluster_edge_betweenness(net) # Community detection using edge betweenness
plot(cnet, 
     net, 
     vertex.size = 10, 
     vertex.label.cex = 0.8)
</code>
</pre>
<img src="{{ '/assets/img/20240123/SNA10.png' | prepend: site.baseurl }}" id="about-img" style="width: 60%; max-width: 800px; border-radius: 10px;">
<p>This visualizes small communities within the network, identifying approximately six significant subgroups in the class.</p>
