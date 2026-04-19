# CI/CD Template Library

Repository nay dung de luu va tai su dung cac file CI/CD da duoc chuan hoa cho nhieu nen tang, nhieu ngon ngu va nhieu delivery model.

## Muc tieu

- Gom template theo `provider -> delivery model -> language -> scenario`.
- De tim, de copy, de mo rong.
- Tach ro `continuous integration`, `continuous delivery` va `continuous deployment`.
- Tach phan mo ta, quy uoc va template thuc te de repo khong bi roi.

## Cau truc de xuat

```text
.
|-- catalog/
|   `-- templates.yml
|-- docs/
|   |-- REPO-STRUCTURE.md
|   `-- TEMPLATE-GUIDELINES.md
|-- templates/
|   |-- github-actions/
|   |   |-- README.md
|   |   |-- continuous-integration/
|   |   |   `-- README.md
|   |   |-- continuous-delivery/
|   |   |   `-- README.md
|   |   `-- continuous-deployment/
|   |       `-- README.md
|   |-- gitlab-ci/
|   |   |-- README.md
|   |   |-- continuous-integration/
|   |   |   `-- README.md
|   |   |-- continuous-delivery/
|   |   |   `-- README.md
|   |   `-- continuous-deployment/
|   |       |-- README.md
|   |       `-- java/
|   |           |-- README.md
|   |           `-- maven-jar-server-tag.yml
|   |-- shared/
|   |   |-- docker/
|   |   |   |-- install/
|   |   |   |   `-- ubuntu/
|   |   |   |       |-- README.md
|   |   |   |       `-- install-docker-and-compose.sh.example
|   |   |   |-- java/
|   |   |   |   |-- README.md
|   |   |   |   `-- maven-jar-openjdk8-jre-alpine.Dockerfile.example
|   |   |   `-- vuejs/
|   |   |       |-- README.md
|   |   |       `-- npm-dist-nginx-alpine.Dockerfile.example
|   |   `-- nginx/
|   |       `-- react-spa-port-3000.conf.example
|   `-- jenkins/
|       |-- README.md
|       |-- continuous-integration/
|       |   `-- README.md
|       |-- continuous-delivery/
|       |   `-- README.md
|       `-- continuous-deployment/
|           `-- README.md
`-- .gitignore
```

## Quy tac dat ten

Mau duong dan:

```text
templates/<provider>/<delivery-model>/<language>/<scenario>.yml
```

Vi du:

- `templates/gitlab-ci/continuous-integration/java/maven-test.yml`
- `templates/github-actions/continuous-delivery/nodejs/npm-build-release-main.yml`
- `templates/gitlab-ci/continuous-deployment/java/maven-jar-server-tag.yml`

## Cach phan loai pipeline

- `continuous-integration`: build, test, lint, scan, package; khong thay doi moi truong dich.
- `continuous-delivery`: tao artifact san sang phat hanh hoac deploy toi staging, UAT, hoac prod nhung con manual gate.
- `continuous-deployment`: sau khi pipeline qua dieu kien, he thong tu dong deploy thang toi moi truong dich ma khong can approve trong pipeline.

Phan `scenario` nen mo ta du 4 y:

1. Build tool hoac artifact: `maven`, `npm`, `poetry`, `docker`
2. Kieu package hoac deploy style: `jar`, `image`, `helm`, `static`
3. Dich den: `server`, `k8s`, `ecs`, `ecr`, `gcr`
4. Dieu kien kich hoat hoac bien the: `tag`, `main`, `mr`, `manual`, `blue-green`

## Danh muc hien co

| Provider | Delivery Model | Language | Scenario | File |
| --- | --- | --- | --- | --- |
| GitLab CI | Continuous Deployment | Java | Maven build + direct server deploy on tag | `templates/gitlab-ci/continuous-deployment/java/maven-jar-server-tag.yml` |
| GitLab CI | Continuous Deployment | React | NPM build + static deploy to server on tag | `templates/gitlab-ci/continuous-deployment/react/npm-static-server-tag.yml` |
| GitLab CI | Continuous Delivery | React | NPM build + manual static deploy to server on tag | `templates/gitlab-ci/continuous-delivery/react/npm-static-server-tag-manual.yml` |

## Cach them template moi

1. Chon dung provider, delivery model va language.
2. Dat ten file theo scenario mo ta ro build, target va trigger.
3. Them mo ta ngan vao `catalog/templates.yml`.
4. Neu co bien bat buoc hoac buoc manual, ghi ro trong README cung cap template.
5. Khong commit secret, token, host cu the, private key.

Tai nguyen dung chung nhu config Nginx, Dockerfile mau, shell snippet, hoac file phu tro co the dat trong `templates/shared/`.

## Tai nguyen dung chung hien co

| Nhom | Ngon ngu | Mo ta | File |
| --- | --- | --- | --- |
| Docker | Ubuntu | Bash script cai Docker Engine va standalone Docker Compose tren Ubuntu | `templates/shared/docker/install/ubuntu/install-docker-and-compose.sh.example` |
| Docker | Java | Multi-stage Maven build, copy JAR sang Alpine runtime va chay bang Java 8 | `templates/shared/docker/java/maven-jar-openjdk8-jre-alpine.Dockerfile.example` |
| Docker | VueJS | Multi-stage npm build, copy `dist` sang Nginx runtime va phuc vu static file | `templates/shared/docker/vuejs/npm-dist-nginx-alpine.Dockerfile.example` |
| Nginx | React SPA | Nginx config mau cho React SPA chay tren port 3000 | `templates/shared/nginx/react-spa-port-3000.conf.example` |

## Goi y mo rong tiep theo

- Them `templates/github-actions/continuous-integration/nodejs/`
- Them `templates/github-actions/continuous-delivery/python/`
- Them `templates/gitlab-ci/continuous-integration/nodejs/`
- Them `templates/gitlab-ci/continuous-deployment/python/`
- Them `templates/jenkins/continuous-delivery/java/`
- Them `examples/` neu muon minh hoa cach inject variable cho tung project
