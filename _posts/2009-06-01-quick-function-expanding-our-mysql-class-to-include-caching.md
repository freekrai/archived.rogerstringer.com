---
layout: "post"
title: "Quick Function: Expanding our MYSQl class to include caching"
tags: 
- "code"
date: "2009-06-01 08:52:16"
ogtype: "article"
bodyclass: "post"
---

Building on last week’s [article ](/2009/05/close-mysql-connections-php) about closing mysql connections, I wanted to pass on this further expanded class, that allows for caching on heavier queries. To start, create a folder in your main web directory called “_cache” Here is our file, save it as DbConn.php same as last time:


    define('DBCACHE_PATH', realpath('.').'/_cache/');
    class DbConn {
        public $host = "localhost";
        public $user = "";
        public $pass = "";
        public $DB  = "";
        public $conn;
        public function __construct($host,$user,$pass,$db) {
            $this->host = $host;
            $this->user = $user;
            $this->pass = $pass;
            $this->DB = $db;
            $this->conn = mysql_connect($this->host, $this->user, $this->pass) or die("Couldn't connection to $host");
            mysql_select_db($this->DB,$this->conn);
        }
        public function __destruct(){
            mysql_close($this->conn);
        }   
        public function query($sql,$cache=true,$cachetime = "" ){
            if(!$cachetime) $cachetime = (60*60*1);
            if( $cache ){
                $oCache = new DBCache($sql, $cachetime );
                if (!$oCache->Check()) {
                    $res = $this->_query($sql);
                    $oCache->Set($res);
                }
                $res = $oCache->Get();
            }else{
                $res = $this->_query($sql);
            }
            return $res;
        }
        public function nonquery($sql){
            $res = myquery($sql,$this->conn);
            mysql_free_result($res);
            return $results;
        }
        private function _query($sql){
            $results = array();
            $res = myquery($sql,$this->conn);
            while( $row = mysql_fetch_assoc($res) ){
                $results[] = $row;
            }
            mysql_free_result($res);
            return $results;
        }
    }
    class DBCache {
        public $sFile;
        public $sFileLock;
        public $iCacheTime;
        public $oCacheObject;
        function __construct($sKey, $iCacheTime) {
            $this->sFile = DBCACHE_PATH.md5($sKey).".txt";
            $this->sFileLock = "$this->sFile.lock";
            $iCacheTime >= 10 ? $this->iCacheTime = $iCacheTime : $this->iCacheTime = 10;
        }
        function Check() {
            $val = 0;
            if (file_exists($this->sFileLock)) return true;
            $val = (file_exists($this->sFile) && ($this->iCacheTime == -1 || time() - filemtime($this->sFile) <= $this->iCacheTime));
            if( !$val ){ if (file_exists($this->sFile)) { unlink($this->sFile); } }
            return $val;
        }
        function Reset(){ if (file_exists($this->sFile)) { unlink($this->sFile); } }
        function Exists() { return (file_exists($this->sFile) || file_exists($this->sFileLock)); }
        function Set($vContents) {
            if (!file_exists($this->sFileLock)) {
                if (file_exists($this->sFile)) { copy($this->sFile, $this->sFileLock); }
                $oFile = fopen($this->sFile, 'w');
                fwrite($oFile, serialize($vContents));
                fclose($oFile);
                if (file_exists($this->sFileLock)) {unlink($this->sFileLock);}
                return true;
            }       
            return false;
        }
        function Get() {
            if (file_exists($this->sFileLock)) {
                return unserialize(file_get_contents($this->sFileLock));
            } else {
                return unserialize(file_get_contents($this->sFile));
            }
        }
        function ReValidate() { touch($this->sFile); }
    }
    


You may notice that we have a few new functions here this time. We still start same as we did last time:


    $dbconn = new DbConn("localhost","mydbuser","mydbpass","mydbname");
    define( "DBH", $dbconn->conn );
    


But now, we can use a few extra functions, namely query and nonquery. You use query like so:


    $results = $dbconn->query("Select * from people",true,(60*60*2));
    


This will return an array containing the records from the people table and will also tell the class to cache it for 2 hours (60 minutes * 60 seconds * 2). This way, we are cleaning up our results, and storing them away. Especially useful for larger tables, that can be slower. Once you return results, you would loop through the array and display them:


    $results = $dbconn->query("Select * from people",true,(60*60*2));
    foreach($results as $row){
        echo $row['name'];
    }
    


Now, if you decided not to do any caching, we would do this:


    $results = $dbconn->query("Select * from people",false);
    foreach($results as $row){
        echo $row['name'];
    }
    


This will return the array of results, without doing any caching. The last function, is a function called nonquery, this function is used for executing queries that wouldn’t return results, like insert or update queries.


    $dbconn->nonquery("update people set name='Roger' where name='Wayne';");
    


Anyhow, this may seem kind of long winded and probably not all that convenient, but when you start getting a larger site with more traffic and start to worry about server load, then knowing about caching your results can be a handy tool.