---
layout: home
title: Recipe Exploration | Yummly Analystics
---

<link href="{{site.baseurl}}/stylesheets/c3.min.css" rel="stylesheet" />
<script src="{{site.baseurl}}/scripts/d3.min.js" charset="utf-8"></script>
<script src="{{site.baseurl}}/scripts/c3.min.js"></script>
<script src="{{site.baseurl}}/scripts/underscore-min.js"></script>
<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.4.0/Chart.min.js"></script>
<script src="{{site.baseurl}}scripts/sigma.min.js"></script>
<script src="{{site.baseurl}}/scripts/sigma.layout.forceAtlas2.min.js"></script>
<script src="{{site.baseurl}}/scripts/sigma.parsers.json.min.js"></script>

<style>
#select-meat{
	position:relative;
	width:100%;
}
.meat-selector{
	width:100%;
}
select{
	width:100%;
	height:25px;
}
.title-recipe{
	position:absolute;
	bottom:0px;
	background-color:rgba(0,0,0,0.5);
	/*width:100%;*/
	text-align:left;
	color:#FFF;
	font-weight:lighter;
	padding: 10px;
	/*padding-right:-10px;*/
}
.current-recipe{
	position:relative;
	margin:auto;
	width:400px;
}
table td{
	position:relative;

}
#meat-o-meter{
	height:500px;
}
#chart{
	height:700px;
}
#network-graph {
	top: 0;
	bottom: 0;
	left: 0;
	right: 0;
	position: absolute;
}
.similarity{
	position: relative;
	color:#feb24c;
}
.recipe-selector{
	position:relative;
	margin-bottom:15px;
}
</style>

*Diets and food habits may vary widely from country to country in terms of ingredients and cooking techniques. Nevertheless, dishes from different regions may share similarities in flavours and tastes. In this article, we explore these similarities and differences using Yummly, a platform mainly used in North America containing recipes from different countries.*

<center> <h1>Exploring cuisines with Yummly</h1> </center>
Yummly provides recipes along with their ingredients and nutritional intake data. We counted 19 unique cuisines in total ranging from the sweetest to the spiciest. Each cuisine counts a number of recipes ranging from 200 to 200K. We are also provided with the flavors defining each recipe and their respective preparation time. 

Let’s dive into data!

<center>
	<img src="{{site.baseurl}}/assets/yummly_homepage.png" style="margin-bottom:20px">
</center>

## Ingredients
Our dataset contains recipes representing different countries around the world, implying that we have a large variety of ingredients (~5K). However, a small subset of the ingredients is commonly present across all cuisines (e.g. salt, olive oil..).

<center>
	<h4>Most used ingredients in the world</h4>
	<img src="{{site.baseurl}}/assets/wc_ing.png" style="margin-bottom:20px">
</center>

As expected, spices like salt/pepper is very common across all cuisines.
Our curiosity leads us to explore them by characteirizing each with its distinctive ingredients.

While it's hardly fair to stereotype an entire national cuisine with a few basic ingredients, it is hard to deny some flavours evoke dishes from certain parts of the world. Indeed, it is not unknown for countries to almost be defined by their *"iconic dishes”*.  Here is what data says:

<center>
	<img src="{{site.baseurl}}/assets/most_used_ing.jpg" style="margin-bottom:20px;width:420px;">
</center>


> ***Fun fact: Is American cuisine just about burgers?***
>
> *According to Yummly, less than 7% of american recipes are about burgers. Also, none of the most distinctive ingredients of the US is used to prepare it. Thus, Americans eating mostly burgers remains just a stereotype.*

Although distinctive ingredients distinguish well enough a cuisine, we often think of a gastronomy as a combination of flavours. As flavours originate from a mix of specific ingredients, one may look for distinctive combination of ingredients for each cuisine: 

<div style="position:relative;height:500px;">
	<div id="network-graph"></div>
</div>

