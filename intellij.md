빌드 오류시
========

In menu "Build > Rebuild project".
In menu "File > Invalidate caches / Restart... > Invalidate and Restart".
Remove last installed/enabled plugins if any.
Check dependencies (especially cyclic-dependencies) in "File > Project Structure... > Modules"
The last chance: make backup & remove .idea folder from your project directory and create new project from scratch.

Cash 지우기
==========
file -> invalidat cashes/restart : project 전체 cash 삭제 후 재시동
gradle 에서 의존성 투입 몇개를 일부러 오류낸 다음  reimport 후 다시 의존성 투입 및 재기동 : gradle 내에서 reimport

단축키
=====
cmd + backspace

톰캣 설치하기
==========
https://whitepaek.tistory.com/13
