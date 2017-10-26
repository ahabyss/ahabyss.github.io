---
layout: page
title : Home
---

<h2>Welcome to Twitch Chat Replay
<a href="https://chrome.google.com/webstore/detail/twitch-chat-replay/kckepnldahdjnfmlpalbgjekincelkkk?hl=en"><img src="assets/TCRIcon128.png" style="float: right"></a></h2>

  <div class="tagline">
  <span class="page-tagline" style="text-transform: uppercase; display:block"><em>
  <img class="ttvemote" src="https://static-cdn.jtvnw.net/emoticons/v1/88/1.0">
  STRONGEST
  <img class="ttvemote" src="https://static-cdn.jtvnw.net/emoticons/v1/88/1.0">
  MAN
  <img class="ttvemote" src="https://static-cdn.jtvnw.net/emoticons/v1/88/1.0">
  In
  <img class="ttvemote" src="https://static-cdn.jtvnw.net/emoticons/v1/88/1.0">
  THE
  <img class="ttvemote" src="https://static-cdn.jtvnw.net/emoticons/v1/88/1.0">
  WORLD
  <img class="ttvemote" src="https://static-cdn.jtvnw.net/emoticons/v1/88/1.0">
  BTW
  <img class="ttvemote" src="https://static-cdn.jtvnw.net/emoticons/v1/88/1.0">
  </em></span>
  <!--<br>-->
  </div>


<div class="manual-post">
 <div class="manual manual-title">
 <strong>Info</strong>
 </div>
 <p>  
  <div class="manual-content">
Twitch Chat Replay is a chat replay tool for the <a href="https://www.twitch.tv/twitchpresents">TwitchPresents</a> anime marathons. Using TCR you can
enjoy all of the enthusiasm and energy that thousands of viewers bring to your favorite anime. 
<br>
<div style='margin-top:0.5em'>TCR only displays chat messages. You must watch the shows with a separate video player or website.</div>
  </div>
 </p>
</div>

<div class="manual-post">
 <div class="manual manual-title">
  <strong>Usage<!-- - <em style="font-weight: 400">See video <a href="/features#vlc">here</a></em>--></strong>
 </div>
 <p>  
 
    <video id="vlcvid" width="740" controls>
        <source src="https://giant.gfycat.com/PastelDeficientChinchilla.webm" type="video/webm">
    </video>
 
  <div class="manual-content">
   Manually:
   <ol>
    <li>Prepare your video player</li> 
    <li>Start the TCR extension</li>
    <li>Using the Anime drop down choose the anime</li>
    <li>Using the Episode drop down choose the episode</li>
    <li>Using the seek bar make sure TCR is synced with your video player</li>
    <li>Press play!</li>
   </ol>
  </div>
 </p>
</div>
   
<div class="manual-post">
 <div class="manual manual-title">
  <strong>VLC Connection</strong>
 </div>
 <p>  
  <div class="manual-content">
   As of extension version 0.9.6 VLC http connection is supported. When using this feature TCR will automatically sync 
   chat with playback.
   Upon startup, details of the file VLC is playing are detected and TCR loads the appropriate chat log 
   so all you need to do is press 'play' <img class="ttvemote" src="https://static-cdn.jtvnw.net/emoticons/v1/41/1.0" srcset="https://static-cdn.jtvnw.net/emoticons/v1/41/2.0 2x" alt="Kreygasm"> 
  </div>
 </p>
</div>

<div class="manual-post">
 <div class="manual manual-title">
  <strong>Interactive Visualizations</strong>
 </div>
 <p>  
  <div class="manual-content">
   If you like playing around with data check out some chat data examples on the <a href="/parser/">parser</a> page.
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
        var plotIndex = ['all'];
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
            
                //'#1f77b4',  // muted blue
                //'#ff7f0e',  // safety orange
                //'#2ca02c',  // cooked asparagus green
                //'#d62728',  // brick red
                //'#9467bd',  // muted purple
                //'#8c564b',  // chestnut brown
                //'#e377c2',  // raspberry yogurt pink
                //'#7f7f7f',  // middle gray
                //'#bcbd22',  // curry yellow-green
                //'#17becf'   // blue-teal
            
            Plotly.newPlot('plotlyMobExample', data, layout);
            
        }

    }
    mobplotly()

    </script>
  </div>
 </p>
</div>
  
  <div class="manual-post">
 <div class="manual manual-title">
  <strong>Latest Update</strong>
 </div>
 </div>





