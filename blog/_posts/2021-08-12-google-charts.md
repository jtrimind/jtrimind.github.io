---
title: "[Jekyll] 파이 차트 만들기(Google Charts)"
---

## `script` 로드

적절한 위치에 Google Charts를 불러오는 `script`를 추가한다.

```html
<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
```

### Jekyll Minimal Mistakes를 사용하는 경우
`head_scripts`에 `"https://www.gstatic.com/charts/loader.js"`를 추가한다.  

```yml
# _config.yml
head_scripts:
- "https://www.gstatic.com/charts/loader.js"
```

## 파이 차트 추가

```html
<div id="chart_div"></div>
<script defer type="text/javascript">
    console.log("start load");
    // Load the Visualization API and the corechart package.
    google.charts.load('current', {'packages':['corechart']});
    // Set a callback to run when the Google Visualization API is loaded.
    google.charts.setOnLoadCallback(drawChart);
    // Callback that creates and populates a data table,
    // instantiates the pie chart, passes in the data and
    // draws it.
    function drawChart() {
    console.log("loaded");
    // Create the data table.
    var data = new google.visualization.DataTable();
    data.addColumn('string', 'Topping');
    data.addColumn('number', 'Slices');
    data.addRows([
        ['Mushrooms', 3],
        ['Onions', 1],
        ['Olives', 1],
        ['Zucchini', 1],
        ['Pepperoni', 2]
    ]);
    // Set chart options
    var options = {'title':'How Much Pizza I Ate Last Night',
                    'width':400,
                    'height':300};
    // Instantiate and draw our chart, passing in some options.
    var chart = new google.visualization.PieChart(document.getElementById('chart_div'));
    chart.draw(data, options);
    }
</script>
```

<div id="chart_div"></div>
<script defer type="text/javascript">
    console.log("start load");
    // Load the Visualization API and the corechart package.
    google.charts.load('current', {'packages':['corechart']});
    // Set a callback to run when the Google Visualization API is loaded.
    google.charts.setOnLoadCallback(drawChart);
    // Callback that creates and populates a data table,
    // instantiates the pie chart, passes in the data and
    // draws it.
    function drawChart() {
    console.log("loaded");
    // Create the data table.
    var data = new google.visualization.DataTable();
    data.addColumn('string', 'Topping');
    data.addColumn('number', 'Slices');
    data.addRows([
        ['Mushrooms', 3],
        ['Onions', 1],
        ['Olives', 1],
        ['Zucchini', 1],
        ['Pepperoni', 2]
    ]);
    // Set chart options
    var options = {'title':'How Much Pizza I Ate Last Night',
                    'width':400,
                    'height':300};
    // Instantiate and draw our chart, passing in some options.
    var chart = new google.visualization.PieChart(document.getElementById('chart_div'));
    chart.draw(data, options);
    }
</script>

## 참고링크
- [Google Charts Quick Start](https://developers.google.com/chart/interactive/docs/quick_start)