The bove network was based on the frequency of pairwise combinations of ingredients using [TF-IDF](#cosine-sim) algorithm. (**TODO**)

In many cuisines, ingredients orbit around a central ingredient: *Fish sauce* for Thai food, *Enchilada Sauce* for Mexican or *Gram masala* for Indian. Moreover, Asian cuisines like Chinese and Japanese share many ingredients as Italian and French do.

## Similarities between cuisines
It is true that cuisines have different distinctive ingredients, nevertheless, similarities between their recipes is not to neglect as can be seen below:
<div id="chart"></div>

Clustering recipes was reduced to a [dimensionality reduction](https://en.wikipedia.org/wiki/Dimensionality_reduction) problem which is then solved with [t-SNE](#cosine-sim) algorithm.

Here again, it is clear that recipes from different cuisines do not always share the same ingredients (e.g. Spanish and Japanese), However, as the English and Irish clusters overlap, one may conclude that there is an influence of one on the other. One possible reason of this similarity might be the geographic proximity of both countries, or also to the shared history between the two cultures.

One way to further analyse similarities between cuisines is to explore the confusion between recipes of different cuisines. For this purpose, we train a simple machine learning model ([logistic regression](https://en.wikipedia.org/wiki/Logistic_regression)) and examine its [confusion matrix](https://en.wikipedia.org/wiki/Confusion_matrix).

<center>
	<h4> Similarities between cuisines </h4>
	<img src="{{site.baseurl}}/assets/confused_sankey.png">
</center>

As expected, we observe similar results, with :
- Spanish and Portuguese
- Irish and English
- Mexican and Cuban
- Chinese and Japanese
- Thai and Chinese

showing strong similarities as they are often confused by our model.
Thus, The latter results support our two hypotheses stating that closer countries will share more similarities.

## Meat'O'Meter
Across several cuisines, meat is a major component of recipes. Its consumption varies depending on cultural or religious orientations, as well as economic conditions. Therefore, exploring meat preferences across cuisines would give us an idea about a region's food habit.
To do so, we analyse the usage of different types of meat across cuisines:

***Select a meat and see who are its big consumers***
<div class="meat-selector"></div>
<div id="meat-o-meter"></div>

From plotting various meat types, we see different distributions with the following being noticeable:
- Indian and Mexican cuisines use poultry much more than red meats.
- Asian, more specificcaly Japanese recipes contain more seafood compared to other cuisines.

Interestingly, some cuisines do not have recipes containing some types of meat.
The results can be explained by the the fact that cuisines mostly use ressources available in their specific region (e.g. tuna and salmon in Japan). Another reason is culture and believes, some countries have specific religions or practices forbidding eating certain types of meats (e.g. beef in India - pork and bacon Morocco)

> ***Fun fact: Who is Avocado's biggest fan?***
>
> *Over the last decade, worldwide comsumption of avocado has tripled showing a high interest in this fruit. Let's find out from our data what cuisine use it the most:*

<center>
	<h4> Avocado appearance in recipes</h4>
	<img src="{{site.baseurl}}/assets/avocado_freq.png">
</center>

## Cooking Time
Another interesting variable to study is their corresponding preparation time which may differ depending on the techniques involved in the cooking process (frying, roasting, steaming etc...) or also the types of ingredients. We aggregate the cooking time of recipes by cuisine and plot their respective cumulative distributions:

<center>
	<h4> Cooking time in cuisines </h4>
	<img src="{{site.baseurl}}/assets/cooking_time.png">
</center>

The distribution of the Asian cuisines (Chinese, Japanese and Thai) are quite similar and different from the European cuisines(English, French and German). We also note that Asian recipes are less time demanding compared to the European ones. This can be explained by the eating habits of Asia in which meals are composed of many small dishes (recipes) each taking less time than cooking a one-dish meal.


## Recipe recommender
As we have now analysed resemblance between cuisines and their ingredients, we build a recipe recommender that suggest similar recipes using the [cosine similarity metric](#cosine-sim). For each result, a score will define the similarity degree of its ingredients with the selected recipe.

***Select a recipe and get suggestions***
<div class="recipe-selector"></div>
<div class="current-recipe"></div>
<div class="similar-recipes"></div>

As recipes don't match exactly in terms of ingredients, they will have a low similarity degree even though the images look alike.

## Summary
From this project, we answered various questions by analysing our dataset. We needed to carefully explore and clean the data during which we faced challenges such as correcting the ingredients names and imputing missing values.  This helped us get satisfying answers, for instance finding the most iconic ingredients per cuisine, clustering recipes by region and recommending recipes.

----------------

**Methodology notes:**
<a name="cosine-sim"></a>
In this section, we go through the methodology used to build different components of our analysis.

- **t-SNE:** To create our recipe cluster, we used a dimensionality reduction technique called t-distributed Stochastic Neighbor Embedding. Our input is  the one-hot vectors of 5000 dimensions. The output of the t-SNE algorithm mapped recipes to a two-dimensional space based on the similarity (cosine-similarity) of their ingredients.

- **Cosine similarity:** Cosine similarity is a common way of calculating the similarity between two vectors by taking the cosine of the angle between them. In our case, that means taking the one hot encoding vector of a recipe and comparing it to that of another. Higher cosine values imply more similarity, with an upper bound of 1 when the vectors are perfectly similar.

For more details concerning the implementation of our analysis, please refer to the [github repository](https://github.com/alialamiidrissi/ADA_Course_Project/tree/master/Project) associated with the project. More details are provided within the Jupyter Notebook.

<script>
	function toUpper(string) {
	    return string.charAt(0).toUpperCase() + string.slice(1);
	}
	function plot_cluster(chart_id, data){
		_.each(data, function(dic){ 
			dic['mode']='markers';
			dic['type']='scatter';
			dic['marker']={size:6};
			dic['name']=toUpper(dic['name']);
		})

		var layout = {
		  title:'Cuisine clustering by ingredients',
		  hovermode: !1
		};

		Plotly.newPlot(chart_id, data, layout);
	}
	function plot_meatometer(chart_id, data, meat){
		data = data[meat]
		
		data['type'] = 'bar';
		data['orientation'] = 'h';
		data['marker'] = {
			color: '#e16120'
		}
		var layout = {
		  xaxis:{
		  	title: 'Percentage of recipes within each cuisine'
		  },
		  
		  title:toUpper(meat)+'-Meter',
		  annotations: [
		    {
		      x: 0.5,
		      y:0,
		      showarrow: false,
      		  text: '     ',
		    }
		  ]
		};
		Plotly.newPlot(chart_id, [data], layout);
	}
	function show_recipe_selector(data){
		var html = '<select id="recipe-select">'
		_.each(Object.keys(data), function(key){
			html += '<option value="'+key+'">'+toUpper(key)+'</option>';
		})
		html += '</select>';
		d3.select('.recipe-selector').html(html);
		document.getElementById('recipe-select').onchange = function() {
		  var index = this.selectedIndex;
		  var inputText = this.children[index].innerHTML.trim();
		  show_recipe(data, inputText);
		}
		show_recipe(data, Object.keys(data)[0]);
	}
	function show_recipe(data, title){
		recipe = data[title];
		html = '<img src="'+recipe['img']+'">';
		html += '<div class="title-recipe">'+title+'</div>';
		d3.select(".current-recipe").html(html);
		
		i=0;
		html = '<table><tr>';
		_.each(data[title]['similars'], function(r){
			html += '<td>';
			html += '<img src="'+r['image']+'">';
			html += '<div class="title-recipe">'+r['title']+' <span class="similarity">('+Math.round(r['similarity']*100,0)+'%)</span></div>';
			html += '</td>'
			i += 1;
			if(i%3 == 0)
				html += '</tr><tr>';
		});
		html += '</tr></table>';
		d3.select(".similar-recipes").html(html);
	}

	function plot_meat_selector(chart_id, data){
		var html = '<select id="meat-select">'
		_.each(Object.keys(data), function(key){
			html += '<option value="'+key+'">'+toUpper(key)+'</option>';
		})
		html += '</select>';
		d3.select('.'+chart_id).html(html);
		document.getElementById('meat-select').onchange = function() {
		  var index = this.selectedIndex;
		  var inputText = this.children[index].innerHTML.trim();
		  plot_meatometer('meat-o-meter', data, inputText)
		}
	}
	function append_legend_network(){
		countries = ['french', 'italian', 'japanese', 'chinese', 'english', 'mexican', 'indian', 'irish'];
		colors = ['#a6cee3','#1f78b4','#b2df8a','#33a02c','#fb9a99','#e31a1c','#fdbf6f','#ff7f00','#cab2d6','#6a3d9a'];
		html = '<table>';
		i = 0;
		_.each(countries, function(c){
			html += '<tr><td style="background-color:'+colors[i]+';">&nbsp;</td>';
			html += '<td>'+toUpper(c)+'</td></tr>';
			i++;
		});
		html += '</table>'

		d3.select('#network-graph').append('div')
								   .html(html)
								   .style('font-size', '9pt')
								   .style('position', 'absolute')
								   .style('bottom', '0px');
	}
	window.onload = function(){
		d3.json("{{site.baseurl}}/assets/recipes_tsne.json", function(error, data) {
		    plot_cluster('chart', data)
		});
		d3.json("{{site.baseurl}}/assets/meat-o-meter.json", function(error, data){
			_.each(data, function(v,k){
				v['x'] = v['x'].reverse()
				v['y'] = v['y'].reverse()
			})
			plot_meatometer('meat-o-meter', data, 'Salmon');
			plot_meat_selector('meat-selector', data);
		});
		d3.json("{{site.baseurl}}/assets/similar-recipes.json", function(error, data){
			show_recipe_selector(data);
			// show_recipe('meat-selector', data);
		});
		sigma.parsers.json( "{{site.baseurl}}/assets/network_ing.json",

		  {container: 'network-graph'});
		append_legend_network();
	}
</script>

