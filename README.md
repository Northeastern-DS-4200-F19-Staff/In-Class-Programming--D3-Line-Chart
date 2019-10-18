# In-Class Programming: D3 Line Chart
In this activity, you are going to create a line-chart using D3.

## Setup
1. Clone this repository to your local machine. E.g., in your terminal / command prompt `CD` to where you want this the folder for this activity to be. Then run `git clone <YOUR_REPO_URL>`

1. `CD` or open a terminal / command prompt window into the cloned folder.

1. Start a simple python webserver. E.g., `python -m http.server`, `python3 -m http.server`, or `py -m http.server`. If you are using python 2 you will need to use `python -m SimpleHTTPServer` instead, but please switch to python 3.

1. Wait for the output: `Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/)`.

1. Now open a web browser and navigate to the URL: http://localhost:8000

## JavaScript Loading
1. Open `index.html`. Just below this HTML comment:
```html
<!-- Load the d3.js library( version 4 minified) -->
```
add this line to load the D3 javascript code:
```html
<script src="https://d3js.org/d3.v4.min.js"></script>
```
1. You also have a file `linechart.js`. We want to load this javascript code as well. In `index.html`, just below this HTML comment:
```html
<!-- Load the line.js file -->
```
add this line:
```html
<script src="linechart.js"></script>
```

## Data Loading

1. Open the file `data.csv` **with a text editor** and see what is inside. It is comma-separated. You will see a header row followed by several data rows with a date in the MM/D/YYYY format, e.g., `10/6/2018`, and a price, e.g., `33.9`. Note that you can open CSV files with Excel or other clever tools but they can make mistakes with parsing the data into their own formats.

1. In `linechart.js` we are going to load this CSV file. Just below this JavaScript comment:
```javascript
// Load and parse data
```
add this line to use D3 to load the file and then pass it to a callback function:
```javascript
d3.csv("data.csv".get(function(error, data){
  console.log(data)
};
```
           This is going to get the data and simply write it in the console. Hence go to your browser, and inspect the console. You will see that an array of 50 objects is there. If you see this in your console, your data has loaded!

**Step 7**: However, in the console of your browser, you will notice that the date and price are both in **" "** and that is not the format that we want our data to be in. Hence we need to parse the data according to how we want it.
To learn about data parsing visit: http://learnjsdata.com/time.html
**To parse our data to the format we want, we are going to define a variable called parseData before the step where we loaded our data.**\

`var parseDate = d3.timeParse("%m/%d/%Y");`   *[Because our data has / in them we are using /. If our data was like 02-02-2009, we would    parse it as (%m-%d-%Y)]*

**Step 8**: Now we are going to go back to the loading data step that will come after the var parseDate step. **Now remove the console.log(data) part as that was only to check if our data has loaded or not.**

**Step 9**: **Now we will call a function that will return our parsedData and the price as a Number instead of a string for each row of your data. We will use:

d3.csv("data.csv")
  .row(function(d){return{date:parseDate(d.date),price:Number(d.price)};})
 .get(function(error, data){

 **Your entire code of creating the line chart will go here inside this function that gets the data you loaded and also gives error if there is something wrong with the csv file**

 });

 ### Inside the get function:

**Step 10**: In order to have the proper scales for your linechart, you need to know the max and min of your data. This is easy manually when your data is small, but with large data, this is cumbersome and can be done instead using d3.

```javascript
var maxDate = d3.max(data,function(d){return d.date;});    finds the max date  and returns it
var minDate = d3.min(data,function(d){return d.date;});    finds the min date  and returns it
var maxPrice = d3.max(data,function(d){return d.price;});  finds the max price and returns it
```

**Step 11**: In your console check whether you can see the 2 max and 1 min value that you defined. If you can then you can later comment out the following commands before proceeding. This is for your own assurance that your variable is indeed returning the value that you are asking for.
console.log(maxDate);
console.log(minDate);
console.log(maxPrice);

**Step 12**: Define the width and height for the svg you will use:\
var width =  //Insert value;
var height = //Insert value;
var margin= {
  bottom://insert value
};

**Step 13**: Define SVG by selecting the body element in the html and then appending the svg in it. Give the svg attributes of width and height and color if you like. Then create a variable for groups which we will use during drawing our axes.
var svg = d3.select('body')
            .append('svg')
            .attr('width',width)
    	  .attr('height',height)
          // .style('background','#e9f7f2');

 var chartGroup = svg.append('g')
                    .attr('transform','translate(50,50)');


**Step 14**: Now we will define the scales for the x and y axis. We will define two variables x and y and set their domain and range.\
var y = d3.scaleLinear() \
          .domain([0,maxPrice])
          .range([height-margin.bottom,0]);


var x = d3.scaleTime()          **This is a scale for Time related data, it works the same way as other scales with domain and range**\
          .domain([minDate,maxDate])
          .range([0,width]);

**Step 15**: Define the axes

var yAxis = d3.axisLeft(y);
chartGroup.append('g')
          .attr('class','y axis')
          .attr('transform','translate(0, -40)')
          .call(yAxis);

var xAxis = d3.axisBottom(x);
chartGroup.append('g')
          .attr('class','x axis')
          .attr('transform','translate(-5, 330)')
          .call(xAxis);


**Step 16**: Draw the line

var line = d3.line()
             .x(function(d){return x(d.date);})    
             .y(function(d){return y(d.price);})

chartGroup.append('path')
          .attr('d',line(data));

**Step 17**: Now if you save this and open the html in your browser, you will see that your linechart is messed up. THis is because, we have not added any CSS styling to it and because the fill of each path in the line chart is by default black and hence it looks that way. **So, open the line.css file and add:**\
```CSS
path{
	stroke: #0000FF;
	stroke-width:2px;
	fill: none;
}
```
