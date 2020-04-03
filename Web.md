document.form.item1.options[form.item1.selectedIndex].value;

-----------------------연린창에 값넘기기(부모창)

function push() { 
window.opener.form.x.value =real_x+mapx;
window.opener.form.y.value =real_y+mapy;
window.opener.form.x1.value=x1;
window.opener.form.x2.value=x2;
window.opener.form.x3.value=x3;
window.opener.form.y1.value=y1;
window.opener.form.y2.value=y2;
window.opener.form.y3.value=y3;

window.close();   


}
-----------------------링크 클릭값 넘기기
function delete1(value){
form.no.value=value;
document.form.action= "./basket_delete.php";
document.form.submit();
}

<a href=# class=text1 onclick=delete1(value); value=$row[no]>클릭</a>


-----------------------form값을 2개 이상 페이지로 전송할경우
<input type=button value="등록"
onclick="action='./login1.php';submit();">


-----------------------버튼클릭시 값을 줄경우
<input type=button value수정" onclick="javascript:location.href='./login.php?mode=input&mode2=hahaha'">



-----------------------자바스크립으로 전송버튼 누르는경우
function zipchange1(){
document.form.action= "./login.php";
document.form.submit();
}

-----------------------값전송하기
<script>
function a(){
bbb="kim";
ccc="kim2";
ddd="kim3";
location.href ="test1.htm?Bcode="+bbb+"&Ccode="+ccc;
}
</script>

<input type=button value=클릭 onclick="a();">
-----------------------타겟으로 값을 넘길경우1
function itemchange4(){
document.form.item4.options[form.item4.selectedIndex].value;
document.form.action= "./top.php";
document.form.target= "top";            //타켓사용
document.form.submit();
}

-----------------------타겟으로 값을 넘길경우2
<form method="get" name="form" target="xxxx">


-----------------------부모창 리로드 값 넘길경우 ㅋㅋ --+(새로고침하기)
<script>
//window.opener.location.reload();
opener.parent.location.reload();
window.close();
</script>

-----------------------프레임 나눌경우 값주기 (확인안해봤는데..;;)
<frameset>
<frame src=top.html?receive=$receive>
...
</frameset>



----------------------걍~ 값넘겨줄때

<script>
location.href='abc.php?a=1&b=1&c=<?=1?>';
</script>

출처: https://develop88.tistory.com/entry/자바스크립트로-각종-값넘기는방법 [왕 Blog]

출처: https://develop88.tistory.com/entry/자바스크립트로-각종-값넘기는방법 [왕 Blog]

출처: https://develop88.tistory.com/entry/자바스크립트로-각종-값넘기는방법 [왕 Blog]
