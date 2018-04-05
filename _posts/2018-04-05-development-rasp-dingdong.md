---
layout: post
title: "[부연설명]DingDong의 노드 서버 코드"
categories:
  - Development
tags:
  - DingDong
  - raspberry pi
---

<br>

### DingDong이라는 초인종 기기에 대해 자세히 알아보겠습니다.

<br>

#### Raspberry pi3 디렉토리 구조입니다.

![rasp_dingdong_structure](https://user-images.githubusercontent.com/31085727/38353685-27da3ba8-38f2-11e8-9eed-76d31cedc15e.JPG)

<br>

#### 핵심 파일인 dingdong.js의 전체 코드입니다.

```javascript
var GPIO = require('onoff').Gpio;
var	button = new GPIO(17, 'in','both');
var	motion = new GPIO(27, 'in','both');
var PythonShell = require('python-shell');
var fetch = require('node-fetch');
var fs = require('fs');
var FormData = require('form-data');
var sleep = require('system-sleep');
var express = require('express');
var app = express();
var firebase = require('firebase');
var admin = require('firebase-admin');
var player = require('play-sound')
({player:'omxplayer'});

var form = new FormData();
var ddkey = null;

var sendFile = function (file){
	console.log("hihi");

    fetch('http://192.168.0.62:8000/notify', {
        method: 'POST',
        body: form
    }).then(function(res){
		console.log("hello");
        return res.json();
    }).then(function(json){
        console.log(json);
		form = new FormData();
    }).catch(function(err){
        console.log(err);
    });
}; 

app.get('/check', function (req, res) {
    var result = null;
    fetch('http://192.168.0.25:8070/ddkey', {
        headers: {'Content-type': 'application/text'},
        method: 'POST',
        // body: "abc"
    }).then(function(res){
        return res.json();
    }).then(function(json){
        // console.log(json);
		console.log(json.key);
		ddkey = json.key;
		fs.writeFile('ddkey/key.txt', ddkey, function(err) {
			if(err) throw err;
			console.log('DDkey saving completed!!');
		});
        res.header("Access-Control-Allow-Origin", "*");
        res.header("Access-Control-Allow-Headers", "X-Requested-With");
        res.send(json);
    }).catch(function(err){
        console.log("err : " + err);
    });
});

app.listen(3000, function () {
	
	var exceptFirst = 0;

	if(fs.existsSync('ddkey/key.txt')){
		fs.readFile('ddkey/key.txt', function (err, data) {
			if(err) throw err;
			ddkey = data.toString();
			//console.log(ddkey);
		});
	}

	setTimeout(function(){
		// Firebase
		// timestamp
	    var serviceAccount = require("/home/pi/Desktop/dingdong/dingdong-34347-firebase-adminsdk-ve0y2-6dee882463.json");
	
	    admin.initializeApp({
	        credential: admin.credential.cert(serviceAccount),
	        databaseURL: "https://dingdong-34347.firebaseio.com/"
		});
	
		if (ddkey){
			//console.log(ddkey);
		    var db = admin.database();
		    var ref = db.ref(ddkey);
		
		    ref.on('value', function(snapshot) {
				//console.log("firebase 변동");
				if(exceptFirst > 0){
					var resVoice = snapshot.val().resVoicePath;
	        		console.log("변동 파일이름 : " + resVoice);
	        	 // console.log("변동 시간 : "+ snapshot.val().timestamp);
					player.play('http://192.168.0.62:8000/resources/' + resVoice, function (err) {
		            	console.log("player");
						if (err) throw err;
					});
				}
				exceptFirst++;
			});
	
		}
		// End Firebase

	}, 100);

	

   	function motion(){
		console.log("motion function 실행")
		PythonShell.run('motion_camera03.py', function(err, results){
		if(err) throw err;
		console.log('motion results: %s' , results);	

        var video = null;
		video = fs.ReadStream('/home/pi/Desktop/dingdong/video/video.mp4');
       	console.log(video);
        if (video != null){
            sendFile(video);
			console.log("비디오보냄");
	       }
		return motion();
		});
			
	}

	function light(err, state){
		if(state == 1 ){
			console.log("버튼안눌림");
		}else if(state ==0){
			console.log("버튼눌림");
//			if(ddkey) console.log(ddkey);
			form.append('deviceID', ddkey);
			PythonShell.run('dingdongsound.py', function(err, results) {
				if(err) throw err;
				console.log('results: %s' , results);
			});

			sleep(2000);
	
			PythonShell.run('camera.py', function(err, results){
				if(err) throw err;
				console.log('results: %s' , results);	
				
				var picture = null;
	        	picture = fs.ReadStream('/home/pi/Desktop/dingdong/capture/image.jpg');
	        
	        	if (picture != null){
	            	form.append('files', picture);
					console.log("사진보냄");
	      			}
			});

			sleep(2000);

			PythonShell.run('whoru.py', function(err, results) {
				if(err) throw err;
				console.log('results: %s' , results);
			});

			PythonShell.run('audio_recording.py', function(err, results) {
				//if(err) throw err;
				console.log('results: %s' , results);
				var audio = null;
		        audio = fs.ReadStream('/home/pi/Desktop/dingdong/test.mp3');
		        
	        	if (audio != null){
					form.append('files', audio);
					//form.append('audio',audio);
					console.log("오디오보냄");
					sendFile(form);
	      			}
			});
		}
	}

	button.watch(light);
	motion();

});        

```
