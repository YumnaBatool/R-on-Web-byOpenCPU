# R-on-Web-byOpenCPU
R code on web using OpenCPU server. You can use the app at http://ronwebsite.tumblr.com/

STEPS:

1- Include opencpu and jquery files in the header. You can download them from internet and use them they are easily avaiable 
<script type="text/javascript" src="../jquery-1.7.2.min.js"></script>
<script type="text/javascript" src="../opencpu-0.5.js"></script>

2- Define html. In my case it was,
<h1><b>Basic plotting</b></h1>

    <b>n (count) </b> <input type="number" id="nfield" value="1000">
    
    <b>distribution type</b> <select id="distfield">
      <option>normal</option>
      <option>uniform</option>
    </select> 
    
    <br />
    <button id="submitbutton" type="button">Submit</button>
    <br /><br />
    <div id="plotdiv"></div>      

    <br /><br />
    <h1><b>Posting Code Snippets</b></h1>
    <p>It shows R output. It will work only for R base functions as I have included R base library only.</p>
    
    <textarea id="input" rows="4" cols="50">
    x = rnorm(10000)
    mean(x)  
    </textarea> 
    <br /> <button id="submitcode" type="button">Submit Code</button>
    <pre><code id="output"></code></pre>
    
3- Define opencpu server calls in javascript as below. You can learn it from OpenCPU tutorials. 
    $(document).ready(function(){

      $("#submitbutton").on("click", function(){
        ocpu.seturl("https://public.opencpu.org/ocpu/library/appdemo/R")

        var nfield = parseInt($("#nfield").val());
        var distfield = $("#distfield").val();
        
        var req = $("#plotdiv").rplot("randomplot", {
          n : nfield,
          dist : distfield
        })

        

      });


      
      $("#submitcode").on("click", function(){
        ocpu.seturl("https://public.opencpu.org/ocpu/library/base/R")

        var mysnippet = new ocpu.Snippet($("#input").val());
        

        var req = ocpu.call("identity", {
            "x" : mysnippet
        }, function(session){
            session.getConsole(function(outtxt){
                $("#output").text(outtxt); 
            });
        });
        
      })
      
      
        
       
    });    


