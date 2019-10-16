# D3-Inclass-Activity
In this activity, you are going to create a line-chart using D3 

**Step 1**: CLone the repository and open a python server.\
**Step 2**: Open linechart.html and uncomment the "Loading d3 js library" (uncomment the version4). \
**Step 3**: Link the line.js with the html by creating `<script type="text/javascript" src="line.js"></script>` tag before the </body> tag (under the comment linking to the line.js file)

**Step 4**: Open the data.csv file and take a look at it.\
**Step 5**: Open the line.js file 

**Step 6**: Now we are going to load the data:
           `d3.csv("data.csv".get(function(error, data){console.log(data)};`              
           This is going to get the data and simply write it in the console. Hence go to your browser, and inspect the console. You will see that an array of 50 objects is there. If you see this in your console, your data has loaded!
           
**Step7**: However, in the console of your browser, you will notice that the date and price are both in **" "** and that is not the format that we want our data to be in. Hence we need to parse the data according to how we want it. 
To learn about data parsing visit: http://learnjsdata.com/time.html 
**To parse our data to the format we want, we are going to define a variable called parseData before the step where we loaded our data.**\

`var parseDate = d3.timeParse("%m/%d/%Y");`   *[Because our data has / in them we are using /. If our data was like 02-02-2009, we would    parse it as (%m-%d-%Y)]*

**Step8**: Now we are going to go back to the loading data step that will come after the var parseDate step. **Now remove the console.log(data) part as that was only to check if our data has loaded or not.**

**Step9**: **Now we will call a function that will return our parsedData and the price as a Number instead of a string for each row of your data. We will use:

`d3.csv("data.csv")` \
  `.row(function(d){return{date:parseDate(d.date),price:Number(d.price)};})`\
 `.get(function(error, data){`
 
 **Your entire code of creating the line chart will go here inside this function that gets the data you loaded and also gives error if there is something wrong with the csv file**
 
 `});` 
 
 ### Inside the get function:
 
**Step10**: In order to have the proper scales for your linechart, you need to know the max and min of your data. This is easy manually when your data is small, but with large data, this is cumbersome and can be done instead using d3.

`var maxDate = d3.max(data,function(d){return d.date;});`    finds the max date and returns it\
`var minDate = d3.min(data,function(d){return d.date;});`    finds the min date and returns it\
`var maxPrice = d3.max(data,function(d){return d.price;});`  finds the max price and returns it\

**Step11**: In your console check whether you can see the 2 max and 1 min value that you defined. If you can then you can later comment out the following commands before proceeding. This is for your own assurance that your variable is indeed returning the value that you are asking for.\
`console.log(maxDate);`\
`console.log(minDate);`\
`console.log(maxPrice);`

**Step12*: Define the width and height for the svg you will use:\
`var width = //Insert value;`\
`var height = //Insert value;`\
`var margin= {\
  bottom://insert value\
 };`

**Step13**: Define SVG by selecting the body element in the html and then appending the svg in it. Give the svg attributes of width and height and color if you like. Then create a variable for groups which we will use during drawing our axes.\
`var svg = d3.select('body')`\
            `.append('svg')`\
            `.attr('width',width)`\
    	  `.attr('height',height)`\
          `// .style('background','#e9f7f2');`

 `var chartGroup = svg.append('g')`\
                    `.attr('transform','translate(50,50)');`


**Step14**: Now we will define the scales for the x and y axis. We will define two variables x and y and set their domain and range.\
`var y = d3.scaleLinear()` \
          `.domain([0,maxPrice])`\
          `.range([height-margin.bottom,0]);`


`var x = d3.scaleTime()`          **This is a scale for Time related data, it works the same way as other scales with domain and range**\
          `.domain([minDate,maxDate])`\
          `.range([0,width]);`

**Step14**: Define the axes

`var yAxis = d3.axisLeft(y);`\
`chartGroup.append('g')`\
          `.attr('class','y axis')`\
          `.attr('transform','translate(0, -40)')`\
          `.call(yAxis);`

`var xAxis = d3.axisBottom(x);`\
`chartGroup.append('g')`\
          `.attr('class','x axis')`\
          `.attr('transform','translate(-5, 330)')`\
          `.call(xAxis);`


**Step15**: Draw the line

`var line = d3.line()`\
             `.x(function(d){return x(d.date);})`    
             `.y(function(d){return y(d.price);})`

`chartGroup.append('path')`\
          `.attr('d',line(data));`

**Step16**: Now if you save this and open the html in your browser, you will see that your linechart is messed up. THis is because, we have not added any CSS styling to it and because the fill of each path in the line chart is by default black and hence it looks that way. **So, open the line.css file and add:**\
`path{`\
	`stroke: *anycolor*;`\
	`stroke-width:2px;`\
	`fill:*make this none*;`\
`}`


