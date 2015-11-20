---
layout: "post"
title: "Quick Function: retrieve your call recordings from Twilio"
tags: 
- "code"
date: "2011-08-17 14:38:04"
ogtype: "article"
bodyclass: "post"
---

A recent client project lead me to an interesting problem, normally for retrieving a call recording in twilio, you would store info afterwards, but some calls, that’s not an option. If it’s a recording between two people, like say, a customer and a sales person to review later, you simply set:


    <dial record=true>phone number</dial>
    


There’s no option there to tell it to forward to another URL afterwards, and the tests that I tried using the action argument ended up failing. So here we were, we had the SID for the call, but no recording. Solution? Build a quick function to search the recordings based on call SID using the api… My only problem, the twilio php api doesn’t include an easy way to get recordings, it lets grab call logs, but that’s it. So my answer was to write this function:


    function getRecording($caSID){
        $version = '2010-04-01';
        $sid = "[YOUR API ID]";
        $token = "[YOUR AUTH TOKEN]";
        $url = "https://api.twilio.com/2010-04-01/Accounts/{$sid}/Calls/{$caSID}/Recordings.xml";
        $ch = curl_init();
        curl_setopt($ch, CURLOPT_URL, $url);
        curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
        curl_setopt($ch, CURLOPT_USERPWD, "{$sid}:{$token}");
        curl_setopt($ch, CURLOPT_HTTPAUTH, CURLAUTH_BASIC);
        $output = curl_exec($ch);
        $info = curl_getinfo($ch);
        curl_close($ch);
        $output = simplexml_load_string($output);
        echo "<table>";
        foreach ($output->Recordings->Recording as $recording) {
            echo "<tr>";
            echo "<td>".$recording->Duration." seconds</td>";
            echo "<td>".$recording->DateCreated."</td>";
            echo '<td><audio src="https://api.twilio.com/2010-04-01/Accounts/'.$sid.'/Recordings/'.$recording->Sid.'.mp3" controls preload="auto" autobuffer></audio></td>';
            echo "</tr>";
        }
        echo "</table>";
    }
    getRecordings("ENTER YOUR CALL SID HERE");
    exit;
    


When called, getRecordings will display a table with recordings for the call in question, Ideally, it would be only one recording, but that all depends on how you’ve set up the rest of the call. Tested it on a couple projects now, and it’s worked well, so enjoy.