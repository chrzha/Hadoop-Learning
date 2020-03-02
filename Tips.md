1. maven无法下载依赖时，可手动下载jar包，然后install在本地repo
mvn install:install-file -Dfile=util-bridges.jar -DgroupId=com.liferay.portal -DartifactId=util-bridges -Dversion=6.2.10 -Dpackaging=jar
