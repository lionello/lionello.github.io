---
layout: post
title: Lies, Damn Lies, and Statistics
categories: []
date: 2016-06-25 16:20:43.000000000 +03:00
---
Inspired by our current 'post-fact democracy', I had the thought that maybe the internet isn't the democratic safeguard that people believed it to be in the 90s and 00s.

Before the internet, information travelled from person to person. This meant that each person was a firewall, in a way: I get to decide what information I give on and what information I ignore. Depending on how stubborn I am, worst case I might stick to my misinformed opinion even though everyone around me is correctly informed.

But with the advent of the internet, there's now the ability to actively look for information. I can google for anything I want and odds are I will be presented with the information I'm looking for. This way, misinformed individuals can get enough of a clout to sustain their believes, and in turn misinforming others.

The simulation below tries to show both these scenarios. The square represents individuals, 255 individuals for each pixel. Each individual might be uninformed (black), misinformed (red), or correctly informed (green), and the color of the pixel represents to what extent the individuals are informed.

When the simulation starts, the (correct) information spreads to each neighbor, from the middle outward. To spread incorrect information (rumours), click anywhere on the canvas.

The "My bias" value controls how much each individual values his/her opinion over their neighbors'. A value of '1' means an individual values his/her own opinion as much as any other's.

The "ratio" value controls the odds of being presented with correct information. Obviously, if the odds of finding correct information is lower than 50%, there's no chance the truth can prevail. However, it's interesting to see that even at a ratio of 0.51, misinformation completely disappears if we change the bias value to 0: extreme open mindedness, where my neighbors can override my wrong opinion.

Use a large bias to simulate "social media", where my own opinion is being amplified. For example, a bias of 10 can be understood to mean that in addition to the 8 neighbors (neighboring pixels) and my own opinion, an individual can find another 9 opinions similar to his/her own, likely overriding the opinion of the neighborhood. With a large bias, the ratio between informed and misinformed approaches 50/50, eventhough the incorrect information is scarcer!

<script type="text/javascript">
var width = 600,
    height = 600;
(function() {
    var ratio = 0.6;
    var threshold = 255*1-1;
    var threshold2 = 255*0;
    var mybias = 1;
    var speed = 51;

    var context, imageData, imageData2, intervalid, lastsim;
    function draw(){
        var yescount = 0;
        var nocount = 0;
        var data = imageData.data;
        var data2 = imageData2.data;
        var stride = width*4;
        for (var v=stride+4; v<data.length-stride-4; v+=4) {
            var no = data[v+stride] + data[v+4] + data[v-4] + data[v-stride];
            no += data[v+stride+4] + data[v+4-stride] + data[v-4+stride] + data[v-stride-4];
            no += data[v]*mybias;

            var w = v + 1;
            var yes = data[w+stride] + data[w+4] + data[w-4] + data[w-stride];
            yes += data[w+stride+4] + data[w+4-stride] + data[w-4+stride] + data[w-stride-4];
            yes += data[w]*mybias;

            data2[v] = data[v];
            data2[w] = data[w];
            if (yes > no+threshold) {
                // My neighbors vote overwhelmingly Yes; me too
                data2[v] -= speed;
                data2[w] += speed;
            }
            else if (no > yes+threshold) {
                // My neighbors vote overwhelmingly No; me too
                data2[v] += speed;
                data2[w] -= speed;
            }
            else {
                if (yes > threshold2 && no > threshold2) {
                    //               } && yes == no) {

                    // My neighbors are in disagreement; ignore?
                    if (Math.random() < ratio) {
                        data2[v] -= speed;
                        data2[w] += speed;
                    }
                    else {
                        data2[v] += speed;
                        data2[w] -= speed;
                    }
                }
            }
            nocount += data2[v];
            yescount += data2[w];
        }

        // Update the stats
        document.getElementById('yescount').innerText = yescount;
        document.getElementById('yescountp').innerText = Math.round(1000*yescount/(yescount+nocount))/10;
        document.getElementById('nocount').innerText = nocount;
        document.getElementById('nocountp').innerText = Math.round(1000*nocount/(yescount+nocount))/10;
        document.getElementById('dunno').innerText = 255*(width*(height-2)-2) - nocount - yescount;

        context.putImageData(imageData2, 0, 0);

        // Swap the buffers
        var t = imageData;
        imageData = imageData2;
        imageData2 = t;

        // Stop the simulation if the numbers don't change
        var thissim = nocount * 65521 + yescount;
        if (thissim === lastsim) {
            clearInterval(intervalid);
            intervalid = undefined;
        }
        lastsim = thissim;
    }

    function start() {
        if (intervalid === undefined)
            intervalid = setInterval(draw, 1000/document.getElementById("fps").value);
    }

    function reset() {
        for (var v=0; v<imageData.data.length; v+=4) {
            imageData.data[v+0] = 0;//Math.random()*ratio*128;
            imageData.data[v+1] = 0;//Math.random()*(1-ratio)*128;
        }

        imageData.data[(height*width/2+2*width/4)*4+1] = 255;//yes
        var x = Math.round(Math.random()*width);
        var y = Math.round(Math.random()*height);
        imageData.data[(y*width+x)*4] = 255;//no

        start();
    }

    function init() {
        var canvas = document.getElementById('canvas');
        //backbuf = document.createElement("canvas");
        canvas.width = width;
        canvas.height = height;
        context = canvas.getContext("2d");
        //backcontext = backbuf.getContext("2d");
        imageData = context.createImageData(width, height);
        imageData2 = context.createImageData(width, height);

        var coords = [];
        for (var v=0; v<imageData.data.length; v+=4) {
            imageData.data[v+3] = 255;
            imageData2.data[v+3] = 255;
            if (Math.random()<0.001)
                coords.push(v + (Math.random()<ratio?0:1));
        }
        console.log(coords);

        reset();

        canvas.addEventListener("click", function(e){
            imageData.data[(e.layerX+width*e.layerY)*4+0] = 255;
            imageData.data[(e.layerX+width*e.layerY)*4+1] = 0;
        });

        document.getElementById("bias").addEventListener("change", function(e) {
            mybias = 1*e.target.value;
            if (intervalid === undefined)
                reset();
            start();
        });
        document.getElementById("ratio").addEventListener("change", function(e) {
            ratio = 1*e.target.value;
            if (intervalid === undefined)
                reset();
            start();
        });
        document.getElementById("reset").addEventListener("click", reset);
        document.getElementById("fps").addEventListener("change", function(e){
            clearInterval(intervalid);
            intervalid = setInterval(draw, 1000/e.target.value);
        });
    }
    window.addEventListener("load", init);
})();
</script>
<canvas id="canvas"></canvas>
<div>
    <label for="bias">My bias:</label><input type="number" id="bias" value="1" min="0" />
    <label for="ratio">Ratio:</label><input step="0.1" type="number" id="ratio" value="0.6" min="0" max="1" />
    <input type="button" id="reset" value="Reset"/>
    <label for="fps">FPS:</label><input type="number" id="fps" value="100" min="1" max="100" />
</div>
<div>
Informed: <span id="yescount">?</span> (<span id="yescountp">?</span>%)<br/>
Misinformed: <span id="nocount">?</span> (<span id="nocountp">?</span>%)<br/>
Uninformed: <span id="dunno">?</span><br/>
</div>
