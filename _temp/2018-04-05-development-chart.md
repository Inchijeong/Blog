---
layout: post
title: "DingDong의 Chart 화면 코드"
categories:
  - Development
tags:
  - DingDong
  - D3.js
  - chart
---

<br>

### 화면에서 보이는 차트 페이지 코드 입니다.

<br>

아래는 JSP의 script 전까지의 코드입니다.

```JSP
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<% // author: Inchijeong %>
<!DOCTYPE html>
<!--[if lt IE 7]><html class="no-js lt-ie9 lt-ie8 lt-ie7" lang="en"> <![endif]-->
<!--[if (IE 7)&!(IEMobile)]><html class="no-js lt-ie9 lt-ie8" lang="en"><![endif]-->
<!--[if (IE 8)&!(IEMobile)]><html class="no-js lt-ie9" lang="en"><![endif]-->
<!--[if (IE 9)]><html class="no-js ie9" lang="en"><![endif]-->
<!--[if gt IE 8]><!--> <html> <!--<![endif]-->
<head>
	<!-- Meta Tags -->
	<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
	
	<title>DingDong</title>   
	
	<meta name="description" content="Insert Your Site Description" /> 
	
	<!-- Mobile Specifics -->
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<meta name="HandheldFriendly" content="true"/>
	<meta name="MobileOptimized" content="320"/>   
	
	<!-- Mobile Internet Explorer ClearType Technology -->
	<!--[if IEMobile]>  <meta http-equiv="cleartype" content="on">  <![endif]-->
	
	<!-- Bootstrap -->
	<link href="/resources/_include/css/bootstrap.min.css" rel="stylesheet">
	
	<!-- Main Style -->
	<link href="/resources/_include/css/main.css" rel="stylesheet">
	
	<!-- Supersized -->
<!-- 	<link href="/resources/_include/css/supersized.css" rel="stylesheet">
	<link href="/resources/_include/css/supersized.shutter.css" rel="stylesheet"> -->
	
	<!-- FancyBox -->
	<link href="/resources/_include/css/fancybox/jquery.fancybox.css" rel="stylesheet">
	
	<!-- Font Icons -->
	<link href="/resources/_include/css/fonts.css" rel="stylesheet">
	
	<!-- Shortcodes -->
	<link href="/resources/_include/css/shortcodes.css" rel="stylesheet">
	
	<!-- Responsive -->
	<link href="/resources/_include/css/bootstrap-responsive.min.css" rel="stylesheet">
	<link href="/resources/_include/css/responsive.css" rel="stylesheet">
	
	<!-- Google Font -->
	<link href='https://fonts.googleapis.com/css?family=Titillium+Web:400,200,200italic,300,300italic,400italic,600,600italic,700,700italic,900' rel='stylesheet' type='text/css'>
	
	<!-- Fav Icon -->
	<link rel="shortcut icon" href="#">
	
	<link rel="apple-touch-icon" href="#">
	<link rel="apple-touch-icon" sizes="114x114" href="#">
	<link rel="apple-touch-icon" sizes="72x72" href="#">
	<link rel="apple-touch-icon" sizes="144x144" href="#">

	<!-- Chart CSS -->
	<link href="/resources/_include/css/chart.css" rel="stylesheet">
		
</head>

<body>
	<%@include file="/WEB-INF/views/common/header.jsp" %>		
	
	<div id="chart" class="page">
    <!-- Title Page -->
     <div class="title-page">
        <h2 class="title">방문 통계</h2>
        <h3 class="title-description">기간별 <span class="emphasis">방문자 통계</span>를 확인하세요.</h3>
    </div>
  	<!-- End Title Page -->
            
    <div class="span6" id="div-chart-btn" >
			<button type="button" class="button chart-btn" id="btn-daily" data-term=1 data-text="하루 동안">일간</button>
			<button type="button" class="button chart-btn" id="btn-weekly" data-term=7 data-text="일주일 동안">주간</button>
			<button type="button" class="button chart-btn" id="btn-monthly" data-term=30 data-text="한달 동안">월간</button>	            	
    </div> 
    
		<div class="clear"></div>
		<div class="div-chart-container">
			<!-- 꺾은선 그래프 -->
			<div id="div-line-chart" class="div-chart">
			    <h4 class="chart-title title">시간별 방문 횟수</h4>	
				<div id="line-chart">
		            <canvas id='c'></canvas>
		            <div class="label">text</div>
				</div>
			</div>
			<div class="clear"></div>
			
			<!-- 성별 그래프 -->
			<div id="div-round-chart" class="div-chart">
				<h4 class="chart-title title">남녀 방문 비율</h4>
				<div id="round-chart" class="chart-wrapper">
				</div>
			</div>

			<!-- 세대 그래프 -->
			<div id="div-round-chart2" class="div-chart">
				<h4 class="chart-title title">연령대별 방문 비율</h4>
				<div id="round-chart2" class="chart-wrapper2">
				</div>
			</div>
		</div>
		
		<div class="clear"></div>
		        
	</div>
	
	<!-- Footer -->
	<footer>
		<p class="credits">&copy;2017. WIPY all right reserved. </p>
	</footer>
	<!-- End Footer -->
	
	<!-- Back To Top -->
	<a id="back-to-top" href="#">
		<i class="font-icon-arrow-simple-up"></i>
	</a>
	<!-- End Back to Top -->
	
	<!-- Js -->
	<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script> <!-- jQuery Core -->
	<script src="/resources/_include/js/bootstrap.min.js"></script> <!-- Bootstrap -->
	<script src="/resources/_include/js/waypoints.js"></script>  <!-- WayPoints -->
	<script src="/resources/_include/js/waypoints-sticky.js"></script>  <!-- Waypoints for Header -->
	<!-- <script src="/resources/_include/js/jquery.isotope.js"></script> --> <!-- Isotope Filter -->
	<script src="/resources/_include/js/jquery.fancybox.pack.js"></script> <!-- Fancybox -->
	<script src="/resources/_include/js/jquery.fancybox-media.js"></script> <!-- Fancybox for Media -->
 	<script src="/resources/_include/js/plugins.js"></script> <!-- Contains: jPreloader, jQuery Easing, jQuery ScrollTo, jQuery One Page Navi -->
	<script src="/resources/_include/js/main.js"></script> <!-- Default JS -->
	<!--jQuery dateFormat -->
	<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/jquery-dateFormat/1.0/jquery.dateFormat.min.js"></script>
	<!-- D3 Js -->
	<script src="https://d3js.org/d3-time-format.v2.min.js"></script>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/3.5.5/d3.min.js"></script>
	<!-- End Js -->
```

