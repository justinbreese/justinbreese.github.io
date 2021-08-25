---
layout: page
title: Spark Shuffle Partition Calculator
---
Calculating the correct number of Spark Shuffle Partitions is something that I help a lot of customers with. So, I decided to give you an easy way to do it - with a calculator!

<script>
function buttonPressed()
{
    var shuffleSize = document.getElementById("shuffleSize").value;
    var cores = parseInt(document.getElementById("cores").value);
    var partitionSize = parseInt(document.getElementById("partitionSize").value);

    var totalSparkPartitions = (shuffleSize*1024)/partitionSize
    var coreCycles = totalSparkPartitions/cores
    var suggestedShufflePartitions = Math.floor(coreCycles)*cores

    var maxPartitionBytesString = 'spark.conf.set("spark.sql.files.maxPartitionBytes", ' + partitionSize * 1024 * 1024 + ')'
    var suggestedShufflePartitionsString = 'spark.conf.set("spark.sql.shuffle.partitions", ' + suggestedShufflePartitions + ')'

    // rewrite the HTML elements on the page
    document.getElementById('suggestedShufflePartitions').innerText = suggestedShufflePartitionsString;
    document.getElementById('maxPartitionBytes').innerText = maxPartitionBytesString;
}
</script>
<p><style>
.item {
    float: left;
    display: table
}
input {
    float: right
}
.inputDesc {
    float: left;
    width: 250px;
    padding: 0 5 0 0;
}
.textbox {
    width: 100px!important
}
</style></p>


<p>&nbsp;</p>
<form action="javascript:void(0);" name="shuffleForm">
<div class="item">
<div class="inputDesc">Shuffle Size (GB):</div>
<input id="shuffleSize" class="textbox" name="textBox" type="number" value="550" onclick="buttonPressed()" onblur="buttonPressed()"></div>
<br><br>
<div class="item">
<div class="inputDesc">Number of cores in the cluster:</div>
<input id="cores" class="textbox" name="textBox" type="number" value="116" onclick="buttonPressed()" onblur="buttonPressed()"></div>
<br><br>
<div class="item">
<div class="inputDesc">Spark Partition Size (MB):</div>
<input id="partitionSize" class="textbox" name="textBox" type="number" value="128" onclick="buttonPressed()" onblur="buttonPressed()"></div>
<br><br><br>
<div class="item"><b>Here are the suggested Spark configs that you may want to use:</b></div>
<br>
<div class="item">
<div id="suggestedShufflePartitions">spark.conf.set("spark.sql.shuffle.partitions", 4292)</div>
</div>
<br>
<div class="item">
<div id="maxPartitionBytes">spark.conf.set("spark.sql.files.maxPartitionBytes", 134217728)</div>
</div>
</form>
<br>
<br>
The code for this calculator is available <a href="https://github.com/justinbreese/databricks-gems#sparkshufflepartitioncalculatorpy" target="new" rel="noopener noreferrer">here</a>.