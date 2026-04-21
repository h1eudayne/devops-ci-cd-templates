# Jenkins Templates

Thu muc nay de luu cac `Jenkinsfile` mau theo ngon ngu va use-case, cung voi mot so tai nguyen phu tro rieng cho Jenkins nhu reverse proxy config va install script.

## Cau truc khuyen nghi

```text
templates/jenkins/
  continuous-integration/
  continuous-delivery/
  continuous-deployment/
  install/
    ubuntu/
  reverse-proxy/
```

## Vi du ten file

- `maven-test.Jenkinsfile`
- `docker-k8s-main.Jenkinsfile`
- `install/ubuntu/install-jenkins.sh.example`
- `reverse-proxy/nginx-jenkins-subdomain.conf.example`
