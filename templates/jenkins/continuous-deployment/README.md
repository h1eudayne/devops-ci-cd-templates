# Jenkins Continuous Deployment

Thu muc nay danh cho `Jenkinsfile` tu dong deploy vao moi truong dich sau khi pipeline pass.

## Template san co

- `maven-jar-linux-deploy.Jenkinsfile`: Build Maven, copy file `.jar` len thu muc deploy tren Linux, kill process cu va chay lai bang `java -jar`.

## Luu y

- Template nay phu hop voi app Java dong goi `.jar` va deploy truc tiep len may Linux.
- Pipeline dang dung `sudo`, `nohup`, va process kill theo ten file artifact, vi vay node Jenkins can co quyen phu hop.
- Can chinh lai cac bien nhu `appUser`, `appName`, `appVersion`, `folderDeploy`, va `label` theo moi truong thuc te.
