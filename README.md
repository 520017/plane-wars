<!DOCTYPE HTML>
<html>
    <head>
        <title>please enter your title</title>
        <meta charset="utf-8">
        <meta name="Author" content="slowly_">
        <style type='text/css'>
            *{ margin:0; padding:0;font-family:"Microsoft yahei";}
            body{ overflow:hidden;}
        </style>
    </head>
    <body>
         
        <script type="text/javascript">
        /*单例模式*/
             
            window.onload = function(){
 
                Game.exe();
 
            };
             
            var Game = {
 
                //启动程序
                exe : function(){
                    document.body.style.background = '#000';
                    var oDiv = document.createElement('div');
                        oDiv.id = 'GameBox';
                        oDiv.style.cssText = 'width:300px;height:500px;border:10px solid #fff;margin:50px auto;text-align:center;position:relative;overflow:hidden;';
                    document.body.appendChild( oDiv );
                    this.init();
                },
 
                score : 0,
 
                ifEnd : false,
                 
                //初始化
                init : function(){
                    var This = this;
                    var oDiv = document.getElementById('GameBox');
                    oDiv.innerHTML = '';
                    Game.score = 0;
                    Game.ifEnd = false;
                    var oH = document.createElement('h1');
                        oH.innerHTML = '打小姗';
                        oH.style.cssText = 'color:#fff;font-size:26px;font-weight:normal;padding-top:50px;';
                        oDiv.appendChild( oH );
                    for (var i=0;i<4;i++ )
                    {
                        var oP = document.createElement('p');
                            oP.index = i;
                            oP.style.cssText = 'font-size:14px;color:#000;width:150px;height:40px;margin:50px auto;text-align:center;line-height:40px;background:#fff;cursor:pointer;';
                        var html = '';
                        oP.onmouseenter = function(){
                            this.style.background = '#f60';
                            this.style.color = '#fff';
                        };
                        oP.onmouseleave = function(){
                            this.style.background = '#fff';
                            this.style.color = '#000';
                        };
                        oP.onclick = function(e){
                            e = e||window.event;
                            This.start( this.index , oDiv , e );
                        };
                        switch ( i )
                        {
                            case 0:
                                html = '简单难度';
                                break;
                            case 1:
                                html = '中等难度';
                                break;
                            case 2:
                                html = '困难难度';
                                break;
                            case 3:
                                html = '开挂';
                                oP.style.color = '#f00';
                                oP.style.fontWeight = 'bold';
                                oP.onmouseenter = function(){
                                    this.style.background = '#f60';
                                };
                                oP.onmouseleave = function(){
                                    this.style.background = '#fff';
                                };
                                break;
                        }
                        oP.innerHTML = html;
                        oDiv.appendChild(oP);
                    };
                },
 
                //游戏开始
                start : function( index , oGameBox , e ){
                    oGameBox.innerHTML = '';
 
                    var oS = document.createElement('span');
                        oS.innerHTML = this.score;
                        oS.style.cssText = 'position:absolute;left:15px;top:10px;font-size:14px;color:#fff';
                    oGameBox.appendChild( oS );
                    this.plane( oGameBox , e , index);
                    this.enemy( oGameBox , oS , index );
                },
 
                //关于飞机
                plane : function( oGameBox , e , index ){
                    var x = e.pageX,
                        y = e.pageY;
                    var oPlane = new Image(); // document.createElement('img');
                        oPlane.src = 'https://ooo.0o0.ooo/2017/06/15/5941db1f125e4.png';
                        oPlane.width = 60;
                        oPlane.height = 36;
                        oPlane.id = 'plane';
                     
                    var tY = oGameBox.offsetTop+parseInt(oGameBox.style.borderWidth)+oPlane.height/2;
                    var lX = oGameBox.offsetLeft+parseInt(oGameBox.style.borderWidth)+oPlane.width/2;
                    window.onresize = function(){
                        lX = oGameBox.offsetLeft+parseInt(oGameBox.style.borderWidth)+oPlane.width/2;
                    };
                    var top = y-tY;
                    var left = x-lX;
                        oPlane.style.cssText = 'display:block;position:absolute;top:'+top+'px;left:'+left+'px;';
                    oGameBox.appendChild( oPlane );
 
                    var leftMin = - oPlane.width/2;
                    var leftMax = oGameBox.clientWidth - oPlane.width/2;
                    var topMin = 0;
                    var topMax = oGameBox.clientHeight - oPlane.height;
 
                    document.onmousemove = function(e){
                        if ( !Game.ifEnd )
                        {
                            e = e || window.event;
                            var top = e.pageY - tY;
                            var left = e.pageX - lX;
                             
                            top = Math.min( top , topMax );
                            top = Math.max( top , topMin );
                            left = Math.min( left , leftMax );
                            left = Math.max( left , leftMin );
                            /*
                            if(top>topMax)top=topMax;
                            if(top<topMin)top=topMin;
                            if(left>leftMax)left=leftMax;
                            if(left<leftMin)left=leftMin;
                            */
                            oPlane.style.left = left + 'px';
                            oPlane.style.top = top + 'px';
                        }
                         
                    };
                    this.biubiubiu( oPlane , oGameBox , index );
                },
                 
                //飞机子弹
                biubiubiu : function( oPlane , oGameBox , index ){
 
                    var speed;
                    switch ( index )
                    {
                        case 0:
                            speed = 200;
                            break;
                        case 1:
                            speed = 300;
                            break;
                        case 2:
                            speed = 400;
                            break;
                        case 3:
                            speed = 10;
                            break;
                    }
 
                    this.BiuTimer = setInterval( function(){
                        var oBiu = new Image();
                            oBiu.src = 'https://ooo.0o0.ooo/2017/06/15/5941db5b19ce4.png';
                            oBiu.width = 6;
                            oBiu.height = 22;
                            oBiu.className = 'biubiubiu';
                        var top = oPlane.offsetTop - oBiu.height + 3;
                        var left = oPlane.offsetLeft + oPlane.width/2 - oBiu.width/2;
                            oBiu.style.cssText = 'position:absolute;top:'+top+'px;left:'+left+'px';
                        oGameBox.appendChild( oBiu );
 
                        oBiu.timer = setInterval( function(){
                            if ( !oBiu.parentNode )
                            {
                                clearInterval( oBiu.timer );
                            }
                            oBiu.style.top = oBiu.offsetTop - 10 + 'px';
                            if ( oBiu.offsetTop < - oBiu.height )
                            {
                                clearInterval( oBiu.timer );
                                oBiu.parentNode.removeChild( oBiu );
                            }
                        } , 13 );
 
                    } , speed); //  ******************  子弹速度
                },
 
                //敌军
                enemy : function(oGameBox , oS , index){
                     
                    var a , x;
                    switch ( index )
                    {
                        case 0:
                            a = 1;
                            x = 500;
                            break;
                        case 1:
                            a = 3;
                            x = 300;
                            break;
                        case 2:
                            a = 5;
                            x = 200;
                            break;
                        case 3:
                            a = 5;
                            x = 100;
                            break;
                    }
 
                    this.EnemyTimer = setInterval( function(){
                        var oEnemy = new Image();
                            oEnemy.src = 'https://ooo.0o0.ooo/2017/06/15/5941dc33a338e.png';
                            oEnemy.width = 23;
                            oEnemy.height = 30;
 
                        var lMin = 0;
                        var lMax = oGameBox.clientWidth - oEnemy.width;
                        var left = Math.random() * (lMax-lMin) + lMin;
                        oEnemy.style.cssText = 'position:absolute;top:'+(-oEnemy.height)+'px;left:'+left+'px;'
                        oGameBox.appendChild( oEnemy );
 
                        var b = Math.random() * a + 1;
 
                        oEnemy.timer = setInterval(function(){
                            oEnemy.style.top = oEnemy.offsetTop + b + 'px'; // ***********敌军下落速度 
                            if ( oEnemy.offsetTop >= oGameBox.clientHeight )
                            {
                                clearInterval( oEnemy.timer );
                                oEnemy.parentNode.removeChild( oEnemy );
                            };
                        },13);
 
                        //和子弹的碰撞检测
                        var allBiu = Game.getClass('biubiubiu');
                        oEnemy.pzBiu = setInterval(function(){
                             
                            for (var i=0;i<allBiu.length;i++ )
                            {
                                if ( Game.boom( oEnemy , allBiu[i] ) )
                                {
                                    Game.score ++;
                                    oS.innerHTML = Game.score;
                                    oEnemy.src = 'img/boom.png';
                                    clearInterval( oEnemy.pzBiu );
                                    clearInterval( oEnemy.pzPlane );
                                    allBiu[i].parentNode.removeChild( allBiu[i] );
                                    setTimeout(function(){
                                        if ( oEnemy.parentNode )
                                        {
                                            oEnemy.parentNode.removeChild( oEnemy );
                                        }
                                    },300);
                                    break;
                                }
                            }
                        },50);
 
                        //和战机的碰撞检测
                        var oPlane = document.getElementById('plane');
                        oEnemy.pzPlane = setInterval(function(){
                             
                            if ( Game.ifEnd )
                            {
                                clearInterval( oEnemy.pzPlane );
                            }
 
                            if ( Game.boom( oEnemy , oPlane ) )
                            {
                                Game.ifEnd = true;
                                clearInterval( oEnemy.pzPlane );
                                clearInterval( Game.BiuTimer );
                                clearInterval( Game.EnemyTimer );
                                oEnemy.src = 'https://ooo.0o0.ooo/2017/06/15/5941dbe965c49.png';
                                oPlane.src = 'https://ooo.0o0.ooo/2017/06/15/5941db862a6b3.png';
                                setTimeout(function(){
                                    Game.over( oGameBox );
                                },1000);
                            }
                        },50);
                    } , x );  // *********** 敌军生成速度
                },
 
                //碰撞检测
                boom : function( obj1 , obj2 ){
                     
                    var T1 = obj1.offsetTop;
                    var B1 = T1 + obj1.clientHeight;
                    var L1 = obj1.offsetLeft;
                    var R1 = L1 + obj1.clientWidth;
 
                    var T2 = obj2.offsetTop;
                    var B2 = T2 + obj2.clientHeight;
                    var L2 = obj2.offsetLeft;
                    var R2 = L2 + obj2.clientWidth;
 
                    if ( R2 < L1 || L2 > R1 || B2 < T1 || T2 > B1 )
                    {
                        return false; // 没撞上
                    }
                    else
                    {
                        return true; // 撞上了
                    }
                },
 
                //游戏结束
                over : function(oGameBox){
                    oGameBox.innerHTML = '';
                    var oDiv = document.createElement('div');
                        oDiv.style.cssText = 'width:200px;height:400px;margin:50px;background:#fff;';
                    var oT = document.createElement('h3');
                        oT.innerHTML = 'Game Over';
                        oT.style.cssText = 'padding-top:50px;;'
                    var oP1 = document.createElement('p');
                        oP1.innerHTML = '您的得分是：' + '<span style="color:#f00;font-weight:bold;">' + this.score + '</span>';
                        oP1.style.cssText = 'font-size:16px;color:#000';
                     
                    var oRestart = document.createElement('div');
                        oRestart.style.cssText = 'width:100px;height:40px;font-size:14px;text-align:center;line-height:40px;color:#000;background:#990;margin:20px auto;cursor:pointer;';
                        oRestart.innerHTML = '重新开始';
                    oRestart.onclick = function(){
                        Game.init();
                    };
                     
                    oDiv.appendChild( oT );
                    oDiv.appendChild( oP1 );
                    oDiv.appendChild( oRestart );
                    oGameBox.appendChild( oDiv );
                },
 
 
                //getClass方法
                getClass : function( cName , parent ){
                    parent = parent || document;
                    if ( document.getElementsByClassName )
                    {
                        return parent.getElementsByClassName(cName);
                    }
                    else
                    {
                        var all = parent.getElementsByTagName('*');
                        var arr = [];
                        for ( var i=0;i<all.length;i++ )
                        {
                            var arrClass = all.className.split(' ');
                            for ( var j=0;j<arrClass.length;j++ )
                            {
                                if ( arrClass[j] == cName )
                                {
                                    arr.push( all[i] );
                                    break;
                                }
                            }
                        }
                        return arr;
                    }
                },
 
 
            };
 
        </script>
    </body>
</html>
