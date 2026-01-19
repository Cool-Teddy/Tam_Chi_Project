<html>
    <head>
        <style>
            .c1
            {
                font-size:50px;
            }
            .c2
            {
                font-size:25px;
            }
            .c3
            {
                font-size:12.5px;
            }
            div#login
            {
                background-image:linear-gradient(45deg,#0ff,#afa);
                width:100%;
                height:100%;
                border:hidden;
                box-sizing:border-box;
            }
            #login h1
            {
                color:black;
                text-align:left;
            }
            #login button
            {
                background-color:#88f;
                border-style:groove;
                border-width:2px;
                border-radius:10px;
            }
            div#food_menu
            {
                background-image:linear-gradient(180deg,#fd0,#ffc);
                width:100%;
                height:100%;
                border:hidden;
                box-sizing:border-box;
                display:none;
            }
            #food_menu h1,h2
            {
                color:black;
                text-align:left;
            }
            #food_menu button
            {
                background-color:#02f;
                color:#eee;
                width:150px;
                height:60px;
                font-size:25;
                text-align:center;
                border-style:groove;
                border-width:2px;
                border-radius:10px;
                box-sizing:border-box;
            }
            #food_menu #suggestion
            {
                width:100%;
                background-image:linear-gradient(45deg,#9f9,#9ff,#f9f);
                border-style:inset;
                border-width:2px;
                border-radius:10px;
                box-sizing: border-box;
            }
            #suggestion p
            {
                text-align:center;
                color:black;
            }
            div#otherbonus
            {
                background-image:linear-gradient(180deg,#fd0,#ffc);
                width:100%;
                height:100%;
                border:hidden;
                box-sizing:border-box;
                display:none;
            }
            #otherbonus button
            {
                background-color:#02f;
                color:#eee;
                width:150px;
                height:60px;
                font-size:25;
                text-align:center;
                border-style:groove;
                border-width:2px;
                border-radius:10px;
                box-sizing:border-box;
            }
            div#statspage
            {
                background-image:linear-gradient(180deg,#afa,#bfb);
                width:100%;
                height:100%;
                border:hidden;
                box-sizing:border-box;
                display:none;
            }
            #statspage #graph
            {
                width:300px;
                height:300px;
                border:hidden;
                border-radius:150px;
                box-sizing:border-box;
                background-image:conic-gradient(red,red 90deg,yellow 90deg);
            }
            #statspage button
            {
                background-color:#02f;
                color:#eee;
                width:150px;
                height:60px;
                font-size:25;
                text-align:center;
                border-style:groove;
                border-width:2px;
                border-radius:10px;
                box-sizing:border-box;
            }
        </style>
    </head>
    <body>
        <div id="login">
            <h1>
                譚仔
            </h1>
            <input id="pn" value="XXXX_XXXX"/>
            <br>
            <button type="button" onclick="redirect('login','food_menu')">
                登入
            </button>
        </div>
        <div id="food_menu">
            <h1>
                菜單
            </h1>
            <h2>
                面底
            </h2>
            米線<input type="radio" id="rice-n" name="noodle"><br>
            紅薯粉<input type="radio" id="potato-n" name="noodle"><br>
            <h2>
                配菜
            </h2>
            生菜<input type="checkbox" id="lettuce"><br>
            魚蛋<input type="checkbox" id="fishball"><br>
            牛肉<input type="checkbox" id="beef"><br>
            鮮菇<input type="checkbox" id="mushroom"><br>
            火腿<input type="checkbox" id="ham"><br>
            炸醬<input type="checkbox" id="mincedpork"><br>
            <h2>
                湯底
            </h2>
            清湯<input type="radio" id="clear_s" name="soup"><br>
            番茄濃湯<input type="radio" id="tomato_s" name="soup"><br>
            麻辣<input type="radio" id="spicy_s" name="soup"><br>
            酸辣湯<input type="radio" id="sousp_s" name="soup"><br>
            <button type="button" onclick="nextp_and_bonus(['beef','ham','mincedpork'])">
                點餐
            </button>
            <br>
            <div id=suggestion>
                <p class="c2">
                    提示：你可以選擇碳足跡較小的食物，例如生菜和魚丸，以換取積分
                </p>
            </div>
        </div>
        <div id="otherbonus">
            <p class="c1">你們有自備餐具嗎？</p>
            <text class="c2">有</text><input type="radio" id="yes" name="ownc">
            <text class="c2">沒有</text><input type="radio" id="no" name="ownc">
            <br>
            <button type="button" onclick="addbonus()">
                提交
            </button>
        </div>
        <div id="statspage">
            <p class="c1">
                用戶積分
            </p>
            <p id="goalmessage"/>
            <div id="graph"> </div>
            <p id="goalsheet"/>
        </div>
    </body>
    <script>
        let bonus=0;
        let targetlist=[500,1000,2500,5000];
        let rewardslist=["二十元折扣","五折優惠","贈送一個碗","免費麵一碗"];
        displaygoal(targetlist,rewardslist);
        function redirect(hide,show)
        {
            document.getElementById(hide).style.display="none";
            document.getElementById(show).style.display="block";
        }
        function deltabonus(delta)
        {
            bonus+=delta;
            let k=0;
            while(targetlist[k]<=bonus)
            {
                k++;
            }
            let targetpoint=targetlist[k];
            graph_deg=(bonus/targetpoint)*360;
            document.getElementById("goalmessage").innerHTML="距離下一個目標："+(targetpoint-bonus)+"/"+targetpoint;
            document.getElementById("graph").style.backgroundImage="conic-gradient(blue,blue "+graph_deg+"deg,yellow "+graph_deg+"deg)";
        }
        function nextp_and_bonus(badfood)
        {
            let foodcheck=true;
            for(let i=0;i<badfood.length;i++)
            {
                if(document.getElementById(badfood[i]).checked)
                {
                    foodcheck=false;
                }
            }
            if(foodcheck===true)
            {
                deltabonus(100);
            }
            redirect("food_menu","otherbonus");
        }
        function addbonus()
        {
            if(document.getElementById("yes").checked)
            {
                 deltabonus(50);
            }
            redirect("otherbonus","statspage");
        }
        function displaygoal(target,rewards)
        {
            let goaltext="<table><tr><th>目標分數</th><th>獎勵</th></tr>";
            for(let i=0;i<target.length;i++)
            {
                goaltext+="<tr><td>"+target[i]+"</td><td>"+rewards[i]+"</td></tr>";
            }
            goaltext+="</table>";
            let redirection="<button type=\"button\" onclick=\"redirect('statspage','food_menu')\">回去點餐</button><button type=\"button\" onclick=\"redirect('statspage','login')\">登出</button>";
            document.getElementById("goalsheet").innerHTML=(goaltext+redirection);
        }
    </script>
</html>
