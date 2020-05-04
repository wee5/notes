# js：jquery中ajax的写法



* 在jQuery中AJAX的写法有3种，$ajax，$post，$get这三种。其中$post和$get是简易写法，高层的实现，在调用他们的时候，会运行底层封装好的$ajax



* get方式

  * ```js
    get方式一
    
    $.get("test.cgi", { name: "John", time: "2pm" },
      function(data){
        alert("Data Loaded: " + data);
      });
    get方式二
    
    $.get("test.php", function(data){
      alert("Data Loaded: " + data);
    });
    get方式三
    
    $.get("test.php", { name: "John", time: "2pm" } );
    get的参数应该可以放在url上，还没试过
    ```

* post方式

  * ```js
    post方式一
    
    $.post("test.php", { name: "John", time: "2pm" } );
    post方式二
    
    $.post("test.php", $("#testform").serialize());
    post方式三
    
    $.post("test.php", function(data){
       alert("Data Loaded: " + data);
     });
    ```

* ajax方式

  * ```js
    $.ajax({
        type: "POST",
        dataType: "json",
        url: "",
        data: ""
        success: function(data){
    
    	},
        error: function(msg){
    
        }
    });
    ```

  * 


