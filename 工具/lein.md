# lien用法
lein deps: 读及合并写在profiles中的依赖。
The deps task reads and merges:
	1. from the project.clj 
	2. and the ~/.lein/profiles.clj 
files all the dependencies (of the simple-sample project) 
and verifies if they have already been cached in the local maven repository.

 If the task returns without messages about not being able to retrieve the two new artifacts your installation is correct, otherwise go back and double check that you did everything right.
 
 
repl:
(clojure.lang.RT/loadLibrary org.opencv.core.Core/NATIVE_LIBRARY_NAME)

加载库　库名/本地库名