<br>

아래는 script 코드입니다.

```javascript
	<!-- Chart Js -->
    <script>
        // 기간 설정
        var term = 7;

        // 사용자 번호
        var userNo = ${user.userNo};
    
                // chart ajax function
                var chartAjax = function(chartText){
                        
                    
                // 기간별 방문 횟수 ajax
                $.ajax({
                    url:"/getVisitCount",
                    type:"GET",
                    data:{userNo:userNo, term:term},
                    dataType: "json"
                }).done(function(visitTimeData){
                        // 기간별 방문 횟수 차트
                        var showVisitCount = function(){
                                
                        var oData = {};
                        for (var i = 1; i < 9; i++) {
                            oData[3 * i - 3 + "~"  + 3 * i] = 0;
                        }
                        for (var i = 0; i < visitTimeData.length; i++){
                            var round = new Date(visitTimeData[i].roundDate).getHours();
                            var population = visitTimeData[i].population;
                            for (var j = 1; j < 9; j++) {
                                if (3 * j - 3 <= round && round < 3 * j) oData[3 * j - 3 + "~"  + 3 * j] += population;		    				
                            } 
                
                    var label = document.querySelector(".label");
                    var c = document.getElementById("c");
                    var ctx = c.getContext("2d");
                    var cw = c.width = 700;
                    var ch = c.height = 350;
                    var cx = cw / 2,
                    cy = ch / 2;
                    var rad = Math.PI / 180;
                    var frames = 0;
        
                    ctx.lineWidth = 1;
                    ctx.fillStyle = "#ccc"; */
                    ctx.strokeStyle = "black";
                    ctx.fillStyle = "#929DAF";
                    ctx.font = "18px Sans-serif";
        
                    var grd = ctx.createLinearGradient(0, 0, 0, ch);
                    grd.addColorStop(1, "hsla(167,72%,60%,0)");
        
                    var valuesRy = [];
                    var propsRy = [];
                    for (var prop in oData) {
        
                    valuesRy.push(oData[prop]);
                    propsRy.push(prop);
                    }
        
        
                    var vData = 4;
                    var hData = valuesRy.length;
                    var offset = 50.5; //offset chart axis
                    var chartHeight = ch - 2 * offset;
                    var chartWidth = cw - 2 * offset;
                    var t = 1 / 7; // curvature : 0 = no curvature 
                    var speed = 2; // for the animation
        
                    var A = {
                    x: offset,
                    y: offset
                    }
                    var B = {
                    x: offset,
                    y: offset + chartHeight
                    }
                    var C = {
                    x: offset + chartWidth,
                    y: offset + chartHeight
                    }
        
                    // CHART AXIS -------------------------
                    ctx.beginPath();
                    ctx.moveTo(A.x, A.y);
                    ctx.lineTo(B.x, B.y);
                    ctx.lineTo(C.x, C.y);
                    ctx.stroke();
        
                    // vertical ( A - B )
                    var aStep = (chartHeight - 50) / (vData);
        
                    var Max = Math.ceil(arrayMax(valuesRy) / 10) * 10;
                    var Min = Math.floor(arrayMin(valuesRy) / 10) * 10;
                    var aStepValue = (Max - Min) / (vData);
                    var verticalUnit = aStep / aStepValue;
        
                    var a = [];
                    ctx.textAlign = "right";
                    ctx.textBaseline = "middle";
                    for (var i = 0; i <= vData; i++) {
        
                    if (i == 0) {
                        a[i] = {
                        x: A.x,
                        y: A.y + 25,
                        val: Max
                        }
                    } else {
                        a[i] = {}
                        a[i].x = a[i - 1].x;
                        a[i].y = a[i - 1].y + aStep;
                        a[i].val = a[i - 1].val - aStepValue;
                    }
                    drawCoords(a[i], 3, 0);
                    }
        
                    //horizontal ( B - C )
                    var b = [];
                    ctx.textAlign = "center";
                    ctx.textBaseline = "hanging";
                    var bStep = chartWidth / (hData + 1);
        
                    for (var i = 0; i < hData; i++) {
                    if (i == 0) {
                        b[i] = {
                        x: B.x + bStep,
                        y: B.y,
                        val: propsRy[0]
                        };
                    } else {
                        b[i] = {}
                        b[i].x = b[i - 1].x + bStep;
                        b[i].y = b[i - 1].y;
                        b[i].val = propsRy[i]
                    }
                    drawCoords(b[i], 0, 3)
                    }
        
                    function drawCoords(o, offX, offY) {
                    ctx.beginPath();
                    ctx.moveTo(o.x - offX, o.y - offY);
                    ctx.lineTo(o.x + offX, o.y + offY);
                    ctx.stroke();
        
                    ctx.fillText(o.val, o.x - 2 * offX, o.y + 2 * offY);
                    }
                    //----------------------------------------------------------
        
                    ctx.strokeStyle = "#21b6a8";
                    ctx.fillStyle = "#b6212d";
                    ctx.font = "14px monospace";
                    
                    // DATA
                    var oDots = [];
                    var oFlat = [];
                    var i = 0;
        
                    for (var prop in oData) {
                    oDots[i] = {}
                    oFlat[i] = {}
        
                    oDots[i].x = b[i].x;
                    oFlat[i].x = b[i].x;
        
                    oDots[i].y = b[i].y - oData[prop] * verticalUnit - 25;
                    oFlat[i].y = b[i].y - 25;
        
                    oDots[i].val = oData[b[i].val];
        
                    i++
                    }
        
        
                    ///// Animation Chart ///////////////////////////
                    //var speed = 3;
                    function animateChart() {
                    requestId = window.requestAnimationFrame(animateChart);
                    frames += speed; //console.log(frames)
                    ctx.clearRect(60, 0, cw, ch - 60);
        
                    for (var i = 0; i < oFlat.length; i++) {
                        if (oFlat[i].y > oDots[i].y) {
                        oFlat[i].y -= speed;
                        }
                    }
                    drawCurve(oFlat);
                    for (var i = 0; i < oFlat.length; i++) {
                        ctx.fillText(oDots[i].val, oFlat[i].x, oFlat[i].y - 25);
                        ctx.beginPath();
                        ctx.arc(oFlat[i].x, oFlat[i].y, 3, 0, 2 * Math.PI);
                        ctx.fill();
                        }
        
                    if (frames >= Max * verticalUnit) {
                        window.cancelAnimationFrame(requestId);
        
                    }
                    }
                    requestId = window.requestAnimationFrame(animateChart);
        
                    /////// EVENTS //////////////////////
                    c.addEventListener("mousemove", function(e) {
                    label.innerHTML = "";
                    label.style.display = "none";
                    this.style.cursor = "default";
        
                    var m = oMousePos(this, e);
                    for (var i = 0; i < oDots.length; i++) {
        
                        output(m, i);
                    }
        
                    }, false);
        
                    function output(m, i) {
                    ctx.beginPath();
                    ctx.arc(oDots[i].x, oDots[i].y, 20, 0, 2 * Math.PI);
                    if (ctx.isPointInPath(m.x, m.y)) {
                        label.style.display = "block";
                        label.style.top = (m.y + 350) + "px";
                        label.style.left = (m.x + 570) + "px";
                        label.innerHTML = "<strong>" + propsRy[i] + "시</strong>: " + valuesRy[i] + "명";
                        c.style.cursor = "pointer";
                    }
                    }
        
                    // CURVATURE
                    function controlPoints(p) {
                    // given the points array p calculate the control points
                    var pc = [];
                    for (var i = 1; i < p.length - 1; i++) {
                        var dx = p[i - 1].x - p[i + 1].x; // difference x
                        var dy = p[i - 1].y - p[i + 1].y; // difference y
                        // the first control point
                        var x1 = p[i].x - dx * t;
                        var y1 = p[i].y - dy * t;
                        var o1 = {
                        x: x1,
                        y: y1
                        };
        
                        // the second control point
                        var x2 = p[i].x + dx * t;
                        var y2 = p[i].y + dy * t;
                        var o2 = {
                        x: x2,
                        y: y2
                        };
        
                        // building the control points array
                        pc[i] = [];
                        pc[i].push(o1);
                        pc[i].push(o2);
                    }
                    return pc;
                    }
        
                    function drawCurve(p) {
        
                    var pc = controlPoints(p); // the control points array
        
                    ctx.beginPath();
                    //ctx.moveTo(p[0].x, B.y- 25);
                    ctx.lineTo(p[0].x, p[0].y);
                    // the first & the last curve are quadratic Bezier
                    // because I'm using push(), pc[i][1] comes before pc[i][0]
                    ctx.quadraticCurveTo(pc[1][1].x, pc[1][1].y, p[1].x, p[1].y);
        
                    if (p.length > 2) {
                        // central curves are cubic Bezier
                        for (var i = 1; i < p.length - 2; i++) {
                        ctx.bezierCurveTo(pc[i][0].x, pc[i][0].y, pc[i + 1][1].x, pc[i + 1][1].y, p[i + 1].x, p[i + 1].y);
                        }
                        // the first & the last curve are quadratic Bezier
                        var n = p.length - 1;
                        ctx.quadraticCurveTo(pc[n - 1][0].x, pc[n - 1][0].y, p[n].x, p[n].y);
                    }
        
                    //ctx.lineTo(p[p.length-1].x, B.y- 25);
                    ctx.stroke();
                    ctx.save();
                    ctx.fillStyle = grd;
                    ctx.fill();
                    ctx.restore();
                    }
        
                    function arrayMax(array) {
                    return Math.max.apply(Math, array);
                    };
        
                    function arrayMin(array) {
                    return Math.min.apply(Math, array);
                    };
        
                    function oMousePos(canvas, evt) {
                    var ClientRect = canvas.getBoundingClientRect();
                    return { //objeto
                        x: Math.round(evt.clientX - ClientRect.left),
                        y: Math.round(evt.clientY - ClientRect.top)
                    }
                    }
                
                        };
                        // 기간별 방문 횟수 차트 끝
                        
                        var $divLineChart = $("#div-line-chart");
                        var visitTimeDataSize = visitTimeData.length;
                        
                        switch(visitTimeDataSize){
                            case 0 :
                                $divLineChart.html("<h4 class='chart-title title'>" + chartText + "의 방문자가 존재하지 않습니다.</h4>");
                                break;
                            default :
                                $divLineChart.html('<div id="line-chart"><canvas id="c"></canvas><div class="label">text</div></div><h4 class="chart-title title">시간별 방문 횟수</h4>');
                                showVisitCount();
                                break;
                        }				  		
                        
                });
                // 기간별 방문 횟수 ajax 끝
            
            
            // 방문자 성별 가져오기 ajax
                $.ajax({
                    url:"/getGenderCount",
                    type:"GET",
                    data:{userNo:userNo, term:term},
                    dataType: "json"
                }).done(function(genderData){
                    // 방문자 성별 차트
                    var showGender = function(){
                    
                        var dataset = [];
                        var dataset2 = [];
                        var totalSum = 0;
                        for (var i = 0; i < genderData.length; i++){
                            totalSum += genderData[i].population;
                        }
                        for (var i = 0; i < genderData.length; i++){
                            var gender = genderData[i].gender;
                            var popu = genderData[i].population;
                            dataset[i] = popu;
                            dataset2[i] = (gender == "male")? "남자" : "여자";
                        }
                    
                    var colors = ['#9e0142', '#d53e4f', '#f46d43', '#fdae61', '#fee08b', '#e6f598', '#abdda4', '#66c2a5', '#3288bd', '#5e4fa2'];
        
                    var width = document.querySelector('.chart-wrapper').offsetWidth;
                    var height = document.querySelector('.chart-wrapper').offsetHeight;
                    var minOfWH = Math.min(width, height) / 2;
                    var initialAnimDelay = 300;
                    var arcAnimDelay = 150;
                    var arcAnimDur = 3000;
                    var secDur = 1000;
                    var secIndividualdelay = 150;
        
                    var radius = void 0;
        
                    // calculate minimum of width and height to set chart radius
                    if (minOfWH > 200) {
                    radius = 200;
                    } else {
                    radius = minOfWH;
                    }
        
                    // append svg
                    var svg = d3.select('.chart-wrapper').append('svg').attr({
                    'width': width,
                    'height': height,
                    'class': 'pieChart'
                    }).append('g');
        
                    svg.attr({
                    'transform': 'translate(' + width / 2 + ', ' + height / 2 + ')'
                    });
        
                    // for drawing slices
                    var arc = d3.svg.arc().outerRadius(radius * 0.6).innerRadius(radius * 0.45);
        
                    // for labels and polylines
                    var outerArc = d3.svg.arc().innerRadius(radius * 0.85).outerRadius(radius * 0.85);
        
                    // d3 color generator
                    // let c10 = d3.scale.category10();
        
                    var pie = d3.layout.pie().value(function (d) {
                    return d;
                    });
        
                    var draw = function draw() {
        
                    svg.append("g").attr("class", "lines");
                    svg.append("g").attr("class", "slices");
                    svg.append("g").attr("class", "labels");
        
                    // define slice
                    var slice = svg.select('.slices').datum(dataset).selectAll('path').data(pie);
                    slice.enter().append('path').attr({
                        'fill': function fill(d, i) {
                        return colors[i];
                        },
                        'd': arc,
                        'stroke-width': '25px',
                        'transform': function transform(d, i) {
                        return 'rotate(-180, 0, 0)';
                        }
                    }).style('opacity', 0).transition().delay(function (d, i) {
                        return i * arcAnimDelay + initialAnimDelay;
                    }).duration(arcAnimDur).ease('elastic').style('opacity', 1).attr('transform', 'rotate(0,0,0)');
        
                    slice.transition().delay(function (d, i) {
                        return arcAnimDur + i * secIndividualdelay;
                    }).duration(secDur).attr('stroke-width', '5px');
        
                    var midAngle = function midAngle(d) {
                        return d.startAngle + (d.endAngle - d.startAngle) / 2;
                    };
        
                    var text = svg.select(".labels").selectAll("text").data(pie(dataset));
        
                    text.enter().append('text').attr('dy', '0.35em').style("opacity", 0).style('fill', function (d, i) {
                        return colors[i];
                    }).text(function (d, i) {
                        return dataset[i] + "명(" + Math.round(dataset[i] / totalSum * 100) + "%)";
                    }).attr('transform', function (d) {
                        // calculate outerArc centroid for 'this' slice
                        var pos = outerArc.centroid(d);
                        // define left and right alignment of text labels 							
                        pos[0] = radius * (midAngle(d) < Math.PI ? 1 : -1);
                        return 'translate(' + pos + ')';
                    }).style('text-anchor', function (d) {
                        return midAngle(d) < Math.PI ? "start" : "end";
                    }).transition().delay(function (d, i) {
                        return arcAnimDur + i * secIndividualdelay;
                    }).duration(secDur).style('opacity', 1);
        
                    var polyline = svg.select(".lines").selectAll("polyline").data(pie(dataset));
        
                    polyline.enter().append("polyline").style("opacity", 0.5).attr('points', function (d) {
                        var pos = outerArc.centroid(d);
                        pos[0] = radius * 0.95 * (midAngle(d) < Math.PI ? 1 : -1);
                        return [arc.centroid(d), arc.centroid(d), arc.centroid(d)];
                    }).transition().duration(secDur).delay(function (d, i) {
                        return arcAnimDur + i * secIndividualdelay;
                    }).attr('points', function (d) {
                        var pos = outerArc.centroid(d);
                        pos[0] = radius * 0.95 * (midAngle(d) < Math.PI ? 1 : -1);
                        return [arc.centroid(d), outerArc.centroid(d), pos];
                    });
                    };
        
                    draw();
        
                    var button = document.querySelector('button');
        
                    // 범주
                        var legendRectSize=20;
                        var legendSpacing=7;
                        var legendHeight=legendRectSize+legendSpacing;
                        
                        
                        var legend=svg.selectAll('.legend')
                                .data(dataset2)
                                .enter()
                                .append('g')
                                .attr({
                                    class:'legend',
                                    transform:function(d,i){
                                        //Just a calculation for x & y position
                                        return 'translate(-35,' + ((i*legendHeight)-65) + ')';
                                    }
                                });
                        legend.append('rect')
                                .attr({
                                    width:legendRectSize,
                                    height:legendRectSize,
                                    rx:20,
                                    ry:20,
                                    y:35,
                                })
                                .style('fill',function (d, i){
                                    return colors[i];
                                })
                                .style('stroke',function (d, i){
                                    return colors[i];
                                });            
                        
                        legend.append('text')
                                .attr({
                                    x:30,
                                    y:55
                                })
                                .text(function(d){
                                    return d;
                                }).style({
                                    fill:'#929DAF',
                                    'font-size':'18px',
                                    'font-weight':'bold'
                                });
                            
                        var replay = function replay() {
                        
                            d3.selectAll('.slices').transition().ease('back').duration(500).delay(0).style('opacity', 0).attr('transform', 'translate(0, 250)').remove();
                            d3.selectAll('.lines').transition().ease('back').duration(500).delay(100).style('opacity', 0).attr('transform', 'translate(0, 250)').remove();
                            d3.selectAll('.labels').transition().ease('back').duration(500).delay(200).style('opacity', 0).attr('transform', 'translate(0, 250)').remove();
                        
                            setTimeout(draw, 800);
                        };
                        
                    };
                    // 방문자 성별 차트 끝
                    
                    var genderDataSize = genderData.length;
                        var $divRoundChart = $("#div-round-chart");
                    
                        switch(genderDataSize){
                            case 0 :
                                $divRoundChart.text("");
                                $divRoundChart.removeClass("div-chart");
                                break;
                            default :
                                $divRoundChart.addClass("div-chart");
                                $divRoundChart.html('<div id="round-chart" class="chart-wrapper"></div><h4 class="chart-title title">남녀 방문 비율</h4>');
                                showGender();
                                break;
                        }		    		
                    
                });
                // 방문자 성별 가져오기 ajax 끝

                
                // 방문자 세대 가져오기 ajax
                $.ajax({
                    url:"/getGeneration",
                    type:"GET",
                    data:{userNo:userNo, term:term},
                    dataType: "json"
                }).done(function(generationData){
                    
                    // 방문자 세대 차트
                    var showGeneration = function(){
                        
                        var dataset = [];
                        var dataset2 = [];
                        var lastIndex = -1;
                        var sumPop = 0;
                        var totalSum = 0;
                        
                        for (var i = 0; i < generationData.length; i++){
                            var data = generationData[i];
                            var gene = data.generation;
                            var popu = data.population;
                            
                            totalSum += popu;
                            
                            if(gene < 50) {
                                dataset[i] = popu;
                                dataset2[i] = gene + "대";
                            }
                            else if(gene >= 50 && lastIndex < 0){
                                sumPop += popu;
                                lastIndex = i;
                            }
                            else {
                                sumPop += popu;
                            }
                            if(lastIndex > 0){
                                dataset[lastIndex] = sumPop; 
                                dataset2[lastIndex] = "50대 이상";
                            }
                            // console.log(dataset);
                            // console.log(dataset2);
                            
                        }
                                
                    var colors = ['#9e0142', '#d53e4f', '#f46d43', '#fdae61', '#fee08b', '#e6f598', '#abdda4', '#66c2a5', '#3288bd', '#5e4fa2'];
        
                    var width = document.querySelector('.chart-wrapper2').offsetWidth;
                    var height = document.querySelector('.chart-wrapper2').offsetHeight;
                    var minOfWH = Math.min(width, height) / 2;
                    var initialAnimDelay = 300;
                    var arcAnimDelay = 150;
                    var arcAnimDur = 3000;
                    var secDur = 1000;
                    var secIndividualdelay = 150;
        
                    var radius = void 0;
        
                    // calculate minimum of width and height to set chart radius
                    if (minOfWH > 200) {
                    radius = 200;
                    } else {
                    radius = minOfWH;
                    }
        
                    // append svg
                    var svg = d3.select('.chart-wrapper2').append('svg').attr({
                    'width': width,
                    'height': height,
                    'class': 'pieChart'
                    }).append('g');
        
                    svg.attr({
                    'transform': 'translate(' + width / 2 + ', ' + height / 2 + ')'
                    });
        
                    // for drawing slices
                    var arc = d3.svg.arc().outerRadius(radius * 0.6).innerRadius(radius * 0.45);
        
                    // for labels and polylines
                    var outerArc = d3.svg.arc().innerRadius(radius * 0.85).outerRadius(radius * 0.85);
        
                    // d3 color generator
                    // let c10 = d3.scale.category10();
        
                    var pie = d3.layout.pie().value(function (d) {
                    return d;
                    });
        
                    var draw = function draw() {
        
                    svg.append("g").attr("class", "lines");
                    svg.append("g").attr("class", "slices");
                    svg.append("g").attr("class", "labels");
        
                    // define slice
                    var slice = svg.select('.slices').datum(dataset).selectAll('path').data(pie);
                    slice.enter().append('path').attr({
                        'fill': function fill(d, i) {
                        return colors[i];
                        },
                        'd': arc,
                        'stroke-width': '25px',
                        'transform': function transform(d, i) {
                        return 'rotate(-180, 0, 0)';
                        }
                    }).style('opacity', 0).transition().delay(function (d, i) {
                        return i * arcAnimDelay + initialAnimDelay;
                    }).duration(arcAnimDur).ease('elastic').style('opacity', 1).attr('transform', 'rotate(0,0,0)');
        
                    slice.transition().delay(function (d, i) {
                        return arcAnimDur + i * secIndividualdelay;
                    }).duration(secDur).attr('stroke-width', '5px');
        
                    var midAngle = function midAngle(d) {
                        return d.startAngle + (d.endAngle - d.startAngle) / 2;
                    };
        
                    var text = svg.select(".labels").selectAll("text").data(pie(dataset));
        
                    text.enter().append('text').attr('dy', '0.35em').style("opacity", 0).style('fill', function (d, i) {
                        return colors[i];
                    }).text(function (d, i) {
                        return dataset[i] + "명(" + Math.round(dataset[i] / totalSum * 100) + "%)";
                    }).attr('transform', function (d) {
                        // calculate outerArc centroid for 'this' slice
                        var pos = outerArc.centroid(d);
                        // define left and right alignment of text labels 							
                        pos[0] = radius * (midAngle(d) < Math.PI ? 1 : -1);
                        return 'translate(' + pos + ')';
                    }).style('text-anchor', function (d) {
                        return midAngle(d) < Math.PI ? "start" : "end";
                    }).transition().delay(function (d, i) {
                        return arcAnimDur + i * secIndividualdelay;
                    }).duration(secDur).style('opacity', 1);
        
                    var polyline = svg.select(".lines").selectAll("polyline").data(pie(dataset));
        
                    polyline.enter().append("polyline").style("opacity", 0.5).attr('points', function (d) {
                        var pos = outerArc.centroid(d);
                        pos[0] = radius * 0.95 * (midAngle(d) < Math.PI ? 1 : -1);
                        return [arc.centroid(d), arc.centroid(d), arc.centroid(d)];
                    }).transition().duration(secDur).delay(function (d, i) {
                        return arcAnimDur + i * secIndividualdelay;
                    }).attr('points', function (d) {
                        var pos = outerArc.centroid(d);
                        pos[0] = radius * 0.95 * (midAngle(d) < Math.PI ? 1 : -1);
                        return [arc.centroid(d), outerArc.centroid(d), pos];
                    });
                    };
        
                    draw();
        
                    var button = document.querySelector('button');
        
                    // 범주
                        var legendRectSize=20;
                        var legendSpacing=7;
                        var legendHeight=legendRectSize+legendSpacing;
                        
                        
                        var legend=svg.selectAll('.legend')
                                .data(dataset2)
                                .enter()
                                .append('g')
                                .attr({
                                    class:'legend',
                                    transform:function(d,i){
                                        //Just a calculation for x & y position
                                        return 'translate(-35,' + ((i*legendHeight)-65) + ')';
                                    }
                                });
                        legend.append('rect')
                                .attr({
                                    width:legendRectSize,
                                    height:legendRectSize,
    // 			                  rx: -10,
                                    x:-25,
                                    ry:20
                                })
                                .style('fill',function (d, i){
                                    return colors[i];
                                })
                                .style('stroke',function (d, i){
                                    return colors[i];
                                });            
                        
                        legend.append('text')
                                .attr({
                                    x:5,
                                    y:15
                                })
                                .text(function(d){
                                    return d;
                                }).style({
                                    fill:'#929DAF',
                                    'font-size':'20px',
                                    'font-weight':'bold'			                  
                                });
                            
                        var replay = function replay() {
                        
                            d3.selectAll('.slices').transition().ease('back').duration(500).delay(0).style('opacity', 0).attr('transform', 'translate(0, 250)').remove();
                            d3.selectAll('.lines').transition().ease('back').duration(500).delay(100).style('opacity', 0).attr('transform', 'translate(0, 250)').remove();
                            d3.selectAll('.labels').transition().ease('back').duration(500).delay(200).style('opacity', 0).attr('transform', 'translate(0, 250)').remove();
                        
                            setTimeout(draw, 800);
                        };
                    };
                    // 방문자 세대 차트 끝
                        
                    var $divRoundChart2 = $("#div-round-chart2");
                    var generationDataSize = generationData.length;
                    
                    switch(generationDataSize){
                        case 0 :
                            $divRoundChart2.text("");
                            $divRoundChart2.removeClass("div-chart");
                            break;
                        default :
                            $divRoundChart2.addClass("div-chart");
                            $divRoundChart2.html('<div id="round-chart2" class="chart-wrapper2"></div><h4 class="chart-title title">연령대별 방문 비율</h4>');
                            showGeneration();
                            break;
                    }		    			
                        
                        
                });
                // 방문자 세대 가져오기 ajax 끝
            
                }; // chart ajax function end
            
                $("#div-chart-btn button").click(function(e){
                    e.preventDefault();
                    term = $(this).data("term");
                    var chartText = $(this).data("text");
                    chartAjax(chartText);
                });	
                
                $btnDaily = $("#btn-monthly");
                $btnDaily.trigger("focus").trigger("click");
    </script>   
</body>
</html>
```

