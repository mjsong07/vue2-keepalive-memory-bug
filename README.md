# vue2-keepalive-memory-bug
The demo can reproduce the vue keepalive memory problem.
See the bug1.png, bug2.png,flow.gif information , we can look the 'a4'page's memory is never tobe garbage collection.
I try to fixed the bug in the vue2.6.14 -> keepalive.js -> render() , see the sourcecode in the path of 'public\cdn\vue-2.6.14.1\src\core\components\keep-alive.js' path,line 121 to 128， now the 'a4'page's memory is never tobe garbage collection
