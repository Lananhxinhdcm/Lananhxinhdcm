

<! DOCTYPE Lananh PUBLIC "- // W3C // DTD Lananh4.0 Chuyển tiếp // EN" >
< Lananh >

< ĐẦU >
    < TITLE > Tài liệu mới </ TITLE >
    < META  NAME = " Trình tạo " CONTENT = " EditPlus " >
    < META  NAME = " Tác giả " CONTENT = "" >
    < META  NAME = " Từ khóa " CONTENT = "" >
    < META  NAME = " Mô tả " CONTENT = "" >
    < style >
        Lananh ,
        cơ thể {
            chiều cao :  100 % ;
            đệm :  0 ;
            lề :  0 ;
            nền :  # 000 ;
        }

        canvas {
            vị trí : tuyệt đối;
            chiều rộng :  100 % ;
            chiều cao :  100 % ;
        }
    </ style >
</ HEAD >

< CƠ THỂ >
    < canvas  id = " pinkboard " > </ canvas >
    < script >
        / *
       * Cài đặt
       * /
        var  settings  =  {
            hạt : {
                chiều dài : 500 ,  // lượng hạt tối đa
                thời lượng : 2 ,  // thời lượng hạt tính bằng giây
                vận tốc : 100 ,  // vận tốc hạt tính bằng pixel / giây
                effect : - 0.75 ,  // chơi với cái này để có hiệu ứng đẹp
                size : 30 ,  // kích thước hạt tính bằng pixel
            } ,
        } ;

        / *
         * RequestAnimationFrame polyfill bởi Erik Möller
         * /
        ( function  ( )  {  var  b  =  0 ;  var  c  =  [ "ms" ,  "moz" ,  "webkit" ,  "o" ] ;  for  ( var  a  =  0 ;  a  <  c . length  &&  ! window . requestAnimationFrame ;  + + a )  {  window . requestAnimationFrame  =  window [ c [ a] + "RequestAnimationFrame"]; window.cancelAnimationFrame = window[c[a] + "CancelAnimationFrame"] || window[c[a] + "CancelRequestAnimationFrame"] } if (!window.requestAnimationFrame) { window.requestAnimationFrame = function (h, e) { var d = new Date().getTime(); var f = Math.max(0, 16 - (d - b)); var g = window.setTimeout(function () { h(d + f) }, f); b = d + f; return g  }  }  if  ( ! window . huỷAnimationFrame )  {  cửa sổ . hủyAnimationFrame  =  function  ( d )  {  clearTimeout ( d )  }  }  } ( ) ) ;

        / *
         * Lớp điểm
         * /
        var  Point  =  ( function  ( )  {
            function  Point ( x ,  y )  {
                cái này . x  =  ( typeof  x  ! ==  'undefined' ) ? x : 0 ;
                cái này . y  =  ( typeof  y  ! ==  'undefined' ) ? y : 0 ;
            }
            Chỉ điểm . nguyên mẫu . clone  =  function  ( )  {
                return  new  Point ( this . x ,  this . y ) ;
            } ;
            Chỉ điểm . nguyên mẫu . length  =  function  ( length )  {
                if  ( typeof  length  ==  'undefined' )
                    trả về  Toán học . sqrt ( this . x  *  this . x  +  this . y  *  this . y ) ;
                cái này . chuẩn hóa ( ) ;
                cái này . x  * =  chiều dài ;
                cái này . y  * =  chiều dài ;
                trả lại  cái này ;
            } ;
            Chỉ điểm . nguyên mẫu . normalize  =  function  ( )  {
                var  length  =  this . chiều dài ( ) ;
                cái này . x  / =  chiều dài ;
                cái này . y  / =  chiều dài ;
                trả lại  cái này ;
            } ;
             điểm trở lại ;
        } ) ( ) ;

        / *
         * Lớp hạt
         * /
        var  Particle  =  ( function  ( )  {
            function  Particle ( )  {
                cái này . position  =  new  Point ( ) ;
                cái này . vận tốc  =  new  Point ( ) ;
                cái này . gia tốc  =  new  Point ( ) ;
                cái này . tuổi  =  0 ;
            }
            Hạt . nguyên mẫu . khởi tạo  =  function  ( x ,  y ,  dx ,  dy )  {
                cái này . chức vụ . x  =  x ;
                cái này . chức vụ . y  =  y ;
                cái này . vận tốc . x  =  dx ;
                cái này . vận tốc . y  =  dy ;
                cái này . gia tốc . x  =  dx  *  cài đặt . các hạt . hiệu lực ;
                cái này . gia tốc . cài đặt y  =  dy  *  . các hạt . hiệu lực ;
                cái này . tuổi  =  0 ;
            } ;
            Hạt . nguyên mẫu . update  =  function  ( deltaTime )  {
                cái này . chức vụ . x  + =  này . vận tốc . x  *  deltaTime ;
                cái này . chức vụ . y  + =  cái này . vận tốc . y  *  deltaTime ;
                cái này . vận tốc . x  + =  này . gia tốc . x  *  deltaTime ;
                cái này . vận tốc . y  + =  cái này . gia tốc . y  *  deltaTime ;
                cái này . age  + =  deltaTime ;
            } ;
            Hạt . nguyên mẫu . draw  =  function  ( bối cảnh ,  hình ảnh )  {
                chức năng  dễ dàng ( t )  {
                    return  ( - t )  *  t  *  t  +  1 ;
                }
                var  size  =  hình ảnh . chiều rộng  *  dễ dàng ( cái này . tuổi  /  cài đặt . hạt . thời gian ) ;
                bối cảnh . globalAlpha  =  1  -  cái này . tuổi  /  cài đặt . các hạt . thời hạn ;
                bối cảnh . drawImage ( image ,  this . position . x  -  size  /  2 
