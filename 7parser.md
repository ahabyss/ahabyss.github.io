---
layout: page
title: Parser
permalink: /parser/
tagline: ["BRING", "THEM", "TO", "THEIR", "REEEEEEEEEEEES"]
tagimg: "https://static-cdn.jtvnw.net/emoticons/v1/114836/1.0"
---


<h2>Chat Parser</h2>
<!--<p>A python tool</p>-->

<div class="manual-post">
 <div class="manual manual-title">
 <strong>Chat Parsing and Analysis</strong>
 </div>
 <p>  
  <div class="manual-content">
    TCR is fundamentally two projects. Without the episode archives created by the TCR parsing tool the extension could not function.
    A majority of message processing is performed at this stage and baked into the archives. This minimizes the processing that the javascript extension must perform, which is important 
    because it must process and output hundreds of chat messages a minute.
    
    
  </div>
 </p>
 
 <p>  
  <div class="manual-content">
    This parsing utiltiy is availiable on <a href="https://github.com/ahabyss/TCRParser">github</a>, It is written in 
    python. To run it you will need to download the <a href="overrustlelogs.net">overrustle logs</a> for the dates of the marathon.
    
  </div>
 </p>
 
 
 <p>  
  <div class="manual-content">
    The episode chat analysis is also done using this utility, these graphs are important for 
    visualizing each anime and episode, ensuring good synchronization. In addition, the episode stats can also show interesting trends and information.
    <br>For example we can see that the meme of REE-ing at snack breaks took hold after only two episodes into the marathon.
    The first two episodes had light REE activity, but every episode afterward (for the entire marathon)
    a predictable spike is found at every snack break.
    <button class="plotlyButton" onclick="javascript:zoomClick()">Zoom to Episodes 2, 3, and 4</button>
    <div id="plotlyMobExample" class="tcrPlotlyPlot"></div>
    <script>
    mobplotly = function() {
        var oReq = new XMLHttpRequest();
        oReq.onload = reqListener;
        oReq.open("get", '../assets/mob1aplot.json', true);
        oReq.send();

        var tcrDataRaw = [];
        var tcrDataRDP = [];
        var tcrDataRDPx = [];
        var tcrDataRDPy = [];
        var plotIndex = ['all', 'ree', 'lul', 'bib', 'pog'];
        var colorIndex = ['#d62728', '#1f77b4', '#2ca02c', '#e377c2', '#8c564b'];
        var legIndex = ['All', 'Ree', 'LUL', 'BibleThump', 'PogChamp'];
        
        function reqListener(e) {
            var treeData = JSON.parse(this.responseText);
            
            for (var i = 0; i < plotIndex.length; i++) {
                var tcrDataRawTemp = [];
                for (var j = 0; j < treeData[0][plotIndex[i]][0].length; j++)
                    tcrDataRawTemp.push([j, treeData[0][plotIndex[i]][0][j]]);
                    
                tcrDataRaw.push(tcrDataRawTemp);
            }
            
            var traces = [];
            var data = [];
            
            for (var i = 0; i < plotIndex.length; i++) {
                //tcrDataRDP.push(RDPsd(tcrDataRaw[i], 0.05));
                tcrDataRDP = tcrDataRaw;
                tcrDataRDPx.push(tcrDataRDP[i].map(function(value, index) {return value[0];}));
                tcrDataRDPy.push(tcrDataRDP[i].map(function(value, index) {return value[1];}));
                
                traces.push({
                    x: tcrDataRDPx[i], 
                    y: tcrDataRDPy[i], 
                    name: legIndex[i],
                    mode: 'lines',
                    type: 'scatter',
                    line: {
                        width: 1.5,
                        simplify: false,
                        color: colorIndex[i]
                    }
                });
                
                data.push(traces[i]);
            }
            
            var tcrEpShapes = [];
            for (var i = 0; i < treeData[1].length; i++) {
                for (var j = 0; j < treeData[1][i].length; j++) {
                    var color;
                    var op;
                    if (treeData[1][i][j][1] == 0) {
                        color = 'rgb(128, 128, 128)';
                        op = 0.1
                    } else {
                        color = 'rgb(200, 200, 0)';
                        op = 0.2
                    }
                    
                    tcrEpShapes.push({
                        type: 'rect',
                        xref: 'x',
                        yref: 'y',
                        x0: treeData[1][i][j][0][0],
                        y0: 0,
                        x1: treeData[1][i][j][0][0] + treeData[1][i][j][0][1],
                        y1: Math.max(...tcrDataRDPy[0]),
                        line: {
                            color: '#000',
                            width: 0
                        },
                        fillcolor: color,
                        opacity: op
                        
                    });
                }
                
                tcrEpShapes.push({
                    type: 'line',
                    x0: treeData[1][i][0][0][0],
                    y0: 0,
                    x1: treeData[1][i][0][0][0],
                    y1: Math.max(...tcrDataRDPy[0]),
                    line: {
                        color: 'rgb(0, 200, 0)',
                        width: 2
                    }
                });
                tcrEpShapes.push({
                    type: 'line',
                    x0: treeData[1][i][treeData[1][i].length - 1][0][0] + treeData[1][i][treeData[1][i].length - 1][0][1],
                    y0: 0,
                    x1: treeData[1][i][treeData[1][i].length - 1][0][0] + treeData[1][i][treeData[1][i].length - 1][0][1],
                    y1: Math.max(...tcrDataRDPy[0]),
                    line: {
                        color: 'rgb(200, 0, 0)',
                        width: 2
                    }
                });
            }
        
            var layout = {
                showlegend: true,
                legend: {
                    x: 0,
                    y: 1,
                    orientation: 'h',
                    traceorder: 'normal',
                    font: {
                        family: '"PT Sans", Helvetica, Arial, sans-serif',
                        color: '#808080'
                    },
                    bgcolor: 'rgba(128,128,128,0.1)',
                    borderwidth: 0
                },
                hovermode: false,
                title: '<b>Mob Psycho 100</b>',
                titlefont: {
                    family: '"PT Sans", Helvetica, Arial, sans-serif',
                    color: '#808080'
                },
                xaxis: {
                    title: 'Seconds',
                    //range: [0, 120],
                    //mirror: 'axis',
                    //showline: true,
                    showticklabels: true,
                    linecolor: '#808080',
                    autotick: true,
                    ticks: 'outside',
                    showticklabels: true,
                    tick0: 0,
                    dtick: 1,
                    tickwidth: 1,
                    tickfont: {
                        family: '"PT Sans", Helvetica, Arial, sans-serif',
                        color: '#808080'
                    },
                    titlefont: {
                        family: '"PT Sans", Helvetica, Arial, sans-serif',
                        color: '#808080'
                    }
                },
                autosize: false,
                width: 740,
                height: 350,
                margin: {
                    l: 50,
                    r: 10,
                    b: 40,
                    t: 40,
                    pad: 0
                },
                yaxis: {
                    title: 'Messages per second',
                    range: [0, Math.max(...tcrDataRDPy[0])],
                    
                    //showline: true,
                    //zeroline: true,
                    showticklabels: true,
                    linecolor: '#808080',
                    autotick: true,
                    ticks: 'outside',
                    showticklabels: true,
                    tick0: 0,
                    tickwidth: 1,
                    rangemode: 'tozero',
                    tickfont: {
                        family: '"PT Sans", Helvetica, Arial, sans-serif',
                        color: '#808080'
                    },
                    titlefont: {
                        family: '"PT Sans", Helvetica, Arial, sans-serif',
                        color: '#808080'
                    }
                },
                paper_bgcolor: 'rgba(0,0,0,0)',
                plot_bgcolor: 'rgba(0,0,0,0)',
                
                shapes: tcrEpShapes
            };
            
            zoomClick = function () {
                Plotly.animate('plotlyMobExample', {
                    data: [{
                    visible: true
                }, {
                    visible: true
                }, {
                    visible: 'legendonly'
                }, {
                    visible: 'legendonly'
                }, {
                    visible: 'legendonly'
                }],
                traces: [0, 1, 2, 3, 4]
                });
                zoom(1600, 6000, 0, Math.max(...tcrDataRDPy[0]));
            };
            
            Plotly.newPlot('plotlyMobExample', data, layout);
            
        }


    function zoom(x1, x2, y1, y2) {
        Plotly.animate('plotlyMobExample', {
            layout: {
                xaxis: {range: [x1, x2]},
                yaxis: {range: [y1, y2]}
            }
        }, {
            transition: {
                duration: 500,
                easing: 'cubic-in-out'
            }
        });
    }
    }
    mobplotly()

    </script>
    <div class="plotlyCap"><b>Toggle emote types by clicking them in the legend.</b> Draw a rectangle to zoom. Drag X axis to pan. Drag at axis edge to scale.
    <br>Vertical green and red bars indicate episode start and end times. Yellow background
    indicates a snack break, light gray background (the majority of the plot) indicates recorded chat areas. 
    Each episode consists of a chat-region, a break-region, and another chat-region.</div>
   
    <p></p>
    We can also see a cool anomaly during the end of Rokka and the Six Flowers 
    in which the number of PogChamps was completely out of proportion from normal. The reason for this? Targeted anti-spoiler 
    spam to prevent spoiling the ending of this mystery anime. For more than 5 minutes chat was dominated by these messages.
    
    <button class="plotlyButton" onclick="javascript:zoomRokkaClick()">Zoom to Ep11 End</button>
    <div id="plotlyRokkaExample" class="tcrPlotlyPlot"></div>
    <script>
    
        var oReq = new XMLHttpRequest();
        oReq.onload = reqListener;
        oReq.open("get", '../assets/rokka1aplot.json', true);
        oReq.send();

        var tcrDataRaw = [];
        var tcrDataRDP = [];
        var tcrDataRDPx = [];
        var tcrDataRDPy = [];
        var plotIndex = ['all', 'ree', 'lul', 'bib', 'pog'];
        var colorIndex = ['#d62728', '#1f77b4', '#2ca02c', '#e377c2', '#8c564b'];
        var legIndex = ['All', 'Ree', 'LUL', 'BibleThump', 'PogChamp'];
        
        function reqListener(e) {
            var treeData = JSON.parse(this.responseText);
            
            for (var i = 0; i < plotIndex.length; i++) {
                var tcrDataRawTemp = [];
                for (var j = 0; j < treeData[0][plotIndex[i]][0].length; j++)
                    tcrDataRawTemp.push([j, treeData[0][plotIndex[i]][0][j]]);
                    
                tcrDataRaw.push(tcrDataRawTemp);
            }
            
            var traces = [];
            var data = [];
            
            for (var i = 0; i < plotIndex.length; i++) {
                //tcrDataRDP.push(RDPsd(tcrDataRaw[i], 0.05));
                tcrDataRDP = tcrDataRaw;
                tcrDataRDPx.push(tcrDataRDP[i].map(function(value, index) {return value[0];}));
                tcrDataRDPy.push(tcrDataRDP[i].map(function(value, index) {return value[1];}));
                
                traces.push({
                    x: tcrDataRDPx[i], 
                    y: tcrDataRDPy[i], 
                    name: legIndex[i],
                    mode: 'lines',
                    type: 'scatter',
                    line: {
                        width: 1.5,
                        simplify: false,
                        color: colorIndex[i]
                    }
                });
                
                data.push(traces[i]);
            }
            
            var tcrEpShapes = [];
            for (var i = 0; i < treeData[1].length; i++) {
                for (var j = 0; j < treeData[1][i].length; j++) {
                    var color;
                    var op;
                    if (treeData[1][i][j][1] == 0) {
                        color = 'rgb(128, 128, 128)';
                        op = 0.1
                    } else {
                        color = 'rgb(200, 200, 0)';
                        op = 0.2
                    }
                    
                    tcrEpShapes.push({
                        type: 'rect',
                        xref: 'x',
                        yref: 'y',
                        x0: treeData[1][i][j][0][0],
                        y0: 0,
                        x1: treeData[1][i][j][0][0] + treeData[1][i][j][0][1],
                        y1: Math.max(...tcrDataRDPy[0]),
                        line: {
                            color: '#000',
                            width: 0
                        },
                        fillcolor: color,
                        opacity: op
                        
                    });
                }
                
                tcrEpShapes.push({
                    type: 'line',
                    x0: treeData[1][i][0][0][0],
                    y0: 0,
                    x1: treeData[1][i][0][0][0],
                    y1: Math.max(...tcrDataRDPy[0]),
                    line: {
                        color: 'rgb(0, 200, 0)',
                        width: 2
                    }
                });
                tcrEpShapes.push({
                    type: 'line',
                    x0: treeData[1][i][treeData[1][i].length - 1][0][0] + treeData[1][i][treeData[1][i].length - 1][0][1],
                    y0: 0,
                    x1: treeData[1][i][treeData[1][i].length - 1][0][0] + treeData[1][i][treeData[1][i].length - 1][0][1],
                    y1: Math.max(...tcrDataRDPy[0]),
                    line: {
                        color: 'rgb(200, 0, 0)',
                        width: 2
                    }
                });
            }
        
            var layout = {
                showlegend: true,
                legend: {
                    x: 0,
                    y: 1,
                    orientation: 'h',
                    traceorder: 'normal',
                    font: {
                        family: '"PT Sans", Helvetica, Arial, sans-serif',
                        color: '#808080'
                    },
                    bgcolor: 'rgba(128,128,128,0.1)',
                    borderwidth: 0
                },
                hovermode: false,
                title: '<b>Rokka-Braves</b>',
                titlefont: {
                    family: '"PT Sans", Helvetica, Arial, sans-serif',
                    color: '#808080'
                },
                xaxis: {
                    title: 'Seconds',
                    //range: [0, 120],
                    //mirror: 'axis',
                    //showline: true,
                    showticklabels: true,
                    linecolor: '#808080',
                    autotick: true,
                    ticks: 'outside',
                    showticklabels: true,
                    tick0: 0,
                    dtick: 1,
                    tickwidth: 1,
                    tickfont: {
                        family: '"PT Sans", Helvetica, Arial, sans-serif',
                        color: '#808080'
                    },
                    titlefont: {
                        family: '"PT Sans", Helvetica, Arial, sans-serif',
                        color: '#808080'
                    }
                },
                autosize: false,
                width: 740,
                height: 350,
                margin: {
                    l: 50,
                    r: 10,
                    b: 40,
                    t: 40,
                    pad: 0
                },
                yaxis: {
                    title: 'Messages per second',
                    range: [0, Math.max(...tcrDataRDPy[0])],
                    
                    //showline: true,
                    //zeroline: true,
                    showticklabels: true,
                    linecolor: '#808080',
                    autotick: true,
                    ticks: 'outside',
                    showticklabels: true,
                    tick0: 0,
                    tickwidth: 1,
                    rangemode: 'tozero',
                    tickfont: {
                        family: '"PT Sans", Helvetica, Arial, sans-serif',
                        color: '#808080'
                    },
                    titlefont: {
                        family: '"PT Sans", Helvetica, Arial, sans-serif',
                        color: '#808080'
                    }
                },
                paper_bgcolor: 'rgba(0,0,0,0)',
                plot_bgcolor: 'rgba(0,0,0,0)',
                
                shapes: tcrEpShapes
            };
            
            zoomRokkaClick = function () {
                Plotly.animate('plotlyRokkaExample', {
                    data: [{
                    visible: true
                }, {
                    visible: 'legendonly'
                }, {
                    visible: 'legendonly'
                }, {
                    visible: 'legendonly'
                }, {
                    visible: true
                }],
                traces: [0, 1, 2, 3, 4]
                });
                zoom(16428, 19190, 0, Math.max(...tcrDataRDPy[0]));
            };
            
            Plotly.newPlot('plotlyRokkaExample', data, layout);
            
        }


    function zoom(x1, x2, y1, y2) {
        Plotly.animate('plotlyRokkaExample', {
            layout: {
                xaxis: {range: [x1, x2]},
                yaxis: {range: [y1, y2]}
            }
        }, {
            transition: {
                duration: 500,
                easing: 'cubic-in-out'
            }
        });
    }

    </script>
  </div>
 </p>
 
 
</div>
